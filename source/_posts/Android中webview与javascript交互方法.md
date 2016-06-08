title: Android中webview与javascript交互方法

date: 2015-07-21 22:42:46

tags: [Android, webview, jsvascript]

----------------------------------

本文主要是记录了Android中webview与javascript交互方法以及注意事项。

## JS调用Android中的方法
* 首页要在Webview中注入Android中的java对象

```
mWebView.addJavascriptInterface(new JavaObject(), "YouAliasName";	
```
例如： JavaObject()中有这样一个方法
```
 @JavascriptInterface
    public void toast(String toast) {
        ToastUtils.show(mContext, uiToast.getMessage());
    }
```

<!--more-->


* 注入后这样调用：
```
YouAliasName.toast('this is web toast')
```

* 注意事项：
	如果是4.2以上版本，调用的方法必须加上'@JavascriptInterface'注解，否则方法会注入失败，无法调用。


## Android中调用JS中的方法

非常简单，直接

```
mWebView.loadUrl("javascript:(" + mMethodName + ")('" + mJSONArgument + "')";)

```
## 其它说明

* 以上两个可以进行封装一下，更加方便调用
* 建议全部是异步回调，参数用Json封装
* Android 4.4之后可以使用evaluateJavascript进行异步回调

```
  private void testEvaluateJavascript(WebView webView) {
        webView.evaluateJavascript("test()",
                new ValueCallback<String>() {
                    @Override
                    public void onReceiveValue(String value) {

                        Log.i(LOGTAG, "onReceiveValue value=" + value);

                    }
                });
    }


```

* js回调后，要在主线程运行，否则会报错

```
mWebView.post(new Runnable() {
    @Override
    public void run() {
        mWebView.loadUrl(YOUR_URL).
    }
});

```

* Alert无法弹出的问题
```
// 你应该是没有设置WebChromeClient,按照以下代码设置
mWebView.setWebChromeClient(new WebChromeClient() {});
```
* js代码可能没有加载完成就调用了js方法，此时会报错：`Uncaught ReferenceError: functionName is not defined`

```
 mWebView.setWebViewClient(new WebViewClient() {

            @Override
            public void onPageFinished(WebView view, String url) {
                super.onPageFinished(view, url);
                //在这里执行你想调用的js函数
            }

        });

```
* 代码混淆问题 需要 -keep class 你的js调用方法类







