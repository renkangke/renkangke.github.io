 

 title: 关于内存清理|getLocationOnScreen|类型转换等要注意的点的记录
 
 date: 2015-05-12 22:19:50
 
 tags: [技术、Android]  

---

1、空对象的类型强转  
2、内存被清理后可能导致的问题   
3、getLocationOnScreen()可能会返回0的问题

<!--more-->

记录一下今天在工作过程中遇到的一些小坑：
   
### 1、空对象的类型强转

对于一个为null的对象，进行类型强转，是不会抛异常的，并且会运行通过，强转后的结果依然是null，这一点我是之前没有想到的。算是一个以后需要注意的点吧

### 2、内存被清理后可能导致的问题

 将应用放在后台，用清理内存的应用进行清理后，会导致如下一些问题：
 1、fragment如果做为内部类且不会静态的话，会导致fragment make sure class name exists is public and has an empty constructor that is public  这个错误的出现，建议将fragment重构为单独的外部类，或者用static的fragment
 2、 使用viewpageadapter时，内存清理后，fragment会重新创建，但getItem不会再调用，此时从这里设置到fragment的变量会在内存中清除掉，只有setArguments的方法设置的参数会被还原，解决办法是用 
 
      /**
         * 此处为解决内存被清理掉后重新打开时 变量 为空的bug
         */
        @Override
        public Object instantiateItem(ViewGroup container, int position) {
            XXXFragment fragment = (XXXFragment) super.instantiateItem(container, position);
            fragment.setXXXX(XXXX);
            return fragment;
        }
        
   另外：某些状态被记录，有些未被记录，需要保存，建议用以下方法保存起来再恢复：
        
    @Override
    protected Parcelable onSaveInstanceState() {
        return super.onSaveInstanceState();
    }

    @Override
    protected void onRestoreInstanceState(Parcelable state) {
        super.onRestoreInstanceState(state);
    }
    
 另个有些控件，比如edittext等，会自动保存他们本身的一些内容等
        
### 3、getLocationOnScreen()可能会返回0的问题

此方法在某些情况下会拿不到，值为0的情况，初步推断是因为View在刷新过程中，info为空，或者其它可能，以下是Android源码 ，如果attachInfo == null 是会返回0的
    
     public void getLocationOnScreen(int[] location) {
        getLocationInWindow(location);

        final AttachInfo info = mAttachInfo;
        if (info != null) {
            location[0] += info.mWindowLeft;
            location[1] += info.mWindowTop;
        }
    }
    
    
以上是今天遇到的坑小结，特记于此
 
