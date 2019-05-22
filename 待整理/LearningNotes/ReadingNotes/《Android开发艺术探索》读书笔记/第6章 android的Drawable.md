## 第6章 Android的Drawable

### 1.表示图片的Drawable
android中表示图片的Drawable有两种，分别为BitmapDrawable和NinePatchDrawable，BitmapDrawable也可以用来表示一张.9图片。

BitmapDrawable对应的xml标签为<bitmap/>，NinePatchDrawable对应xml标签为<nine-patch/>

#### 1.1 <bitmap/>标签下的属性
<bitmap/>标签没有子标签，通常情况下它都是根标签

android:src 图片资源；
android:antialias="true"    是否抗锯齿，true为抗锯齿，抗锯齿功能能让图片看起来更平滑，但是图片的清晰度会稍微降低，但是没有太明显的影响；
android:dither="true"   是否开启抖动效果，如果图片的色彩模式为ARGB8888，但是显示的屏幕色彩模式仅支持RGB555，那么这个时候显示的图片就会失真，如果开启了抖动效果，那么失真效果会大大降低；
android:filter="true"   是否开启过滤效果，这在图片被拉伸或者压缩的时候会有明显的作用，让图片能够保持较好的显示效果；
android:gravity  当图片的尺寸小于容器的大小的时候，gravity能够决定图片在容器中显示的位置；
android:tileMode="clamp"    平铺模式，这个属性有三个值，分别为clamp，repeat，mirror；

<nine-patch/>标签中的属性和<bitmap/>中一样，它也表示一张图片，区别在于<nine-patch/>标签表示一张.9.png的图片(<bitmap/>也可以表示一张.9.png图片)

### 2.ShapeDrawable和GradientDrawable
ShapeDrawable和GradientDrawable都是可以用<shape/>标签来实现对应效果，<shape/>标签被解析实例化后得到的是GradientDrawable。

#### 2.1 <shape/>标签属性
<gradient/>标签表示渐变，它有九种属性，渐变方式有三种，由android:type控制：
android:type="radial|linear|sweep"
type属性有以上三个值，分别表示放射式渐变，线性渐变，扫描式渐变(顺时针渐变)

android:type="radial"表示放射式渐变，在该渐变模式下，如下属性将会起作用：
android:gradientRadius表示渐变的半径，单位px和dp都可以；
android:startColor表示渐变的起始颜色值；
android:endColor表示渐变的最终颜色值；
android:centerColor表示渐变的中间颜色值，假如android:gradientRadius值为50dp，即渐变的半径为50dp，那么起始颜色值会渐变25dp成中间颜色值，中间颜色值再渐变25dp成最终颜色值；
android:centerX表示x方向渐变的起始点位置，符合android视图坐标系的规律，即以View左上角为(0,0)点，0.5表示渐变起始点在相对于View视图坐标系x方向的中点(也就是View长度一半的地方)，1的话就在View的右边界；
android:centerY表示y方向渐变的起始点位置，规律和android:centerX相似，0.5在View宽度一半的地方，1在View底部边界。

不起作用的属性：
android:angle

anddroid:type="sweep"表示扫描式渐变(顺时针渐变)，如下属性会起作用：
android:startColor
android:centerColor
android:endColor
android:centerX
android:centerY

不起作用的属性：
android:angle
android:gradientRadius

android:type="linear"表示线性渐变，如下属性会起作用：
android:startColor
android:centerColor
android:endColor
android:centerX
android:centerY
android:angle

不起作用的属性：
android:gradientRadius

当type为linear时，如果没有设置渐变的中间色android:centerColor，那么centerX和centerY都不会起作用；
如果存在android:centerColor属性，那么想要改变centerColor的位置可以使用android:centerX属性。

**颜色的渐变范围其实是由View的大小来决定的，当我们使用shape为ring时，设置了圆环的厚度以及内圆的半径，那么这个圆环的大小就被确定了，但是我们的View可以比这个圆环要大，这个时候，我们看到的圆环颜色渐变色就不是完整的。有点像渐变色铺满整个View，然后ring在这个View上画出了一个环形，环形的大小是可变的，相应的环形的渐变色范围就不一样。**

android:angle属性值如果设置成负值且是45的倍数，效果始终是从上向下渐变，和值为270时的效果一样，角度对应的渐变方向和直角坐标系一致，即0度是从左向右渐变，45度从左下向右上渐变，90度从下向上渐变...
angle属性只有在type为linear的时候才会起作用

android:gradientRadius表示渐变的半径，只有在type为radial时才起作用，单位px和dp都可；

对于android:type="ring"来说，android:thicknessRatio="4":表示的是环的厚度占View宽度的几分之几，这里是四分之一;android:innerRadiusRatio="4"表示的是内圆的半径占View宽度的几分之几，这里是四分之一。

<size/>标签可以表示Drawable的固定大小，在所有的Drawable中，只有<bitmap/>和<nine-patch/>标签表示的Drawable有固定的大小，因为他们表示的是图片，固定大小就是图片的大小，但是其他Drawable理论上是没有固定大小这个概念的，就算给Drawable设置了size，还不能说这就是Drawable的固定大小，因为他们仍然会随着View的大小而拉伸或者缩小，一种情况下这个size会起作用，就是View的宽高设置为了wrap_content。

### 3.LayerDrawable
LayerDrawable对应<layer-list/>标签，它表示Drawable的集合，基本的结构如下：
```java
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">

    <item
        android:bottom="dimension"
        android:drawable="@drawable/drawable_resource"
        android:left="dimension"
        android:right="dimension"
        android:top="dimension" />
    
</layer-list>
```
上面是常用的几种属性，其余属性可输入"android:"查看。

LayerDrawable是具有层级的Drawable，在<layer-list/>标签中可有多个<item/>，下面的item覆盖上面的item，我们可通过left,right,top,bottom，这些偏移量来实现一些叠加效果；

### 4.StateListDrawable
StateListDrawable对应的是<selector/>标签，需要注意的点有如下：
1.<selector/>标签有两个属性需要注意
- android:constantSize：该属性从字面意思上也能看出个大概，就是固定大小，由于<selector/>中可能会有多个<item/>，每个<item/>都是一个Drawable，那么就会存在Drawable不一样大小的情况，这个时候，android:constantSize属性就起作用了，当它为true时就表示这个StateListDrawable有一个固定大小，具体的大小就是所有Drawable中的最大值，这样当View处于不同的状态时，背景就可以是一样大了；如果android:constantSize属性值为false，那么View的每种状态都可能有一个不一样大小的背景。
**这里有个疑问：除了表示图片的Drawable外，其他类型的Drawable实际上是没有固定大小的，他们会随着View的大小而拉伸或者缩小，这里提到的所有Drawable固定大小的最大值是指<size/>标签设置的宽高吗？嗯，应该是**

- android:variablePadding：从字面意思上也很好理解，翻译为可变的Padding，如果这个属性的值为true，那么就表示StateListDrawable在不同状态下可能会有不同的padding值，因为每种状态的Drawable可能有不一样的padding；如果该属性值为false，那么StateListDrawable会取所有Drawable中padding值最大的那个作为StateListDrawable的padding值，这样，不管在什么状态下，padding值都是相同的。

2.<selector/>标签中可以有多个<item/>标签，每个<item/>都表示一种状态下的Drawable；

3.状态的匹配规则
View根据当前的状态从上到下逐个匹配，匹配到哪个状态就用该状态下的Drawable，所以一般情况下我们会把没有任何状态的Drawable放在最后一条，这样当上面的状态View都没有匹配到的时候，就能够匹配到最后一个Drawable，没有绑定状态的Drawable能够匹配View的任何状态。

### 5.LevelListDrawable
LevelListDrawable也表示一个Drawable的集合，它对应于<level-list/>标签，每个Drawable都有一个等级(level)的概念，根据不同的等级，LevelListDrawable会切换为不同的Drawable。基本结构如下：
```java
<?xml version="1.0" encoding="utf-8"?>
<level-list xmlns:android="http://schemas.android.com/apk/res/android">

    <item
        android:drawable="@drawable/icon_circle"
        android:maxLevel="100"
        android:minLevel="0" />
    ...
</level-list>
```
当然我们也可以只设置android:maxLevel和android:minLevel中的一个。

**匹配规则应该也是从第一个Drawable逐个往下匹配，处于level范围的Drawable将会被使用。**

level是有范围的：0~10000，最小等级是0，最大等级是10000。

注意：如果我们给ImageView的src设置了LevelListDrawable,我们可以通过setImageLevel来设置ImageView显示不同的Drawable.

### 6.TransitionDrawable
TransitionDrawable对应标签<transition/>,TransitionDrawable使用需要注意：<transition/>标签下一般会有两个<item/>，只有一个的话在运行的时候直接崩溃,报NPE异常，多于两个<item/>，其余的<item/>不会起作用，只有前面两个会有淡入淡出的效果。
基本结构：
```java
<?xml version="1.0" encoding="utf-8"?>
<transition xmlns:android="http://schemas.android.com/apk/res/android">

    <item android:drawable="@color/colorPrimary" />

    <item android:drawable="@color/colorAccent" />
    
</transition>

TransitionDrawable transitionDrawable = (TransitionDrawable) findViewById(R.id.tv_test_drawable).getBackground();
//淡入淡出效果，正向过程
transitionDrawable.startTransition(5000);
//逆向过程
transitionDrawable.reverseTransition(5000);
```
TransitionDrawable也可以使用在ImageView上。

### 7.InsetDrawable
对应标签<inset/>，InsetDrawable可以将其他Drawable内嵌到自己中，并在四周留出一定的距离，基本结构：
```java
<?xml version="1.0" encoding="utf-8"?>
<inset xmlns:android="http://schemas.android.com/apk/res/android"
    android:insetLeft="10dp"
    android:insetTop="10dp"
    android:insetRight="10dp"
    android:insetBottom="10dp">

    <shape android:shape="rectangle">
        <solid android:color="@color/colorPrimary" />
    </shape>

</inset>
```
**疑问：内嵌多个Drawable是什么效果？**

### 8.ScaleDrawable
ScaleDrawable对应标签为<scale/>，基本写法如下：
```java
<?xml version="1.0" encoding="utf-8"?>
<scale xmlns:android="http://schemas.android.com/apk/res/android"
    android:drawable="@drawable/icon_circle"
    android:level="1"
    android:scaleWidth="70%"
    android:scaleHeight="70%"
    android:scaleGravity="center">

    <--如果在这里内嵌了诸如<shape/>一类的Drawable，scale标签下的android:drawable属性将会被覆盖-->
    ...
</scale>
```
使用ScaleDrawable必须要设置level属性，level的范围从0~10000，如果level的值为0，那么ScaleDrawable将无法绘制出图像;

level的值越大，ScaleDrawable看起来就越大；

android:scaleWidth和android:scaleHeight属性表示的是将drawable缩放为容器View宽高的(100%-属性值)；

android:scaleGravity表示的是drawable在容器View中的位置(如果Drawable的大小小于View的大小的话)。

### 9.ClipDrawable
ClipDrawable对应<clip/>标签，它可以根据当前的等级level来裁剪**另一个**Drawable，裁剪方向可以根据android:clipOrientation和android:gravity来共同控制；

基本用法：
```java
<?xml version="1.0" encoding="utf-8"?>
<clip xmlns:android="http://schemas.android.com/apk/res/android"
    android:clipOrientation="vertical"
    android:drawable="@drawable/icon_circle"
    android:gravity="bottom">
</clip>

ClipDrawable clipDrawable = (ClipDrawable) findViewById(R.id.tv_test_drawable).getBackground();
//2000表示只保留Drawable的20%，其余80%裁剪掉
clipDrawable.setLevel(2000);
```

### 10.自定义Drawable
自定义Drawable最核心的就是覆写draw方法，可参考ShapeDrawable的源码。

### 疑问
1.对于android:type="ring"的shape来说，为什么要加android:useLevel="false"才会起作用？

### 参考链接
https://www.cnblogs.com/lang-yu/p/6112052.html

https://blog.csdn.net/u010687392/article/details/47399719

https://blog.csdn.net/lmj623565791/article/details/43752383


