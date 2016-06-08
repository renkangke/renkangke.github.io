title: 自定义点击取词长按复制textView
date: 2015-04-24 21:37:32 
tags: [技术,Android]

---


很久没有更新博客了…… 更新一篇‘自定义点击取词长按复制textView’的文章。

## 自定义EditText长按复制：

这个功能很小，但是你可能会遇到一些小坑，比如：当listview中嵌套editText,而你恰巧又没有将ActionBar显示出来的时候，此时需要将ActionBar显示出来，才能显示出复制、粘贴等选项。

其次你如果想自定义复制粘贴的选项，比如更换选项，更换icon等，像知乎那样自定义，你需要重写ActionMode.Callback 这里可以自定义menu.

<!--more-->

## 如何获取当手点击textView时，当前单词是哪个

方法是先判读当前点击的是哪个字母，找到它的位置：

    private int getLineAtCoordinate(TextView textView2, float y) {
        if (textView2 == null) {
            return 0;
        }
        try {
            y -= textView2.getTotalPaddingTop();
            y = Math.max(0.0f, y);
            y = Math.min(textView2.getHeight() - textView2.getTotalPaddingBottom() - 1, y);
            y += textView2.getScrollY();
            return textView2.getLayout().getLineForVertical((int) y);
        } catch (Exception e) {
            e.printStackTrace();
        }

        return 0;
    }
    
  获取字母在textview中哪一个位置
  
    private int getOffsetAtCoordinate(TextView textView2, int line, float x) {
        x = convertToLocalHorizontalCoordinate(textView2, x);
        return textView2.getLayout().getOffsetForHorizontal(line, x);
    }
    

然后再左右遍历出不是字母为止，然后就当前的这个单词挑出来

获取点击处是哪一行：

接下来，将此单词获取释义 ，再显示出一个popupwindow就可以了

未完待完善。。






