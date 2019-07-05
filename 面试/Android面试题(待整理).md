### 谈一下自定义 View 的流程

https://jsonchao.github.io/2018/10/28/Android%20View%E7%9A%84%E7%BB%98%E5%88%B6%E6%B5%81%E7%A8%8B/

需要注意的点：
1. UNSPECIFIED 这个模式一定要充分理解，了解使用场景，早期的博客很多都不了解，所以都一带而过这个模式，造成很多人通过看博客学习的，其实自己也不清楚，实际上面试我会很注意这个细节，很少有人答出来。

2. 关于LayoutParams，会涉及到好几个方法的重载，很多人不会注意到。

3. View的状态存储于恢复不说了。

4. 关于自定义View + 动画，要有一定的认识。

### Service

https://my.oschina.net/youranhongcha/blog/710046

### 说一下IntentService的原理？

https://my.oschina.net/youranhongcha/blog/785387

### 说一下AsyncTask的原理？

https://my.oschina.net/youranhongcha/blog/1561107

### 聊一下对Context的认识？

https://my.oschina.net/youranhongcha/blog/1807189

### 谈谈你是如何检测应用UI卡顿的？

https://blog.csdn.net/lmj623565791/article/details/58626355

http://blog.zhaiyifan.cn/2016/01/16/BlockCanaryTransparentPerformanceMonitor/

https://mp.weixin.qq.com/s/MthGj4AwFPL2JrZ0x1i4fw

### 谈一谈对Binder机制的理解？

https://www.jianshu.com/p/adaa1a39a274

1.ListView 中图片错位的问题是如何产生的?
2.混合开发有了解吗？
3.知道哪些混合开发的方式？说出它们的优缺点和各自使用场景？（解答：比如:RN，weex，4.H5，小程序，WPA等。做Android的了解一些前端js等还是很有好处的)；
5.屏幕适配的处理技巧都有哪些?
6.服务器只提供数据接收接口，在多线程或多进程条件下，如何保证数据的有序到达？
7.动态布局的理解
8.怎么去除重复代码？
9.画出 Android 的大体架构图
10Recycleview和ListView的区别
11.ListView图片加载错乱的原理和解决方案

ListView item缓存机制：为了使得性能更优，ListView会缓存行item(某行对应的View)。ListView通过adapter的getView函数获得每行的item。
滑动过程中
1）如果某行item已经滑出屏幕，若该item不在缓存内，则put进缓存，否则更新缓存；
2）获取滑入屏幕的行item之前会先判断缓存中是否有可用的item，如果有，做为convertView参数传递给adapter的getView。

出现的问题：

1）行item图片显示重复，当前行item显示了之前某行item的图片。
比如ListView滑动到第2行会异步加载某个图片，但是加载很慢，加载过程中listView已经滑动到了第14行，且滑动过程中该图片加载结束，第2行已不在屏幕内，根据上面介绍的缓存原理，第2行的view可能被第14行复用，这样我们看到的就是第14行显示了本该属于第2行的图片，造成显示重复。

2）行item图片显示闪烁
如果第14行图片又很快加载结束，所以我们看到第14行先显示了第2行的图片，立马又显示了自己的图片进行覆盖造成闪烁错乱。

解决方法
通过上面的分析我们知道了出现错乱的原因是异步加载及对象被复用造成的，如果每次getView能给对象一个标识，在异步加载完成时比较标识与当前行item的标识是否一致，一致则显示，否则不做处理即可。

12.动态权限适配方案，权限组的概念
13.Android系统为什么会设计ContentProvider？
14.下拉状态栏是不是影响activity的生命周期
15.如果在onStop的时候做了网络请求，onResume的时候怎么恢复？
16.Bitmap 使用时候注意什么？
17.Bitmap的recycler()
18.Android中开启摄像头的主要步骤
19.ViewPager使用细节，如何设置成每次只初始化当前的Fragment，其他的不初始化？
20.点击事件被拦截，但是想传到下面的View，如何操作？
21.微信主页面的实现方式
22.微信上消息小红点的原理
23.CAS介绍

初级面试专题（中小厂）
1、导致内存泄露的原因有哪些？

内存泄露的根本原因：长生命周期的对象持有短生命周期的对象。短周期对象就无法及时释放。

静态内部类非静态内部类的区别(Handler 引起的内存泄漏。)

静态集合类引起内存泄露

单例模式引起的内存泄漏。

解决：Context是ApplicationContext，由于ApplicationContext的生命周期是和app一致的，不会导致内存泄漏

注册/反注册未成对使用引起的内存泄漏。

集合对象没有及时清理引起的内存泄漏。通常会把一些对象装入到集合中，当不使用的时候一定要记得及时清理集合，让相关对象不再被引用。

 内存分析工具的使用

 

减少内存对象的占用

I.ArrayMap/SparseArray代替hashmap

II.避免在android里面使用Enum

III.减少bitmap的内存占用

inSampleSize：缩放比例，在把图片载入内存之前，我们需要先计算出一个合适的缩放比例，避免不必要的大图载入。

decode format：解码格式，选择ARGB_8888/RBG_565/ARGB_4444/ALPHA_8，存在很大差异。

IV.减少资源图片的大小，过大的图片可以考虑分段加载

 

2、理解Activity，View,Window三者关系

这个问题真的很不好回答。所以这里先来个算是比较恰当的比喻来形容下它们的关系吧。Activity像一个工匠（控制单元），Window像窗户（承载模型），View像窗花（显示视图）LayoutInflater像剪刀，Xml配置像窗花图纸。

1：Activity构造的时候会初始化一个Window，准确的说是PhoneWindow。

2：这个PhoneWindow有一个“ViewRoot”，这个“ViewRoot”是一个View或者说ViewGroup，是最初始的根视图。

3：“ViewRoot”通过addView方法来一个个的添加View。比如TextView，Button等

4：这些View的事件监听，是由WindowManagerService来接受消息，并且回调Activity函数。比如onClickListener，onKeyDown等。

3、Handler的原理

所以就有了handler，它的作用就是实现线程之间的通信。

handler整个流程中，主要有四个对象，handler，Message,MessageQueue,Looper。当应用创建的时候，就会在主线程中创建handler对象，

我们通过要传送的消息保存到Message中，handler通过调用sendMessage方法将Message发送到MessageQueue中，Looper对象就会不断的调用loop()方法

不断的从MessageQueue中取出Message交给handler进行处理。从而实现线程之间的通信。

4、View，ViewGroup事件分发

1. Touch事件分发中只有两个主角:ViewGroup和View。ViewGroup包含onInterceptTouchEvent、dispatchTouchEvent、onTouchEvent三个相关事件。View包含dispatchTouchEvent、onTouchEvent两个相关事件。其中ViewGroup又继承于View。

2.ViewGroup和View组成了一个树状结构，根节点为Activity内部包含的一个ViwGroup。

3.触摸事件由Action_Down、Action_Move、Aciton_UP组成，其中一次完整的触摸事件中，Down和Up都只有一个，Move有若干个，可以为0个。

4.当Acitivty接收到Touch事件时，将遍历子View进行Down事件的分发。ViewGroup的遍历可以看成是递归的。分发的目的是为了找到真正要处理本次完整触摸事件的View，这个View会在onTouchuEvent结果返回true。

5.当某个子View返回true时，会中止Down事件的分发，同时在ViewGroup中记录该子View。接下去的Move和Up事件将由该子View直接进行处理。由于子View是保存在ViewGroup中的，多层ViewGroup的节点结构时，上级ViewGroup保存的会是真实处理事件的View所在的ViewGroup对象:如ViewGroup0-ViewGroup1-TextView的结构中，TextView返回了true，它将被保存在ViewGroup1中，而ViewGroup1也会返回true，被保存在ViewGroup0中。当Move和UP事件来时，会先从ViewGroup0传递至ViewGroup1，再由ViewGroup1传递至TextView。

6.当ViewGroup中所有子View都不捕获Down事件时，将触发ViewGroup自身的onTouch事件。触发的方式是调用super.dispatchTouchEvent函数，即父类View的dispatchTouchEvent方法。在所有子View都不处理的情况下，触发Acitivity的onTouchEvent方法。

7.onInterceptTouchEvent有两个作用：1.拦截Down事件的分发。2.中止Up和Move事件向目标View传递，使得目标View所在的ViewGroup捕获Up和Move事件。

5、onNewIntent()什么时候调用?(singleTask)

6、mvc 和 mvp mvvm

·  1.mvc:数据、View、Activity，View将操作反馈给Activity，Activitiy去获取数据，数据通过观察者模式刷新给View。循环依赖

1.Activity重，很难单元测试

2.View和Model耦合严重

·  2.mvp:数据、View、Presenter，View将操作给Presenter，Presenter去获取数据，数据获取好了返回给Presenter，Presenter去刷新View。PV，PM双向依赖

1.接口爆炸

2.Presenter很重

·  3.mvvm:数据、View、ViewModel，View将操作给ViewModel，ViewModel去获取数据，数据和界面绑定了，数据更新界面更新。

1.viewModel的业务逻辑可以单独拿来测试

2.一个view 对应一个 viewModel 业务逻辑可以分离，不会出现全能类

3.数据和界面绑定了，不用写垃圾代码，但是复用起来不舒服

7、自定义控件

View的绘制流程：OnMeasure()——>OnLayout()——>OnDraw()

第一步：OnMeasure()：测量视图大小。从顶层父View到子View递归调用measure方法，measure方法又回调OnMeasure。

第二步：OnLayout()：确定View位置，进行页面布局。从顶层父View向子View的递归调用view.layout方法的过程，即父View根据上一步measure子View所得到的布局大小和布局参数，将子View放在合适的位置上。 

第三步：OnDraw()：绘制视图。ViewRoot创建一个Canvas对象，然后调用OnDraw()。六个步骤：①、绘制视图的背景；②、保存画布的图层（Layer）；③、绘制View的内容；④、绘制View子视图，如果没有就不用；

⑤、还原图层（Layer）；⑥、绘制滚动条。

8、Serializable和Parcelable 的区别
1.P 消耗内存小

2.网络传输用S 程序内使用P

3.S将数据持久化方便

4.S使用了反射 容易触发垃圾回收 比较慢

高端技术面试题
这里讲的是大公司需要用到的一些高端Android技术，这里专门整理了一个文档，希望大家都可以看看。这些题目有点技术含量，需要好点时间去研究一下的。


一.图片

1、图片库对比
2、LRUCache原理
LruCache是个泛型类，主要原理是：把最近使用的对象用强引用存储在LinkedHashMap中，当缓存满时，把最近最少使用的对象从内存中移除，并提供get/put方法完成缓存的获取和添加。LruCache是线程安全的，因为使用了synchronized关键字。

当调用put()方法，将元素加到链表头，如果链表中没有该元素，大小不变，如果没有，需调用trimToSize方法判断是否超过最大缓存量，trimToSize()方法中有一个while(true)死循环，如果缓存大小大于最大的缓存值,会不断删除LinkedHashMap中队尾的元素，即最少访问的，直到缓存大小小于最大缓存值。当调用LruCache的get方法时，LinkedHashMap会调用recordAccess方法将此元素加到链表头部。

3、图片加载原理
 

4、自己去实现图片库，怎么做？
 

5、Glide源码解析
1）Glide.with(context)创建了一个RequestManager，同时实现加载图片与组件生命周期绑定：在Activity上创建一个透明的ReuqestManagerFragment加入到FragmentManager中，通过添加的Fragment感知Activty\Fragment的生命周期。因为添加到Activity中的Fragment会跟随Activity的生命周期。在RequestManagerFragment中的相应生命周期方法中通过liftcycle传递给在lifecycle中注册的LifecycleListener

2）RequestManager.load(url) 创建了一个RequestBuilder<T>对象 T可以是Drawable对象或是ResourceType等

3) RequestBuilder.into(view)

-->into(glideContext.buildImageViewTarget(view, transcodeClass))返回的是一个DrawableImageViewTarget, Target用来最终展示图片的，buildImageViewTarget-->ImageViewTargetFactory.buildTarget()根据传入class参数不同构建不同的Target对象，这个Class是根据构建Glide时是否调用了asBitmap()方法，如果调用了会构建出BitmapImageViewTarget，否则构建的是GlideDrawableImageViewTarget对象。

-->GenericRequestBuilder.into(Target),该方法进行了构建Request，并用RequestTracker.runRequest()

Request request = buildRequest(target);//构建Request对象，Request是用来发出加载图片的，它调用了buildRequestRecursive()方法以，内部调用了GenericRequest.obtain()方法
target.setRequest(request);
lifecycle.addListener(target);
requestTracker.runRequest(request);//判断Glide当前是不是处于暂停状态，若不是则调用Request.begin()方法来执行Request，否则将Request添加到待执行队列里，等暂停态解除了后再执行
-->GenericRequest.begin()

1）onSizeReady()--> Engine.load(signature, width, height, dataFetcher, loadProvider, transformation, transcoder,
            priority, isMemoryCacheable, diskCacheStrategy, this) --> a)先构建EngineKey; b) loadFromCache从缓存中获取EngineResource，如果缓存中获取到cache就调用cb.onResourceReady(cached)； c)如果缓存中不存在调用loadFromActiveResources从active中获取，如果获取到就调用cb.onResourceReady(cached)；d)如果active中也不存在，调用EngineJob.start(EngineRunnable), 从而调用decodeFromSource()/decodeFromCache()-->如果是调用decodeFromSource()-->ImageVideoFetcher.loadData()-->HttpUrlFetcher()调用HttpUrlConnection进行网络请求资源-->得于InputStream()后,调用decodeFromSourceData()-->loadProvider.getSourceDecoder().decode()方法解码-->GifBitmapWrapperResourceDecoder.decode()-->decodeStream()先从流中读取2个字节判断是GIF还是普通图，若是GIF调用decodeGifWrapper()来解码，若是普通静图则调用decodeBitmapWrapper()来解码-->bitmapDecoder.decode()

6、Glide使用什么缓存？
1) 内存缓存：LruResourceCache(memory)+弱引用activeResources

Map<Key, WeakReference<EngineResource<?>>> activeResources正在使用的资源，当acquired变量大于0，说明图片正在使用，放到activeResources弱引用缓存中，经过release()后，acquired=0,说明图片不再使用，会把它放进LruResourceCache中

2）磁盘缓存：DiskLruCache,这里分为Source(原始图片)和Result（转换后的图片）

第一次获取图片，肯定网络取，然后存active\disk中，再把图片显示出来，第二次读取相同的图片，并加载到相同大小的imageview中，会先从memory中取，没有再去active中获取。如果activity执行到onStop时，图片被回收，active中的资源会被保存到memory中，active中的资源被回收。当再次加载图片时，会从memory中取，再放入active中，并将memory中对应的资源回收。

之所以需要activeResources，它是一个随时可能被回收的资源，memory的强引用频繁读写可能造成内存激增频繁GC，而造成内存抖动。资源在使用过程中保存在activeResources中，而activeResources是弱引用，随时被系统回收，不会造成内存过多使用和泄漏。

7、Glide内存缓存如何控制大小？
Glide内存缓存最大空间(maxSize)=每个进程可用最大内存*0.4（低配手机是   每个进程可用最大内存*0.33）

磁盘缓存大小是250MB   int DEFAULT_DISK_CACHE_SIZE = 250 * 1024 * 1024;



二网络和安全机制

1.网络框架对比和源码分析
2.自己去设计网络请求框架，怎么做？
okhttp源码
3.网络请求缓存处理，okhttp如何处理网络缓存的；
(1)网络缓存优先考虑强制缓存，再考虑对比缓存

--首先判断强制缓存中的数据的是否在有效期内。如果在有效期，则直接使用缓存。如果过了有效期，则进入对比缓存。
--在对比缓存过程中，判断ETag是否有变动，如果服务端返回没有变动，说明资源未改变，使用缓存。如果有变动，判断Last-Modified。
--判断Last-Modified，如果服务端对比资源的上次修改时间没有变化，则使用缓存，否则重新请求服务端的数据，并作缓存工作。
（2）okhttp缓存

开启使用Okhttp的缓存其实很简单，只需要给OkHttpClient对象设置一个Cache对象即可，创建一个Cache时指定缓存保存的目录和缓存最大的大小即可。

//新建一个cache，指定目录为外部目录下的okhttp_cache目录，大小为100M
Cache cache = new Cache(new File(Environment.getExternalStorageDirectory() + "/okhttp_cache/"), 100 * 1024 * 1024);
//将cache设置到OkHttpClient中，这样缓存就开始生效了。
OkHttpClient client = new OkHttpClient.Builder().cache(cache).build();

相关的类有：

1）CacheControl( HTTP中的Cache-Control和Pragma缓存控制）：指定缓存规则

2）Cache(缓存类)

3）DiskLruCache(文件化的LRU缓存类）

（1）读取缓存：先获限OkHttpClient的Cache缓存对象，就是上面创建OkHttpClient设置的Cahce; 传Request请求到Cache的get方法查找缓存响应数据Response；构造一个缓存策略，再调用它的get去决策使用网络请求还是缓存响应。若使用缓存，它的cacheResponse不为空,networkRequest为空，用缓存构造响应直接返回。若使用请求，则cacheResponse为空,networkRequest不为空，开始网络请求流程。

Cache的get获取缓存方法，计算request的key值（请求url进行md5加密），根据key值去DisLruCache查找是否存在缓存内容，存则则创建绘存Entry实体。ENTRY_METADATA代表响应头信息，ENTRY_BODY代表响应体信息。如果缓存存在，在指定目录下会有两个文件****.0    *****.1分别存储某个请求缓存响应头和响应体信息。

CacheStrategy的get方法：1）若缓存响应为空或 2）请求是https但缓存响应没有握手信息；3）请求和缓存响应都是不可缓存的；4）请求是onCache，并且又包含if-Modified-Since或If-None-Match则不使用缓存； 再计算请求有效时间是否符合响应的过期时间，若响应在有效范围内，则缓存策略使用缓存，否则创建一个新的有条件的请求，返回有条件的缓存策略。

（2）存储缓存流程：从HttpEngine的readResponse()发送请求开始，判断hasBody(userResponse),如果缓存的话，maybeCache()缓存响应头信息，unzip(cacheWritingResponse(storeRequest, userResponse))缓存响应体。

 
4.从网络加载一个10M的图片，说下注意事项
5.TCP的3次握手和四次挥手
6.TCP与UDP的区别
7.TCP与UDP的应用
8.HTTP协议
9.HTTP1.0与2.0的区别
10.HTTP报文结构
11.HTTP与HTTPS的区别以及如何实现安全性
12.如何验证证书的合法性?
13.https中哪里用了对称加密，哪里用了非对称加密，对加密算法（如RSA）等是否有了解?
14.client如何确定自己发送的消息被server收到?
15.谈谈你对WebSocket的理解
16.WebSocket与socket的区别
17.谈谈你对安卓签名的理解。
18.请解释安卓为啥要加签名机制?
19.视频加密传输
20.App 是如何沙箱化，为什么要这么做？
21.权限管理系统（底层的权限是如何进行 grant 的）？
三.数据库

1.sqlite升级，增加字段的语句
2.数据库框架对比和源码分析
3.数据库的优化
4.数据库数据迁移问题

四.算法

1.排序算法有哪些？
2.最快的排序算法是哪个？
3.手写一个冒泡排序
4.手写快速排序代码
5.快速排序的过程、时间复杂度、空间复杂度
6.手写堆排序
7.堆排序过程、时间复杂度及空间复杂度
8.写出你所知道的排序算法及时空复杂度，稳定性
9.二叉树给出根节点和目标节点，找出从根节点到目标节点的路径
10给阿里2万多名员工按年龄排序应该选择哪个算法？
11.GC算法(各种算法的优缺点以及应用场景)
12.蚁群算法与蒙特卡洛算法
13.子串包含问题(KMP 算法)写代码实现
14一个无序，不重复数组，输出N个元素，使得N个元素的和相加为M，给出时间复杂度、.空间复杂度。手写算法
15.万亿级别的两个URL文件A和B，如何求出A和B的差集C(提示：Bit映射->hash分组->多文件读写效率->磁盘寻址以及应用层面对寻址的优化)
16.百度POI中如何试下查找最近的商家功能(提示：坐标镜像+R树)。
17.两个不重复的数组集合中，求共同的元素。
18.两个不重复的数组集合中，这两个集合都是海量数据，内存中放不下，怎么求共同的元素？
19.一个文件中有100万个整数，由空格分开，在程序中判断用户输入的整数是否在此文件中。说出最优的方法
20.一张Bitmap所占内存以及内存占用的计算

一张图片(bitmap)占用的内存影响因素：图片原始长、宽，手机屏幕密度，图片存放路径下的密度，单位像素占用字节数

bitmapSize=图片长度*（inTargetDensity手机的density / inDensity图片存放目录的density）*宽度*（手机的inTargetDensity / inDensity目标存放目录的density）*单位像素占用的字节数（图片长宽单位是像素）

1）图片长宽单位是像素：单位像素字节数由其参数BitmapFactory.Options.inPreferredConfig变量决定，它是Bitmap.Config类型，包括以下几种值：ALPHA_8图片只有alpha值，占用一个字节；ARGB_4444 一个像素占用2个字节,A\R\G\B各占4bits；ARGB_8888一个像素占用4个字节，A\R\G\B各占8bits（高质量图片格式，bitmap默认格式）；ARGB_565一个像素占用2字节，不支持透明和半透明，R占5bit, Green占6bit, Blue占用5bit. 从Android4.0开始该项无效。

2） inTargetDensity 手机的屏幕密度(跟手机分辨率有关系)

inDensity原始资源密度（mdpi:160;   hdpi:240;   xhdpi:320;   xxhdpi:480; xxxhdpi:640）

 

当Bitmap对象在不使用时，应该先调用recycle()，再将它设置为null，虽然Bitmap在被回收时可通过BitmapFinalizer来回收内存。但只有系统垃圾回收时才会回收。Android4.0之前，Bitmap内存分配在Native堆中，Android4.0开始，Bitmap的内存分配在dalvik堆中，即Java堆中，调用recycle()并不能立即释放Native内存。


21. 2000万个整数，找出第五十大的数字？
22.烧一根不均匀的绳，从头烧到尾总共需要1个小时。现在有若干条材质相同的绳子，问如何用烧绳的方法来计时一个小时十五分钟呢？
23.求1000以内的水仙花数以及40亿以内的水仙花数
24.  5枚硬币，2正3反如何划分为两堆然后通过翻转让两堆中正面向上的硬8币和反面向上的硬币个数相同
25.时针走一圈，时针分针重合几次
26.N*N的方格纸,里面有多少个正方形
27.x个苹果，一天只能吃一个、两个、或者三个，问多少天可以吃完？

五.插件化、模块化、组件化、热修复、增量更新、Gradle

1.对热修复和插件化的理解
2.插件化原理分析
3.模块化实现（好处，原因）
4.热修复,插件化
5.项目组件化的理解
6.描述清点击 Android Studio 的 build 按钮后发生了什么

六.架构设计和设计模式

1.谈谈你对Android设计模式的理解
2.MVC MVP MVVM原理和区别
3.你所知道的设计模式有哪些？
4.项目中常用的设计模式
5.手写生产者/消费者模式
6.写出观察者模式的代码
7.适配器模式，装饰者模式，外观模式的异同？
8.用到的一些开源框架，介绍一个看过源码的，内部实现过程。
9.谈谈对RxJava的理解
RxJava是基于响应式编程，基于事件流、实现异步操（类似于Android中的AsyncTask、Handler作用）作的库，基于事件流的链式调用，使得RxJava逻辑简洁、使用简单。RxJava原理是基于一种扩展的观察者模式，有四种角色：被观察者Observable 观察者Observer 订阅subscribe 事件Event。RxJava原理可总结为：被观察者Observable通过订阅(subscribe)按顺序发送事件（Emitter)给观察者(Observer)， 观察者按顺序接收事件&作出相应的响应动作。

RxJava中的操作符：

1）defer():直到有观察者(Observer)订阅时，才会动态创建被观察者对象(Observer)&发送事件，通过Observer工厂方法创建被观察者对象，每次订阅后，都会得到一个刚创建的最新的Observer对象，可以确保Observer对象里的数据是最新的。defer()方法只会定义Observable对象，只有订阅操作才会创建对象。

Observable<T> observable = Observable.defer(new Callable<ObservableSource<? extends T>>() {
    @Override
    public ObservableSource<? extends T> call() throws Exception {
        return Observable.just();
    }
}

2）timer() 快速创建一个被观察者(Observable)，延迟指定时间后，再发送事件

Observable.timer(2, TimeUnit.SECONDS)//也可以自定义线程timer(long, TimeUnit, Scheduler)
    .subscribe(new Observer<Long>() {
        @Override
        public void onSubscribe(Disposable d) {
        }
        ...
 
     });

3) interval() intervalRange() 快速创建一个被观察者对象（Observable)，每隔指定时间就发送事件

//interval三个参数，参数1：第一次延迟时间  参数2：间隔时间数字   参数3：时间单位
Observable.interval(3, 1, TimeUnit.SECONDS).subscribe();
//intervalRange五个参数，参数1：事件序列起始点  参数2：事件数量  参数3：第一次延迟时间 参数4：间隔时间数字   参数5：时间单位
Observable.intervalRange(3, 10, 2, 1, TimeUnit.SECONDS).subscribe();
RxJava的功能与原理实现

10.Rxjava发送事件步骤：

1）创建被观察者对象Observable&定义需要发送的事件

Observable.create(new ObservableOnSubscribe<T>(){
    @Override
    public void subscribe(ObservableEmitter<T> emitter) throws Exception {
        //定义发送事件的行为
    }
});

Observable.create()方法实际创建了一个ObservableCreate对象，它是Observable的子类，传入一个ObservableOnSubscribe对象，复写了发送事件行为的subscribe()方法。

2）创建观察者对象Observer&定义响应事件的行为

Observer observer = new Observer<T>() {
 
    @Override
    public void onSubscribe(Disposable d){//Disposable对象可用于结束事件
        //默认最先调用
    }
    
    @Override
    public void onNext(T t){
    
    }
 
    @Override
    public void onError(Throwable d){
    
    }
 
    @Override
    public void onComplete(){
    
    }
 
}

3）通过subscribe()方法使观察者订阅被观察者

Observable.subscribe(Observer observer);//实际调用的是ObservableCreate.subscribeActual()方法，具体实现如下
 
protected void subscribeActual(Observer<? super T> observer) {
 
              // 1. 创建1个CreateEmitter对象用于发射事件（封装成1个Disposable对象）
            CreateEmitter<T> parent = new CreateEmitter<T>(observer);
            // 2. 调用观察者（Observer）的onSubscribe（）
            observer.onSubscribe(parent);
            try {
                // 3. 调用source对象的（ObservableOnSubscribe对象）subscribe（）
                source.subscribe(parent);
            } catch (Throwable ex) {
                Exceptions.throwIfFatal(ex);
                parent.onError(ex);
            }
}

 

11.RxJava的作用，与平时使用的异步操作来比的优缺点
12.说说EventBus作用，实现方式，代替EventBus的方式
13.从0设计一款App整体架构，如何去做？
14.说一款你认为当前比较火的应用并设计(比如：直播APP，P2P金融，小视频等)
15.谈谈对java状态机理解
16.Fragment如果在Adapter中使用应该如何解耦？
17.Binder机制及底层实现
18.对于应用更新这块是如何做的？(解答：灰度，强制更新，分区域更新)？
19.实现一个Json解析器(可以通过正则提高速度)
20.统计启动时长,标准

七.性能优化

1.如何对Android 应用进行性能分析以及优化?
2.ddms 和 traceView
3.性能优化如何分析systrace？
4.用IDE如何分析内存泄漏？
5.Java多线程引发的性能问题，怎么解决？
6.启动页白屏及黑屏解决？
7.启动太慢怎么解决？
8.怎么保证应用启动不卡顿？
9.App启动崩溃异常捕捉
10自定义View注意事项
11.现在下载速度很慢,试从网络协议的角度分析原因,并优化(提示：网络的5层都可以涉及)。
12.Https请求慢的解决办法（提示：DNS，携带数据，直接访问IP）
13.如何保持应用的稳定性
14.RecyclerView和ListView的性能对比
15.ListView的优化
16.RecycleView优化
17.View渲染
18.Bitmap如何处理大图，如一张30M的大图，如何预防OOM
19.java中的四种引用的区别以及使用场景
20.强引用置为null，会不会被回收？

八.NDK、jni、Binder、AIDL、进程通信有关

1.请介绍一下NDK
2.什么是NDK库?
3.jni用过吗？
4.如何在jni中注册native函数，有几种注册方式?
5.Java如何调用c、c++语言？
6.jni如何调用java层代码？
7.进程间通信的方式？
8.Binder机制
9.简述IPC？
10.什么是AIDL？
11.AIDL解决了什么问题？
12.AIDL如何使用？
13.Android 上的 Inter-Process-Communication 跨进程通信时如何工作的？
14.多进程场景遇见过么？
15.Android进程分类？
16.进程和 Application 的生命周期？
17.进程调度
18.谈谈对进程共享和线程安全的认识
19谈谈对多进程开发的理解以及多进程应用场景
20.什么是协程？

九.framework层、ROM定制、Ubuntu、Linux之类的问题

1.java虚拟机的特性
2.谈谈对jvm的理解
3.JVM内存区域，开线程影响哪块内存
4.对Dalvik、ART虚拟机有什么了解？
5.Art和Dalvik对比
6.虚拟机原理，如何自己设计一个虚拟机(内存管理，类加载，双亲委派)
7.谈谈你对双亲委派模型理解
8.JVM内存模型，内存区域
9.类加载机制
10.谈谈对ClassLoader(类加载器)的理解
11.谈谈对动态加载（OSGI）的理解
12.内存对象的循环引用及避免
13.内存回收机制、GC回收策略、GC原理时机以及GC对象
14.垃圾回收机制与调用System.gc()区别
15.Ubuntu编译安卓系统
16.系统启动流程是什么？（提示：Zygote进程 –> SystemServer进程 –> 各种系统服务 –> 应用进程）
17.大体说清一个应用程序安装到手机上时发生了什么
18.简述Activity启动全部过程
19.App启动流程，从点击桌面开始
20.逻辑地址与物理地址，为什么使用逻辑地址？
21.Android为每个应用程序分配的内存大小是多少？
22.Android中进程内存的分配，能不能自己分配定额内存？
23.进程保活的方式
24.如何保证一个后台服务不被杀死？（相同问题：如何保证service在后台不被kill？）比较省电的方式是什么？
25.App中唤醒其他进程的实现方式


安卓面试突破专题课程
Android 基础与底层机制
1.数据库的操作类型有哪些，如何导入外部数据库？
读懂题目。如果碰到问题比较模糊的时候可以适当问问面试官。
配合面试官来面试：面试是一个相互了解的过程，要充分利用面试的题目和时间把自己的能力和技术展现出来，面试官能够看到你的真实技术。
1）使用数据库的方式有哪些？
（1）openOrCreateDatabase(String path);
（2）继承SqliteOpenHelper类对数据库及其版本进行管理(onCreate,onUpgrade)
当在程序当中调用这个类的方法getWritableDatabase()或者getReadableDatabase();的时候才会打开数据库。如果当时没有数据库文件的时候，系统就会自动生成一个数据库。
2）操作的类型：增删改查CRUD
直接操作SQL语句：SQliteDatabase.execSQL(sql);
面向对象的操作方式：SQLiteDatabase.insert(table, nullColumnHack, ContentValues);
如何导入外部数据库？
一般外部数据库文件可能放在SD卡或者res/raw或者assets目录下面。
写一个DBManager的类来管理，数据库文件搬家，先把数据库文件复制到”/data/data/包名/databases/”目录下面，然后通过db.openOrCreateDatabase(db文件),打开数据库使用。
我上一个项目就是这么做的，由于app上架之前就有一些初始数据需要内置，也会碰到数据的升级等问题，我是这么做的…… 同时我碰到最有意思的问题就是关于数据库并发操作的问题，比如：多线程操作数据库的时候，我采取的是封装使用互斥锁来解决……
2.是否使用过本地广播，和全局广播有什么差别？
引入本地广播的机制是为了解决安全性的问题：
1）正在发送的广播不会脱离应用程序，比用担心app的数据泄露；
2）其他的程序无法发送到我的应用程序内部，不担心安全漏洞。（比如：如何做一个杀不死的服务---监听火的app 比如微信、友盟、极光的广播，来启动自己。）
3）发送本地广播比发送全局的广播高效。（全局广播要维护的广播集合表 效率更低。全局广播，意味着可以跨进程，就需要底层的支持。）
本地广播不能用静态注册。----静态注册：可以做到程序停止后还能监听。
使用：
（1）注册
LocalBroadcastManager.getInstance(this).registerReceiver(new XXXBroadCastReceiver(), new IntentFilter(action));
（2）取消注册：
LocalBroadcastManager.getInstance(this).unregisterReceiver(receiver)

3.是否使用过 IntentService，作用是什么， AIDL 解决了什么问题？ (小米) 
如果有一个任务，可以分成很多个子任务，需要按照顺序来完成，如果需要放到一个服务中完成，那么使用IntentService是最好的选择。
一般我们所使用的Service是运行在主线程当中的，所以在service里面编写耗时的操作代码，则会卡主线程会ANR。为了解决这样的问题，谷歌引入了IntentService.
IntentService的优点：
（1）它创建一个独立的工作线程来处理所有一个一个intent。
（2）创建了一个工作队列，来逐个发送intent给onHandleIntent()
（3）不需要主动调用stopSelf()来结束服务，因为源码里面自己实现了自动关闭。
（4）默认实现了onBind()返回的null。
（5）默认实现的onStartCommand()的目的是将intent插入到工作队列。
总结：使用IntentService的好处有哪些。首先，省去了手动开线程的麻烦；第二，不用手动停止service；第三，由于设计了工作队列，可以启动多次---startService(),但是只有一个service实例和一个工作线程。一个一个熟悉怒执行。
AIDL 解决了什么问题？
AIDL的全称：Android Interface Definition Language，安卓接口定义语言。
由于Android系统中的进程之间不能共享内存，所以需要提供一些机制在不同的进程之间进行数据通信。
远程过程调用：RPC—Remote Procedure Call。  安卓就是提供了一种IDL的解决方案来公开自己的服务接口。AIDL:可以理解为双方的一个协议合同。双方都要持有这份协议---文本协议 xxx.aidl文件（安卓内部编译的时候会将aidl协议翻译生成一个xxx.java文件---代理模式：Binder驱动有关的，Linux底层通讯有关的。）
在系统源码里面有大量用到aidl，比如系统服务。
电视机顶盒系统开发。你的服务要暴露给别的开发者来使用。
讲解Binder机制。
4.Activity、 Window、 View 三者的差别， fragment 的特点？（360）
Activity、 Window、 View 三者如何协同显示界面的。---考点：显示的过程(view 绘制流程)源码的熟悉度。
Activity剪窗花的人（控制的）；Window窗户（承载的一个模型）；View窗花（要显示的视图View）；LayoutInflater剪刀---将布局（图纸）剪成窗花。
（Alt+方向箭头）

fragment 的特点？（你用fragment有没有领略到一些乐趣，或者有没有踩过什么坑？）
fragment的设计主要是把Activity界面包括其逻辑打碎成很多个独立的模块，这样便于模块的重用和更灵活地组装呈现多样的界面。
1）Fragment可以作为Activity界面的一个部分组成；
2）可以在一个Activity里面出现多个Fragment，并且一个fragment可以在多个Activity中使用；
3）在Activity运行中，可以动态地添加、删除、替换Fragment。
4）Fragment有自己的生命周期的，可以响应输入事件。
踩过的坑：1.重叠；2. 注解newAPI（兼容包解决）；3. Setarguement()初始化数据;4. 不能在onsave...（）方法后，commit; 5. 入栈出栈问题; --事务。像Activity跳转一样的效果，同时返回的时候还能回到之前的页面(fragment)并且状态都还在。replace(f1,f2)严重影响生命周期:add()+show+hide

5. 描述一次网络请求的流程（新浪）（Jason）
6. Handler、 Thread 和 HandlerThread 的差别（小米）（Jason）
7. 低版本 SDK 实现高版本 api（小米）
	两种情况：
1）一般很多高版本的新的API都会在兼容包里面找到替代的实现。比如fragment。
Notification，在v4兼容包里面有NotificationCompat类。5.0+出现的backgroundTint，minSdk小于5.0的话会包检测错误，v4兼容包DrawableCompat类。

2）没有替代实现就自己手动实现。比如：控件的水波纹效果—第三方实现。
或者直接在低版本去除这个效果。
3）补充:如果设置了minSDK但是代码里面使用了高版本的API，会出现检测错误。需要在代码里面使用声明编译检测策略，比如：@SuppressLint和@TargetApi注解提示编译器编译的规则。@SuppressLint是忽略检测；@TargetApi=23，会根据你函数里面使用的API，严格地匹配SDK版本，给出相应的编译错误提示。
4）为了避免位置的错误，最好不要使用废弃api。（一般情况下不会有兼容性问题，后面可能会随时删除这个API方法；性能方面的问题。）
5）http://chinagdg.org/2016/01/picking-your-compilesdkversion-minsdkversion-targetsdkversion/

8. launch mode 应用场景（百度、小米、乐视）
栈：先进后出
标准模式
SingleTop：使用场景：浏览器的书签；通讯消息聊天界面。
SingleTask：使用场景：某个Activity当做主界面的时候。
SingleInstance：使用场景：比如浏览器BrowserActivity很耗内存，很多app都会要调用它，这样就可以把该Activity设置成单例模式。比如：闹钟闹铃。
9. touch 事件传递流程（小米）
10. view 绘制流程（百度）
Measure：测量，测量自己。如果是ViewGroup就需要测量里面的所有childview.
	测量的结果怎么办？setMeasuredDimension(resolveSizeAndState(maxWidth, widthMeasureSpec, childState), heightSizeAndState);设置自己的大小。
Layout: 摆放，把自己摆放在哪个位置。如果是ViewGroup就需要发放里面的所有childview.
	怎么去具体摆放呢？
Draw:绘制
        /*
         * Draw traversal performs several drawing steps which must be executed
         * in the appropriate order:
         *
         *      1. Draw the background
         *      2. If necessary, save the canvas' layers to prepare for fading
         *      3. Draw view's content
         *      4. Draw children
         *      5. If necessary, draw the fading edges and restore layers
         *      6. Draw decorations (scrollbars for instance)
         */
11. 什么情况导致内存泄漏（美团）
1）什么是内存泄漏：最好解释清楚GC垃圾回收机制以及概念GC Root。
2）为什么会有内存泄漏：因为内存泄漏是属于人为的失误造成的。而且面向对象开发关系复杂、多线程的关系，很容易出现引用层级关系很深以及很混乱。
3）什么情况容易导致内存泄漏：
4）如何解决内存泄漏：
12. ANR 定位和修正
可以通过查看/data/anr/traces.txt查看ANR信息。
根本原因是：主线程被卡了，导致应用在5秒时间未响应用户的输入事件。
很多种ANR错误出现的场景：
1）主线程当中执行IO/网络操作，容易阻塞。
2）主线程当中执行了耗时的计算。----自定义控件的时候onDraw方法里面经常这么做。
（同时聊一聊自定义控件的性能优化：在onDraw里面创建对象容易导致内存抖动---绘制动作会大量不断调用，产生大量垃圾对象导致GC很频繁就造成了内存抖动。）内存抖动就容易造成UI出现掉帧卡顿的问题
3）BroadCastReceiver没有在10秒内完成处理。
4）BroadCastReceiver的onReceived代码中也要尽量减少耗时的操作，建议使用IntentService处理。
5）Service执行了耗时的操作，因为service也是在主线程当中执行的，所以耗时操作应该在service里面开启子线程来做。
6）使用AsyncTask处理耗时的IO等操作。
7）使用Thread或者HandlerThread时，使用Process.setThreadPriority(Process.THREAD_PRIORITY_BACKGROUND)或者java.lang.Thread.setPriority （int priority）设置优先级为后台优先级，这样可以让其他的多线程并发消耗CPU的时间会减少，有利于主线程的处理。
8）Activity的onCreate和onResume回调中尽量耗时的操作。
13. 什么情况导致 oom（乐视、美团）
	OOM产生的原因：内存不足，android系统为每一个应用程序都设置了一个硬性的条件：DalvikHeapSize最大阀值64M/48M/24M.如果你的应用程序内存占用接近这个阀值，此时如果再尝试内存分配的时候就会造成OOM。
1)内存泄露多了就容易导致OOM
2)大图的处理。压缩图片。平时开发就要注意对象的频繁创建和回收。
3）可以适当的检测：ActivityManager.getMemoryClass()可以用来查询当前应用的HeapSize阀值。可以通过命名adb shellgetProp | grep dalvik.vm.heapxxxlimit查看。
如何避免内存泄露：
1）减小对象的内存占用：
a)使用更加轻量级的数据结构：
考虑适当的情况下替代HashMap等传统数据结构而使用安卓专门为手机研发的数据结构类ArrayMap/SparseArray。SparseLongMap/SparseIntMap/SparseBoolMap更加高效。
HashMap.put(string,Object);Object o = map.get(string);会导致一些没必要的自动装箱和拆箱。
b)适当的避免在android中使用Enum枚举，替代使用普通的static常量。（一般还是提倡多用枚举---软件的架构设计方面；如果碰到这个枚举需要大量使用的时候就应该更加倾向于解决性能问题。）。
c)较少Bitmap对象的内存占用。
使用inSampleSize:计算图片压缩比例进行图片压缩，可以避免大图加载造成OOM; decodeformat：图片的解码格式选择，ARGB_8888/RGB_565/ARGB_4444/ALPHA_8,还可以使用WebP。
d)使用更小的图片
资源图片里面，是否存在还可以继续压缩的空间。
2）内存对象的重复利用：
使用对象池技术，两种：1.自己写；2.利用系统既有的对象池机制。比如LRU(Last Recently Use)算法。
a)ListView/GridView源码可以看到重用的情况ConvertView的复用。RecyclerView中Recycler源码。
b)Bitmap的复用
Listview等要显示大量图片。需要使用LRU缓存机制来复用图片。
C)	避免在onDraw方法里面执行对象的创建，要复用。避免内存抖动。
D) 常见的java基础问题---StringBuilder等
3）避免对象的内存泄露：
4）使用一些内存的优化策略：
看文档
14.Android Service 与 Activity 之间通信的几种方式
	1）通过Binder
	2）通过广播
15. Android 各个版本 API 的区别
	把几个关键版本的特性记住：3.0/4.0、4.4、5.0、6.0/7.0
16.如何保证一个后台服务不被杀死,比较省电的方式是什么？（百度）
17. Requestlayout， onlayout， onDraw， DrawChild 区别与联系（猎豹）
RequestLayout()方法：会导致调用Measure()方法和layout()。将会根据标志位判断是否需要onDraw();
onLayout()：摆放viewGroup里面的子控件
onDraw()：绘制视图本身；（ViewGroup还需要绘制里面的所有子控件）
drawChild(): 重新回调每一个子视图的draw方法。child.draw(canvas, this, drawingTime);
18. invalidate()和 postInvalidate() 的区别及使用（百度）
invalidate()：在主线程当中刷新；
postInvalidate()：在子线程当中刷新；其实最终调用的就是invalidate，原理依然是通过工作线程向主线程发送消息这一机制。
    public void postInvalidate() {
        postInvalidateDelayed(0);
    }
    public void postInvalidateDelayed(long delayMilliseconds) {
        // We try only with the AttachInfo because there's no point in invalidating
        // if we are not attached to our window
        final AttachInfo attachInfo = mAttachInfo;
        if (attachInfo != null) {
            attachInfo.mViewRootImpl.dispatchInvalidateDelayed(this, delayMilliseconds);
        }
    }

    public void dispatchInvalidateDelayed(View view, long delayMilliseconds) {
        Message msg = mHandler.obtainMessage(MSG_INVALIDATE, view);
        mHandler.sendMessageDelayed(msg, delayMilliseconds);
    }
public void handleMessage(Message msg) {
            switch (msg.what) {
            case MSG_INVALIDATE:
                ((View) msg.obj).invalidate();
                break;

19. Android 动画框架实现原理（腾讯）
传统的动画框架：View.startAnimation();
弊端：移动后不能点击。原因？跟实现机制有关系。
所有的透明度、旋转、平移、缩放动画，都是在view不断刷新调用draw的情况下实现的。
调用的canvas.translate(xxx),canvas.scaleX(xxx)…. Xxx:matrix像素矩阵来控制动画的数据。记得看源码，结合多只缩放的demo看源码。

20.Android 为每个应用程序分配的内存大小是多少？（美团）
看具体的手机平台，常见的有：64M/32M等

21.LinearLayout 对比 RelativeLayout（百度）
性能对比：LinearLayout的性能要比RelativeLayout好。
因为RelativeLayout会测量两次。而默认情况下（没有设置weight）LinearLayout只会测量一次。
为什么RelativeLayout会测量两次？首先RelativeLayout中的子view排列方式是基于彼此依赖的关系，而这个依赖可能和布局中view的顺序无关，在确定每一个子view的位置的时候，就需要先给每一个子view排一下序。又因为RelativeLayout允许横向和纵向相互依赖，所以需要横向纵向分别进行一次排序测量。

22. 优化自定义 view（百度、乐视、小米）
	1）减少在onDraw里面大量计算和对象创建和大量内存分配。
	2）应该尽量少用invalidate()次数。
3）view里面耗时的操作layout。减少requestLayout（）避免让UI系统重新遍历整棵树。Mearsure。
4）如果你有一个很复杂的布局，不如将这个复杂的布局直接使用你自己的写的ViewGroup来实现。减少了一个树的层次关系 全部都是自己测量和layout，达到优化的目的。（Facebook就经常这么干）
24. ContentProvider（乐视）
提示：跨进程通信。进程之间进行数据交互共享。；源码来一剁。
25.fragment 生命周期
26.26. volley 解析（美团、乐视）
27.27. Android Glide 源码解析
28.28. Android 属性动画特性（乐视、小米）

1)现在有 T1、T2、T3 三个线程，你怎样保证 T2 在 T1 执行完后执行，T3 在 T2 执行完后执 行?
这个线程问题通常会在第一轮或电话面试阶段被问到，目的是检测你对”join”方法是否熟 悉。这个多线程问题比较简单，可以用 join 方法实现。
2)在 Java 中 Lock 接口比 synchronized 块的优势是什么?你需要实现一个高效的缓存，它允 许多个用户读，但只允许一个用户写，以此来保持它的完整性，你会怎样去实现它?
lock 接口在多线程和并发编程中最大的优势是它们为读和写分别提供了锁，它能满足你写 像 ConcurrentHashMap 这样的高性能数据结构和有条件的阻塞。Java 线程面试的问题越来 越会根据面试者的回答来提问。我强烈建议在你去参加多线程的面试之前认真读一下 Locks，因为当前其大量用于构建电子交易终统的客户端缓存和交易连接空间。
3)在 java 中 wait 和 sleep 方法的不同?
通常会在电话面试中经常被问到的 Java 线程面试问题。最大的不同是在等待时 wait 会释放
锁，而 sleep 一直持有锁。Wait 通常被用于线程间交互，sleep 通常被用于暂停执行。
4)用 Java 实现阻塞队列。
这是一个相对艰难的多线程面试问题，它能达到很多的目的。第一，它可以检测侯选者是 否能实际的用 Java 线程写程序;第二，可以检测侯选者对并发场景的理解，并且你可以根 据这个问很多问题。如果他用 wait()和 notify()方法来实现阻塞队列，你可以要求他用最新 的 Java 5 中的并发类来再写一次。
5)用 Java 写代码来解决生产者——消费者问题。
与上面的问题很类似，但这个问题更经典，有些时候面试都会问下面的问题。在 Java 中怎 么解决生产者——消费者问题，当然有很多解决方法，我已经分享了一种用阻塞队列实现 的方法。有些时候他们甚至会问怎么实现哲学家进餐问题。
6)用 Java 编程一个会导致死锁的程序，你将怎么解决?
这是我最喜欢的 Java 线程面试问题，因为即使死锁问题在写多线程并发程序时非常普遍， 但是很多侯选者并不能写 deadlock free code(无死锁代码?)，他们很挣扎。只要告诉他 们，你有 N 个资源和 N 个线程，并且你需要所有的资源来完成一个操作。为了简单这里的 n 可以替换为 2，越大的数据会使问题看起来更复杂。通过避免 Java 中的死锁来得到关于 死锁的更多信息。
7) 什么是原子操作，Java 中的原子操作是什么?
非常简单的 java 线程面试问题，接下来的问题是你需要同步一个原子操作。

8) Java 中的 volatile 关键是什么作用?怎样使用它?在 Java 中它跟 synchronized 方法有什 么不同?
自从 Java 5 和 Java 内存模型改变以后，基于 volatile 关键字的线程问题越来越流行。应该 准备好回答关于 volatile 变量怎样在并发环境中确保可见性。
9) 什么是竞争条件?你怎样发现和解决竞争?
这是一道出现在多线程面试的高级阶段的问题。大多数的面试官会问最近你遇到的竞争条 件，以及你是怎么解决的。有些时间他们会写简单的代码，然后让你检测出代码的竞争条 件。可以参考我之前发布的关于 Java 竞争条件的文章。在我看来这是最好的 java 线程面试 问题之一，它可以确切的检测候选者解决竞争条件的经验，or writing code which is free of data race or anyotherrace condition。关于这方面最好的书是《Concurrency practices in Java》。
10) 你将如何使用 threaddump?你将如何分析 Thread dump?
在 UNIX 中你可以使用 kill -3，然后 thread dump 将会打印日志，在 windows 中你可以使 用”CTRL+Break”。非常简单和专业的线程面试问题，但是如果他问你怎样分析它，就会很 棘手。
11) 为什么我们调用 start()方法时会执行 run()方法，为什么我们不能直接调用 run()方法?
这是另一个非常经典的 java 多线程面试问题。这也是我刚开始写线程程序时候的困惑。现 在这个问题通常在电话面试或者是在初中级 Java 面试的第一轮被问到。这个问题的回答应 该是这样的，当你调用 start()方法时你将创建新的线程，并且执行在 run()方法里的代码。 但是如果你直接调用 run()方法，它不会创建新的线程也不会执行调用线程的代码。阅读我 之前写的《start 与 run 方法的区别》这篇文章来获得更多信息。
12) Java 中你怎样唤醒一个阻塞的线程?
这是个关于线程和阻塞的棘手的问题，它有很多解决方法。如果线程遇到了 IO 阻塞，我并 且不认为有一种方法可以中止线程。如果线程因为调用 wait()、sleep()、或者 join()方法而 导致的阻塞，你可以中断线程，并且通过抛出 InterruptedException 来唤醒它。我之前写的 《How to deal with blocking methods in java》有很多关于处理线程阻塞的信息。
13)在 Java 中 CycliBarriar 和 CountdownLatch 有什么区别? 这个线程问题主要用来检测你是否熟悉 JDK5 中的并发包。这两个的区别是 CyclicBarrier 可
以重复使用已经通过的障碍，而 CountdownLatch 不能重复使用。
14) 什么是不可变对象，它对写并发应用有什么帮助? 另一个多线程经典面试问题，并不直接跟线程有关，但间接帮助很多。这个 java 面试问题

可以变的非常棘手，如果他要求你写一个不可变对象，或者问你为什么 String 是不可变 的。
15) 你在多线程环境中遇到的常见的问题是什么?你是怎么解决它的?
多线程和并发程序中常遇到的有 Memory-interface、竞争条件、死锁、活锁和饥饿。问题 是没有止境的，如果你弄错了，将很难发现和调试。这是大多数基于面试的，而不是基于 实际应用的 Java 线程问题。

2019Android多线程总结
1.什么是线程
线程就是进程中运行的多个子任务，是操作系统调用的最小单元
2.线程的状态
New:新建状态，new出来，还没有调用start
Runnable:可运行状态，调用start进入可运行状态，可能运行也可能没有运行，取决于操作系统的调度
Blocked:阻塞状态，被锁阻塞，暂时不活动，阻塞状态是线程阻塞在进入synchronized关键字修饰的方法或代码块(获取锁)时的状态。
Waiting：等待状态，不活动，不运行任何代码，等待线程调度器调度，wait sleep
Timed Waiting:超时等待，在指定时间自行返回
Terminated:终止状态，包括正常终止和异常终止
3.线程的创建
a.继承Thread重写run方法
b.实现Runnable重写run方法
c.实现Callable重写call方法
实现Callable和实现Runnable类似，但是功能更强大，具体表现在
a.可以在任务结束后提供一个返回值，Runnable不行
b.call方法可以抛出异常，Runnable的run方法不行
c.可以通过运行Callable得到的Fulture对象监听目标线程调用call方法的结果，得到返回值，（fulture.get(),调用后会阻塞，直到获取到返回值）
4.线程中断
一般情况下，线程不执行完任务不会退出，但是在有些场景下，我们需要手动控制线程中断结束任务，Java中有提供线程中断机制相关的Api,每个线程都一个状态位用于标识当前线程对象是否是中断状态
public boolean isInterrupted() //判断中断标识位是否是true，不会改变标识位public void interrupt()  //将中断标识位设置为truepublic static boolean interrupted() //判断当前线程是否被中断，并且该方法调用结束的时候会清空中断标识位
需要注意的是interrupt（）方法并不会真的中断线程，它只是将中断标识位设置为true,具体是否要中断由程序来判断，如下，只要线程中断标识位为false,也就是没有中断就一直执行线程方法
new Thread(new Runnable(){
      while(!Thread.currentThread().isInterrupted()){
              //执行线程方法
      }
}).start();
前边我们提到了线程的六种状态，New Runnable Blocked Waiting Timed Waiting Terminated，那么在这六种状态下调用线程中断的代码会怎样呢，New和Terminated状态下，线程不会理会线程中断的请求，既不会设置标记位，在Runnable和Blocked状态下调用interrupt会将标志位设置位true,在Waiting和Timed Waiting状态下会发生InterruptedException异常，针对这个异常我们如何处理？
1.在catch语句中通过interrupt设置中断状态，因为发生中断异常时，中断标志位会被复位，我们需要重新将中断标志位设置为true，这样外界可以通过这个状态判断是否需要中断线程
try{
    ....
}catch(InterruptedException e){
    Thread.currentThread().interrupt();
}
2.更好的做法是，不捕获异常，直接抛出给调用者处理，这样更灵活
5.Thread为什么不能用stop方法停止线程
从SUN的官方文档可以得知，调用Thread.stop()方法是不安全的，这是因为当调用Thread.stop()方法时，会发生下面两件事：
1.即刻抛出ThreadDeath异常，在线程的run()方法内，任何一点都有可能抛出ThreadDeath Error，包括在catch或finally语句中。
2.释放该线程所持有的所有的锁。调用thread.stop()后导致了该线程所持有的所有锁的突然释放，那么被保护数据就有可能呈现不一致性，其他线程在使用这些被破坏的数据时，有可能导致一些很奇怪的应用程序错误。
6.重入锁与条件对象，同步方法和同步代码块
。。。
7.volatile关键字
volatile为实例域的同步访问提供了免锁机制，如果声明一个域为volatile，那么编译器和虚拟机就直到该域可能被另一个线程并发更新
8.java内存模型
堆内存是被所有线程共享的运行时内存区域，存在可见性的问题。线程之间共享变量存储在主存中，每个线程都有一个私有的本地内存，本地内存存储了该线程共享变量的副本（本地内存是一个抽象概念，并不真实存在），两个线程要通信的话，首先A线程把本地内存更新过的共享变量更新到主存中，然后B线程去主存中读取A线程更新过的共享变量，也就是说假设线程A执行了i = 1这行代码更新主线程变量i的值，会首先在自己的工作线程中堆变量i进行赋值，然后再写入主存当中，而不是直接写入主存
9.原子性 可见性 有序性
原子性：对基本数据类型的读取和赋值操作是原子性操作，这些操作不可被中断，是一步到位的，例如x=3是原子性操作，而y = x就不是，它包含两步：第一读取x，第二将x写入工作内存；x++也不是原子性操作，它包含三部，第一，读取x，第二，对x加1，第三，写入内存。原子性操作的类如：AtomicInteger AtomicBoolean AtomicLong AtomicReference
可见性：指线程之间的可见性，既一个线程修改的状态对另一个线程是可见的。volatile修饰可以保证可见性，它会保证修改的值会立即被更新到主存，所以对其他线程是可见的，普通的共享变量不能保证可见性，因为被修改后不会立即写入主存，何时被写入主存是不确定的，所以其他线程去读取的时候可能读到的还是旧值
有序性：Java中的指令重排序（包括编译器重排序和运行期重排序）可以起到优化代码的作用，但是在多线程中会影响到并发执行的正确性，使用volatile可以保证有序性，禁止指令重排
volatile可以保证可见性 有序性，但是无法保证原子性，在某些情况下可以提供优于锁的性能和伸缩性，替代sychronized关键字简化代码，但是要严格遵循使用条件。
10.线程池ThreadPoolExecutor
线程池的工作原理：线程池可以减少创建和销毁线程的次数，从而减少系统资源的消耗，当一个任务提交到线程池时
a. 首先判断核心线程池中的线程是否已经满了，如果没满，则创建一个核心线程执行任务，否则进入下一步
b. 判断工作队列是否已满，没有满则加入工作队列，否则执行下一步
c. 判断线程数是否达到了最大值，如果不是，则创建非核心线程执行任务，否则执行饱和策略，默认抛出异常
11.线程池的种类
1.FixedThreadPool:可重用固定线程数的线程池，只有核心线程，没有非核心线程，核心线程不会被回收，有任务时，有空闲的核心线程就用核心线程执行，没有则加入队列排队
2.SingleThreadExecutor:单线程线程池，只有一个核心线程，没有非核心线程，当任务到达时，如果没有运行线程，则创建一个线程执行，如果正在运行则加入队列等待，可以保证所有任务在一个线程中按照顺序执行，和FixedThreadPool的区别只有数量
3.CachedThreadPool:按需创建的线程池，没有核心线程，非核心线程有Integer.MAX_VALUE个，每次提交
任务如果有空闲线程则由空闲线程执行，没有空闲线程则创建新的线程执行，适用于大量的需要立即处理的并且耗时较短的任务
4.ScheduledThreadPoolExecutor:继承自ThreadPoolExecutor,用于延时执行任务或定期执行任务，核心线程数固定，线程总数为Integer.MAX_VALUE
12.线程同步机制与原理，举例说明
为什么需要线程同步？当多个线程操作同一个变量的时候，存在这个变量何时对另一个线程可见的问题，也就是可见性。每一个线程都持有主存中变量的一个副本，当他更新这个变量时，首先更新的是自己线程中副本的变量值，然后会将这个值更新到主存中，但是是否立即更新以及更新到主存的时机是不确定的，这就导致当另一个线程操作这个变量的时候，他从主存中读取的这个变量还是旧的值，导致两个线程不同步的问题。线程同步就是为了保证多线程操作的可见性和原子性，比如我们用synchronized关键字包裹一端代码，我们希望这段代码执行完成后，对另一个线程立即可见，另一个线程再次操作的时候得到的是上一个线程更新之后的内容，还有就是保证这段代码的原子性，这段代码可能涉及到了好几部操作，我们希望这好几步的操作一次完成不会被中间打断，锁的同步机制就可以实现这一点。一般说的synchronized用来做多线程同步功能，其实synchronized只是提供多线程互斥，而对象的wait()和notify()方法才提供线程的同步功能。JVM通过Monitor对象实现线程同步，当多个线程同时请求synchronized方法或块时，monitor会设置几个虚拟逻辑数据结构来管理这些多线程。新请求的线程会首先被加入到线程排队队列中，线程阻塞，当某个拥有锁的线程unlock之后，则排队队列里的线程竞争上岗（synchronized是不公平竞争锁，下面还会讲到）。如果运行的线程调用对象的wait()后就释放锁并进入wait线程集合那边，当调用对象的notify()或notifyall()后，wait线程就到排队那边。这是大致的逻辑。
13.arrayList与linkedList的读写时间复杂度
（1）ArrayList：ArrayList是一个泛型类，底层采用数组结构保存对象。数组结构的优点是便于对集合进行快速的随机访问，即如果需要经常根据索引位置访问集合中的对象，使用由ArrayList类实现的List集合的效率较好。数组结构的缺点是向指定索引位置插入对象和删除指定索引位置对象的速度较慢，并且插入或删除对象的索引位置越小效率越低，原因是当向指定的索引位置插入对象时，会同时将指定索引位置及之后的所有对象相应的向后移动一位。
（2）LinkedList：LinkedList是一个泛型类，底层是一个双向链表，所以它在执行插入和删除操作时比ArrayList更加的高效，但也因为链表的数据结构，所以在随机访问方面要比ArrayList差。
ArrayList 是线性表（数组）
get() 直接读取第几个下标，复杂度 O(1)
add(E) 添加元素，直接在后面添加，复杂度O（1）
add(index, E) 添加元素，在第几个元素后面插入，后面的元素需要向后移动，复杂度O（n）
remove（）删除元素，后面的元素需要逐个移动，复杂度O（n）
LinkedList 是链表的操作
get() 获取第几个元素，依次遍历，复杂度O(n)
add(E) 添加到末尾，复杂度O(1)
add(index, E) 添加第几个元素后，需要先查找到第几个元素，直接指针指向操作，复杂度O(n)
remove（）删除元素，直接指针指向操作，复杂度O(1)
14.为什么HashMap线程不安全（hash碰撞与扩容导致）
HashMap的底层存储结构是一个Entry数组，每个Entry又是一个单链表，一旦发生Hash冲突的的时候，HashMap采用拉链法解决碰撞冲突，因为hashMap的put方法不是同步的，所以他的扩容方法也不是同步的，在扩容过程中，会新生成一个新的容量的数组，然后对原数组的所有键值对重新进行计算和写入新的数组，之后指向新生成的数组。当多个线程同时检测到hashmap需要扩容的时候就会同时调用resize操作，各自生成新的数组并rehash后赋给该map底层的数组table，结果最终只有最后一个线程生成的新数组被赋给table变量，其他线程的均会丢失。而且当某些线程已经完成赋值而其他线程刚开始的时候，就会用已经被赋值的table作为原始数组，这样也会有问题。扩容的时候 可能会引发链表形成环状结构
15.进程线程的区别
1.地址空间：同一进程的线程共享本进程的地址空间，而进程之间则是独立的地址空间。
2.资源拥有：同一进程内的线程共享本进程的资源如内存、I/O、cpu等，但是进程之间的资源是独立的。
3.一个进程崩溃后，在保护模式下不会对其他进程产生影响，但是一个线程崩溃整个进程都死掉。所以多进程要比多线程健壮。
4.进程切换时，消耗的资源大，效率不高。所以涉及到频繁的切换时，使用线程要好于进程。同样如果要求同时进行并且又要共享某些变量的并发操作，只能用线程不能用进程
5.执行过程：每个独立的进程程有一个程序运行的入口、顺序执行序列和程序入口。但是线程不能独立执行，必须依存在应用程序中，由应用程序提供多个线程执行控制。
6.线程是处理器调度的基本单位，但是进程不是。
7.两者均可并发执行。
16.Binder的内存拷贝过程
相比其他的IPC通信，比如消息机制、共享内存、管道、信号量等，Binder仅需一次内存拷贝，即可让目标进程读取到更新数据，同共享内存一样相当高效，其他的IPC通信机制大多需要2次内存拷贝。Binder内存拷贝的原理为：进程A为Binder客户端，在IPC调用前，需将其用户空间的数据拷贝到Binder驱动的内核空间，由于进程B在打开Binder设备(/dev/binder)时，已将Binder驱动的内核空间映射(mmap)到自己的进程空间，所以进程B可以直接看到Binder驱动内核空间的内容改动
17.传统IPC机制的通信原理（2次内存拷贝）
1.发送方进程通过系统调用（copy_from_user）将要发送的数据存拷贝到内核缓存区中。
2.接收方开辟一段内存空间，内核通过系统调用（copy_to_user）将内核缓存区中的数据拷贝到接收方的内存缓存区。
种传统IPC机制存在2个问题：
1.需要进行2次数据拷贝，第1次是从发送方用户空间拷贝到内核缓存区，第2次是从内核缓存区拷贝到接收方用户空间。
2.接收方进程不知道事先要分配多大的空间来接收数据，可能存在空间上的浪费。
18.Java内存模型（记住堆栈是内存分区，不是模型）
Java内存模型(即Java Memory Model，简称JMM)本身是一种抽象的概念，并不真实存在，它描述的是一组规则或规范，通过这组规范定义了程序中各个变量（包括实例字段，静态字段和构成数组对象的元素）的访问方式。由于JVM运行程序的实体是线程，而每个线程创建时JVM都会为其创建一个工作内存(有些地方称为栈空间)，用于存储线程私有的数据，而Java内存模型中规定所有变量都存储在主内存，主内存是共享内存区域，所有线程都可以访问，但线程对变量的操作(读取赋值等)必须在工作内存中进行，首先要将变量从主内存拷贝的自己的工作内存空间，然后对变量进行操作，操作完成后再将变量写回主内存，不能直接操作主内存中的变量，工作内存中存储着主内存中的变量副本拷贝，前面说过，工作内存是每个线程的私有数据区域，因此不同的线程间无法访问对方的工作内存，线程间的通信(传值)必须通过主内存来完成
19.类的加载过程
类加载过程主要包含加载、验证、准备、解析、初始化、使用、卸载七个方面，下面一一阐述。
1.加载：获取定义此类的二进制字节流，生成这个类的java.lang.Class对象
2.验证：保证Class文件的字节流包含的信息符合JVM规范，不会给JVM造成危害
3.准备：准备阶段为变量分配内存并设置类变量的初始化
4.解析：解析过程是将常量池内的符号引用替换成直接引用
5.初始化：不同于准备阶段，本次初始化，是根据程序员通过程序制定的计划去初始化类的变量和其他资源。这些资源有static{}块，构造函数，父类的初始化等
6.使用：使用过程就是根据程序定义的行为执行
7.卸载：卸载由GC完成。
20.什么情况下会触发类的初始化
1、遇到new，getstatic，putstatic，invokestatic这4条指令；
2、使用java.lang.reflect包的方法对类进行反射调用；
3、初始化一个类的时候，如果发现其父类没有进行过初始化，则先初始化其父类（注意！如果其父类是接口的话，则不要求初始化父类）；
4、当虚拟机启动时，用户需要指定一个要执行的主类（包含main方法的那个类），虚拟机会先初始化这个主类；
5、当使用jdk1.7的动态语言支持时，如果一个java.lang.invoke.MethodHandle实例最后的解析结果REF_getstatic，REF_putstatic,REF_invokeStatic的方法句柄，并且这个方法句柄所对应的类没有进行过初始化，则先触发其类初始化；
21.双亲委托模式
类加载器查找class所采用的是双亲委托模式，所谓双亲委托模式就是判断该类是否已经加载，如果没有则不是自身去查找而是委托给父加载器进行查找，这样依次进行递归，直到委托到最顶层的Bootstrap ClassLoader,如果Bootstrap ClassLoader找到了该Class,就会直接返回，如果没找到，则继续依次向下查找，如果还没找到则最后交给自身去查找
22.双亲委托模式的好处
1.避免重复加载，如果已经加载过一次Class，则不需要再次加载，而是直接读取已经加载的Class
2.更加安全，确保，java核心api中定义类型不会被随意替换，比如，采用双亲委托模式可以使得系统在Java虚拟机启动时旧加载了String类，也就无法用自定义的String类来替换系统的String类，这样便可以防止核心API库被随意篡改。
23.死锁的产生条件，如何避免死锁
死锁的四个必要条件
1.互斥条件：一个资源每次只能被一个进程使用
2.请求与保持条件：进程已经保持了至少一个资源，但又提出了新的资源请求，而该资源 已被其他进程占有，此时请求进程被阻塞，但对自己已获得的资源保持不放。
3.不可剥夺条件:进程所获得的资源在未使用完毕之前，不能被其他进程强行夺走，即只能 由获得该资源的进程自己来释放（只能是主动释放)。
4.循环等待条件: 若干进程间形成首尾相接循环等待资源的关系
这四个条件是死锁的必要条件，只要系统发生死锁，这些条件必然成立，而只要上述条件之一不满足，就不会发生死锁。
避免死锁的方法：系统对进程发出每一个系统能够满足的资源申请进行动态检查,并根据检查结果决定是否分配资源,如果分配后系统可能发生死锁,则不予分配,否则予以分配，这是一种保证系统不进入死锁状态的动态策略。
在资源的动态分配过程中，用某种方法去防止系统进入不安全状态，从而避免发生死锁。 一般来说互斥条件是无法破坏的，所以在预防死锁时主要从其他三个方面入手 ：
(1)破坏请求和保持条件：在系统中不允许进程在已获得某种资源的情况下，申请其他资源，即要想出一个办法，阻止进程在持有资源的同时申请其它资源。
方法一：在所有进程开始运行之前，必须一次性的申请其在整个运行过程中所需的全部资源，
方法二：要求每个进程提出新的资源申请前，释放它所占有的资源
(2)破坏不可抢占条件：允许对资源实行抢夺。
方式一：如果占有某些资源的一个进程进行进一步资源请求被拒绝，则该进程必须释放它最初占有的资源，如果有必要，可再次请求这些资源和另外的资源。
方式二：如果一个进程请求当前被另一个进程占有的资源，则操作系统可以抢占另一个进程，要求它释放资源，只有在任意两个进程的优先级都不相同的条件下，该方法才能预防死锁。
(3)破坏循环等待条件
对系统所有资源进行线性排序并赋予不同的序号，这样我们便可以规定进程在申请资源时必须按照序号递增的顺序进行资源的申请，当以后要申请时需检查要申请的资源的编号大于当前编号时，才能进行申请。
利用银行家算法避免死锁：
所谓银行家算法，是指在分配资源之前先看清楚，资源分配后是否会导致系统死锁。如果会死锁，则不分配，否则就分配。
按照银行家算法的思想，当进程请求资源时，系统将按如下原则分配系统资源：
24.App启动流程
App启动时，AMS会检查这个应用程序所需要的进程是否存在，不存在就会请求Zygote进程启动需要的应用程序进程，Zygote进程接收到AMS请求并通过fock自身创建应用程序进程，这样应用程序进程就会获取虚拟机的实例，还会创建Binder线程池（ProcessState.startThreadPool()）和消息循环(ActivityThread looper.loop),然后App进程，通过Binder IPC向sytem_server进程发起attachApplication请求；system_server进程在收到请求后，进行一系列准备工作后，再通过Binder IPC向App进程发送scheduleLaunchActivity请求；App进程的binder线程（ApplicationThread）在收到请求后，通过handler向主线程发送LAUNCH_ACTIVITY消息；主线程在收到Message后，通过反射机制创建目标Activity，并回调Activity.onCreate()等方法。到此，App便正式启动，开始进入Activity生命周期，执行完onCreate/onStart/onResume方法，UI渲染结束后便可以看到App的主界面。
25.Android单线程模型
Android单线程模型的核心原则就是：只能在UI线程(Main Thread)中对UI进行处理。当一个程序第一次启动时，Android会同时启动一个对应的 主线程（Main Thread），主线程主要负责处理与UI相关的事件，如：用户的按键事件，用户接触屏幕的事件以及屏幕绘图事 件，并把相关的事件分发到对应的组件进行处理。所以主线程通常又被叫做UI线 程。在开发Android应用时必须遵守单线程模型的原则： Android UI操作并不是线程安全的并且这些操作必须在UI线程中执行。
Android的单线程模型有两条原则：
1.不要阻塞UI线程。
2.不要在UI线程之外访问Android UI toolkit（主要是这两个包中的组件：android.widget and android.view
26.RecyclerView在很多方面能取代ListView，Google为什么没把ListView划上一条过时的横线？
ListView采用的是RecyclerBin的回收机制在一些轻量级的List显示时效率更高。
27.HashMap如何保证元素均匀分布
hash & (length-1)
通过Key值的hashCode值和hashMap长度-1做与运算
hashmap中的元素，默认情况下，数组大小为16，也就是2的4次方，如果要自定义HashMap初始化数组长度，也要设置为2的n次方大小，因为这样效率最高。因为当数组长度为2的n次幂的时候，不同的key算出的index相同的几率较小，那么数据在数组上分布就比较均匀，也就是说碰撞的几率小，相对的，查询的时候就不用遍历某个位置上的链表，这样查询效率也就较高了


2019Android高级面试题总结
1.说下你所知道的设计模式与使用场景
a.建造者模式：
将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。
使用场景比如最常见的AlertDialog,拿我们开发过程中举例，比如Camera开发过程中，可能需要设置一个初始化的相机配置，设置摄像头方向，闪光灯开闭，成像质量等等，这种场景下就可以使用建造者模式
装饰者模式：动态的给一个对象添加一些额外的职责，就增加功能来说，装饰模式比生成子类更为灵活。装饰者模式可以在不改变原有类结构的情况下曾强类的功能，比如Java中的BufferedInputStream 包装FileInputStream，举个开发中的例子，比如在我们现有网络框架上需要增加新的功能，那么再包装一层即可，装饰者模式解决了继承存在的一些问题，比如多层继承代码的臃肿，使代码逻辑更清晰
观察者模式：
代理模式：
门面模式：
单例模式：
生产者消费者模式：
2.java语言的特点与OOP思想
这个通过对比来描述，比如面向对象和面向过程的对比，针对这两种思想的对比，还可以举个开发中的例子，比如播放器的实现，面向过程的实现方式就是将播放视频的这个功能分解成多个过程，比如，加载视频地址，获取视频信息，初始化解码器，选择合适的解码器进行解码，读取解码后的帧进行视频格式转换和音频重采样，然后读取帧进行播放，这是一个完整的过程，这个过程中不涉及类的概念，而面向对象最大的特点就是类，封装继承和多态是核心，同样的以播放器为例，一面向对象的方式来实现，将会针对每一个功能封装出一个对象，吧如说Muxer，获取视频信息，Decoder,解码，格式转换器，视频播放器，音频播放器等，每一个功能对应一个对象，由这个对象来完成对应的功能，并且遵循单一职责原则，一个对象只做它相关的事情
3.说下java中的线程创建方式，线程池的工作原理。
java中有三种创建线程的方式，或者说四种
1.继承Thread类实现多线程
2.实现Runnable接口
3.实现Callable接口
4.通过线程池
线程池的工作原理：线程池可以减少创建和销毁线程的次数，从而减少系统资源的消耗，当一个任务提交到线程池时
a. 首先判断核心线程池中的线程是否已经满了，如果没满，则创建一个核心线程执行任务，否则进入下一步
b. 判断工作队列是否已满，没有满则加入工作队列，否则执行下一步
c. 判断线程数是否达到了最大值，如果不是，则创建非核心线程执行任务，否则执行饱和策略，默认抛出异常
4.说下handler原理
Handler，Message，looper和MessageQueue构成了安卓的消息机制，handler创建后可以通过sendMessage将消息加入消息队列，然后looper不断的将消息从MessageQueue中取出来，回调到Hander的handleMessage方法，从而实现线程的通信。
从两种情况来说，第一在UI线程创建Handler,此时我们不需要手动开启looper，因为在应用启动时，在ActivityThread的main方法中就创建了一个当前主线程的looper，并开启了消息队列，消息队列是一个无限循环，为什么无限循环不会ANR?因为可以说，应用的整个生命周期就是运行在这个消息循环中的，安卓是由事件驱动的，Looper.loop不断的接收处理事件，每一个点击触摸或者Activity每一个生命周期都是在Looper.loop的控制之下的，looper.loop一旦结束，应用程序的生命周期也就结束了。我们可以想想什么情况下会发生ANR，第一，事件没有得到处理，第二，事件正在处理，但是没有及时完成，而对事件进行处理的就是looper，所以只能说事件的处理如果阻塞会导致ANR，而不能说looper的无限循环会ANR
另一种情况就是在子线程创建Handler,此时由于这个线程中没有默认开启的消息队列，所以我们需要手动调用looper.prepare(),并通过looper.loop开启消息
主线程Looper从消息队列读取消息，当读完所有消息时，主线程阻塞。子线程往消息队列发送消息，并且往管道文件写数据，主线程即被唤醒，从管道文件读取数据，主线程被唤醒只是为了读取消息，当消息读取完毕，再次睡眠。因此loop的循环并不会对CPU性能有过多的消耗。
5.内存泄漏的场景和解决办法
1.非静态内部类的静态实例
非静态内部类会持有外部类的引用，如果非静态内部类的实例是静态的，就会长期的维持着外部类的引用，组织被系统回收，解决办法是使用静态内部类
2.多线程相关的匿名内部类和非静态内部类
匿名内部类同样会持有外部类的引用，如果在线程中执行耗时操作就有可能发生内存泄漏，导致外部类无法被回收，直到耗时任务结束，解决办法是在页面退出时结束线程中的任务
3.Handler内存泄漏
Handler导致的内存泄漏也可以被归纳为非静态内部类导致的，Handler内部message是被存储在MessageQueue中的，有些message不能马上被处理，存在的时间会很长，导致handler无法被回收，如果handler是非静态的，就会导致它的外部类无法被回收，解决办法是1.使用静态handler，外部类引用使用弱引用处理2.在退出页面时移除消息队列中的消息
4.Context导致内存泄漏
根据场景确定使用Activity的Context还是Application的Context,因为二者生命周期不同，对于不必须使用Activity的Context的场景（Dialog）,一律采用Application的Context,单例模式是最常见的发生此泄漏的场景，比如传入一个Activity的Context被静态类引用，导致无法回收
5.静态View导致泄漏
使用静态View可以避免每次启动Activity都去读取并渲染View，但是静态View会持有Activity的引用，导致无法回收，解决办法是在Activity销毁的时候将静态View设置为null（View一旦被加载到界面中将会持有一个Context对象的引用，在这个例子中，这个context对象是我们的Activity，声明一个静态变量引用这个View，也就引用了activity）
6.WebView导致的内存泄漏
WebView只要使用一次，内存就不会被释放，所以WebView都存在内存泄漏的问题，通常的解决办法是为WebView单开一个进程，使用AIDL进行通信，根据业务需求在合适的时机释放掉
7.资源对象未关闭导致
如Cursor，File等，内部往往都使用了缓冲，会造成内存泄漏，一定要确保关闭它并将引用置为null
8.集合中的对象未清理
集合用于保存对象，如果集合越来越大，不进行合理的清理，尤其是入股集合是静态的
9.Bitmap导致内存泄漏
bitmap是比较占内存的，所以一定要在不使用的时候及时进行清理，避免静态变量持有大的bitmap对象
10.监听器未关闭
很多需要register和unregister的系统服务要在合适的时候进行unregister,手动添加的listener也需要及时移除
6.如何避免OOM?
1.使用更加轻量的数据结构：如使用ArrayMap/SparseArray替代HashMap,HashMap更耗内存，因为它需要额外的实例对象来记录Mapping操作，SparseArray更加高效，因为它避免了Key Value的自动装箱，和装箱后的解箱操作
2.便面枚举的使用，可以用静态常量或者注解@IntDef替代
3.Bitmap优化:
a.尺寸压缩：通过InSampleSize设置合适的缩放
b.颜色质量：设置合适的format，ARGB_6666/RBG_545/ARGB_4444/ALPHA_6，存在很大差异
c.inBitmap:使用inBitmap属性可以告知Bitmap解码器去尝试使用已经存在的内存区域，新解码的Bitmap会尝试去使用之前那张Bitmap在Heap中所占据的pixel data内存区域，而不是去问内存重新申请一块区域来存放Bitmap。利用这种特性，即使是上千张的图片，也只会仅仅只需要占用屏幕所能够显示的图片数量的内存大小，但复用存在一些限制，具体体现在：在Android 4.4之前只能重用相同大小的Bitmap的内存，而Android 4.4及以后版本则只要后来的Bitmap比之前的小即可。使用inBitmap参数前，每创建一个Bitmap对象都会分配一块内存供其使用，而使用了inBitmap参数后，多个Bitmap可以复用一块内存，这样可以提高性能
4.StringBuilder替代String: 在有些时候，代码中会需要使用到大量的字符串拼接的操作，这种时候有必要考虑使用StringBuilder来替代频繁的“+”
5.避免在类似onDraw这样的方法中创建对象，因为它会迅速占用大量内存，引起频繁的GC甚至内存抖动
6.减少内存泄漏也是一种避免OOM的方法
7.说下Activity的启动模式，生命周期，两个Activity跳转的生命周期，如果一个Activity跳转另一个Activity再按下Home键在回到Activity的生命周期是什么样的
启动模式
Standard模式:Activity可以有多个实例，每次启动Activity，无论任务栈中是否已经有这个Activity的实例，系统都会创建一个新的Activity实例
SingleTop模式:当一个singleTop模式的Activity已经位于任务栈的栈顶，再去启动它时，不会再创建新的实例,如果不位于栈顶，就会创建新的实例
SingleTask模式:如果Activity已经位于栈顶，系统不会创建新的Activity实例，和singleTop模式一样。但Activity已经存在但不位于栈顶时，系统就会把该Activity移到栈顶，并把它上面的activity出栈
SingleInstance模式:singleInstance模式也是单例的，但和singleTask不同，singleTask只是任务栈内单例，系统里是可以有多个singleTask Activity实例的，而singleInstance Activity在整个系统里只有一个实例，启动一singleInstanceActivity时，系统会创建一个新的任务栈，并且这个任务栈只有他一个Activity
生命周期
onCreate onStart onResume onPause onStop onDestroy
两个Activity跳转的生命周期
1.启动A
onCreate - onStart - onResume
2.在A中启动B
ActivityA onPause
ActivityB onCreate
ActivityB onStart
ActivityB onResume
ActivityA onStop
3.从B中返回A（按物理硬件返回键）
ActivityB onPause
ActivityA onRestart
ActivityA onStart
ActivityA onResume
ActivityB onStop
ActivityB onDestroy
4.继续返回
ActivityA onPause
ActivityA onStop
ActivityA onDestroy
8.onRestart的调用场景
（1）按下home键之后，然后切换回来，会调用onRestart()。
（2）从本Activity跳转到另一个Activity之后，按back键返回原来Activity，会调用onRestart()；
（3）从本Activity切换到其他的应用，然后再从其他应用切换回来，会调用onRestart()；
说下Activity的横竖屏的切换的生命周期，用那个方法来保存数据，两者的区别。触发在什么时候在那个方法里可以获取数据等。
9.是否了SurfaceView，它是什么？他的继承方式是什么？他与View的区别(从源码角度，如加载，绘制等)。
SurfaceView中采用了双缓冲机制，保证了UI界面的流畅性，同时SurfaceView不在主线程中绘制，而是另开辟一个线程去绘制，所以它不妨碍UI线程；
SurfaceView继承于View，他和View主要有以下三点区别：
（1）View底层没有双缓冲机制，SurfaceView有；
（2）view主要适用于主动更新，而SurfaceView适用与被动的更新，如频繁的刷新
（3）view会在主线程中去更新UI，而SurfaceView则在子线程中刷新；
SurfaceView的内容不在应用窗口上，所以不能使用变换（平移、缩放、旋转等）。也难以放在ListView或者ScrollView中，不能使用UI控件的一些特性比如View.setAlpha()
View：显示视图，内置画布，提供图形绘制函数、触屏事件、按键事件函数等；必须在UI主线程内更新画面，速度较慢。
SurfaceView：基于view视图进行拓展的视图类，更适合2D游戏的开发；是view的子类，类似使用双缓机制，在新的线程中更新画面所以刷新界面速度比view快，Camera预览界面使用SurfaceView。
GLSurfaceView：基于SurfaceView视图再次进行拓展的视图类，专用于3D游戏开发的视图；是SurfaceView的子类，openGL专用。
10.如何实现进程保活
a: Service设置成START_STICKY kill 后会被重启(等待5秒左右)，重传Intent，保持与重启前一样
b: 通过 startForeground将进程设置为前台进程， 做前台服务，优先级和前台应用一个级别，除非在系统内存非常缺，否则此进程不会被 kill
c: 双进程Service： 让2个进程互相保护对方，其中一个Service被清理后，另外没被清理的进程可以立即重启进程
d: 用C编写守护进程(即子进程) : Android系统中当前进程(Process)fork出来的子进程，被系统认为是两个不同的进程。当父进程被杀死的时候，子进程仍然可以存活，并不受影响(Android5.0以上的版本不可行）联系厂商，加入白名单
e.锁屏状态下，开启一个一像素Activity
11.说下冷启动与热启动是什么，区别，如何优化，使用场景等。
app冷启动： 当应用启动时，后台没有该应用的进程，这时系统会重新创建一个新的进程分配给该应用， 这个启动方式就叫做冷启动（后台不存在该应用进程）。冷启动因为系统会重新创建一个新的进程分配给它，所以会先创建和初始化Application类，再创建和初始化MainActivity类（包括一系列的测量、布局、绘制），最后显示在界面上。
app热启动： 当应用已经被打开， 但是被按下返回键、Home键等按键时回到桌面或者是其他程序的时候，再重新打开该app时， 这个方式叫做热启动（后台已经存在该应用进程）。热启动因为会从已有的进程中来启动，所以热启动就不会走Application这步了，而是直接走MainActivity（包括一系列的测量、布局、绘制），所以热启动的过程只需要创建和初始化一个MainActivity就行了，而不必创建和初始化Application
冷启动的流程
当点击app的启动图标时，安卓系统会从Zygote进程中fork创建出一个新的进程分配给该应用，之后会依次创建和初始化Application类、创建MainActivity类、加载主题样式Theme中的windowBackground等属性设置给MainActivity以及配置Activity层级上的一些属性、再inflate布局、当onCreate/onStart/onResume方法都走完了后最后才进行contentView的measure/layout/draw显示在界面上
冷启动的生命周期简要流程：
Application构造方法 –> attachBaseContext()–>onCreate –>Activity构造方法 –> onCreate() –> 配置主体中的背景等操作 –>onStart() –> onResume() –> 测量、布局、绘制显示
冷启动的优化主要是视觉上的优化，解决白屏问题，提高用户体验，所以通过上面冷启动的过程。能做的优化如下：
1、减少onCreate()方法的工作量
2、不要让Application参与业务的操作
3、不要在Application进行耗时操作
4、不要以静态变量的方式在Application保存数据
5、减少布局的复杂度和层级
6、减少主线程耗时
12.为什么冷启动会有白屏黑屏问题？
原因在于加载主题样式Theme中的windowBackground等属性设置给MainActivity发生在inflate布局当onCreate/onStart/onResume方法之前，而windowBackground背景被设置成了白色或者黑色，所以我们进入app的第一个界面的时候会造成先白屏或黑屏一下再进入界面。解决思路如下
1.给他设置windowBackground背景跟启动页的背景相同，如果你的启动页是张图片那么可以直接给windowBackground这个属性设置该图片那么就不会有一闪的效果了
<style name=``"Splash_Theme"` `parent=``"@android:style/Theme.NoTitleBar"``>`
    <item name=``"android:windowBackground"``>@drawable/splash_bg</item>`
    <item name=``"android:windowNoTitle"``>``true``</item>`</style>`
2.采用世面的处理方法，设置背景是透明的，给人一种延迟启动的感觉。,将背景颜色设置为透明色,这样当用户点击桌面APP图片的时候，并不会"立即"进入APP，而且在桌面上停留一会，其实这时候APP已经是启动的了，只是我们心机的把Theme里的windowBackground的颜色设置成透明的，强行把锅甩给了手机应用厂商（手机反应太慢了啦）
<style name=``"Splash_Theme"` `parent=``"@android:style/Theme.NoTitleBar"``>`
    <item name=``"android:windowIsTranslucent"``>``true``</item>`
    <item name=``"android:windowNoTitle"``>``true``</item>`</style>`
3.以上两种方法是在视觉上显得更快，但其实只是一种表象，让应用启动的更快，有一种思路，将Application中的不必要的初始化动作实现懒加载，比如，在SpashActivity显示后再发送消息到Application，去初始化，这样可以将初始化的动作放在后边，缩短应用启动到用户看到界面的时间
13.Android中的线程有那些,原理与各自特点
AsyncTask,HandlerThread,IntentService
AsyncTask原理：内部是Handler和两个线程池实现的，Handler用于将线程切换到主线程，两个线程池一个用于任务的排队，一个用于执行任务，当AsyncTask执行execute方法时会封装出一个FutureTask对象，将这个对象加入队列中，如果此时没有正在执行的任务，就执行它，执行完成之后继续执行队列中下一个任务，执行完成通过Handler将事件发送到主线程。AsyncTask必须在主线程初始化，因为内部的Handler是一个静态对象，在AsyncTask类加载的时候他就已经被初始化了。在Android3.0开始，execute方法串行执行任务的，一个一个来，3.0之前是并行执行的。如果要在3.0上执行并行任务，可以调用executeOnExecutor方法
HandlerThread原理：继承自Thread，start开启线程后，会在其run方法中会通过Looper创建消息队列并开启消息循环，这个消息队列运行在子线程中，所以可以将HandlerThread中的Looper实例传递给一个Handler，从而保证这个Handler的handleMessage方法运行在子线程中，Android中使用HandlerThread的一个场景就是IntentService
IntentService原理：继承自Service，它的内部封装了HandlerThread和Handler，可以执行耗时任务，同时因为它是一个服务，优先级比普通线程高很多，所以更适合执行一些高优先级的后台任务，HandlerThread底层通过Looper消息队列实现的，所以它是顺序的执行每一个任务。可以通过Intent的方式开启IntentService，IntentService通过handler将每一个intent加入HandlerThread子线程中的消息队列，通过looper按顺序一个个的取出并执行，执行完成后自动结束自己，不需要开发者手动关闭
14.ANR的原因
1.耗时的网络访问
2.大量的数据读写
3.数据库操作
4.硬件操作（比如camera)
5.调用thread的join()方法、sleep()方法、wait()方法或者等待线程锁的时候
6.service binder的数量达到上限
7.system server中发生WatchDog ANR
8.service忙导致超时无响应
9.其他线程持有锁，导致主线程等待超时
10.其它线程终止或崩溃导致主线程一直等待
15.三级缓存原理
当Android端需要获得数据时比如获取网络中的图片，首先从内存中查找（按键查找），内存中没有的再从磁盘文件或sqlite中去查找，若磁盘中也没有才通过网络获取
16.LruCache底层实现原理：
LruCache中Lru算法的实现就是通过LinkedHashMap来实现的。LinkedHashMap继承于HashMap，它使用了一个双向链表来存储Map中的Entry顺序关系，
对于get、put、remove等操作，LinkedHashMap除了要做HashMap做的事情，还做些调整Entry顺序链表的工作。
LruCache中将LinkedHashMap的顺序设置为LRU顺序来实现LRU缓存，每次调用get(也就是从内存缓存中取图片)，则将该对象移到链表的尾端。
调用put插入新的对象也是存储在链表尾端，这样当内存缓存达到设定的最大值时，将链表头部的对象（近期最少用到的）移除。
17.说下你对Collection这个类的理解。
Collection是集合框架的顶层接口，是存储对象的容器,Colloction定义了接口的公用方法如add remove clear等等，它的子接口有两个，List和Set,List的特点有元素有序，元素可以重复，元素都有索引（角标），典型的有
Vector:内部是数组数据结构，是同步的（线程安全的）。增删查询都很慢。
ArrayList:内部是数组数据结构，是不同步的（线程不安全的）。替代了Vector。查询速度快，增删比较慢。
LinkedList:内部是链表数据结构，是不同步的（线程不安全的）。增删元素速度快。
而Set的是特点元素无序，元素不可以重复
HashSet：内部数据结构是哈希表，是不同步的。
Set集合中元素都必须是唯一的，HashSet作为其子类也需保证元素的唯一性。
判断元素唯一性的方式：
通过存储对象（元素）的hashCode和equals方法来完成对象唯一性的。
如果对象的hashCode值不同，那么不用调用equals方法就会将对象直接存储到集合中；
如果对象的hashCode值相同，那么需调用equals方法判断返回值是否为true，
若为false, 则视为不同元素，就会直接存储；
若为true， 则视为相同元素，不会存储。
如果要使用HashSet集合存储元素，该元素的类必须覆盖hashCode方法和equals方法。一般情况下，如果定义的类会产生很多对象，通常都需要覆盖equals，hashCode方法。建立对象判断是否相同的依据。
TreeSet：保证元素唯一性的同时可以对内部元素进行排序，是不同步的。
判断元素唯一性的方式：
根据比较方法的返回结果是否为0，如果为0视为相同元素，不存；如果非0视为不同元素，则存。
TreeSet对元素的排序有两种方式：
方式一：使元素（对象）对应的类实现Comparable接口，覆盖compareTo方法。这样元素自身具有比较功能。
方式二：使TreeSet集合自身具有比较功能，定义一个比较器Comparator，将该类对象作为参数传递给TreeSet集合的构造函数
18.JVM老年代和新生代的比例
Java 中的堆是 JVM 所管理的最大的一块内存空间，主要用于存放各种类的实例对象。
在 Java 中，堆被划分成两个不同的区域：新生代 ( Young )、老年代 ( Old )。新生代 ( Young ) 又被划分为三个区域：Eden、From Survivor、To Survivor。
这样划分的目的是为了使 JVM 能够更好的管理堆内存中的对象，包括内存的分配以及回收。
堆大小 = 新生代 + 老年代。其中，堆的大小可以通过参数 –Xms、-Xmx 来指定。
（本人使用的是 JDK1.6，以下涉及的 JVM 默认值均以该版本为准。）
默认的，新生代 ( Young ) 与老年代 ( Old ) 的比例的值为 1:2 ( 该值可以通过参数 –XX:NewRatio 来指定 )，即：新生代 ( Young ) = 1/3 的堆空间大小。老年代 ( Old ) = 2/3 的堆空间大小。其中，新生代 ( Young ) 被细分为 Eden 和 两个 Survivor 区域，这两个 Survivor 区域分别被命名为 from 和 to，以示区分。
默认的，Edem : from : to = 8 : 1 : 1 ( 可以通过参数 –XX:SurvivorRatio 来设定 )，即： Eden = 8/10 的新生代空间大小，from = to = 1/10 的新生代空间大小。
JVM 每次只会使用 Eden 和其中的一块 Survivor 区域来为对象服务，所以无论什么时候，总是有一块 Survivor 区域是空闲着的。
因此，新生代实际可用的内存空间为 9/10 ( 即90% )的新生代空间
19.jvm，jre以及jdk三者之间的关系？JDK（Java Development Kit）是针对Java开发员的产品，是整个Java的核心，包括了Java运行环境JRE、Java工具和Java基础类库。
Java Runtime Environment（JRE）是运行JAVA程序所必须的环境的集合，包含JVM标准实现及Java核心类库。
JVM是Java Virtual Machine（Java虚拟机）的缩写，是整个java实现跨平台的最核心的部分，能够运行以Java语言写作的软件程序。
20.谈谈你对 JNIEnv 和 JavaVM 理解？
1.JavaVm
JavaVM 是虚拟机在 JNI 层的代表，一个进程只有一个 JavaVM，所有的线程共用一个 JavaVM。
2.JNIEnv
JNIEnv 表示 Java 调用 native 语言的环境，是一个封装了几乎全部 JNI 方法的指针。
JNIEnv 只在创建它的线程生效，不能跨线程传递，不同线程的 JNIEnv 彼此独立。
native 环境中创建的线程，如果需要访问 JNI，必须要调用 AttachCurrentThread 关联，并使用 DetachCurrentThread 解除链接。
21.Serializable与Parcable的区别？
1.Serializable （java 自带）
方法：对象继承 Serializable类即可实现序列化，就是这么简单，也是它最吸引我们的地方
2.Parcelable（Android专用）：Parcelable方式的实现原理是将一个完整的对象进行分解，用起来比较麻烦
1）在使用内存的时候，Parcelable比Serializable性能高，所以推荐使用Parcelable。
2）Serializable在序列化的时候会产生大量的临时变量，从而引起频繁的GC。
3）Parcelable不能使用在要将数据存储在磁盘上的情况，因为Parcelable不能很好的保证数据的持续性,在外界有变化的情况下。尽管Serializable效率低点，但此时还是建议使用Serializable 。
4）android上应该尽量采用Parcelable，效率至上，效率远高于Serializable


混合开发面试题
大厂除了技术深度之外，还要求你具备一些广度的知识，比如你要会前端知识，会混合开发，至少会一种脚本语言，C c++更不用说了，也是必会的。

1.Hybrid做过吗？
2.Hybrid通信原理是什么，有做研究吗？
3.react native有多少了解？讲一下原理。
4.weex了解吗？如何自己实现类似技术？
5.flutter了解吗？内部是如何实现跨平台的？
6.Dart语言有研究贵吗？
7.快应用了解吗？跟其她方式相比有什么优缺点？
8.说说你用过的混合开发技术有哪些？各有什么优缺点？
9.Python会吗？
10.会不会PHP？
11.Gradle了解多少？groovy语法会吗？


何谓悲观锁与乐观锁
乐观锁对应于生活中乐观的人总是想着事情往好的方向发展，悲观锁对应于生
活中悲观的人总是想着事情往坏的方向发展。这两种人各有优缺点，不能不以
场景而定说一种人好于另外一种人。
悲观锁
总是假设最坏的情况，每次去拿数据的时候都认为别人会修改，所以每次在拿 数据的时候都会上锁，这样别人想拿这个数据就会阻塞直到它拿到锁(共享资 源每次只给一个线程使用，其它线程阻塞，用完后再把资源转让给其它线 程)。传统的关系型数据库里边就用到了很多这种锁机制，比如行锁，表锁
等，读锁，写锁等，都是在做操作之前先上锁。Java 中 synchronized 和 ReentrantLock 等独占锁就是悲观锁思想的实现。
乐观锁
总是假设最好的情况，每次去拿数据的时候都认为别人不会修改，所以不会上
锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以
使用版本号机制和 CAS 算法实现。乐观锁适用于多读的应用类型，这样可以提 高吞吐量，像数据库提供的类似于 write_condition 机制，其实都是提供的乐 观锁。在 Java 中 java.util.concurrent.atomic 包下面的原子变量类就是使用了 乐观锁的一种实现方式 CAS 实现的。
两种锁的使用场景
从上面对两种锁的介绍，我们知道两种锁各有优缺点，不可认为一种好于另一 种，像乐观锁适用于写比较少的情况下(多读场景)，即冲突真的很少发生的 时候，这样可以省去了锁的开销，加大了系统的整个吞吐量。但如果是多写的
情况，一般会经常产生冲突，这就会导致上层应用会不断的进行 retry，这样反 倒是降低了性能，所以一般多写的场景下用悲观锁就比较合适。
乐观锁常见的两种实现方式
乐观锁一般会使用版本号机制或 CAS 算法实现。

1. 版本号机制
一般是在数据表中加上一个数据版本号 version 字段，表示数据被修改的次 数，当数据被修改时，version 值会加一。当线程 A 要更新数据值时，在读取数 据的同时也会读取 version 值，在提交更新时，若刚才读取到的 version 值为当 前数据库中的 version 值相等时才更新，否则重试更新操作，直到更新成功。
举一个简单的例子: 假设数据库中帐户信息表中有一个 version 字段，当前值 为 1 ;而当前帐户余额字段( balance )为 $100 。
1. 操作员 A 此时将其读出( version=1 )，并从其帐户余额中扣除 $50 ( $100-$50 )。
2. 在操作员 A 操作的过程中，操作员 B 也读入此用户信息( version=1 )，并从其帐户余额中扣除 $20 ( $100-$20 )。
3. 操作员 A 完成了修改工作，将数据版本号加一( version=2 )，连同
帐户扣除后余额( balance=$50 )，提交至数据库更新，此时由于提 交数据版本大于数据库记录当前版本，数据被更新，数据库记录 version 更新为 2 。
4. 操作员 B 完成了操作，也将版本号加一( version=2 )试图向数据库 提交数据( balance=$80 )，但此时比对数据库记录版本时发现，操 作员 B 提交的数据版本号为 2 ，数据库记录当前版本也为 2 ，不满 足 “ 提交版本必须大于记录当前版本才能执行更新 “ 的乐观锁策略， 因此，操作员 B 的提交被驳回。

这样，就避免了操作员 B 用基于 version=1 的旧数据修改的结果覆盖操作员 A 的操作结果的可能。
2. CAS 算法
即 compare and swap(比较与交换)，是一种有名的无锁算法。无锁编程， 即不使用锁的情况下实现多线程之间的变量同步，也就是在没有线程被阻塞的 情况下实现变量的同步，所以也叫非阻塞同步(Non-blocking
Synchronization)。CAS 算法涉及到三个操作数
 需要读写的内存值 V
 进行比较的值 A
 拟写入的新值 B
当且仅当 V 的值等于 A 时，CAS 通过原子方式用新值 B 来更新 V 的值，否则 不会执行任何操作(比较和替换是一个原子操作)。一般情况下是一个自旋操
作，即不断的重试。
关于自旋锁，大家可以看一下这篇文章，非常不错:《 面试必备之深入理解自
旋锁》
乐观锁的缺点
ABA 问题是乐观锁一个常见的问题 1 ABA 问题
如果一个变量 V 初次读取的时候是 A 值，并且在准备赋值的时候检查到它仍然 是 A 值，那我们就能说明它的值没有被其他线程修改过了吗?很明显是不能

的，因为在这段时间它的值可能被改为其他值，然后又改回 A，那 CAS 操作就 会误认为它从来没有被修改过。这个问题被称为 CAS 操作的 "ABA"问题。
JDK 1.5 以后的 AtomicStampedReference 类就提供了此种能力，其中的 compareAndSet 方法就是首先检查当前引用是否等于预期引用，并且当前标志是
否等于预期标志，如果全部相等，则以原子方式将该引用和该标志的值设置为
给定的更新值。
2 循环时间长开销大
自旋 CAS(也就是不成功就一直循环执行直到成功)如果长时间不成功，会给 CPU 带来非常大的执行开销。 如果 JVM 能支持处理器提供的 pause 指令那么 效率会有一定的提升，pause 指令有两个作用，第一它可以延迟流水线执行指
令(de-pipeline),使 CPU 不会消耗过多的执行资源，延迟的时间取决于具体 实现的版本，在一些处理器上延迟时间是零。第二它可以避免在退出循环的时 候因内存顺序冲突(memory order violation)而引起 CPU 流水线被清空
(CPU pipeline flush)，从而提高 CPU 的执行效率。 3 只能保证一个共享变量的原子操作
CAS 只对单个共享变量有效，当操作涉及跨多个共享变量时 CAS 无效。但是
从 JDK 1.5 开始，提供了 AtomicReference 类来保证引用对象之间的原子性，你
可以把多个变量放在一个对象里来进行 CAS 操作.所以我们可以使用锁或者利 用 AtomicReference 类把多个共享变量合并成一个共享变量来操作。
CAS 与 synchronized 的使用情景

简单的来说 CAS 适用于写比较少的情况下(多读场景，冲突一般较少)， synchronized 适用于写比较多的情况下(多写场景，冲突一般较多)
1. 对于资源竞争较少(线程冲突较轻)的情况，使用synchronized同步锁 进行线程阻塞和唤醒切换以及用户态内核态间的切换操作额外浪费消耗 cpu 资源;而 CAS 基于硬件实现，不需要进入内核，不需要切换线程， 操作自旋几率较少，因此可以获得更高的性能。
2. 对于资源竞争严重(线程冲突严重)的情况，CAS自旋的概率会比较 大，从而浪费更多的 CPU 资源，效率低于 synchronized。
补充: Java 并发编程这个领域中 synchronized 关键字一直都是元老级的角 色，很久之前很多人都会称它为 “重量级锁” 。但是，在 JavaSE 1.6 之后进行了 主要包括为了减少获得锁和释放锁带来的性能消耗而引入的 偏向锁 和 轻量级 锁 以及其它各种优化之后变得在某些情况下并不是那么重了。synchronized 的
底层实现主要依靠 Lock-Free 的队列，基本思路是 自旋后阻塞，竞争切换后继 续竞争锁，稍微牺牲了公平性，但获得了高吞吐量。在线程冲突较少的情况 下，可以获得和 CAS 类似的性能;而线程冲突严重的情况下，性能远高于
CAS。


Android源码相关面试专题
1、Android属性动画实现原理
工作原理：在一定时间间隔内，通过不断对值进行改变，并不断将该值赋给对象的属性，从而实现该对象在该属性上的动画效果。



1）ValueAnimator：通过不断控制值的变化（初始值->结束值），将值手动赋值给对象的属性，再不断调用View的invalidate()方法，去不断onDraw重绘view，达到动画的效果。

主要的三种方法：

a) ValueAnimator.ofInt(int values)：估值器是整型估值器IntEaluator

b) ValueAnimator.ofFloat(float values):估值器是浮点型估值器FloatEaluator

c) ValueAnimator.ofObject(ObjectEvaluator, start, end):将初始值以对象的形式过渡到结束值，通过操作对象实现动画效果，需要实现Interpolator接口，自定义估值器  

估值器TypeEvalutor，设置动画如何从初始值过渡到结束值的逻辑。插值器(Interpolator）决定值的变化模式（匀速、加速等）;估值器（TypeEvalutor）决定值的具体变化数值。

// 自定义估值器，需要实现TypeEvaluator接口
public class ObjectEvaluator implements TypeEvaluator{  
 
// 复写evaluate（），在evaluate（）里写入对象动画过渡的逻辑
    @Override  
    public Object evaluate(float fraction, Object startValue, Object endValue) {  
        // 参数说明
        // fraction：表示动画完成度（根据它来计算当前动画的值）
        // startValue、endValue：动画的初始值和结束值
 
        ... // 写入对象动画过渡的逻辑
        
        return value;  
        // 返回对象动画过渡的逻辑计算后的值
    } 
2) ObjectAnimator:直接对对象的属性值进行改变操作，从而实现动画效果

ObjectAnimator继承自ValueAnimator类，底层的动画实现机制还是基本值的改变。它是不断控制值的变化，再不断自动赋给对象的属性，从而实现动画效果。这里的自动赋值，是通过调用对象属性的set/get方法进行自动赋值，属性动画初始值如果有就直接取，没有则调用属性的get()方法获取，当值更新变化时，通过属性的set()方法进行赋值。每次赋值都是调用view的postInvalidate()/invalidate()方法不断刷新视图（实际调用了onDraw()方法进行了重绘视图）。

//Object 需要操作的对象； propertyName 需要操作的对象的属性； values动画初始值&结束值，
//如果是两个值，则从a->b值过渡，如果是三值，则从a->b->c
ObjectAnimator animator = ObjectAnimator.ofFloat(Object object, String propertyName, float ...values);
如果采用ObjectAnimator类实现动画，操作的对象的属性必须有get()和set()方法。

其他用法：

1）AnimatorSet组合动画 

AnimatorSet.play(Animator anim)   ：播放当前动画
AnimatorSet.after(long delay)   ：将现有动画延迟x毫秒后执行
AnimatorSet.with(Animator anim)   ：将现有动画和传入的动画同时执行
AnimatorSet.after(Animator anim)   ：将现有动画插入到传入的动画之后执行
AnimatorSet.before(Animator anim) ：  将现有动画插入到传入的动画之前执行
2) ViewPropertyAnimator直接对属性操作，View.animate()返回的是一个ViewPropertyAnimator对象，之后的调用方法都是基于该对象的操作，调用每个方法返回值都是它自身的实例

View.animate().alpha(0f).x(500).y(500).setDuration(500).setInterpolator()

3)设置动画监听

Animation.addListener(new AnimatorListener() {
          @Override
          public void onAnimationStart(Animation animation) {
              //动画开始时执行
          }
      
           @Override
          public void onAnimationRepeat(Animation animation) {
              //动画重复时执行
          }
 
         @Override
          public void onAnimationCancel()(Animation animation) {
              //动画取消时执行
          }
    
          @Override
          public void onAnimationEnd(Animation animation) {
              //动画结束时执行
          }
      });
 

2、补间动画实现原理
主要有四种AlpahAnimation\ ScaleAnimation\ RotateAnimation\ TranslateAnimation四种，对透明度、缩放、旋转、位移四种动画。在调用View.startAnimation时，先调用View.setAnimation(Animation)方法给自己设置一个Animation对象，再调用invalidate来重绘自己。在View.draw(Canvas, ViewGroup, long)方法中进行了getAnimation(), 并调用了drawAnimation(ViewGroup, long, Animation, boolean)方法，此方法调用Animation.getTranformation()方法，再调用applyTranformation()方法，该方法中主要是对Transformation.getMatrix().setTranslate/setRotate/setAlpha/setScale来设置相应的值，这个方法系统会以60FPS的频率进行调用。具体是在调Animation.start()方法中会调用animationHandler.start()方法，从而调用了scheduleAnimation()方法，这里会调用mChoreographer.postCallback(Choregrapher.CALLBACK_ANIMATION, this, null)放入事件队列中，等待doFrame()来消耗事件。

当一个 ChildView 要重画时，它会调用其成员函数 invalidate() 函数将通知其 ParentView 这个 ChildView 要重画，这个过程一直向上遍历到 ViewRoot，当 ViewRoot 收到这个通知后就会调用ViewRoot 中的 draw 函数从而完成绘制。View::onDraw() 有一个画布参数 Canvas, 画布顾名思义就是画东西的地方，Android 会为每一个 View 设置好画布，View 就可以调用 Canvas 的方法，比如：drawText, drawBitmap, drawPath 等等去画内容。每一个 ChildView 的画布是由其 ParentView 设置的，ParentView 根据 ChildView 在其内部的布局来调整 Canvas，其中画布的属性之一就是定义和 ChildView 相关的坐标系，默认是横轴为 X 轴，从左至右，值逐渐增大，竖轴为 Y 轴，从上至下，值逐渐增大。



Android 补间动画就是通过 ParentView 来不断调整 ChildView 的画布坐标系来实现的，在ParentView的dispatchDraw方法会被调用。

dispatchDraw() 
{ 
.... 
Animation a = ChildView.getAnimation() 
Transformation tm = a.getTransformation(); 
Use tm to set ChildView's Canvas; 
Invalidate(); 
.... 
}
这里有两个类：Animation 和 Transformation，这两个类是实现动画的主要的类，Animation 中主要定义了动画的一些属性比如开始时间、持续时间、是否重复播放等，这个类主要有两个重要的函数：getTransformation 和 applyTransformation，在 getTransformation 中 Animation 会根据动画的属性来产生一系列的差值点，然后将这些差值点传给 applyTransformation，这个函数将根据这些点来生成不同的 Transformation，Transformation 中包含一个矩阵和 alpha 值，矩阵是用来做平移、旋转和缩放动画的，而 alpha 值是用来做 alpha 动画的（简单理解的话，alpha 动画相当于不断变换透明度或颜色来实现动画），调用 dispatchDraw 时会调用 getTransformation 来得到当前的 Transformation。某一个 View 的动画的绘制并不是由他自己完成的而是由它的父 view 完成。

1）补间动画TranslateAnimation,View位置移动了，可是点击区域还在原来的位置，为什么？

View在做动画是，根据动画时间的插值，计算出一个Matrix，不停的invalidate，在onDraw中的Canvas上使用这个计算出来的Matrix去draw view的内容。某个view的动画绘制并不是由它自己完成，而是由它的父view完成，使它的父view画布进行了移动，而点击时还是点击原来的画布。使得它看起来变化了。


3、Android各个版本API的区别

主要记住一些大版本变化:

android3.0 代号Honeycomb, 引入Fragments, ActionBar,属性动画，硬件加速

android4.0 代号Ice Cream，API14：截图功能，人脸识别，虚拟按键，3D优化驱动

android5.0 代号Lollipop API21：调整桌面图标及部件透明度等

android6.0 代号M Marshmallow API23，软件权限管理，安卓支付，指纹支持，App关联，

android7.0 代号N Preview API24，多窗口支持(不影响Activity生命周期)，增加了JIT编译器，引入了新的应用签名方案APK Signature Scheme v2（缩短应用安装时间和更多未授权APK文件更改保护）,严格了权限访问

android8.0 代号O  API26，取消静态广播注册，限制后台进程调用手机资源，桌面图标自适应

android9.0 代号P API27，加强电池管理，系统界面添加了Home虚拟键，提供人工智能API，支持免打扰模式

4、Requestlayout，onlayout，onDraw，DrawChild区别与联系
requestLayout()方法 ：会导致调用measure()过程 和 layout()过程 。 说明：只是对View树重新布局layout过程包括measure()和layout()过程，如果view的l,t,r,b没有必变，那就不会触发onDraw；但是如果这次刷新是在动画里，mDirty非空，就会导致onDraw。

onLayout()方法(如果该View是ViewGroup对象，需要实现该方法，对每个子视图进行布局)

onDraw()方法绘制视图本身 (每个View都需要重载该方法，ViewGroup不需要实现该方法)

drawChild()去重新回调每个子视图的draw()方法

5、invalidate和postInvalidate的区别及使用
View.invalidate(): 层层上传到父级，直到传递到ViewRootImpl后触发了scheduleTraversals()，然后整个View树开始重新按照View绘制流程进行重绘任务。

invalidate:在ui线程刷新view
postInvalidate：在工作线程刷新view（底层还是handler）其实它的原理就是invalidate+handler

View.postInvalidate最终会调用ViewRootImpl.dispatchInvalidateDelayed()方法

public void dispatchInvalidateDelayed(View view, long delayMilliseconds) {
        Message msg = mHandler.obtainMessage(MSG_INVALIDATE, view);
        mHandler.sendMessageDelayed(msg, delayMilliseconds);
    }
这里的mHandler是ViewRootHandler实例，在该Handler的handleMessage方法中调用了view.invalidate()方法。

case MSG_INVALIDATE:
                    ((View) msg.obj).invalidate();
                    break;
6、Activity-Window-View三者的差别
Activity：是安卓四大组件之一，负责界面展示、用户交互与业务逻辑处理；
Window：就是负责界面展示以及交互的职能部门，就相当于Activity的下属，Activity的生命周期方法负责业务的处理；
View：就是放在Window容器的元素，Window是View的载体，View是Window的具体展示。
三者的关系： Activity通过Window来实现视图元素的展示，window可以理解为一个容器，盛放着一个个的view，用来执行具体的展示工作。
 



7、谈谈对Volley的理解
8、如何优化自定义View
1）在要在onDraw或是onLayout()中去创建对象，因为onDraw()方法可能会被频繁调用，可以在view的构造函数中进行创建对象；

2）降低view的刷新频率，尽可能减少不必要的调用invalidate()方法。或是调用带四种参数不同类型的invalidate()，而不是调用无参的方法。无参变量需要刷新整个view，而带参数的方法只需刷新指定部分的view。在onDraw()方法中减少冗余代码。

3）使用硬件加速，GPU硬件加速可以带来性能增加。

4）状态保存与恢复，如果因内存不足，Activity置于后台被杀重启时，View应尽可能保存自己属性，可以重写onSaveInstanceState和onRestoreInstanceState方法，状态保存。

9、低版本SDK如何实现高版本api？
使用@TargetApi注解·

当代码中有比AndroidManifest中设置的android:minSdkVersion版本更高的方法，此时编译器会提示警告，解决方法是在方法上加上@SuppressLint("NewApi"）或者@TargetApi()。但它们仅是屏蔽了android lint错误，在方法中还要判断版本做不同的操作。

@SuppressLint("NewApi"）屏蔽一切新api中才能使用的方法报的android lint错误

@TargetApi() 只屏蔽某一新api中才能使用的方法报的android lint错误，如@TargetApi(11)如果在方法中用了只有API14才开始有的方法，还是会报错。

10、描述一次网络请求的流程
1)域名解析

浏览器会先搜索自身DNS缓存且对应的IP地址没有过期；若未找到则搜索操作系统自身的DNS缓存；若还未找到则读本地的hotsts文件；还未找到会在TCP/IP设置的本地DNS服务器上找，如果要查询的域名在本地配置的区域资源中，则完成解析；否则根据本地DNS服务器会请求根DNS服务器；根DNS服务器是13台根DNS，会一级一级往下找。

2）TCP三次握手

客户端先发送SYN=1，ACK=0，序列号seq=x报文；（SYN在连接建立时用来同步序号，SYN=1，ACK=0代表这是一个连接请求报文，对方若同意建立连接，则应在响应报文中使SYN=1，ACK=1）

服务器返回SYN=1，ACK=1，seq=y, ack=x+1；

客户端再一次确认，但不用SYN了，回复服务端, ACK=1, seq=x+1, ack=y+1

3）建立TCP连接后发起HTTP请求

客户端按照指定的格式开始向服务端发送HTTP请求，HTTP请求格式由四部分组成，分别是请求行、请求头、空行、消息体，服务端接收到请求后，解析HTTP请求，处理完成逻辑，最后返回一个具有标准格式的HTTP响应给客户端。

4）服务器响应HTTP请求

服务器接收处理完请求后返回一个HTTP响应消息给客户端，HTTP响应信息格式包括：状态行、响应头、空行、消息体

5）浏览器解析HTML代码，请求HTML代码中的资源

浏览器拿到html文件后，就开始解析其中的html代码，遇到js/css/image等静态资源时，向服务器发起一个http请求，如果返回304状态码，浏览器会直接读取本地的缓存文件。否则开启线程向服务器请求下载。

6）浏览器对页面进行渲染并呈现给用户

7）TCP的四次挥手

当客户端没有东西要发送时就要释放连接（提出中断连接可以是Client也可以是Server），客户端会发送一个FIN=1的没有数据的报文，进入FIN_WAIT状态，服务端收到后会给客户端一个确认，此时客户端不能发送数据，但可接收信息。

11、HttpUrlConnection 和 okhttp关系
两者都可以用来实现网络请求，android4.4之后的HttpUrlConnection的实现是基于okhttp

-Bitmap对象的理解
-looper架构
-ActivityThread，AMS，WMS的工作原理
-自定义View如何考虑机型适配
在onMeasure()的getDefaultSize()的默认实现中，当view的测量模式是AT_MOST或EXACTLY时，View的大小都会被设置成子View MeasureSpec的specSize.子view的MeasureSpec值是根据子View的布局参数和父容器的MeasureSpec值计算得来。当子view的布局参数是wrap_content时，对应的测量模式是AT_MOST，大小是parentSize,

-自定义View的事件
-AstncTask+HttpClient 与 AsyncHttpClient有什么区别？
-LaunchMode应用场景
-AsyncTask 如何使用?
-SpareArray原理
-请介绍下ContentProvider 是如何实现数据共享的？
-AndroidService与Activity之间通信的几种方式
-IntentService原理及作用是什么？
原理：IntentService是继承Service的一个抽象类，它在onCreate()方法中创建了一个HandlerThread,并启动该线程。HandlerThread是带有自己消息队列和Looper的线程，根据HandlerThread的looper创建一个Handler，这样IntentService的ServiceHandler的handleMessage()方法就运行在子线程中。handleMessage中调用了onHandleIntent()方法，它是一个抽象方法，继承IntentService类需要实现该方法，把耗时操作放在onHandleIntent()方法中，等耗时操作运行完成后，会调用stopSelf()方法，服务会调用onDestory()方法消毁自己。如果onHandleIntent()中的耗时操作未运行完前就调用了stopSelf()方法，服务调用onDestory()方法，但耗时操作会继续运行，直至运行完毕。如果同时多次启动IntentService，任务会放在一个队列中，onCreate()和onDestory()方法都只会运行一次。

作用：用来处理后台耗时操作，如读取数据库或是本地文件等。

-说说Activity、Intent、Service 是什么关系
-ApplicationContext和ActivityContext的区别
-SP是进程同步的吗?有什么方法做到同步？
-谈谈多线程在Android中的使用
-进程和 Application 的生命周期
-封装View的时候怎么知道view的大小
-RecycleView原理
-AndroidManifest的作用与理解


2019Android网络编程总结
1.网络分层
OSI七层模型
OSI七层协议模型主要是：应用层（Application）、表示层（Presentation）、会话层（Session）、传输层（Transport）、网络层（Network）、数据链路层（Data Link）、物理层（Physical）。
2.TCP/IP五层模型
TCP/IP五层模型：应用层（Application）、传输层（Transport）、网络层（Network）、数据链路层（Data Link）、物理层（Physical）。
3.三次握手与四次挥手
第一次握手：客户端发送syn包(syn=j)到服务器，并进入SYN_SEND状态，等待服务器确认；
第二次握手：服务器收到syn包，必须确认客户的SYN（ack=j+1），同时自己也发送一个SYN包（syn=k），即SYN+ACK包，此时服务器进入SYN_RECV状态；
第三次握手：客户端收到服务器的SYN＋ACK包，向服务器发送确认包ACK(ack=k+1)，此包发送完毕，客户端和服务器进入ESTABLISHED状态，完成三次握手。
握手过程中传送的包里不包含数据，三次握手完毕后，客户端与服务器才正式开始传送数据。理想状态下，TCP连接一旦建立，在通信双方中的任何一方主动关闭连接之前，TCP 连接都将被一直保持下去。断开连接时服务器和客户端均可以主动发起断开TCP连接的请求，断开过程需要经过“四次握手”
第一次挥手：客户端发送报文告诉服务器没有数据要发送了
第二次挥手：服务端收到，再发送给客户端告诉它我收到了
第三次挥手：服务端向客户端发送报文，请求关闭连接
第四次挥手：客户端收到关闭连接的请求，向服务端发送报文，服务端关闭连接
4.TCP为什么三次握手不是两次握手，为什么两次握手不安全
为了实现可靠数据传输， TCP 协议的通信双方， 都必须维护一个序列号， 以标识发送出去的数据包中， 哪些是已经被对方收到的。 三次握手的过程即是通信双方相互告知序列号起始值， 并确认对方已经收到了序列号起始值的必经步骤
如果只是两次握手， 至多只有连接发起方的起始序列号能被确认， 另一方选择的序列号则得不到确认
5.为什么TCP是可靠的，UDP早不可靠的?为什么UDP比TCP快?
TCP/IP协议拥有三次握手双向机制，这一机制保证校验了数据，保证了他的可靠性。
UDP就没有了，udp信息发出后,不验证是否到达对方,所以不可靠。
6.http协议
http协议是一个基于请求与响应模式的无连接，无状态，应用层的协议，支持c/s模式，简单快速，灵活
简单快速：协议简单，通信速度快
灵活：允许传输任意类型的数据对象，由Content-Type标记
无连接：每次处理一个请求，处理完成后既断开
无状态：对事务处理没有记忆能力
http有两种报文：请求报文和响应报文
请求报文由请求行，请求报头，和请求数据组成
请求行：抓包第一行，包括请求方法，url和http版本
请求报头：指的就是题目中“里面的协议头部”
请求数据：指post方式提交的表单数据
响应报文由状态行，响应报头，响应正文组成
状态行：状态码
响应报头：同请求报头
响应正文：服务器返回的资源数据
接下来是http头部，既请求报头和响应报头，统称消息报头，消息报头可以分为通用报头，请求报头，响应报头，实体报头等
通用报头和实体报头既可以出现在请求报头中，也可以出现在响应报头中，通用报头包含的字段如：Date Connection Cache-Control,实体报头中有Content-Type Content-Length Content-Language Content-Encoding.
请求报头中包含的字段有：
Host,User-Agent,Accept-Encoding,Accept-Language,Connection
响应报头包含的字段：
Location，Server
7.http的get和post的区别
http是应用层的协议，底层基于TCP/IP协议，所以本质上，get和post请求都是TCP请求。所以二者的区别都是体现在应用层上（HTTP的规定和浏览器/服务器的限制）
1.参数的传输方式：GET参数通过URL传递，POST放在Request body中。
2.GET请求在URL中传送的参数是有长度限制的，而POST没有。
3.对于GET方式的请求，浏览器会把http header和data一并发送出去，服务器响应200（返回数据）；而对于POST，浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200 ok（返回数据）。不过要注意，并不是所有浏览器都会在POST中发送两次包，比如火狐
4.对参数的数据类型，GET只接受ASCII字符，而POST没有限制。
5.GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息。
6.GET请求只能进行url编码，而POST支持多种编码方式。
7.GET在浏览器回退时是无害的，而POST会再次提交请求。
8.GET产生的URL地址可以被Bookmark，而POST不可以。
9.GET请求会被浏览器主动cache，而POST不会，除非手动设置。
8.socket和http的区别：
Http协议：简单的对象访问协议，对应于应用层。Http协议是基于TCP链接的。
tcp协议：对应于传输层
ip协议：对应与网络层
TCP/IP是传输层协议，主要解决数据如何在网络中传输；而Http是应用层协议，主要解决如何包装数据。
Socket是对TCP/IP协议的封装，Socket本身并不是协议，而是一个调用接口（API），通过Socket，我们才能使用TCP/IP协议。
Http连接：http连接就是所谓的短连接，及客户端向服务器发送一次请求，服务器端相应后连接即会断掉。
socket连接：socket连接及时所谓的长连接，理论上客户端和服务端一旦建立连接，则不会主动断掉；但是由于各种环境因素可能会是连接断开，比如说：服务器端或客户端主机down了，网络故障，或者两者之间长时间没有数据传输，网络防火墙可能会断开该链接已释放网络资源。所以当一个socket连接中没有数据的传输，那么为了位置连续的连接需要发送心跳消息，具体心跳消息格式是开发者自己定义的。
9.TCP与UDP区别总结：
1、TCP面向连接（如打电话要先拨号建立连接）;UDP是无连接的，即发送数据之前不需要建立连接
2、TCP提供可靠的服务。也就是说，通过TCP连接传送的数据，无差错，不丢失，不重复，且按序到达;UDP尽最大努力交付，即不保 证可靠交付
3、TCP面向字节流，实际上是TCP把数据看成一连串无结构的字节流;UDP是面向报文的
UDP没有拥塞控制，因此网络出现拥塞不会使源主机的发送速率降低（对实时应用很有用，如IP电话，实时视频会议等）
4、每一条TCP连接只能是点到点的;UDP支持一对一，一对多，多对一和多对多的交互通信
5、TCP首部开销20字节;UDP的首部开销小，只有8个字节
6、TCP的逻辑通信信道是全双工的可靠信道，UDP则是不可靠信道
10.https
HTTPS(全称：Hyper Text Transfer Protocol over Secure Socket Layer)，是以安全为目标的HTTP通道，简单讲是HTTP的安全版。HTTP是应用层协议，位于HTTP协议之下是传输协议TCP。TCP负责传输，HTTP则定义了数据如何进行包装，在HTTP跟TCP中间加多了一层加密层TLS/SSL，SSL是个加密套件，负责对HTTP的数据进行加密。TLS是SSL的升级版。现在提到HTTPS，加密套件基本指的是TLS。
传输加密的流程：http是应用层将数据直接给到TCP进行传输，https是应用层将数据给到TLS/SSL，将数据加密后，再给到TCP进行传输。
HTTPS是如何加密数据的：
一般来说，加密分为对称加密、非对称加密
1. 对称加密：
对称加密的意思就是，加密数据用的密钥，跟解密数据用的密钥是一样的。
对称加密的优点在于加密、解密效率通常比较高。缺点在于，数据发送方、数据接收方需要协商、共享同一把密钥，并确保密钥不泄露给其他人。传输过程中容易被截获。
网上一个很形象的例子：假如现在小客与小服要进行一次私密的对话，他们不希望这次对话内容被其他外人知道。可是，我们平时的数据传输过程中又是明文传输的，万一被某个黑客把他们的对话内容给窃取了，那就难受了。为了解决这个问题，小服这家伙想到了一个方法来加密数据，让黑客看不到具体的内容。该方法是这样子的：在每次数据传输之前，小服会先传输小客一把密钥，然后小服在之后给小客发消息的过程中，会用这把密钥对这些消息进行加密。小客在收到这些消息后，会用之前小服给的那把密钥对这些消息进行解密，这样，小客就能得到密文里面真正的数据了。如果小客要给小服发消息，也同样用这把密钥来对消息进行加密，小服收到后也用这把密钥进行解密。 这样，就保证了数据传输的安全性。
2.非对称加密
非对称加密的意思就是，加密数据用的密钥（公钥），跟解密数据用的密钥（私钥）是不一样的。
网上一个很形象的例子：小服还是挺聪明的，得意了一会之后，小服意识到了密钥会被截取这个问题。倔强的小服又想到了另外一种方法：用非对称加密的方法来加密数据。该方法是这样的：小服和小客都拥有两把钥匙，一把钥匙的公开的（全世界都知道也没关系），称之为公钥；而另一把钥匙是保密（也就是只有自己才知道），称之为私钥。并且，用公钥加密的数据，只有对应的私钥才能解密；用私钥加密的数据，只有对应的公钥才能解密。所以在传输数据的过程中，小服在给小客传输数据的过程中，会用小客给他的公钥进行加密，然后小客收到后，再用自己的私钥进行解密。小客给小服发消息的时候，也一样会用小服给他的公钥进行加密，然后小服再用自己的私钥进行解密。 这样，数据就能安全着到达双方。是什么原因导致非对称加密这种方法的不安全性呢？它和对称加密方法的不安全性不同。非对称加密之所以不安全，是因为小客收到了公钥之后，无法确定这把公钥是否真的是小服。
解决的办法就是数字证书：小服再给小客发公钥的过程中，会把公钥以及小服的个人信息通过Hash算法生成消息摘要，为了防止摘要被人调换，小服还会用CA提供的私钥对消息摘要进行加密来形成数字签名，当小客拿到这份数字证书之后，就会用CA提供的公钥来对数字证书里面的数字签名进行解密得到消息摘要，然后对数字证书里面小服的公钥和个人信息进行Hash得到另一份消息摘要，然后把两份消息摘要进行对比，如果一样，则证明这些东西确实是小服的，否则就不是。
11.加密算法
1.对称加密算法
Data Encryption Standard(DES)
DES 是一种典型的块加密方法：将固定长度的明文通过一系列复杂的操作变成同样长度的密文，块的长度为64位。同时，DES 使用的密钥来自定义变换过程，因此算法认为只有持有加密所用的密钥的用户才能解密密文。 DES 的密钥表面上是64位的，实际有效密钥长度为56位，其余8位可以用于奇偶校验。
DES 现在已经不被视为一种安全的加密算法，主要原因是它使用的56位密钥过短。
为了提供实用所需的安全性，可以使用 DES 的派生算法 3DES 来进行加密 (虽然3DES 也存在理论上的攻击方法)。
Advanced Encryption Standard(AES)
AES 在密码学中又称 Rijndael 加密法，用来替代原先的 DES，已经被多方分析且广泛使用。
DES与AES的比较
自DES 算法公诸于世以来，学术界围绕它的安全性等方面进行了研究并展开了激烈的争论。在技术上，对DES的批评主要集中在以下几个方面：
1、作为分组密码，DES 的加密单位仅有64 位二进制，这对于数据传输来说太小，因为每个分组仅含8 个字符，而且其中某些位还要用于奇偶校验或其他通讯开销。
2、DES 的密钥的位数太短，只有56 比特，而且各次迭代中使用的密钥是递推产生的，这种相关必然降低密码体制的安全性，在现有技术下用穷举法寻找密钥已趋于可行。
3、DES 不能对抗差分和线性密码分析。
4、DES 用户实际使用的密钥长度为56bit，理论上最大加密强度为256。DES 算法要提高加密强度（例如增加密钥长度），则系统开销呈指数增长。除采用提高硬件功能和增加并行处理功能外，从算法本身和软件技术方面都无法提高DES 算法的加密强度。
2. 非对称加密算法
RSA
1977年由 MIT 的 Ron Rivest、Adi Shamir 和 Leonard Adleman 一起提出，以他们三人姓氏开头字母命名，是一种获得广泛使用的非对称加密算法。
对极大整数做因数分解的难度 (The Factoring Problem) 决定了 RSA 算法的可靠性。换言之，对一个极大整数做因数分解愈困难，RSA 算法就愈可靠。假如有人找到一种快速因数分解的算法的话，那么用 RSA 加密的信息的可靠性就肯定会极度下降。目前看来找到这样的算法的可能性非常小。
DES与RSA的比较
RSA算法的密钥很长，具有较好的安全性，但加密的计算量很大，加密速度较慢限制了其应用范围。为减少计算量，在传送信息时，常采用传统加密方法与公开密钥加密方法相结合的方式，即信息采用改进的DES对话密钥加密，然后使用RSA密钥加密对话密钥和信息摘要。对方收到信息后，用不同的密钥解密并可核对信息摘要。
采用DES与RSA相结合的应用，使它们的优缺点正好互补，即DES加密速度快，适合加密较长的报文，可用其加密明文；RSA加密速度慢，安全性好，应用于DES 密钥的加密，可解决DES 密钥分配的问题。
目前这种RSA和DES结合的方法已成为EMAIL保密通信标准。
12.Volley
1、Volley的特点
Volley是谷歌大会上推出的网络通信框架（2.3之前使用HttpClient，之后使用HttpUrlConnection），它既可以访问网络获取数据，也可以加载图片，并且在性能方面进行了大幅度的调整，它的设计目的就是适合进行数据量不大但通信频繁的网络操作，而对于大数据量的操作，比如文件下载，表现很糟糕，因为volley处理http返回的默认实现是BasicNetwork，它会把返回的流全部导入内存中，下载大文件会发生内存溢出
2、Volley执行的过程：
默认情况下，Volley中开启四个网络调度线程和一个缓存调度线程，首先请求会加入缓存队列，，缓存调度线程从缓存队列中取出线程，如果找到该请求的缓存就直接读取该缓存并解析，然后回调给主线程，如果没有找到缓存的响应，则将这个请求加入网络队列，然后网络调度线程会轮询取出网络队列中的请求，发起http请求，解析响应并将响应存入缓存，回调给主线程
3、Volley为什么不适合下载上传大文件？为什么适合数据量小的频率高的请求？
1.volley基于请求队列，Volley的网络请求线程池默认大小为4。意味着可以并发进行4个请求，大于4个，会排在队列中。并发量小所以适合数据量下频率高的请求
2.因为Volley下载文件会将流存入内存中（是一个小于4k的缓存池），大文件会导致内存溢出，所以不能下载大文件，不能上传大文件的原因和1中差不多，设想你上传了四个大文件，同时占用了volley的四个线程，导致其他网络请求都阻塞在队列中，造成反应慢的现象
13.OKHttp
1、OKHttp的特点
1.相较于Volley，它的最大并发量为64
2.使用连接池技术，支持5个并发的socket连接默认keepAlive时间为5分钟，解决TCP握手和挥手的效率问题，减少握手次数
3.支持Gzip压缩，且操作对用户透明，可以通过header设置，在发起请求的时候自动加入header，Accept-Encoding: gzip，而我们的服务器返回的时候header中有Content-Encoding: gzip
4.利用响应缓存来避免重复的网络请求
5.很方便的添加拦截器，通常情况下，拦截器用来添加，移除，转换请求和响应的头部信息，比如添加公参等
6.请求失败，自动重连，发生异常时重连，看源码调用recover方法重连了一次
7.支持SPDY协议(SPDY是Google开发的基于TCP的应用层协议，用以最小化网络延迟，提升网络速度，优化用户的网络使用体验。SPDY并不是一种用于替代HTTP的协议，而是对HTTP协议的增强。新协议的功能包括数据流的多路复用、请求优先级以及HTTP报头压缩。谷歌表示，引入SPDY协议后，在实验室测试中页面加载速度比原先快64%)
8.使用Okio来简化数据的访问与存储，提高性能
2、 OkHttp的缺点
1.消息回来需要切到主线程，主线程要自己去写。
2.调用比较复杂，需要自己进行封装。
3.缓存失效：网络请求时一般都会获取手机的一些硬件或网络信息，比如使用的网络环境。同时为了信息传输的安全性，可能还会对请求进行加密。在这些情况下OkHttp的缓存系统就会失效了，导致用户在无网络情况下不能访问缓存。
3、 OkHttp框架中都用到了哪些设计模式
1.最明显的Builder设计模式，如构建对象OkHttpClient，还有单利模式
2.工厂方法模式，如源码中的接口Call
3.观察者模式如EventListener，监听请求和响应
4.策略模式
5.责任链模式，如拦截器
14.Retrofit
Retrofit底层是基于OkHttp实现的，与其他网络框架不同的是，它更多使用运行时注解的方式提供功能
1、原理
通过java接口以及注解来描述网络请求，并用动态代理的方式生成网络请求的request，然后通过client调用相应的网络框架（默认okhttp）去发起网络请求，并将返回的response通过converterFactorty转换成相应的数据model，最后通过calladapter转换成其他数据方式（如rxjava Observable）
2、Retrofit流程
（1）通过解析 网络请求接口的注解 配置 网络请求参数
（2）通过 动态代理 生成 网络请求对象
（3）通过 网络请求适配器 将 网络请求对象 进行平台适配
（4）通过 网络请求执行器 发送网络请求
（5）通过 数据转换器 解析服务器返回的数据
（6）通过 回调执行器 切换线程（子线程 ->>主线程）
（7）用户在主线程处理返回结果
3、 Retrofit优点
1.可以配置不同HTTP client来实现网络请求，如okhttp、httpclient等；
2.请求的方法参数注解都可以定制；
3.支持同步、异步和RxJava；
4.超级解耦；
5.可以配置不同的反序列化工具来解析数据，如json、xml等
6.框架使用了很多设计模式


Android中高级面试题
1、Activity生命周期？

onCreate() -> onStart() -> onResume() -> onPause() -> onStop() -> onDetroy()

2、Service生命周期？

service 启动方式有两种，一种是通过startService()方式进行启动，另一种是通过bindService()方式进行启动。不同的启动方式他们的生命周期是不一样.

通过startService()这种方式启动的service，生命周期是这样：调用startService() --> onCreate()--> onStartConmon()--> onDestroy()。这种方式启动的话，需要注意一下几个问题，第一：当我们通过startService被调用以后，多次在调用startService(),onCreate()方法也只会被调用一次，而onStartConmon()会被多次调用当我们调用stopService()的时候，onDestroy()就会被调用，从而销毁服务。第二：当我们通过startService启动时候，通过intent传值，在onStartConmon()方法中获取值的时候，一定要先判断intent是否为null。

通过bindService()方式进行绑定，这种方式绑定service，生命周期走法：bindService-->onCreate()-->onBind()-->unBind()-->onDestroy()  bingservice 这种方式进行启动service好处是更加便利activity中操作service，比如加入service中有几个方法，a,b ，如果要在activity中调用，在需要在activity获取ServiceConnection对象，通过ServiceConnection来获取service中内部类的类对象，然后通过这个类对象就可以调用类中的方法，当然这个类需要继承Binder对象

 

3、Activity的启动过程（不要回答生命周期）

app启动的过程有两种情况，第一种是从桌面launcher上点击相应的应用图标，第二种是在activity中通过调用startActivity来启动一个新的activity。

我们创建一个新的项目，默认的根activity都是MainActivity，而所有的activity都是保存在堆栈中的，我们启动一个新的activity就会放在上一个activity上面，而我们从桌面点击应用图标的时候，由于launcher本身也是一个应用，当我们点击图标的时候，系统就会调用startActivitySately(),一般情况下，我们所启动的activity的相关信息都会保存在intent中，比如action，category等等。我们在安装这个应用的时候，系统也会启动一个PackaManagerService的管理服务，这个管理服务会对AndroidManifest.xml文件进行解析，从而得到应用程序中的相关信息，比如service，activity，Broadcast等等，然后获得相关组件的信息。当我们点击应用图标的时候，就会调用startActivitySately()方法，而这个方法内部则是调用startActivty(),而startActivity()方法最终还是会调用startActivityForResult()这个方法。而在startActivityForResult()这个方法。因为startActivityForResult()方法是有返回结果的，所以系统就直接给一个-1，就表示不需要结果返回了。而startActivityForResult()这个方法实际是通过Instrumentation类中的execStartActivity()方法来启动activity，Instrumentation这个类主要作用就是监控程序和系统之间的交互。而在这个execStartActivity()方法中会获取ActivityManagerService的代理对象，通过这个代理对象进行启动activity。启动会就会调用一个checkStartActivityResult()方法，如果说没有在配置清单中配置有这个组件，就会在这个方法中抛出异常了。当然最后是调用的是Application.scheduleLaunchActivity()进行启动activity，而这个方法中通过获取得到一个ActivityClientRecord对象，而这个ActivityClientRecord通过handler来进行消息的发送，系统内部会将每一个activity组件使用ActivityClientRecord对象来进行描述，而ActivityClientRecord对象中保存有一个LoaderApk对象，通过这个对象调用handleLaunchActivity来启动activity组件，而页面的生命周期方法也就是在这个方法中进行调用。

 

 

4、Broadcast注册方式与区别 

此处延伸：什么情况下用动态注册

 

Broadcast广播，注册方式主要有两种.

第一种是静态注册，也可成为常驻型广播，这种广播需要在Androidmanifest.xml中进行注册，这中方式注册的广播，不受页面生命周期的影响，即使退出了页面，也可以收到广播这种广播一般用于想开机自启动啊等等，由于这种注册的方式的广播是常驻型广播，所以会占用CPU的资源。

 

第二种是动态注册，而动态注册的话，是在代码中注册的，这种注册方式也叫非常驻型广播，收到生命周期的影响，退出页面后，就不会收到广播，我们通常运用在更新UI方面。这种注册方式优先级较高。最后需要解绑，否会会内存泄露

广播是分为有序广播和无序广播。

 

5、HttpClient与HttpUrlConnection的区别 

此处延伸：Volley里用的哪种请求方式（2.3前HttpClient，2.3后HttpUrlConnection）

 

首先HttpClient和HttpUrlConnection 这两种方式都支持Https协议，都是以流的形式进行上传或者下载数据，也可以说是以流的形式进行数据的传输，还有ipv6,以及连接池等功能。HttpClient这个拥有非常多的API，所以如果想要进行扩展的话，并且不破坏它的兼容性的话，很难进行扩展，也就是这个原因，Google在Android6.0的时候，直接就弃用了这个HttpClient.

而HttpUrlConnection相对来说就是比较轻量级了，API比较少，容易扩展，并且能够满足Android大部分的数据传输。比较经典的一个框架volley，在2.3版本以前都是使用HttpClient,在2.3以后就使用了HttpUrlConnection。

 

6、java虚拟机和Dalvik虚拟机的区别 

Java虚拟机：

1、java虚拟机基于栈。 基于栈的机器必须使用指令来载入和操作栈上数据，所需指令更多更多。

2、java虚拟机运行的是java字节码。（java类会被编译成一个或多个字节码.class文件）

Dalvik虚拟机：

1、dalvik虚拟机是基于寄存器的

2、Dalvik运行的是自定义的.dex字节码格式。（java类被编译成.class文件后，会通过一个dx工具将所有的.class文件转换成一个.dex文件，然后dalvik虚拟机会从其中读取指令和数据

3、常量池已被修改为只使用32位的索引，以 简化解释器。

4、一个应用，一个虚拟机实例，一个进程（所有android应用的线程都是对应一个linux线程，都运行在自己的沙盒中，不同的应用在不同的进程中运行。每个android dalvik应用程序都被赋予了一个独立的linux PID(app_*)）

 

7、进程保活（不死进程）

此处延伸：进程的优先级是什么

当前业界的Android进程保活手段主要分为** 黑、白、灰 **三种，其大致的实现思路如下：

黑色保活：不同的app进程，用广播相互唤醒（包括利用系统提供的广播进行唤醒）

白色保活：启动前台Service

灰色保活：利用系统的漏洞启动前台Service

黑色保活

所谓黑色保活，就是利用不同的app进程使用广播来进行相互唤醒。举个3个比较常见的场景：

场景1：开机，网络切换、拍照、拍视频时候，利用系统产生的广播唤醒app

场景2：接入第三方SDK也会唤醒相应的app进程，如微信sdk会唤醒微信，支付宝sdk会唤醒支付宝。由此发散开去，就会直接触发了下面的 场景3

场景3：假如你手机里装了支付宝、淘宝、天猫、UC等阿里系的app，那么你打开任意一个阿里系的app后，有可能就顺便把其他阿里系的app给唤醒了。（只是拿阿里打个比方，其实BAT系都差不多）

白色保活

白色保活手段非常简单，就是调用系统api启动一个前台的Service进程，这样会在系统的通知栏生成一个Notification，用来让用户知道有这样一个app在运行着，哪怕当前的app退到了后台。如下方的LBE和QQ音乐这样：

灰色保活

灰色保活，这种保活手段是应用范围最广泛。它是利用系统的漏洞来启动一个前台的Service进程，与普通的启动方式区别在于，它不会在系统通知栏处出现一个Notification，看起来就如同运行着一个后台Service进程一样。这样做带来的好处就是，用户无法察觉到你运行着一个前台进程（因为看不到Notification）,但你的进程优先级又是高于普通后台进程的。那么如何利用系统的漏洞呢，大致的实现思路和代码如下：

思路一：API < 18，启动前台Service时直接传入new Notification()；

思路二：API >= 18，同时启动两个id相同的前台Service，然后再将后启动的Service做stop处理

熟悉Android系统的童鞋都知道，系统出于体验和性能上的考虑，app在退到后台时系统并不会真正的kill掉这个进程，而是将其缓存起来。打开的应用越多，后台缓存的进程也越多。在系统内存不足的情况下，系统开始依据自身的一套进程回收机制来判断要kill掉哪些进程，以腾出内存来供给需要的app。这套杀进程回收内存的机制就叫 Low Memory Killer ，它是基于Linux内核的 OOM Killer（Out-Of-Memory killer）机制诞生。

进程的重要性，划分5级：

前台进程 (Foreground process)

可见进程 (Visible process)

服务进程 (Service process)

后台进程 (Background process)

空进程 (Empty process)

 

了解完 Low Memory Killer，再科普一下oom_adj。什么是oom_adj？它是linux内核分配给每个系统进程的一个值，代表进程的优先级，进程回收机制就是根据这个优先级来决定是否进行回收。对于oom_adj的作用，你只需要记住以下几点即可：

进程的oom_adj越大，表示此进程优先级越低，越容易被杀回收；越小，表示进程优先级越高，越不容易被杀回收

普通app进程的oom_adj>=0,系统进程的oom_adj才可能<0

有些手机厂商把这些知名的app放入了自己的白名单中，保证了进程不死来提高用户体验（如微信、QQ、陌陌都在小米的白名单中）。如果从白名单中移除，他们终究还是和普通app一样躲避不了被杀的命运，为了尽量避免被杀，还是老老实实去做好优化工作吧。

所以，进程保活的根本方案终究还是回到了性能优化上，进程永生不死终究是个彻头彻尾的伪命题！

 

 

8、讲解一下Context 

Context是一个抽象基类。在翻译为上下文，也可以理解为环境，是提供一些程序的运行环境基础信息。Context下有两个子类，ContextWrapper是上下文功能的封装类，而ContextImpl则是上下文功能的实现类。而ContextWrapper又有三个直接的子类， ContextThemeWrapper、Service和Application。其中，ContextThemeWrapper是一个带主题的封装类，而它有一个直接子类就是Activity，所以Activity和Service以及Application的Context是不一样的，只有Activity需要主题，Service不需要主题。Context一共有三种类型，分别是Application、Activity和Service。这三个类虽然分别各种承担着不同的作用，但它们都属于Context的一种，而它们具体Context的功能则是由ContextImpl类去实现的，因此在绝大多数场景下，Activity、Service和Application这三种类型的Context都是可以通用的。不过有几种场景比较特殊，比如启动Activity，还有弹出Dialog。出于安全原因的考虑，Android是不允许Activity或Dialog凭空出现的，一个Activity的启动必须要建立在另一个Activity的基础之上，也就是以此形成的返回栈。而Dialog则必须在一个Activity上面弹出（除非是System Alert类型的Dialog），因此在这种场景下，我们只能使用Activity类型的Context，否则将会出错。

 

getApplicationContext()和getApplication()方法得到的对象都是同一个application对象，只是对象的类型不一样。

Context数量 = Activity数量 + Service数量 + 1 （1为Application）

 

9、理解Activity，View,Window三者关系

这个问题真的很不好回答。所以这里先来个算是比较恰当的比喻来形容下它们的关系吧。Activity像一个工匠（控制单元），Window像窗户（承载模型），View像窗花（显示视图）LayoutInflater像剪刀，Xml配置像窗花图纸。

1：Activity构造的时候会初始化一个Window，准确的说是PhoneWindow。

2：这个PhoneWindow有一个“ViewRoot”，这个“ViewRoot”是一个View或者说ViewGroup，是最初始的根视图。

3：“ViewRoot”通过addView方法来一个个的添加View。比如TextView，Button等

4：这些View的事件监听，是由WindowManagerService来接受消息，并且回调Activity函数。比如onClickListener，onKeyDown等。

 

10、四种LaunchMode及其使用场景

此处延伸：栈(First In Last Out)与队列(First In First Out)的区别

栈与队列的区别：

1. 队列先进先出，栈先进后出

2. 对插入和删除操作的"限定"。 栈是限定只能在表的一端进行插入和删除操作的线性表。 队列是限定只能在表的一端进行插入和在另一端进行删除操作的线性表。

3. 遍历数据速度不同

standard 模式

这是默认模式，每次激活Activity时都会创建Activity实例，并放入任务栈中。使用场景：大多数Activity。

singleTop 模式

如果在任务的栈顶正好存在该Activity的实例，就重用该实例( 会调用实例的 onNewIntent() )，否则就会创建新的实例并放入栈顶，即使栈中已经存在该Activity的实例，只要不在栈顶，都会创建新的实例。使用场景如新闻类或者阅读类App的内容页面。

singleTask 模式

如果在栈中已经有该Activity的实例，就重用该实例(会调用实例的 onNewIntent() )。重用时，会让该实例回到栈顶，因此在它上面的实例将会被移出栈。如果栈中不存在该实例，将会创建新的实例放入栈中。使用场景如浏览器的主界面。不管从多少个应用启动浏览器，只会启动主界面一次，其余情况都会走onNewIntent，并且会清空主界面上面的其他页面。

singleInstance 模式

在一个新栈中创建该Activity的实例，并让多个应用共享该栈中的该Activity实例。一旦该模式的Activity实例已经存在于某个栈中，任何应用再激活该Activity时都会重用该栈中的实例( 会调用实例的 onNewIntent() )。其效果相当于多个应用共享一个应用，不管谁激活该 Activity 都会进入同一个应用中。使用场景如闹铃提醒，将闹铃提醒与闹铃设置分离。singleInstance不要用于中间页面，如果用于中间页面，跳转会有问题，比如：A -> B (singleInstance) -> C，完全退出后，在此启动，首先打开的是B。

 

11、View的绘制流程

自定义控件：

1、组合控件。这种自定义控件不需要我们自己绘制，而是使用原生控件组合成的新控件。如标题栏。

2、继承原有的控件。这种自定义控件在原生控件提供的方法外，可以自己添加一些方法。如制作圆角，圆形图片。

3、完全自定义控件：这个View上所展现的内容全部都是我们自己绘制出来的。比如说制作水波纹进度条。

 

View的绘制流程：OnMeasure()——>OnLayout()——>OnDraw()

 

第一步：OnMeasure()：测量视图大小。从顶层父View到子View递归调用measure方法，measure方法又回调OnMeasure。

 

第二步：OnLayout()：确定View位置，进行页面布局。从顶层父View向子View的递归调用view.layout方法的过程，即父View根据上一步measure子View所得到的布局大小和布局参数，将子View放在合适的位置上。

 

第三步：OnDraw()：绘制视图。ViewRoot创建一个Canvas对象，然后调用OnDraw()。六个步骤：①、绘制视图的背景；②、保存画布的图层（Layer）；③、绘制View的内容；④、绘制View子视图，如果没有就不用；

⑤、还原图层（Layer）；⑥、绘制滚动条。

 

12、View，ViewGroup事件分发



1. Touch事件分发中只有两个主角:ViewGroup和View。ViewGroup包含onInterceptTouchEvent、dispatchTouchEvent、onTouchEvent三个相关事件。View包含dispatchTouchEvent、onTouchEvent两个相关事件。其中ViewGroup又继承于View。

2.ViewGroup和View组成了一个树状结构，根节点为Activity内部包含的一个ViwGroup。

3.触摸事件由Action_Down、Action_Move、Aciton_UP组成，其中一次完整的触摸事件中，Down和Up都只有一个，Move有若干个，可以为0个。

4.当Acitivty接收到Touch事件时，将遍历子View进行Down事件的分发。ViewGroup的遍历可以看成是递归的。分发的目的是为了找到真正要处理本次完整触摸事件的View，这个View会在onTouchuEvent结果返回true。

5.当某个子View返回true时，会中止Down事件的分发，同时在ViewGroup中记录该子View。接下去的Move和Up事件将由该子View直接进行处理。由于子View是保存在ViewGroup中的，多层ViewGroup的节点结构时，上级ViewGroup保存的会是真实处理事件的View所在的ViewGroup对象:如ViewGroup0-ViewGroup1-TextView的结构中，TextView返回了true，它将被保存在ViewGroup1中，而ViewGroup1也会返回true，被保存在ViewGroup0中。当Move和UP事件来时，会先从ViewGroup0传递至ViewGroup1，再由ViewGroup1传递至TextView。

6.当ViewGroup中所有子View都不捕获Down事件时，将触发ViewGroup自身的onTouch事件。触发的方式是调用super.dispatchTouchEvent函数，即父类View的dispatchTouchEvent方法。在所有子View都不处理的情况下，触发Acitivity的onTouchEvent方法。

7.onInterceptTouchEvent有两个作用：1.拦截Down事件的分发。2.中止Up和Move事件向目标View传递，使得目标View所在的ViewGroup捕获Up和Move事件。

 

13、保存Activity状态

onSaveInstanceState(Bundle)会在activity转入后台状态之前被调用，也就是onStop()方法之前，onPause方法之后被调用；

 

14、Android中的几种动画

帧动画：指通过指定每一帧的图片和播放时间，有序的进行播放而形成动画效果，比如想听的律动条。

补间动画：指通过指定View的初始状态、变化时间、方式，通过一系列的算法去进行图形变换，从而形成动画效果，主要有Alpha、Scale、Translate、Rotate四种效果。注意：只是在视图层实现了动画效果，并没有真正改变View的属性，比如滑动列表，改变标题栏的透明度。

属性动画：在Android3.0的时候才支持，通过不断的改变View的属性，不断的重绘而形成动画效果。相比于视图动画，View的属性是真正改变了。比如view的旋转，放大，缩小。

 

15、Android中跨进程通讯的几种方式

Android 跨进程通信，像intent，contentProvider,广播，service都可以跨进程通信。

intent：这种跨进程方式并不是访问内存的形式，它需要传递一个uri,比如说打电话。

contentProvider：这种形式，是使用数据共享的形式进行数据共享。

service：远程服务，aidl

广播

 

16、AIDL理解

此处延伸：简述Binder

 

AIDL: 每一个进程都有自己的Dalvik VM实例，都有自己的一块独立的内存，都在自己的内存上存储自己的数据，执行着自己的操作，都在自己的那片狭小的空间里过完自己的一生。而aidl就类似与两个进程之间的桥梁，使得两个进程之间可以进行数据的传输，跨进程通信有多种选择，比如 BroadcastReceiver , Messenger 等，但是 BroadcastReceiver 占用的系统资源比较多，如果是频繁的跨进程通信的话显然是不可取的；Messenger 进行跨进程通信时请求队列是同步进行的，无法并发执行。

 

Binde机制简单理解:

在Android系统的Binder机制中，是有Client,Service,ServiceManager,Binder驱动程序组成的，其中Client，service，Service Manager运行在用户空间，Binder驱动程序是运行在内核空间的。而Binder就是把这4种组件粘合在一块的粘合剂，其中核心的组件就是Binder驱动程序，Service Manager提供辅助管理的功能，而Client和Service正是在Binder驱动程序和Service Manager提供的基础设施上实现C/S 之间的通信。其中Binder驱动程序提供设备文件/dev/binder与用户控件进行交互，

Client、Service，Service Manager通过open和ioctl文件操作相应的方法与Binder驱动程序进行通信。而Client和Service之间的进程间通信是通过Binder驱动程序间接实现的。而Binder Manager是一个守护进程，用来管理Service，并向Client提供查询Service接口的能力。

 

 

 

17、Handler的原理

Android中主线程是不能进行耗时操作的，子线程是不能进行更新UI的。所以就有了handler，它的作用就是实现线程之间的通信。

handler整个流程中，主要有四个对象，handler，Message,MessageQueue,Looper。当应用创建的时候，就会在主线程中创建handler对象，

我们通过要传送的消息保存到Message中，handler通过调用sendMessage方法将Message发送到MessageQueue中，Looper对象就会不断的调用loop()方法

不断的从MessageQueue中取出Message交给handler进行处理。从而实现线程之间的通信。

 

 

18、Binder机制原理

在Android系统的Binder机制中，是有Client,Service,ServiceManager,Binder驱动程序组成的，其中Client，service，Service Manager运行在用户空间，Binder驱动程序是运行在内核空间的。而Binder就是把这4种组件粘合在一块的粘合剂，其中核心的组件就是Binder驱动程序，Service Manager提供辅助管理的功能，而Client和Service正是在Binder驱动程序和Service Manager提供的基础设施上实现C/S 之间的通信。其中Binder驱动程序提供设备文件/dev/binder与用户控件进行交互，Client、Service，Service Manager通过open和ioctl文件操作相应的方法与Binder驱动程序进行通信。而Client和Service之间的进程间通信是通过Binder驱动程序间接实现的。而Binder Manager是一个守护进程，用来管理Service，并向Client提供查询Service接口的能力。

 

19、热修复的原理

我们知道Java虚拟机 —— JVM 是加载类的class文件的，而Android虚拟机——Dalvik/ART VM 是加载类的dex文件，

而他们加载类的时候都需要ClassLoader,ClassLoader有一个子类BaseDexClassLoader，而BaseDexClassLoader下有一个

数组——DexPathList，是用来存放dex文件，当BaseDexClassLoader通过调用findClass方法时，实际上就是遍历数组，

找到相应的dex文件，找到，则直接将它return。而热修复的解决方法就是将新的dex添加到该集合中，并且是在旧的dex的前面，

所以就会优先被取出来并且return返回。

 

 

 

 

 

20、Android内存泄露及管理

（1）内存溢出（OOM）和内存泄露（对象无法被回收）的区别。 

（2）引起内存泄露的原因

(3) 内存泄露检测工具 ------>LeakCanary

 

内存溢出 out of memory：是指程序在申请内存时，没有足够的内存空间供其使用，出现out of memory；比如申请了一个integer,但给它存了long才能存下的数，那就是内存溢出。内存溢出通俗的讲就是内存不够用。

内存泄露 memory leak：是指程序在申请内存后，无法释放已申请的内存空间，一次内存泄露危害可以忽略，但内存泄露堆积后果很严重，无论多少内存,迟早会被占光

内存泄露原因：

一、Handler 引起的内存泄漏。

解决：将Handler声明为静态内部类，就不会持有外部类SecondActivity的引用，其生命周期就和外部类无关，

如果Handler里面需要context的话，可以通过弱引用方式引用外部类

二、单例模式引起的内存泄漏。

解决：Context是ApplicationContext，由于ApplicationContext的生命周期是和app一致的，不会导致内存泄漏

三、非静态内部类创建静态实例引起的内存泄漏。

解决：把内部类修改为静态的就可以避免内存泄漏了

四、非静态匿名内部类引起的内存泄漏。

解决：将匿名内部类设置为静态的。

五、注册/反注册未成对使用引起的内存泄漏。

注册广播接受器、EventBus等，记得解绑。

六、资源对象没有关闭引起的内存泄漏。

在这些资源不使用的时候，记得调用相应的类似close（）、destroy（）、recycler（）、release（）等方法释放。

七、集合对象没有及时清理引起的内存泄漏。

通常会把一些对象装入到集合中，当不使用的时候一定要记得及时清理集合，让相关对象不再被引用。

 

21、Fragment与Fragment、Activity通信的方式

1.直接在一个Fragment中调用另外一个Fragment中的方法

2.使用接口回调

3.使用广播

4.Fragment直接调用Activity中的public方法

 

22、Android UI适配

字体使用sp,使用dp，多使用match_parent，wrap_content，weight

图片资源，不同图片的的分辨率，放在相应的文件夹下可使用百分比代替。

23、app优化

app优化:(工具：Hierarchy Viewer 分析布局  工具：TraceView 测试分析耗时的)

App启动优化

布局优化

响应优化

内存优化

电池使用优化

网络优化

 

App启动优化(针对冷启动)

App启动的方式有三种：

冷启动：App没有启动过或App进程被killed, 系统中不存在该App进程, 此时启动App即为冷启动。

热启动：热启动意味着你的App进程只是处于后台, 系统只是将其从后台带到前台, 展示给用户。

 

介于冷启动和热启动之间, 一般来说在以下两种情况下发生:

 

(1)用户back退出了App, 然后又启动. App进程可能还在运行, 但是activity需要重建。

(2)用户退出App后, 系统可能由于内存原因将App杀死, 进程和activity都需要重启, 但是可以在onCreate中将被动杀死锁保存的状态(saved instance state)恢复。

 

优化：

Application的onCreate（特别是第三方SDK初始化），首屏Activity的渲染都不要进行耗时操作，如果有，就可以放到子线程或者IntentService中

 

布局优化

尽量不要过于复杂的嵌套。可以使用<include>，<merge>，<ViewStub>

 

响应优化

Android系统每隔16ms会发出VSYNC信号重绘我们的界面(Activity)。

页面卡顿的原因：

(1)过于复杂的布局.

(2)UI线程的复杂运算

(3)频繁的GC,导致频繁GC有两个原因:1、内存抖动, 即大量的对象被创建又在短时间内马上被释放.2、瞬间产生大量的对象会严重占用内存区域。

 

内存优化：参考内存泄露和内存溢出部分

 

电池使用优化(使用工具：Batterystats & bugreport)

(1)优化网络请求

(2)定位中使用GPS, 请记得及时关闭

 

网络优化(网络连接对用户的影响:流量,电量,用户等待)可在Android studio下方logcat旁边那个工具Network Monitor检测

API设计：App与Server之间的API设计要考虑网络请求的频次, 资源的状态等. 以便App可以以较少的请求来完成业务需求和界面的展示.

Gzip压缩：使用Gzip来压缩request和response, 减少传输数据量, 从而减少流量消耗.

图片的Size：可以在获取图片时告知服务器需要的图片的宽高, 以便服务器给出合适的图片, 避免浪费.

网络缓存：适当的缓存, 既可以让我们的应用看起来更快, 也能避免一些不必要的流量消耗.

 

24、图片优化

(1)对图片本身进行操作。尽量不要使用setImageBitmap、setImageResource、BitmapFactory.decodeResource来设置一张大图，因为这些方法在完成decode后，

最终都是通过java层的createBitmap来完成的，需要消耗更多内存.

(2)图片进行缩放的比例，SDK中建议其值是2的指数值,值越大会导致图片不清晰。

(3)不用的图片记得调用图片的recycle()方法

 

25、HybridApp WebView和JS交互

Android与JS通过WebView互相调用方法，实际上是：

Android去调用JS的代码

1. 通过WebView的loadUrl(),使用该方法比较简洁，方便。但是效率比较低，获取返回值比较困难。

2. 通过WebView的evaluateJavascript(),该方法效率高，但是4.4以上的版本才支持，4.4以下版本不支持。所以建议两者混合使用。

JS去调用Android的代码

1. 通过WebView的addJavascriptInterface（）进行对象映射 ，该方法使用简单，仅将Android对象和JS对象映射即可，但是存在比较大的漏洞。

 

漏洞产生原因是：当JS拿到Android这个对象后，就可以调用这个Android对象中所有的方法，包括系统类（java.lang.Runtime 类），从而进行任意代码执行。

解决方式：

(1)Google 在Android 4.2 版本中规定对被调用的函数以 @JavascriptInterface进行注解从而避免漏洞攻击。

(2)在Android 4.2版本之前采用拦截prompt（）进行漏洞修复。

 

2. 通过 WebViewClient 的shouldOverrideUrlLoading ()方法回调拦截 url 。这种方式的优点：不存在方式1的漏洞；缺点：JS获取Android方法的返回值复杂。(ios主要用的是这个方式)

 

(1)Android通过 WebViewClient 的回调方法shouldOverrideUrlLoading ()拦截 url

(2)解析该 url 的协议

(3)如果检测到是预先约定好的协议，就调用相应方法

 

3. 通过 WebChromeClient 的onJsAlert()、onJsConfirm()、onJsPrompt（）方法回调拦截JS对话框alert()、confirm()、prompt（） 消息

这种方式的优点：不存在方式1的漏洞；缺点：JS获取Android方法的返回值复杂。

 

26、JAVA GC原理

垃圾收集算法的核心思想是：对虚拟机可用内存空间，即堆空间中的对象进行识别，如果对象正在被引用，那么称其为存活对象

，反之，如果对象不再被引用，则为垃圾对象，可以回收其占据的空间，用于再分配。垃圾收集算法的选择和垃圾收集系统参数的合理调节直接影响着系统性能。

 

27、ANR

ANR全名Application Not Responding, 也就是"应用无响应". 当操作在一段时间内系统无法处理时, 系统层面会弹出上图那样的ANR对话框.

产生原因：

(1)5s内无法响应用户输入事件(例如键盘输入, 触摸屏幕等).

(2)BroadcastReceiver在10s内无法结束

(3)Service 20s内无法结束（低概率）

 

解决方式：

(1)不要在主线程中做耗时的操作，而应放在子线程中来实现。如onCreate()和onResume()里尽可能少的去做创建操作。

(2)应用程序应该避免在BroadcastReceiver里做耗时的操作或计算。

(3)避免在Intent Receiver里启动一个Activity，因为它会创建一个新的画面，并从当前用户正在运行的程序上抢夺焦点。

(4)service是运行在主线程的，所以在service中做耗时操作，必须要放在子线程中。

28、设计模式

此处延伸：Double Check的写法被要求写出来。

单例模式：分为恶汉式和懒汉式

恶汉式：

public class Singleton 

{ 

    private static Singleton instance = new Singleton(); 

     

    public static Singleton getInstance() 

    { 

        return instance ; 

    } 

}

 

懒汉式：

 

public class Singleton02 

{ 

    private static Singleton02 instance; 

 

    public static Singleton02 getInstance() 

    { 

        if (instance == null) 

        { 

            synchronized (Singleton02.class) 

            { 

                if (instance == null) 

                { 

                    instance = new Singleton02(); 

                } 

            } 

        } 

        return instance; 

    } 

}

 

 

 

29、RxJava

30、MVP，MVC，MVVM

此处延伸：手写mvp例子，与mvc之间的区别，mvp的优势

MVP模式，对应着Model--业务逻辑和实体模型,view--对应着activity，负责View的绘制以及与用户交互,Presenter--负责View和Model之间的交互,MVP模式是在MVC模式的基础上，将Model与View彻底分离使得项目的耦合性更低，在Mvc中项目中的activity对应着mvc中的C--Controllor,而项目中的逻辑处理都是在这个C中处理，同时View与Model之间的交互，也是也就是说，mvc中所有的逻辑交互和用户交互，都是放在Controllor中，也就是activity中。View和model是可以直接通信的。而MVP模式则是分离的更加彻底，分工更加明确Model--业务逻辑和实体模型，view--负责与用户交互，Presenter 负责完成View于Model间的交互，MVP和MVC最大的区别是MVC中是允许Model和View进行交互的，而MVP中很明显，Model与View之间的交互由Presenter完成。还有一点就是Presenter与View之间的交互是通过接口的

 

31、手写算法（选择冒泡必须要会）

32、JNI 

(1)安装和下载Cygwin，下载 Android NDK

(2)在ndk项目中JNI接口的设计

(3)使用C/C++实现本地方法

(4)JNI生成动态链接库.so文件

(5)将动态链接库复制到java工程，在java工程中调用，运行java工程即可

 

33、RecyclerView和ListView的区别

RecyclerView可以完成ListView,GridView的效果，还可以完成瀑布流的效果。同时还可以设置列表的滚动方向（垂直或者水平）；

RecyclerView中view的复用不需要开发者自己写代码，系统已经帮封装完成了。

RecyclerView可以进行局部刷新。

RecyclerView提供了API来实现item的动画效果。

在性能上：

如果需要频繁的刷新数据，需要添加动画，则RecyclerView有较大的优势。

如果只是作为列表展示，则两者区别并不是很大。

 

34、Universal-ImageLoader，Picasso，Fresco，Glide对比

Fresco 是 Facebook 推出的开源图片缓存工具，主要特点包括：两个内存缓存加上 Native 缓存构成了三级缓存，

优点：

1. 图片存储在安卓系统的匿名共享内存, 而不是虚拟机的堆内存中, 图片的中间缓冲数据也存放在本地堆内存, 所以, 应用程序有更多的内存使用, 不会因为图片加载而导致oom, 同时也减少垃圾回收器频繁调用回收 Bitmap 导致的界面卡顿, 性能更高。

 

2. 渐进式加载 JPEG 图片, 支持图片从模糊到清晰加载。

 

3. 图片可以以任意的中心点显示在 ImageView, 而不仅仅是图片的中心。

 

4. JPEG 图片改变大小也是在 native 进行的, 不是在虚拟机的堆内存, 同样减少 OOM。

 

5. 很好的支持 GIF 图片的显示。

 

缺点:

1. 框架较大, 影响 Apk 体积

2. 使用较繁琐

 

Universal-ImageLoader：（估计由于HttpClient被Google放弃，作者就放弃维护这个框架）

优点：

1.支持下载进度监听

2.可以在 View 滚动中暂停图片加载，通过 PauseOnScrollListener 接口可以在 View 滚动中暂停图片加载。

3.默认实现多种内存缓存算法 这几个图片缓存都可以配置缓存算法，不过 ImageLoader 默认实现了较多缓存算法，如 Size 最大先删除、使用最少先删除、最近最少使用、先进先删除、时间最长先删除等。

4.支持本地缓存文件名规则定义

     

Picasso 优点

1. 自带统计监控功能。支持图片缓存使用的监控，包括缓存命中率、已使用内存大小、节省的流量等。

 

2.支持优先级处理。每次任务调度前会选择优先级高的任务，比如 App 页面中 Banner 的优先级高于 Icon 时就很适用。

 

3.支持延迟到图片尺寸计算完成加载

 

4.支持飞行模式、并发线程数根据网络类型而变。 手机切换到飞行模式或网络类型变换时会自动调整线程池最大并发数，比如 wifi 最大并发为 4，4g 为 3，3g 为 2。  这里 Picasso 根据网络类型来决定最大并发数，而不是 CPU 核数。

 

5.“无”本地缓存。无”本地缓存，不是说没有本地缓存，而是 Picasso 自己没有实现，交给了 Square 的另外一个网络库 okhttp 去实现，这样的好处是可以通过请求 Response Header 中的 Cache-Control 及 Expired 控制图片的过期时间。

 

 Glide 优点

1. 不仅仅可以进行图片缓存还可以缓存媒体文件。Glide 不仅是一个图片缓存，它支持 Gif、WebP、缩略图。甚至是 Video，所以更该当做一个媒体缓存。

 

2. 支持优先级处理。

 

 

3. 与 Activity/Fragment 生命周期一致，支持 trimMemory。Glide 对每个 context 都保持一个 RequestManager，通过 FragmentTransaction 保持与 Activity/Fragment 生命周期一致，并且有对应的 trimMemory 接口实现可供调用。

 

4. 支持 okhttp、Volley。Glide 默认通过 UrlConnection 获取数据，可以配合 okhttp 或是 Volley 使用。实际 ImageLoader、Picasso 也都支持 okhttp、Volley。

 

 

5. 内存友好。Glide 的内存缓存有个 active 的设计，从内存缓存中取数据时，不像一般的实现用 get，而是用 remove，再将这个缓存数据放到一个 value 为软引用的 activeResources map 中，并计数引用数，在图片加载完成后进行判断，如果引用计数为空则回收掉。内存缓存更小图片，Glide 以 url、view_width、view_height、屏幕的分辨率等做为联合 key，将处理后的图片缓存在内存缓存中，而不是原始图片以节省大小与 Activity/Fragment 生命周期一致，支持 trimMemory。图片默认使用默认 RGB_565 而不是 ARGB_888，虽然清晰度差些，但图片更小，也可配置到 ARGB_888。

 

6.Glide 可以通过 signature 或不使用本地缓存支持 url 过期

 

42、Xutils, OKhttp, Volley, Retrofit对比

Xutils这个框架非常全面，可以进行网络请求，可以进行图片加载处理，可以数据储存，还可以对view进行注解，使用这个框架非常方便，但是缺点也是非常明显的，使用这个项目，会导致项目对这个框架依赖非常的严重，一旦这个框架出现问题，那么对项目来说影响非常大的。、

OKhttp：Android开发中是可以直接使用现成的api进行网络请求的。就是使用HttpClient,HttpUrlConnection进行操作。okhttp针对Java和Android程序，封装的一个高性能的http请求库，支持同步，异步，而且okhttp又封装了线程池，封装了数据转换，封装了参数的使用，错误处理等。API使用起来更加的方便。但是我们在项目中使用的时候仍然需要自己在做一层封装，这样才能使用的更加的顺手。

Volley：Volley是Google官方出的一套小而巧的异步请求库，该框架封装的扩展性很强，支持HttpClient、HttpUrlConnection， 甚至支持OkHttp，而且Volley里面也封装了ImageLoader，所以如果你愿意你甚至不需要使用图片加载框架，不过这块功能没有一些专门的图片加载框架强大，对于简单的需求可以使用，稍复杂点的需求还是需要用到专门的图片加载框架。Volley也有缺陷，比如不支持post大数据，所以不适合上传文件。不过Volley设计的初衷本身也就是为频繁的、数据量小的网络请求而生。

Retrofit：Retrofit是Square公司出品的默认基于OkHttp封装的一套RESTful网络请求框架，RESTful是目前流行的一套api设计的风格， 并不是标准。Retrofit的封装可以说是很强大，里面涉及到一堆的设计模式,可以通过注解直接配置请求，可以使用不同的http客户端，虽然默认是用http ，可以使用不同Json Converter 来序列化数据，同时提供对RxJava的支持，使用Retrofit + OkHttp + RxJava + Dagger2 可以说是目前比较潮的一套框架，但是需要有比较高的门槛。

Volley VS OkHttp

Volley的优势在于封装的更好，而使用OkHttp你需要有足够的能力再进行一次封装。而OkHttp的优势在于性能更高，因为 OkHttp基于NIO和Okio ，所以性能上要比 Volley更快。IO 和 NIO这两个都是Java中的概念，如果我从硬盘读取数据，第一种方式就是程序一直等，数据读完后才能继续操作这种是最简单的也叫阻塞式IO,还有一种是你读你的,程序接着往下执行，等数据处理完你再来通知我，然后再处理回调。而第二种就是 NIO 的方式，非阻塞式， 所以NIO当然要比IO的性能要好了,而 Okio是 Square 公司基于IO和NIO基础上做的一个更简单、高效处理数据流的一个库。理论上如果Volley和OkHttp对比的话，更倾向于使用 Volley，因为Volley内部同样支持使用OkHttp,这点OkHttp的性能优势就没了，  而且 Volley 本身封装的也更易用，扩展性更好些。

OkHttp VS Retrofit

毫无疑问，Retrofit 默认是基于 OkHttp 而做的封装，这点来说没有可比性，肯定首选 Retrofit。

Volley VS Retrofit

这两个库都做了不错的封装，但Retrofit解耦的更彻底,尤其Retrofit2.0出来，Jake对之前1.0设计不合理的地方做了大量重构， 职责更细分，而且Retrofit默认使用OkHttp,性能上也要比Volley占优势，再有如果你的项目如果采用了RxJava ，那更该使用  Retrofit 。所以这两个库相比，Retrofit更有优势，在能掌握两个框架的前提下该优先使用 Retrofit。但是Retrofit门槛要比Volley稍高些，要理解他的原理，各种用法，想彻底搞明白还是需要花些功夫的，如果你对它一知半解，那还是建议在商业项目使用Volley吧。

 

Java

1、线程中sleep和wait的区别

(1)这两个方法来自不同的类，sleep是来自Thread，wait是来自Object；

(2)sleep方法没有释放锁，而wait方法释放了锁。

(3)wait,notify,notifyAll只能在同步控制方法或者同步控制块里面使用，而sleep可以在任何地方使用。

 

2、Thread中的start()和run()方法有什么区别

start()方法是用来启动新创建的线程，而start()内部调用了run()方法，这和直接调用run()方法是不一样的，如果直接调用run()方法，

则和普通的方法没有什么区别。

 

 

3、关键字final和static是怎么使用的。

final:

1、final变量即为常量，只能赋值一次。

2、final方法不能被子类重写。

3、final类不能被继承。

 

static：

1、static变量：对于静态变量在内存中只有一个拷贝（节省内存），JVM只为静态分配一次内存，

在加载类的过程中完成静态变量的内存分配，可用类名直接访问（方便），当然也可以通过对象来访问（但是这是不推荐的）。

 

2、static代码块

 static代码块是类加载时，初始化自动执行的。

 

3、static方法

static方法可以直接通过类名调用，任何的实例也都可以调用，因此static方法中不能用this和super关键字，

不能直接访问所属类的实例变量和实例方法(就是不带static的成员变量和成员成员方法)，只能访问所属类的静态成员变量和成员方法。

 

4、String,StringBuffer,StringBuilder区别

1、三者在执行速度上：StringBuilder > StringBuffer > String (由于String是常量，不可改变，拼接时会重新创建新的对象)。

2、StringBuffer是线程安全的，StringBuilder是线程不安全的。（由于StringBuffer有缓冲区）

 

5、Java中重载和重写的区别：

1、重载：一个类中可以有多个相同方法名的，但是参数类型和个数都不一样。这是重载。

2、重写：子类继承父类，则子类可以通过实现父类中的方法，从而新的方法把父类旧的方法覆盖。

 

6、Http https区别

此处延伸：https的实现原理

 

1、https协议需要到ca申请证书，一般免费证书较少，因而需要一定费用。

2、http是超文本传输协议，信息是明文传输，https则是具有安全性的ssl加密传输协议。

3、http和https使用的是完全不同的连接方式，用的端口也不一样，前者是80，后者是443。

4、http的连接很简单，是无状态的；HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全。

 

https实现原理：

（1）客户使用https的URL访问Web服务器，要求与Web服务器建立SSL连接。

（2）Web服务器收到客户端请求后，会将网站的证书信息（证书中包含公钥）传送一份给客户端。

（3）客户端的浏览器与Web服务器开始协商SSL连接的安全等级，也就是信息加密的等级。

（4）客户端的浏览器根据双方同意的安全等级，建立会话密钥，然后利用网站的公钥将会话密钥加密，并传送给网站。

（5）Web服务器利用自己的私钥解密出会话密钥。

（6）Web服务器利用会话密钥加密与客户端之间的通信。

 

 

 

 

7、Http位于TCP/IP模型中的第几层？为什么说Http是可靠的数据传输协议？

tcp/ip的五层模型：

从下到上：物理层->数据链路层->网络层->传输层->应用层

其中tcp/ip位于模型中的网络层，处于同一层的还有ICMP（网络控制信息协议）。http位于模型中的应用层

由于tcp/ip是面向连接的可靠协议，而http是在传输层基于tcp/ip协议的，所以说http是可靠的数据传输协议。

 

8、HTTP链接的特点

HTTP连接最显著的特点是客户端发送的每次请求都需要服务器回送响应，在请求结束后，会主动释放连接。

从建立连接到关闭连接的过程称为“一次连接”。

 

9、TCP和UDP的区别

tcp是面向连接的，由于tcp连接需要三次握手，所以能够最低限度的降低风险，保证连接的可靠性。

udp 不是面向连接的，udp建立连接前不需要与对象建立连接，无论是发送还是接收，都没有发送确认信号。所以说udp是不可靠的。

由于udp不需要进行确认连接，使得UDP的开销更小，传输速率更高，所以实时行更好。

 

10、Socket建立网络连接的步骤

建立Socket连接至少需要一对套接字，其中一个运行与客户端--ClientSocket，一个运行于服务端--ServiceSocket

1、服务器监听：服务器端套接字并不定位具体的客户端套接字，而是处于等待连接的状态，实时监控网络状态，等待客户端的连接请求。

2、客户端请求：指客户端的套接字提出连接请求，要连接的目标是服务器端的套接字。注意：客户端的套接字必须描述他要连接的服务器的套接字，

指出服务器套接字的地址和端口号，然后就像服务器端套接字提出连接请求。

3、连接确认：当服务器端套接字监听到客户端套接字的连接请求时，就响应客户端套接字的请求，建立一个新的线程，把服务器端套接字的描述

发给客户端，一旦客户端确认了此描述，双方就正式建立连接。而服务端套接字则继续处于监听状态，继续接收其他客户端套接字的连接请求。

 

 

11、Tcp／IP三次握手，四次挥手



Android常见原理性面试专题

1.Handler机制和底层实现
2.Handler、Thread和HandlerThread的差别
1）Handler线程的消息通讯的桥梁，主要用来发送消息及处理消息。

2）Thread普通线程，如果需要有自己的消息队列，需要调用Looper.prepare()创建Looper实例，调用loop()去循环消息。

3）HandlerThread是一个带有Looper的线程，在HandleThread的run()方法中调用了Looper.prepare()创建了Looper实例，并调用Looper.loop()开启了Loop循环，循环从消息队列中获取消息并交由Handler处理。利用该线程的Looper创建Handler实例，此Handler的handleMessage()方法是运行在子线程中的。即Handler利用哪个线程的Looper创建的实例，它就和相应的线程绑定到一起，处理该线程上的消息，它的handleMessage()方法就是在那个线程中运行的，无参构造默认是主线程。HandlerThread提供了quit()/quitSafely()方法退出HandlerThread的消息循环，它们分别调用Looper的quit和quitSafely方法，quit会将消息队列中的所有消息移除，而quitSafely会将消息队列所有延迟消息移除，非延迟消息派发出去让Handler去处理。

HandlerThread适合处理本地IO读写操作（读写数据库或文件），因为本地IO操作耗时不长，对于单线程+异步队列不会产生较大阻塞，而网络操作相对比较耗时，容易阻塞后面的请求，因此HandlerThread不适合加入网络操作。

--handler发消息给子线程，looper怎么启动？
--关于Handler，在任何地方new Handler 都是什么线程下?
--ThreadLocal原理，实现及如何保证Local属性？
--请解释下在单线程模型中Message、Handler、Message Queue、Looper之间的关系
--请描述一下View事件传递分发机制
--Touch事件传递流程
--事件分发中的onTouch 和onTouchEvent 有什么区别，又该如何使用？
--View和ViewGroup分别有哪些事件分发相关的回调方法
--View刷新机制
--View绘制流程
--自定义控件原理
--自定义View如何提供获取View属性的接口？
--Android代码中实现WAP方式联网
--AsyncTask机制
--AsyncTask原理及不足
--如何取消AsyncTask？
--为什么不能在子线程更新UI？
--ANR产生的原因是什么？
--ANR定位和修正
--oom是什么？
（oom(Out Of Memory)内存溢出）
--什么情况导致oom？
--有什么解决方法可以避免OOM？
--Oom 是否可以try catch？为什么？
（可以，当）
--内存泄漏是什么？
内存泄露就是指该被GC垃圾回收的，但被一个生命周期比它长的对象仍然在引用它，导致无法回收，造成内存泄露，过多的内存泄露会导致OOM。

--什么情况导致内存泄漏？
1）非静态内部类、匿名内部类：非静态内部类、匿名内部类 都会持有外部类的一个引用，如果有一个静态变量引用了非静态内部类或者匿名内部类，导致非静态内部类或者匿名内部类的生命周期比外部类（Activity）长，就会导致外部类在该被回收的时候，无法被回收掉，引起内存泄露, 除非外部类被卸载。

解决办法：将非静态内部类、匿名内部类 改成静态内部类，或者直接抽离成一个外部类。 如果在静态内部类中，需要引用外部类对象，那么可以将这个引用封装在一个WeakReference中。
2）静态的View：当一个Activity经常启动，但是对应的View读取非常耗时，我们可以通过静态View变量来保持对该Activity的rootView引用。这样就可以不用每次启动Activity都去读取并渲染View了。但View attach到我们的Window上，就会持有一个Context(即Activity)的引用。而我们的View有事一个静态变量，所以导致Activity不被回收。

解决办法： 在使用静态View时，需要确保在资源回收时，将静态View detach掉。
3）Handler：在Activity中定义Handler对象，那么Handler持有Activty的引用。而每个Message对象是持有Handler的引用的（Message对象的target属性持有Handler引用），从而导致Message间接引用到了Activity。如果在Activty destroy之后，消息队列中还有Message对象，Activty是不会被回收的。
解决办法： 将Handler放入单独的类或者将Handler放入到静态内部类中（静态内部类不会持有外部类的引用）。如果想要在handler内部去调用所在的外部类Activity，可以在handler内部使用弱引用的方式指向所在Activity，在onDestory时，调用相应的方法移除回调和删除消息。
4）监听器（各种需要注册的Listener，Watcher等）：当我们需要使用系统服务时，比如执行某些后台任务、为硬件访问提供接口等等系统服务。我们需要把自己注册到服务的监听器中。然而，这会让服务持有 activity 的引用，如果程序员忘记在 activity 销毁时取消注册，那就会导致 activity 泄漏了。

解决办法：在onDestory中移除注册
5）. 资源对象没关闭造成内存泄漏：当我们打开资源时，一般都会使用缓存。比如读写文件资源、打开数据库资源、使用Bitmap资源等等。当我们不再使用时，应该关闭它们，使得缓存内存区域及时回收。

解决办法：使用try finally结合，在try块中打开资源，在finally中关闭资源
6）. 属性动画：在使用ValueAnimator或者ObjectAnimator时，如果没有及时做cancel取消动画，就可能造成内存泄露。因为在cancel方法里，最后调用了endAnimation(); ，在endAnimation里，有个AnimationHandler的单例，会持有属性动画对象的引用。

解决办法：在onDestory中调用动画的cancel方法

7）. RxJava：在使用RxJava时，如果在发布了一个订阅后，由于没有及时取消，导致Activity/Fragment无法销毁，导致的内存泄露。

解决办法：使用RxLifeCycle

--内存泄漏和内存溢出区别？
 

（1）内存泄漏

1）内存泄漏：指程序中已动态分配的堆内存由于某种原因未释放或无法释放，造成系统内存的浪费，导致程序运行速度减慢甚至系统奔溃等严重后果。

2）一次内存泄漏似乎不会有大的影响，但内存泄漏后堆积的结果就是内存溢出。

3）内存泄漏具有隐蔽性，积累性的特征，比其他内存非法访问错误更难检测。这是因为内存泄漏产生的原因是内存块未被释放，属于遗漏型缺陷而不是过错型缺陷。此外，内存泄漏不会直接产生可观察的错误，而是逐渐积累，降低系统的整体性性能。

4）如何有效的进行内存分配和释放，防止内存泄漏，是软件开发人员的关键问题，比如一个服务器应用软件要长时间服务多个客户端，若存在内存泄漏，则会逐渐堆积，导致一系列严重后果。

(2)内存溢出

指程序在申请内存时，没有足够的内存供申请者使用，或者说，给了你一块存储int类型数据的存储空间，但是你却存储long类型的数据，就会导致内存不够用，报错OOM，即出现内存溢出的错误。

--LruCache默认缓存大小
（4MB）
--ContentProvider的权限管理(解答：读写分离，权限控制-精确到表级，URL控制)
--如何通过广播拦截和abort一条短信？
--广播是否可以请求网络？
--广播引起anr的时间限制是多少？
--计算一个view的嵌套层级
--Activity栈
--Android线程有没有上限？
--线程池有没有上限？
--ListView重用的是什么？
--Android为什么引入Parcelable？
--有没有尝试简化Parcelable的使用？


Android面试常问基础知识点

1、四大组件是什么
1）Activity：用户可操作的可视化界面，为用户提供一个完成操作指令的窗口。一个Activity通常是一个单独的屏幕，Activity通过Intent来进行通信。Android中会维持一个Activity Stack，当一个新Activity创建时，它就会放到栈顶，这个Activity就处于运行状态。

2）Service：服务，运行在手机后台，适合执行不需和用户交互且还需长期运行的任务。

3）ContentProvider：内容提供者，使一个应用程序的指定数据集提供给其他应用程序，其他应用可通过ContentResolver类从该内容提供者中获取或存入数据。它提供了一种跨进程数据共享的方式，当数据被修改后，ContentResolver接口的notifyChange函数通知那些注册监控特定URI的ContentObserver对象。

如果ContentProvider和调用者在同一进程中，ContentProvider的方法(query/insert/update/delete等)和调用者在同一线程中；如果ContentProvider和调用者不在同一进程，ContentProvider方法会运行在它自身进程的一个Binder线程中。

4）Broadcast Receiver: 广播接收者，运用在应用程序间传输信息，可以使用广播接收器来让应用对一个外部事件做出响应。

2、四大组件的生命周期和简单用法
1）Activity：onCreate()->onStart()->onResume()->onPause()->onStop()->onDestory()

onCreate()：为Activity设置布局，此时界面还不可见；

onStart(): Activity可见但还不能与用户交互，不能获得焦点

onRestart(): 重新启动Activity时被回调

onResume(): Activity可见且可与用户进行交互

onPause(): 当前Activity暂停，不可与用户交互，但还可见。在新Activity启动前被系统调用保存现有的Activity中的持久数据、停止动画等。

onStop(): 当Activity被新的Activity覆盖不可见时被系统调用

onDestory(): 当Activity被系统销毁杀掉或是由于内存不足时调用

2）Service

a) onBind方式绑定的：onCreate->onBind->onUnBind->onDestory（不管调用bindService几次，onCreate只会调用一次，onStart不会被调用，建立连接后，service会一直运行，直到调用unBindService或是之前调用的bindService的Context不存在了，系统会自动停止Service,对应的onDestory会被调用）

b) startService启动的：onCreate->onStartCommand->onDestory(start多次，onCreate只会被调用一次，onStart会调用多次，该service会在后台运行，直至被调用stopService或是stopSelf)

c) 又被启动又被绑定的服务，不管如何调用onCreate()只被调用一次，startService调用多少次，onStart就会被调用多少次，而unbindService不会停止服务，必须调用stopService或是stopSelf来停止服务。必须unbindService和stopService(stopSelf）同时都调用了才会停止服务。

3）BroadcastReceiver

a) 动态注册：存活周期是在Context.registerReceiver和Context.unregisterReceiver之间，BroadcastReceiver每次收到广播都是使用注册传入的对象处理的。

b) 静态注册：进程在的情况下，receiver会正常收到广播，调用onReceive方法；生命周期只存活在onReceive函数中，此方法结束，BroadcastReceiver就销毁了。onReceive()只有十几秒存活时间，在onReceive()内操作超过10S，就会报ANR。

进程不存在的情况，广播相应的进程会被拉活，Application.onCreate会被调用，再调用onReceive。

4）ContentProvider：应该和应用的生命周期一样，它属于系统应用，应用启动时，它会跟着初始化，应用关闭或被杀，它会跟着结束。

3、Activity之间的通信方式
1）通过Intent方式传递参数跳转

2）通过广播方式

3）通过接口回调方式

4）借助类的静态变量或全局变量

5）借助SharedPreference或是外部存储，如数据库或本地文件

4、Activity各种情况下的生命周期
1) 两个Activity(A->B)切换(B正常的Activity)的生命周期：onPause(A)->onCreate(B)->onStart(B)->onResume(B)->oStop(A)

这时如果按回退键回退到A  onPause(B)->onRestart(A)->onStart(A)->onResume(A)->oStop(B)

如果在切换到B后调用了A.finish()，则会走到onDestory(A)，这时点回退键会退出应用

2) 两个Activity(A->B)切换(B透明主题的Activity或是Dialog风格的Acivity)的生命周期：onPause(A)->onCreate(B)->onStart(B)->onResume(B)

这时如果回退到A  onPause(B)->onResume(A)->oStop(B)->onDestory(B)

3) Activity(A)启动后点击Home键再回到应用的生命周期：onPause(A)->oStop(A)->onRestart(A)->onStart(A)->onResume(A)

5、横竖屏切换的时候，Activity 各种情况下的生命周期
1）切换横屏时：onSaveInstanceState->onPause->onStop->onDestory->onCreate->onStart->onRestoreInstanceState->onResume

2) 切换竖屏时：会打印两次相同的log   

onSaveInstanceState->onPause->onStop->onDestory->onCreate->onStart->onRestoreInstanceState->onResume->onSaveInstanceState->onPause->onStop->onDestory->onCreate->onStart->onRestoreInstanceState->onResume

3) 如果在AndroidMainfest.xml中修改该Activity的属性，添加android:configChanges="orientation"

横竖屏切换，打印的log一样，同1)

4) 如果AndroidMainfest.xml中该Activity中的android:configChanges="orientation|keyboardHidden"，则只会打印

onConfigurationChanged->

6、Activity与Fragment之间生命周期比较
Fragment生命周期：onAttach->onCreate->onCreateView->onActivityCreated->onStart->onResume->onPause->onStop->onDestoryView->onDestory->onDetach

切换到该Fragment：onAttach->onCreate->onCreateView->onActivityCreated->onStart->onResume

按下Power键：onPause->onSaveInstanceState->onStop

点亮屏幕解锁：onStart->onRestoreInstanceState->onResume

切换到其他Fragment: onPause->onStop->onDestoryView

切回到该Fragment: onCreateView->onActivityCreated->onStart->onResume

退出应用：onPause->onStop->onDestoryView->onDestory->onDetach

7、Activity上有Dialog的时候按Home键时的生命周期
AlertDialog并不会影响Activity的生命周期，按Home键后才会使Activity走onPause->onStop，AlertDialog只是一个组件，并不会使Activity进入后台。

8、两个Activity 之间跳转时必然会执行的是哪几个方法？
前一个Activity的onPause，后一个Activity的onResume

9、前台切换到后台，然后再回到前台，Activity生命周期回调方法。弹出Dialog，生命值周期回调方法。
1）前台切换到后台，会执行onPause->onStop，再回到前台，会执行onRestart->onStart->onResume

2) 弹出Dialog，并不会影响Activity生命周期

10、Activity的四种启动模式对比
1）standard：标准启动模式（默认），每启动一次Activity，都会创建一个实例，即使从ActivityA startActivity ActivityA,也会再次创建A的实例放于栈顶，当回退时，回到上一个ActivityA的实例。

2) singleTop：栈顶复用模式，每次启动Activity，如果待启动的Activity位于栈顶，则不会重新创建Activity的实例，即不会走onCreate->onStart，会直接进入Activity的onPause->onNewIntent->onResume方法

3) singleInstance: 单一实例模式，整个手机操作系统里只有一个该Activity实例存在，没有其他Actvity,后续请求均不会创建新的Activity。若task中存在实例，执行实例的onNewIntent()。应用场景：闹钟、浏览器、电话

4) singleTask：栈内复用，启动的Activity如果在指定的taskAffinity的task栈中存在相应的实例，则会把它上面的Activity都出栈，直到当前Activity实例位于栈顶，执行相应的onNewIntent()方法。如果指定的task不存在，创建指定的taskAffinity的task,taskAffinity的作用，进入指写taskAffinity的task,如果指定的task存在，将task移到前台，如果指定的task不存在，创建指定的taskAffinity的task. 应用场景：应用的主页面

11、Activity状态保存于恢复
Activity被主动回收时，如按下Back键，系统不会保存它的状态，只有被动回收时，虽然这个Activity实例已被销毁，但系统在新建一个Activity实例时，会带上先前被回收Activity的信息。在当前Activity被销毁前调用onSaveInstanceState(onPause和onStop之间保存)，重新创建Activity后会在onCreate后调用onRestoreInstanceState（onStart和onResume之间被调用），它们的参数Bundle用来数据保存和读取的。

保存View状态有两个前提：View的子类必须实现了onSaveInstanceState; 必须要设定Id，这个ID作为Bundle的Key

12、fragment各种情况下的生命周期
正常情况下的生命周期：onAttach->onCreate->onCreateView->onActivityCreated->onStart->onResume->onPause->onStop->onDestoryView->onDestory->onDetach

1)Fragment在Activity中replace  onPause(旧)->onAttach->onCreate->onCreateView->onActivityCreated->onStart->onResume->onStop(旧)->onDestoryView(旧)

如果添加到backStack中，调用remove()方法fragment的方法会走到onDestoryView，但不会执行onDetach()，即fragment本身的实例是存在的，成员变量也存在，但是view被销毁了。如果新替换的Fragment已在BackStack中，则不会执行onAttach->onCreate

13、Fragment状态保存onSaveInstanceState是哪个类的方法，在什么情况下使用？
在对应的FragmentActivity.onSaveInstanceState方法会调用FragmentController.saveAllState，其中会对mActive中各个Fragment的实例状态和View状态分别进行保存.当Activity在做状态保存和恢复的时候, 在它其中的fragment自然也需要做状态保存和恢复.


14、Fragment.startActivityForResult是和FragmentActivity的startActivityForResult？
如果希望在Fragment的onActivityResult接收数据，就要调用Fragment.startActivityForResult， 而不是Fragment.getActivity().startActivityForResult。Fragment.startActivityForResult->FragmentActivitymHost.HostCallbacks.onStartActivityFromFragment->FragmentActivity.startActivityFromFragment。如果request=-1则直接调用FragmentActivity.startActivityForResult，它会重新计算requestCode，使其大于0xfffff。


15、如何实现Fragment的滑动？
ViewPager+FragmentPagerAdapter+List<Fragment>

16、fragment之间传递数据的方式？
1)在相应的fragment中编写方法，在需要回调的fragment里获取对应的Fragment实例，调用相应的方法；

2）采用接口回调的方式进行数据传递；

a) 在Fragment1中创建一个接口及接口对应的set方法; b) 在Fragment1中调用接口的方法；c)在Fragment2中实现该接口；

3）利用第三方开源框架EventBus

17、service和activity怎么进行数据交互？
1）通过bindService启动服务，可以在ServiceConnection的onServiceConnected中获取到Service的实例，这样就可以调用service的方法，如果service想调用activity的方法，可以在service中定义接口类及相应的set方法，在activity中实现相应的接口，这样service就可以回调接口言法；

2）通过广播方式

18、说说ContentProvider、ContentResolver、ContentObserver 之间的关系
ContentProvider实现各个应用程序间数据共享，用来提供内容给别的应用操作。如联系人应用中就使用了ContentProvider，可以在自己应用中读取和修改联系人信息，不过需要获取相应的权限。它也只是一个中间件，真正的数据源是文件或SQLite等。

ContentResolver内容解析者，用于获取内容提供者提供的数据，通过ContentResolver.notifyChange(uri)发出消息

ContentObserver内容监听者，可以监听数据的改变状态，观察特定Uri引起的数据库变化，继而做一些相应的处理，类似于数据库中的触发器，当ContentObserver所观察的Uri发生变化时，便会触发它。

19、请描述一下广播BroadcastReceiver的理解
BroadcastReceiver是一种全局监听器，用来实现系统中不同组件之间的通信。有时候也会用来作为传输少量而且发送频率低的数据，但是如果数据的发送频率比较高或者数量比较大就不建议用广播接收者来接收了，因为这样的效率很不好，因为BroadcastReceiver接收数据的开销还是比较大的。

20、广播的分类
1）普通广播：完全异步的，可以在同一时刻（逻辑上）被所有接收者接收到，消息传递的效率比较高，并且无法中断广播的传播。

2）有序广播：发送有序广播后，广播接收者将按预先声明的优先级依次接收Broadcast。优先级高的优先接收到广播，而在其onReceiver()执行过程中，广播不会传播到下一个接收者，此时当前的广播接收者可以abortBroadcast()来终止广播继续向下传播，也可以将intent中的数据进行修改设置，然后将其传播到下一个广播接收者。 sendOrderedBroadcast(intent, null);//发送有序广播

3）粘性广播：sendStickyBroadcast()来发送该类型的广播信息，这种的广播的最大特点是，当粘性广播发送后，最后的一个粘性广播会滞留在操作系统中。如果在粘性广播发送后的一段时间里，如果有新的符合广播的动态注册的广播接收者注册，将会收到这个广播消息，虽然这个广播是在广播接收者注册之前发送的，另外一点，对于静态注册的广播接收者来说，这个等同于普通广播。

21、广播使用的方式和场景
1）App全局监听：在AndroidManifest中静态注册的广播接收器，一般我们在收到该消息后，需要做一些相应的动作，而这些动作与当前App的组件，比如Activity或者Service的是否运行无关，比如我们在集成第三方Push SDK时，一般都会添加一个静态注册的BroadcastReceiver来监听Push消息，当有Push消息过来时，会在后台做一些网络请求或者发送通知等等。

2）组件局部监听：这种主要是在Activity或者Service中使用registerReceiver()动态注册的广播接收器，因为当我们收到一些特定的消息，比如网络连接发生变化时，我们可能需要在当前Activity页面给用户一些UI上的提示，或者将Service中的网络请求任务暂停。所以这种动态注册的广播接收器适合特定组件的特定消息处理。

22、在manifest 和代码中如何注册和使用BroadcastReceiver?
1）mainfest中注册:静态注册的广播接收者就是一个常驻在系统中的全局监听器，也就是说如果你应用中配置了一个静态的BroadcastReceiver，而且你安装了应用而无论应用是否处于运行状态，广播接收者都是已经常驻在系统中了。

<receiver android:name=".MyBroadcastReceiver">
    <intent-filter>
        <action android:name="com.smilexie.test.intent.mybroadcastreceiver"/>
    </intent-filter>
</receiver>
2) 动态注册:动态注册的广播接收者只有执行了registerReceiver(receiver, filter)才会开始监听广播消息，并对广播消息作为相应的处理。

IntentFilter fiter = new IntentFilter("com.smilexie.test.intent.mybroadcastreceiver");
MyBroadcastReceiver receiver = new MyBroadcastReceiver();
registerReceiver(receiver, filter);
 
//撤销广播接受者的动态注册
unregisterReceiver(receiver);
23、本地广播和全局广播有什么差别？
1）LocalBroadcastReceiver仅在自己的应用内发送接收广播，也就是只有自己的应用能收到，数据更加安全。广播只在这个程序里，而且效率更高。只能动态注册，在发送和注册的时候采用LocalBroadcastManager的sendBroadcast方法和registerReceiver方法。

2）全局广播：发送的广播事件可被其他应用程序获取，也能响应其他应用程序发送的广播事件（可以通过 exported–是否监听其他应用程序发送的广播 在清单文件中控制） 全局广播既可以动态注册，也可以静态注册。

24、AlertDialog,popupWindow,Activity区别
（1）Popupwindow在显示之前一定要设置宽高，Dialog无此限制。

（2）Popupwindow默认不会响应物理键盘的back，除非显示设置了popup.setFocusable(true);而在点击back的时候，Dialog会消失。

（3）Popupwindow不会给页面其他的部分添加蒙层，而Dialog会。

（4）Popupwindow没有标题，Dialog默认有标题，可以通过dialog.requestWindowFeature(Window.FEATURE_NO_TITLE);取消标题

（5）二者显示的时候都要设置Gravity。如果不设置，Dialog默认是Gravity.CENTER。

（6）二者都有默认的背景，都可以通过setBackgroundDrawable(new ColorDrawable(android.R.color.transparent));去掉。
（7）Popupwindow弹出后，取得了用户操作的响应处理权限，使得其他UI控件不被触发。而AlertDialog弹出后，点击背景，AlertDialog会消失。

25、Application 和 Activity 的 Context 对象的区别
1）Application Context是伴随应用生命周期；不可以showDialog, startActivity, LayoutInflation

可以startService\BindService\sendBroadcast\registerBroadcast\load Resource values

2）Activity Context指生命周期只与当前Activity有关，而Activity Context这些操作都可以，即凡是跟UI相关的，都得用Activity做为Context来处理。

一个应用Context的数量=Activity数量+Service数量+1（Application数量）

--Android属性动画特性
--如何导入外部数据库?
--LinearLayout、RelativeLayout、FrameLayout的特性及对比，并介绍使用场景。
--谈谈对接口与回调的理解
--回调的原理
--写一个回调demo
--介绍下SurfView
--RecycleView的使用
--序列化的作用，以及Android两种序列化的区别
--差值器
实现Interpolator接口，设置值的变化趋势，SDK中包含了匀速插值器、加速插值器、减速插值器、先加速再减速、弹
--估值器
实现TypeEvaluatior接口

--Android中数据存储方式


Android面试帮助篇
1、要做一个尽可能流畅的ListView，你平时在工作中如何进行优化的？①Item布局，层级越少越好，使用hierarchyview工具查看优化。

②复用convertView

③使用ViewHolder

④item中有图片时，异步加载

⑤快速滑动时，不加载图片

⑥item中有图片时，应对图片进行适当压缩

⑦实现数据的分页加载

2、对于Android 的安全问题，你知道多少

①错误导出组件

② 参数校验不严

③WebView引入各种安全问题,webview中的js注入

④不混淆、不防二次打包

⑤明文存储关键信息

⑦ 错误使用HTTPS

⑧山寨加密方法

⑨滥用权限、内存泄露、使用debug签名

3、如何缩减APK包大小？

代码

保持良好的编程习惯，不要重复或者不用的代码，谨慎添加libs，移除使用不到的libs。

使用proguard混淆代码，它会对不用的代码做优化，并且混淆后也能够减少安装包的大小。

native code的部分，大多数情况下只需要支持armabi与x86的架构即可。如果非必须，可以考虑拿掉x86的部分。

资源

使用Lint工具查找没有使用到的资源。去除不使用的图片，String，XML等等。 assets目录下的资源请确保没有用不上的文件。

生成APK的时候，aapt工具本身会对png做优化，但是在此之前还可以使用其他工具如tinypng对图片进行进一步的压缩预处理。

jpeg还是png，根据需要做选择，在某些时候jpeg可以减少图片的体积。 对于9.png的图片，可拉伸区域尽量切小，另外可以通过使用9.png拉伸达到大图效果的时候尽量不要使用整张大图。

策略

有选择性的提供hdpi，xhdpi，xxhdpi的图片资源。建议优先提供xhdpi的图片，对于mdpi，ldpi与xxxhdpi根据需要提供有差异的部分即可。

尽可能的重用已有的图片资源。例如对称的图片，只需要提供一张，另外一张图片可以通过代码旋转的方式实现。

能用代码绘制实现的功能，尽量不要使用大量的图片。例如减少使用多张图片组成animate-list的AnimationDrawable，这种方式提供了多张图片很占空间。

4、Android与服务器交互的方式中的对称加密和非对称加密是什么?对称加密，就是加密和解密数据都是使用同一个key，这方面的算法有DES。

非对称加密，加密和解密是使用不同的key。发送数据之前要先和服务端约定生成公钥和私钥，使用公钥加密的数据可以用私钥解密，反之。这方面的算法有RSA。ssh 和 ssl都是典型的非对称加密。

5、设备横竖屏切换的时候，接下来会发生什么？

1、不设置Activity的android:configChanges时，切屏会重新调用各个生命周期，切横屏时会执行一次，切竖屏时会执行两次

2、设置Activity的android:configChanges=”orientation”时，切屏还是会重新调用各个生命周期，切横、竖屏时只会执行一次

3、设置Activity的android:configChanges=”orientation|keyboardHidden”时，切屏不会重新调用各个生命周期，只会执行onConfigurationChanged方法

6、Android启动Service的两种方式是什么? 它们的适用情况是什么?

如果后台服务开始后基本可以独立运行的话，可以用startService。音乐播放器就可以这样用。它们会一直运行直到你调用 stopSelf或者stopService。你可以通过发送Intent或者接收Intent来与正在运行的后台服务通信，但大部分时间，你只是启动服务并让它独立运行。如果你需要与后台服务通过一个持续的连接来比较频繁地通信，建议使用bind()。比如你需要定位服务不停地把更新后的地理位置传给UI。Binder比Intent开发起来复杂一些，但如果真的需要，你也只能使用它。

startService：生命周期与调用者不同。启动后若调用者未调用stopService而直接退出，Service仍会运行

bindService：生命周期与调用者绑定，调用者一旦退出，Service就会调用unBind->onDestroy

7、谈谈你对Android中Context的理解?

Context:包含上下文信息(外部值) 的一个参数. Android 中的 Context 分三种,Application Context ,Activity Context ,Service Context.

它描述的是一个应用程序环境的信息，通过它我们可以获取应用程序的资源和类，也包括一些应用级别操作，例如：启动一个Activity，发送广播，接受Intent信息等

8、Service的onCreate回调在UI线程中吗？

Service生命周期的各个回调和其他的应用组件一样，是跑在主线程中，会影响到你的UI操作或者阻塞主线程中的其他事情

9、请介绍下AsyncTask的内部实现，适用的场景是？

AsyncTask内部也是Handler机制来完成的，只不过Android提供了执行框架来提供线程池来执行相应地任务，因为线程池的大小问题，所以AsyncTask只应该用来执行耗时时间较短的任务，比如HTTP请求，大规模的下载和数据库的更改不适用于AsyncTask，因为会导致线程池堵塞，没有线程来执行其他的任务，导致的情形是会发生AsyncTask根本执行不了的问题。

10、谈谈你对binder机制的理解?

binder是一种IPC机制,进程间通讯的一种工具.

Java层可以利用aidl工具来实现相应的接口.

11、Android中进程间通信有哪些实现方式？

Intent，Binder（AIDL），Messenger，BroadcastReceiver

12、介绍下实现一个自定义view的基本流程

1、自定义View的属性 编写attr.xml文件

2、在layout布局文件中引用，同时引用命名空间

3、在View的构造方法中获得我们自定义的属性 ，在自定义控件中进行读取（构造方法拿到attr.xml文件值）

4、重写onMesure

5、重写onDraw

13、Android中touch事件的传递机制是怎样的?

1、Touch事件传递的相关API有dispatchTouchEvent、onTouchEvent、onInterceptTouchEvent

2、Touch事件相关的类有View、ViewGroup、Activity

3、Touch事件会被封装成MotionEvent对象，该对象封装了手势按下、移动、松开等动作

4、Touch事件通常从Activity#dispatchTouchEvent发出，只要没有被消费，会一直往下传递，到最底层的View。

5、如果Touch事件传递到的每个View都不消费事件，那么Touch事件会反向向上传递,最终交由Activity#onTouchEvent处理.

6、onInterceptTouchEvent为ViewGroup特有，可以拦截事件.

7、Down事件到来时，如果一个View没有消费该事件，那么后续的MOVE/UP事件都不会再给它

14、Android多线程的实现方式有哪些?

Thread & AsyncTask

Thread 可以与Loop 和 Handler 共用建立消息处理队列

AsyncTask 可以作为线程池并行处理多任务

15、Android开发中何时使用多进程？使用多进程的好处是什么？

要想知道如何使用多进程，先要知道Android里的多进程概念。一般情况下，一个应用程序就是一个进程，这个进程名称就是应用程序包名。我们知道进程是系统分配资源和调度的基本单位，所以每个进程都有自己独立的资源和内存空间，别的进程是不能任意访问其他进程的内存和资源的。

那如何让自己的应用拥有多个进程？

很简单，我们的四大组件在AndroidManifest文件中注册的时候，有个属性是android:process，

1、这里可以指定组件的所处的进程。默认就是应用的主进程。指定为别的进程之后，系统在启动这个组件的时候，就先创建（如果还没创建的话）这个进程，然后再创建该组件。你可以重载Application类的onCreate方法，打印出它的进程名称，就可以清楚的看见了。再设置android:process属性时候，有个地方需要注意：如果是android:process=”:deamon”，以:开头的名字，则表示这是一个应用程序的私有进程，否则它是一个全局进程。私有进程的进程名称是会在冒号前自动加上包名，而全局进程则不会。一般我们都是有私有进程，很少使用全局进程。他们的具体区别不知道有没有谁能补充一下。

2、使用多进程显而易见的好处就是分担主进程的内存压力。我们的应用越做越大，内存越来越多，将一些独立的组件放到不同的进程，它就不占用主进程的内存空间了。当然还有其他好处，有心人会发现Android后台进程里有很多应用是多个进程的，因为它们要常驻后台，特别是即时通讯或者社交应用，不过现在多进程已经被用烂了。典型用法是在启动一个不可见的轻量级私有进程，在后台收发消息，或者做一些耗时的事情，或者开机启动这个进程，然后做监听等。还有就是防止主进程被杀守护进程，守护进程和主进程之间相互监视，有一方被杀就重新启动它。应该还有还有其他好处，这里就不多说了。

3、坏处的话，多占用了系统的空间，大家都这么用的话系统内存很容易占满而导致卡顿。消耗用户的电量。应用程序架构会变复杂，应为要处理多进程之间的通信。这里又是另外一个问题了。

16、ANR是什么？怎样避免和解决ANR?

ANR:Application Not Responding，即应用无响应

ANR一般有三种类型：

1：KeyDispatchTimeout(5 seconds) –主要类型

按键或触摸事件在特定时间内无响应

2：BroadcastTimeout(10 seconds)

BroadcastReceiver在特定时间内无法处理完成

3：ServiceTimeout(20 seconds) –小概率类型

Service在特定的时间内无法处理完成

超时的原因一般有两种：

(1)当前的事件没有机会得到处理（UI线程正在处理前一个事件没有及时完成或者looper被某种原因阻塞住）

(2)当前的事件正在处理，但没有及时完成

UI线程尽量只做跟UI相关的工作，耗时的工作（数据库操作，I/O，连接网络或者其他可能阻碍UI线程的操作）放入单独的线程处理，尽量用Handler来处理UI thread和thread之间的交互。

UI线程主要包括如下：

Activity:onCreate(), onResume(), onDestroy(), onKeyDown(), onClick()

AsyncTask: onPreExecute(), onProgressUpdate(), onPostExecute(), onCancel()

Mainthread handler: handleMessage(), post(runnable r)

other

17、Android下解决滑动冲突的常见思路是什么?

相关的滑动组件 重写onInterceptTouchEvent，然后判断根据xy值，来决定是否要拦截当前操作

18、如何把一个应用设置为系统应用？

成为系统应用，首先要在 对应设备的 Android 源码 SDK 下编译，编译好之后：

此 Android 设备是 Debug 版本，并且已经 root，直接将此 apk 用 adb 工具 push 到 system/app 或 system/priv-app 下即可。

如果非 root 设备，需要编译后重新烧写设备镜像即可。

有些权限(如 WRITE_SECURE_SETTINGS )，是不开放给第三方应用的，只能在对应设备源码中编译然后作为系统 app 使用。

19、Android内存泄露研究

Android内存泄漏指的是进程中某些对象（垃圾对象）已经没有使用价值了，但是它们却可以直接或间接地引用到gc roots导致无法被GC回收。无用的对象占据着内存空间，使得实际可使用内存变小，形象地说法就是内存泄漏了。

场景

类的静态变量持有大数据对象

静态变量长期维持到大数据对象的引用，阻止垃圾回收。

非静态内部类的静态实例

非静态内部类会维持一个到外部类实例的引用，如果非静态内部类的实例是静态的，就会间接长期维持着外部类的引用，阻止被回收掉。

资源对象未关闭

资源性对象如Cursor、File、Socket，应该在使用后及时关闭。未在finally中关闭，会导致异常情况下资源对象未被释放的隐患。

注册对象未反注册

未反注册会导致观察者列表里维持着对象的引用，阻止垃圾回收。

Handler临时性内存泄露

Handler通过发送Message与主线程交互，Message发出之后是存储在MessageQueue中的，有些Message也不是马上就被处理的。在Message中存在一个 target，是Handler的一个引用，如果Message在Queue中存在的时间越长，就会导致Handler无法被回收。如果Handler是非静态的，则会导致Activity或者Service不会被回收。

由于AsyncTask内部也是Handler机制，同样存在内存泄漏的风险。

此种内存泄露，一般是临时性的。

20、内存泄露检测有什么好方法？

检测：

1、DDMS Heap发现内存泄露

dataObject totalSize的大小，是否稳定在一个范围内，如果操作程序，不断增加，说明内存泄露

2、使用Heap Tool进行内存快照前后对比

BlankActivity手动触发GC进行前后对比，对象是否被及时回收

定位：

1、MAT插件打开.hprof具体定位内存泄露：

查看histogram项，选中某一个对象，查看它的GC引用链，因为存在GC引用链的，说明无法回收

2、AndroidStudio的Allocation Tracker：

观测到期间的内存分配，哪些对象被创建，什么时候创建，从而准确定位


2019Android View总结
1.View的滑动方式
a.layout(left,top,right,bottom):通过修改View四个方向的属性值来修改View的坐标，从而滑动View
b.offsetLeftAndRight() offsetTopAndBottom():指定偏移量滑动view
c.LayoutParams,改变布局参数：layoutParams中保存了view的布局参数，可以通过修改布局参数的方式滑动view
d.通过动画来移动view：注意安卓的平移动画不能改变view的位置参数，属性动画可以
e.scrollTo/scrollBy:注意移动的是view的内容，scrollBy(50,50)你会看到屏幕上的内容向屏幕的左上角移动了，这是参考对象不同导致的，你可以看作是它移动的是手机屏幕，手机屏幕向右下角移动，那么屏幕上的内容就像左上角移动了
f.scroller:scroller需要配置computeScroll方法实现view的滑动，scroller本身并不会滑动view，它的作用可以看作一个插值器，它会计算当前时间点view应该滑动到的距离，然后view不断的重绘，不断的调用computeScroll方法，这个方法是个空方法，所以我们重写这个方法，在这个方法中不断的从scroller中获取当前view的位置，调用scrollTo方法实现滑动的效果
2.View的事件分发机制
点击事件产生后，首先传递给Activity的dispatchTouchEvent方法，通过PhoneWindow传递给DecorView,然后再传递给根ViewGroup,进入ViewGroup的dispatchTouchEvent方法，执行onInterceptTouchEvent方法判断是否拦截，再不拦截的情况下，此时会遍历ViewGroup的子元素，进入子View的dispatchToucnEvent方法，如果子view设置了onTouchListener,就执行onTouch方法，并根据onTouch的返回值为true还是false来决定是否执行onTouchEvent方法，如果是false则继续执行onTouchEvent，在onTouchEvent的Action Up事件中判断，如果设置了onClickListener ,就执行onClick方法。
3.View的加载流程
View随着Activity的创建而加载，startActivity启动一个Activity时，在ActivityThread的handleLaunchActivity方法中会执行Activity的onCreate方法，这个时候会调用setContentView加载布局创建出DecorView并将我们的layout加载到DecorView中，当执行到handleResumeActivity时，Activity的onResume方法被调用，然后WindowManager会将DecorView设置给ViewRootImpl,这样，DecorView就被加载到Window中了，此时界面还没有显示出来，还需要经过View的measure，layout和draw方法，才能完成View的工作流程。我们需要知道View的绘制是由ViewRoot来负责的，每一个DecorView都有一个与之关联的ViewRoot,这种关联关系是由WindowManager维护的，将DecorView和ViewRoot关联之后，ViewRootImpl的requestLayout会被调用以完成初步布局，通过scheduleTraversals方法向主线程发送消息请求遍历，最终调用ViewRootImpl的performTraversals方法，这个方法会执行View的measure layout 和draw流程
4.View的measure layout 和 draw流程
在上边的分析中我们知道，View绘制流程的入口在ViewRootImpl的performTraversals方法，在方法中首先调用performMeasure方法，传入一个childWidthMeasureSpec和childHeightMeasureSpec参数，这两个参数代表的是DecorView的MeasureSpec值，这个MeasureSpec值由窗口的尺寸和DecorView的LayoutParams决定，最终调用View的measure方法进入测量流程
measure：
View的measure过程由ViewGroup传递而来，在调用View.measure方法之前，会首先根据View自身的LayoutParams和父布局的MeasureSpec确定子view的MeasureSpec，然后将view宽高对应的measureSpec传递到measure方法中，那么子view的MeasureSpec获取规则是怎样的？分几种情况进行说明
1.父布局是EXACTLY模式：
    a.子view宽或高是个确定值，那么子view的size就是这个确定值，mode是EXACTLY（是不是说子view宽高可以超过父view？见下一个）
    b.子view宽或高设置为match_parent,那么子view的size就是占满父容器剩余空间，模式就是EXACTLY
    c.子view宽或高设置为wrap_content,那么子view的size就是占满父容器剩余空间，不能超过父容器大小，模式就是AT_MOST
2.父布局是AT_MOST模式：
a.子view宽或高是个确定值，那么子view的size就是这个确定值，mode是EXACTLY
   b.子view宽或高设置为match_parent,那么子view的size就是占满父容器剩余空间,不能超过父容器大小，模式就是AT_MOST
   c.子view宽或高设置为wrap_content,那么子view的size就是占满父容器剩余空间，不能超过父容器大小，模式就是AT_MOST
3.父布局是UNSPECIFIED模式：
a.子view宽或高是个确定值，那么子view的size就是这个确定值，mode是EXACTLY
b.子view宽或高设置为match_parent,那么子view的size就是0，模式就是UNSPECIFIED
c.子view宽或高设置为wrap_content,那么子view的size就是0，模式就是UNSPECIFIED
获取到宽高的MeasureSpec后，传入view的measure方法中来确定view的宽高，这个时候还要分情况
1.当MeasureSpec的mode是UNSPECIFIED,此时view的宽或者高要看view有没有设置背景，如果没有设置背景，就返回设置的minWidth或minHeight,这两个值如果没有设置默认就是0，如果view设置了背景，就取minWidth或minHeight和背景这个drawable固有宽或者高中的最大值返回
2.当MeasureSpec的mode是AT_MOST和EXACTLY，此时view的宽高都返回从MeasureSpec中获取到的size值，这个值的确定见上边的分析。因此如果要通过继承view实现自定义view，一定要重写onMeasure方法对wrap_conten属性做处理，否则，他的match_parent和wrap_content属性效果就是一样的
layout:
layout方法的作用是用来确定view本身的位置，onLayout方法用来确定所有子元素的位置，当ViewGroup的位置确定之后，它在onLayout中会遍历所有的子元素并调用其layout方法，在子元素的layout方法中onLayout方法又会被调用。layout方法的流程是，首先通过setFrame方法确定view四个顶点的位置，然后view在父容器中的位置也就确定了，接着会调用onLayout方法，确定子元素的位置，onLayout是个空方法，需要继承者去实现。
getMeasuredHeight和getHeight方法有什么区别？getMeasuredHeight（测量高度）形成于view的measure过程，getHeight（最终高度）形成于layout过程，在有些情况下，view需要measure多次才能确定测量宽高，在前几次的测量过程中，得出的测量宽高有可能和最终宽高不一致，但是最终来说，还是会相同，有一种情况会导致两者值不一样，如下，此代码会导致view的最终宽高比测量宽高大100px
public void layout(int l,int t,int r, int b){
    super.layout(l,t,r+100,b+100);}
draw:
View的绘制过程遵循如下几步：
a.绘制背景 background.draw(canvas)
b.绘制自己（onDraw）
c.绘制children（dispatchDraw）
d.绘制装饰（onDrawScrollBars）
View绘制过程的传递是通过dispatchDraw来实现的，它会遍历所有的子元素的draw方法，如此draw事件就一层一层的传递下去了
ps：view有一个特殊的方法setWillNotDraw，如果一个view不需要绘制内容，即不需要重写onDraw方法绘制，可以开启这个标记，系统会进行相应的优化。默认情况下，View没有开启这个标记，默认认为需要实现onDraw方法绘制，当我们继承ViewGroup实现自定义控件，并且明确知道不需要具备绘制功能时，可以开启这个标记，如果我们重写了onDraw,那么要显示的关闭这个标记
子view宽高可以超过父view？能
1.android:clipChildren = "false" 这个属性要设置在父 view 上。代表其中的子View 可以超出屏幕。
2.子view 要有具体的大小，一定要比父view 大 才能超出。比如 父view 高度 100px 子view 设置高度150px。子view 比父view大，这样超出的属性才有意义。（高度可以在代码中动态赋值，但不能用wrap_content / match_partent）。
3.对父布局还有要求，要求使用linearLayout(反正我用RelativeLayout 是不行)。你如果必须用其他布局可以在需要超出的view上面套一个linearLayout 外面再套其他的布局。
4.最外面的布局如果设置的padding 不能超出
5.自定义view需要注意的几点
1.让view支持wrap_content属性，在onMeasure方法中针对AT_MOST模式做专门处理，否则wrap_content会和match_parent效果一样（继承ViewGroup也同样要在onMeasure中做这个判断处理）
if(widthMeasureSpec == MeasureSpec.AT_MOST && heightMeasureSpec == MeasureSpec.AT_MOST){
    setMeasuredDimension(200,200); // wrap_content情况下要设置一个默认值，200只是举个例子，最终的值需要计算得到刚好包裹内容的宽高值
}else if(widthMeasureSpec == MeasureSpec.AT_MOST){
    setMeasuredDimension(200,heightMeasureSpec );
}else if(heightMeasureSpec == MeasureSpec.AT_MOST){
    setMeasuredDimension(heightMeasureSpec ,200);
}
2.让view支持padding（onDraw的时候，宽高减去padding值，margin由父布局控制，不需要view考虑），自定义ViewGroup需要考虑自身的padding和子view的margin造成的影响
3.在view中尽量不要使用handler，使用view本身的post方法
4.在onDetachedFromWindow中及时停止线程或动画
5.view带有滑动嵌套情形时，处理好滑动冲突
ACTION_DOWN没有拦截，ACTION_MOVE ACTION_UP还会拦截吗



