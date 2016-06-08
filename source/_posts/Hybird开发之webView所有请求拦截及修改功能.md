title: Android-hybird开发之webView所有请求拦截及修改功能

date: 2015-09-27 08:54:16

tags: [Android,hybird,webview]

------------------------


> `Hybird`本身的意思是混合的，其实用在这里，就是指的是原生和`Web`开发混合起来，各展所长。

* 最近在做`Android` `hybird`方面的研究和开发。有一些关于`WebView`开发的心得体会，特分享于大家。
* 之前我在这方面有两篇相关博客，分别介绍了[Android中webview与javascript交互方法](http://renkangke.github.io/2015/07/21/Android%E4%B8%ADwebview%E4%B8%8Ejavascript%E4%BA%A4%E4%BA%92%E6%96%B9%E6%B3%95/)以及[Android JS Debug技巧](http://renkangke.github.io/2015/08/27/Android-JS-Debug%E6%8A%80%E5%B7%A7/)。这两篇文章对一些`WebView`的基本操作、使用以及调试进行了总结。
* 今天我会对在开发Web`离线包`遇到的问题、对`webView`请求`请求拦截以及调整`这些方面做介绍。

## Hybird离线包

因为`hybird`方案使用`webView`加载，所以速度上有点慢，我们采用在本地使用离线包的形式、这样加载来提升速度，从而不受网络的影响。

这样做就需要使用 `file:///`协议来加载本地离线`web`页面，这样使用起来发现会导致一个问题，服务端去拿存储进去的`cookie`值，在大部分`Android`手机和部分`iPhone`手机拿不到，


```java
// 加载本地文件
mWebView.loadUrl("file:///android_asset/test.html");
```

经过分析，发现应该是因为协议的问题，我们用的是`file:///`协议,而用`http`协议就没有这个问题。

这样的话离线就遇到这样的问题，要么服务器端做一个兼容，不用`cookie`,这样很麻烦。或者是使用JS调用原生方法，每一个网络请求都起客户端进行封装，这样也是要改动非常多。

最终发现`webView`有这样一个方法`shouldInterceptRequest`,这个方法会在每一个请求执行前，进行拦截，然后开发者可以任意处理后，再返回一个处理后的网络请求`WebResourceResponse`，这个方法真是太酷了。

*  与是我们就可以将本地`file`协议先伪装成`http`协议，先随便请求一个网络地址，这个地址是什么不重要，只要首页一样就行了

```java
private String mLocalUrlPrefix = "http://test.com/app/";

```
```java
mWebView.loadUrl(mLocalUrlPrefix + currentVersion + "/test.html");
```

* 加载后，在此处进行拦截所有的请求，然后做处理，将所有的请求全部转换为本地文件

```java

@Override
    public WebResourceResponse shouldInterceptRequest(final WebView webView, WebResourceRequest webResourceRequest) {
        final String url = webResourceRequest.getUrl().toString();

        webResourceRequest.getRequestHeaders().put(HJ_USER_AGENT, RunTimeManager.instance().getUserAgent());

        if (url.contains(mLocalUrlPrefix)) {
            return handleRequest(url);
        } else {
            return getWebResourceResponse(url);
        }
    }
```

* 其中 `WebResourceResponse` 主要是由三个部分组成

```java
 public WebResourceResponse(String mimeType, String encoding, InputStream data) {
        mMimeType = mimeType;
        mEncoding = encoding;
        mInputStream = data;
    }
```

其中 `mimeType`为请求文件的类型、`encoding`为文件的编码、`data`为文件的`inputStream`

`mimeType`这个可以根据文件后缀来映射，或者用第三方开源的工具，`encoding`我们一般就用`utf-8`，文件流就直接读取就行了。

例如这样读取：

```java
if (TextUtils.equals(version, OfflineHtmlManager.DEFAULT_VERSION)) {
            inputStream = AssetUtils.getFromAssets(this, fileName);
        } else {
            try {
                String path = getApplicationContext().getCacheDir().getAbsolutePath() + "/" + fileName;
                File file = new File(path);
                inputStream = new FileInputStream(file);
            } catch (FileNotFoundException e) {
                e.printStackTrace();
            }
        }
```

* 这样就完美的将本地`web`页面`file`协议请求伪装成了`http`协议的请求，这样cookie的问题就解决了。


## webView中的所有网络请求都要添加自定义header

* 肯定有很多产品会希望webView中的所有网络请求都要添加自定义header,但webView只提供了一种添加header的方法

```java
  public void loadUrl(String url, Map<String, String> additionalHttpHeaders) {
        throw new RuntimeException("Stub!");
    }
```

但这种方法只能在`url`中添加，其它页面中的请求就添加不上了，那怎么办呢？

此时我们一思考就会发现，用上面`shouldInterceptRequest`这个拦截所有请求的方法就能解决这个问题。
我们在所有网络请求到达时，拦截，然后用`http`请求的方法，先添加header，然后去请求这个文件流，然后返回组装成`webView`需要的`WebResourceResponse `是不是很赞，哈哈。

```java
  try {
  			  if (TextUtils.equals(method, HttpGet.METHOD_NAME)) {
                httpRequest = new HttpGet(url);
            } else if (TextUtils.equals(method, HttpPost.METHOD_NAME)) {
                httpRequest = new HttpPost(url);
            } else if (TextUtils.equals(method, HttpHead.METHOD_NAME)) {
                httpRequest = new HttpHead(url);
            } else if (TextUtils.equals(method, HttpPut.METHOD_NAME)) {
                httpRequest = new HttpPut(url);
            } else if (TextUtils.equals(method, HttpDelete.METHOD_NAME)) {
                httpRequest = new HttpDelete(url);
            } else if (TextUtils.equals(method, HttpTrace.METHOD_NAME)) {
                httpRequest = new HttpTrace(url);
            } else if (TextUtils.equals(method, HttpOptions.METHOD_NAME)) {
                httpRequest = new HttpOptions(url);
            }
            DefaultHttpClient client = new DefaultHttpClient();
            HttpPost httpPost = new HttpPost(url);
            httpPost.setHeader(MY_USER_AGENT, RunTimeManager.instance().getUserAgent());
            HttpResponse httpResponse = client.execute(httpPost);
            InputStream responseInputStream = httpResponse.getEntity().getContent();

            String[] temp = url.split("\\.");
            String suffix = temp[temp.length - 1];
            String encoding = UTF_8;

			  // get file suffix
            suffix = suffix.replace("?","-");
            String[] suffixStrings = suffix.split("-");

            suffix = suffixStrings[0];
            // According to the suffix to get content type.
            String type = Html5MimeUtil.getMimeContentType(this, Html5MimeUtil.MIME_TXT_NAME).get(suffix);
            return new WebResourceResponse(type, encoding, responseInputStream);
        } catch (ClientProtocolException e) {
            //return null to tell WebView we failed to fetch it WebView should try again.
            return null;
        } catch (IOException e) {
            //return null to tell WebView we failed to fetch it WebView should try again.
            return null;
        }
```


## Cookie问题

* 在使用第三方微博登录时，发现当用户没有安装微博时，微博web端会在登陆成功后清除整个应用`webView`的`cookie`,这个就导致此时我们的`cookie`丢失，失效的问题，怎么解决呢？
* 其实仔细研究发现`webView`也为我们提供了非常有用的cookie设置和cookie读取问题

* 我们可以首先要读取`cookie`，放在内存中

```java

if (cookieManager.hasCookies()) {
            mCookie = cookieManager.getCookie(LoginActivity.DAMAIN);
        }    
```
* 然后在微博将`cookie`清除后，将`cookie`再保存进去
```java
private void saveCookies() {
        if (!TextUtils.isEmpty(mCookie)) {
            CookieSyncManager.createInstance(mContext);
            CookieManager.getInstance().setAcceptCookie(true);
            CookieManager.getInstance().setCookie(LoginActivity.DAMAIN, mCookie);
            CookieSyncManager.getInstance().sync();
        }
    }
```

这样问题就方便地解决了。


## 总结 

* `Hybird`是一种很好的方案，`webView`提供的功能很强大，后续有更多有好的想法会持续分享给大家。
* 这是一个种很好的方案，两个平台解决方案都是想通的，iOS里的这个方法叫`cachedResponseForRequest`.


