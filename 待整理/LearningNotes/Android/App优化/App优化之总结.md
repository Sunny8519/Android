## App优化 总结

#### APK优化

**1.使用svg图片**
a.可以使用一套svg图片生成指定的png图片
![svg](https://upload-images.jianshu.io/upload_images/5231076-f8a3345115053670.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![svg_1](https://upload-images.jianshu.io/upload_images/5231076-95cbc6398f8beb74.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

b.使用svg图片生成vector标签的xml文件，对于不同分辨率的vector文件，系统可自动适配，可有效降低多套图带来的apk体积增大

**2.Tint着色器**
android:tint=""
android:backgroundTint=""
有关着色器的属性在ImageView中有两种，android:tint是针对src的，android:backgroundTint是针对background的。

应用：在selector中，正常和按压状态下的图片都是一样的，仅颜色不一样，我们就可以给ImageView设置一张图片的src，但是tint使用正常和按压状态的color selector，就可以达到使用两张图片的selector的src的效果。

**3.string.xml资源配置**
![svg_2](https://upload-images.jianshu.io/upload_images/5231076-059c1c3142542d96.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在国际化的时候，我们可以有选择的进行国际化，不需要把所有的国家语言的string.xml文件都打包进APK中，上图就提供了这样一种方式，通过如图的配置，最终打完包，国际化的资源只有两种。

**4.so库配置**
目前市面主流的手机CPU架构基本都是arme，因此我们可以把不是arme架构的so库删除。

**5.移除项目中无用的resource资源，这种方式在操作的时候要慎重，一旦删除就是不可逆的。**

**6.源代码混淆，代码混淆也能降低apk大小**

![svg_3](https://upload-images.jianshu.io/upload_images/5231076-cd37d41fa1599728.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**7.资源压缩**

![svg_3](https://upload-images.jianshu.io/upload_images/5231076-cd37d41fa1599728.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在启用资源压缩时，我们必须要启用minifyEnabled(代码压缩，代码混淆)

**自定义要保留的资源**

我们可以在res/raw/文件中新建一个keep.xml文件，用于定义我们要保留或者舍弃的资源。

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources xmlns:tools="http://schemas.android.com/tools"
    tools:keep="@layout/l_used*_c,@layout/l_used_a,@layout/l_used_b*"
    tools:discard="@layout/unused2" />
```

tools:keep表示要保留的资源
tools:discard表示要舍弃的资源

tools:keep和tools:discard属性都支持逗号分隔的资源名称列表，而且我们也可以使用*当做通配符来匹配资源。

**8.webp**
webp格式的图片要比相同尺寸的png或者jpeg图片大小要小得多，图片质量却是相差不多(几乎可以忽略)。比如一张Splash图片1.2MB，在转为webp格式的图片后，大小可能为100kb左右。

**9.压缩对齐**

### 一. 内存优化

#### 1. 节制使用Service

用Service来执行后台任务，应该在需要的时候才开启。因为启动一个Service，系统会倾向于将这个Service依赖的进程保留，导致系统在LRUCache中缓存的进程数减少，这样可能会导致切换别的应用程序时消耗更多的性能来创建进程。我们可以使用IntentService，在后台任务执行完之后能够自动停止。

#### 2. 界面不可见时释放内存

当用户离开了应用程序，应用程序的界面已经不可见了，我们可以把跟界面相关的资源释放。重写Activity的onTrimMemory(int)方法，在该方法中监听TRIM_MEMORY_UI_HIDDEN这个级别，一旦触发该级别说明用户离开了该页面，我们就可以做一些相关的资源释放操作。

TRIM_MEMORY_UI_HIDDEN是接口ComponentCallbacks2中的常量，该接口中还有其他类似的常量，可在不同的情况下监听这些常量。

#### 3. 避免加载不需要的分辨率的Bitmap

压缩图片。[Bitmap压缩策略](http://sunny8519.top/2018/03/31/Bitmap%E5%8E%8B%E7%BC%A9%E7%AD%96%E7%95%A5/)

#### 4. 使用优化过的数据集合

在Android中我们可以使用诸如SparseArray、SparseBooleanArray、LongSparseArray等经过优化的集合。

#### 5. 开发者应该知晓内存开支情况

- 使用枚举会比静态常量消耗两倍以上的内存；
- 任何一个Java类，包括匿名类、内部类，都要占用大约500字节的内存空间；
- 任何一个类的实例会占用12~16字节的内存，尽量避免频繁创建对象实例；

### 二. 代码优化

#### 1. 避免在循环条件中使用复杂的表达式

复杂的表达式能从循环中抽取出来的尽量抽取出来，比如：

```java
public class Test{
  public static void main(String[] args){
    for(int i = 0;i < list.size(); i++){
      ...
    }
  }
}

//改进
public class Test{
  public static void main(String[] args){
    int size = list.size();
    for(int i = 0;i < size; i++){
      ...
    }
  }
}
//或者
public class Test{
  public static void main(String[] args){
    for(int i = 0,size = list.size();i < size; i++){
      ...
    }
  }
}
```

#### 2. 类中的getter和setter方法如果能使用final修饰尽量使用final修饰

#### 3. 将try/catch块移出循环

#### 4. boolean值不要用==判断

比如：

```java
public boolean foo(String arg){
  return "a".equals(arg) == true;
}
//改进
public boolean foo(String arg){
  return "a".equals(arg); //没必要多此一举
}
```

#### 5. 尽量使用条件操作符代替if/else

#### 6. 尽量不要在循环体重实例化变量

#### 7. 尽量使用局部变量

#### 8. 尽量使用final修饰符

#### 9. 采用需要时再创建策略

参考书籍：

《Java程序性能优化》