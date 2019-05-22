### View(视图)动画
View动画包括四种动画效果，分别为：TranslateAnimation(平移动画)，ScaleAnimation(缩放动画)，AlphaAnimation(透明度动画)，RotateAnimation(旋转动画)。

名称 | 标签 | 对应类
---- | ---- | ----
平移动画 | `<translate/>` | TranslateAnimation
缩放动画 | `<scale/>` | ScaleAnimation
旋转动画 | `<rotate/>` | RotateAnimation
透明动画 | `<alpha/>` | AlphaAnimation
动画集合 | `<set/>` | AnimationSet
以上动画涉及到的类都是Animation抽象类的子类。

View动画一般情况下我们都会使用xml的形式创建，因为可读性相对更好。

在android工程中，View动画文件放置的路径为:`res/anim/filename.xml`

View动画写法示例：
```xml
<set xmlns:android="http://schemas.android.com/apk/res/android"
    android:interpolator="@[package:]anim/interpolator_resource"
    android:shareInterpolator="[true | false]">

    <translate .../>

    <scale .../>

    <alpha .../>

    <rotate .../>

    <set .../>

</set>
```
从写法示例中我们能看到View动画可以是一种动画，也可以是多种动画的集合，甚至在动画集合中嵌套其他动画集合。

**android:interpolator**为动画插值器，它可以用来调整动画的速度，当没有指定动画插值器时，系统默认使用的是@android:anim/accelerate_decelerate_interpolator，即加速减速插值器，动画实际在播放的过程中会有一个先加速再减速的过程。

**android:shareInterpolator**表示集合中的动画是否和集合共享一个插值器，如果该值为false，表示集合中的动画不和集合共享插值器，那么集合中的动画就需要自己设置插值器或者使用默认值。

**缩放动画**
```xml
<?xml version="1.0" encoding="utf-8"?>
<scale xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="5000"
    android:fromXScale="0.0"
    android:fromYScale="0.0"
    android:interpolator="@android:anim/accelerate_decelerate_interpolator"
    android:pivotY="100%"
    android:toXScale="1.4"
    android:repeatCount="0"
    android:startOffset="4000"
    android:repeatMode ="reverse"
    android:fillAfter="true"
    android:toYScale="1.4" />
```
```java
final ImageView imageView = findViewById(R.id.iv_panda_pose);
// 通过AnimationUtils的loadAnimation()方法加载动画资源文件
Animation scaleAnimation = AnimationUtils.loadAnimation(AnimTestActivity.this, R.anim.anim_scale);
imageView.startAnimation(scaleAnimation);
```
缩放动画中必须的四个属性是android:fromXScale,android:fromYScale,android:toXScale,android:toYScale；缺少任何一个缩放动画都将不生效。

android:pivoX,android:pivoY属性值有三种，分别为数值(比如50)，百分数(比如50%)，百分数p(比如50%p):
- 数值：动画缩放起点为当前View左上角坐标加上指定的数值像素；
- 百分数：动画缩放起点为当前View左上角坐标加上View宽度或高度的百分比；
- 百分数p：动画缩放起点为当前View左上角坐标加上父控件宽度或高度的百分比。

android:repeatCount的值为0时动画播放一次，值为1时动画播放两次，其他值以此类推。

当android:repeatCount="4"和android:repeatMode="reverse"同时存在时，动画从0.0->1.4的过程算执行一次，从1.4->0.0的过程也算执行一次，因此看不到5次0.0->1.4的过程。

android:fromXScale="0.5"表示动画开始是从View宽度的0.5倍开始的。

android:fillAfter="true"表示动画执行完后停留在最后一帧，false时表示动画执行完后恢复原来模样。

android:startOffset="4000"表示延迟4秒再执行动画，延迟4秒不是说在这4秒时间内View一直处于原本的状态，而是处于fromXScale和fromYScale指定的初始状态。

### 帧动画
帧动画的英文名叫Frame Animation，通俗点来讲就是把一堆图片攒在一块，然后一张一张的播放，基本用法如下：
```xml
<?xml version="1.0" encoding="utf-8"?>
<animation-list xmlns:android="http://schemas.android.com/apk/res/android"
    android:oneshot="false">

    <item
        android:drawable="@drawable/bg_shape1"
        android:duration="150" />
    <item
        android:drawable="@drawable/bg_shape2"
        android:duration="150" />
    <item
        android:drawable="@drawable/bg_shape3"
        android:duration="150" />

</animation-list>
```
- 文件放于drawable文件夹下；
- `<animation-list/>`标签最终会解析成Java代码中的AnimationDrawable类；
- AnimationDrawable可以作为View的background，也可以作为ImageView的src；
- 不论作为View的background还是ImageView的src，在动画还没有开始执行时，界面上默认显示的是第一个`<item/>`表示的图片；
- AnimationDrawable中`start()`方法表示开始执行动画，`stop()`方法表示停止动画，当我们执行了`stop()`方法后再执行`start()`方法，动画重新从第一张图片开始执行；
- `<animation-list/>`标签下的`android:oneshot="true|false"`属性表示动画是否只执行一次，true表示只执行一次，false表示循环执行，直到调用`stop()`方法；
- `<item/>`标签下的`android:duration`属性表示在该图片下停留的时长，因此，该属性能够控制帧动画播放的速度；

例子：
```java
//点击图片执行动画，再点击停止，再点击执行动画...
final ImageView imageView = findViewById(R.id.iv_panda_pose);
//获取ImageView的src
final AnimationDrawable animationDrawable = (AnimationDrawable)imageView.getDrawable();
imageView.setOnClickListener(new View.OnClickListener() {
        int count = 0;
        @Override
        public void onClick(View v) {
            if (animationDrawable != null) {
                if (count % 2 == 0) {
                    animationDrawable.start();
                } else {
                    animationDrawable.stop();
                }
            }
            count++;
        }
    });
```

### LayoutAnimation
LayoutAnimation作用于ViewGroup，它指定了ViewGroup中子View的出场动画。ListView的Item出场动画通常都是通过LayoutAnimation来实现的。

使用步骤：
```java

```
问题总结：
1.自定义View动画中涉及到的Camera是什么？
2.AnimationSet默认情况下有插值器吗？