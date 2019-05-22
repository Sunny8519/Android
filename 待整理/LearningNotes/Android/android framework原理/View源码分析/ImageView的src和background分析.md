## ImageView的src和background分析

### 一.android:src属性的背后
在ImageView中，android:src属性是ImageView特有的属性，它表示ImageView的“前景”，是不是跟background(背景)有点相对的意思，没错，在代码层面上它们就是一前一后的关系，这也决定了它们最后的显示顺序以及效果。

我们先来分析一下android:src对应的部分源码，从这部分源码中我们大概能了解如下几点内容：
1.ImageView设置的padding值对android:src起作用吗？
2.为什么android:src设置的图片一般不会出现拉伸的现象？


### 二.background背后的原理
从background这部分源码中，我们大概能了解到如下几点内容：
1.为什么通过background设置的图片会经常出现拉伸现象？
2.padding值对background不起任何作用？


### 三.总结


