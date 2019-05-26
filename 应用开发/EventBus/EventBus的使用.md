## EventBus的使用

### 添加gradle依赖

```groovy
// 当前版本为3.1.1，版本视Github为准
implementation 'org.greenrobot:eventbus:3.1.1'
```

Github链接：[EventBus](https://github.com/greenrobot/EventBus)
[EventBus官方文档](http://greenrobot.org/eventbus/documentation/)

### 基本使用步骤

#### 1.定义消息包装类

```java
public class MessageEvent {
    // 定义属性字段
}
```

定义消息包装类是Github文档上建议这么做的(我把它理解为定义消息包装类)，为什么要定义它呢？我觉得可能是这个原因：
**在日常开发中，使用EventBus传递各种各样的数据都有，为了有一个集中管理这些数据类型的地方，需要一个类似于`MessageEvent`的包装类**

#### 2.注册和反注册EventBus

```java
@Override
protected void onCreate(@Nullable Bundle savedInstanceState) {
    uper.onCreate(savedInstanceState);
    EventBus.getDefault().register(this);
}

@Override
protected void onDestroy() {
    EventBus.getDefault().unregister(this);
    super.onDestroy();
}
```

**注意：EventBus的注册和反注册是成对出现的，如果只有注册没有反注册会造成内存泄漏。**

#### 3.监听消息(消息处理)

```java
@Subscribe(threadMode = ThreadMode.MAIN)
public void onMessageEvent(MessageEvent event) {/* Do something */};
```

使用`@Subscribe`注解方法，方法可以是任何合法的方法名，但是需要注意的是方法必须是`public`的，不然会抛如下异常：
```
Caused by: org.greenrobot.eventbus.EventBusException: Subscriber class com.sunny.demo.eventbus.EventBusEntranceActivity and its super classes have no public methods with the @Subscribe annotation
```

方法参数类型为发布的消息类型，发布消息类型和消息处理方法参数类型不同的话，消息处理方法不会被调用。

**[ThreadMode](http://greenrobot.org/eventbus/documentation/delivery-threads-threadmode/)有五种模式：**

**ThreadMode.POSTING**

这是EventBus默认的线程模式，使用该模式意味着**事件处理方法将在发布事件的同一线程中被调用**，事件传递是同步完成的，一旦事件发布完成，所有事件处理方法都会被调用。由于该模式没有涉及到线程切换，因此是开销最小的一种模式。使用该模式时如果发布事件的线程是主线程，那么应该避免在事件处理方法中做大量耗时操作。

```java
// Called in the same thread (default)
// ThreadMode is optional here
@Subscribe(threadMode = ThreadMode.POSTING)
public void onMessage(MessageEvent event) {
    log(event.message);
}
```

**ThreadMode: MAIN**

**事件处理方法将在主线程中调用。** 如果事件发布线程是主线程，那么事件处理方法会被直接调用，没有线程切换，但是如果事件发布线程是后台线程，在事件处理方法被调用之前线程会被切换到主线程，所以这种模式下的事件处理方法中同样不能有耗时操作，以免阻塞主线程造成卡顿或ANR。

```java
// Called in Android UI's main thread
@Subscribe(threadMode = ThreadMode.MAIN)
public void onMessage(MessageEvent event) {
    textField.setText(event.message);
}
```

**ThreadMode: MAIN_ORDERED**

**该模式下的事件处理方法同样是在主线程中被调用。** 它与Thread.MAIN的区别在于该模式下的事件处理方法将会被有序调用(发布的事件会进入队列等待，一个一个被拿到事件处理方法中执行)。对于Thread.MAIN模式下的事件处理方法，现在先在后台线程发布一个事件，紧接着在主线程中也发布一个事件，由于主线程中发布事件，事件处理方法会被直接调用(没有线程切换这一步)，大概率在主线程中发布的事件被先执行，而后再执行后台线程发布的事件(后发布的消息却被先处理了)。但是在Thread.MAIN_ORDERED模式下，不论发布事件的线程是主线程还是后台线程，发布的事件都会按照顺序被处理。

```java
// Called in Android UI's main thread
@Subscribe(threadMode = ThreadMode.MAIN_ORDERED)
public void onMessage(MessageEvent event) {
    textField.setText(event.message);
}
```

发布消息代码示例：
```java
new Thread(new Runnable() {
    @Override
    public void run() {
        publishMessageEvent("事件1");
    }
}, "sub-thread").start();
publishMessageEvent("事件2");
```
先起后台线程发布一个消息，然后再在主线程中发布一个消息，对于`ThreadMode.MAIN`，事件处理方法会先接受到事件2，然后再接收到事件1，但是对于`ThreadMode.MAIN_ORDERED`，事件会按消息发布的代码顺序被接收，也就是事件1先被事件处理方法接收，然后再是事件2。

**ThreadMode: BACKGROUND**

**事件处理方法将在后台线程被调用。** 如果发布事件的线程不是主线程，那么事件处理方法将在发布事件的线程中直接被调用，如果发布事件的线程是主线程，EventBus将从线程池中启动一个后台线程调用事件处理方法(事件将被有序的传递到事件处理方法中)。

```java
// Called in the background thread
@Subscribe(threadMode = ThreadMode.BACKGROUND)
public void onMessage(MessageEvent event){
    saveToDisk(event.message);
}
```

**ThreadMode: ASYNC**

**事件处理方法将在单独的线程中执行。** 不管事件发布线程是主线程还是后台线程，都将会启动一个独立线程调用事件处理方法，发布事件一旦发布完成会立即返回。**该模式适用于事件处理方法中有耗时操作的情况，比如网络请求。** 为了更好的重用线程，EventBus使用了线程池去重用已完成了事件处理方法的线程。

```java
// Called in a separate thread
@Subscribe(threadMode = ThreadMode.ASYNC)
public void onMessage(MessageEvent event){
    backend.send(event.message);
}
```

#### 4.发布消息

在EventBus中，关于post的方法只有两个public修饰的方法，因此，可以说EventBus发布消息只有两种情况：
```java
public void post(Object event)
public void postSticky(Object event)
```

#### 5.粘性事件

前面说过发布消息有两种方法可供选择，`post(Object event)`方法就不说了，`postSticky(Object event)`用来干嘛的呢？它就是用来发布粘性事件的，那么什么叫粘性事件？？

我们都知道Activity在某些情况下会异常销毁然后重建，重建后的Activity可以通过Intent获取上次传递给Activity的数据，这个例子就和粘性事件比较相似了，通过`postSticky(Object event)`方法发布的消息会被已订阅的消息处理方法处理，并且缓存在内存中(只缓存最近一次发布的消息)，当有新的消息处理方法订阅该消息时，新的消息处理方法将收到该消息。

关于粘性事件不仅仅是发布消息的方法不同，消息处理方法上的注解也需要调整：
```java
@Subscribe(threadMode = ThreadMode.MAIN, sticky = true)
public void onMessageEvent(MessageEvent messageEvent) {
    // to something
}
```
添加`sticky = true`。

既然对粘性事件有缓存，就有移除粘性事件的方法：
```java
// 移除指定类型的粘性事件
public <T> T removeStickyEvent(Class<T> eventType)
// 移除某个具体的粘性事件
public boolean removeStickyEvent(Object event)
// 移除所有粘性事件
public void removeAllStickyEvents()
```

#### 6.事件优先级

事件优先级建议只在相同的线程模式下使用，不同的线程模式使用事件优先级结果就是事件处理方法被调用的顺序不固定。

事件优先级使用：
```java
@Subscribe(threadMode = ThreadMode.BACKGROUND, priority = 1)
public void onMainMessageEvent1(MessageEvent messageEvent) {
    // ... handle message
}
```

在相同模式下，priority值越大表示优先级越高，会被先调用。

在ThreadMode.POSTING模式下还可以取消事件的继续传递：
```java
@Subscribe(threadMode = ThreadMode.POSTING, priority = 1)
public void onMainMessageEvent1(MessageEvent messageEvent) {
    // ... handle message
    EventBus.getDefault().cancelEventDelivery(messageEvent);
}
```

如果`cancelEventDelivery(Object event)`在非`ThreadMode.POSTING`模式下使用会抛出如下异常，但不会导致程序崩溃：
```
org.greenrobot.eventbus.EventBusException: This method may only be called from inside event handling methods on the posting thread
```

#### 7.订阅者索引

EventBus查找订阅者中的订阅方法使用的是遍历+反射(没分析源码前的猜测)，如果一个类中的方法太多，那么在运行时去遍历查找订阅方法是一件耗时的操作，能否在编译时就把订阅者中的订阅方法信息找出来？EventBus 3就提供了这么一个特性：使用订阅者索引加速订阅者注册。原理：使用EventBus的注解处理器在构建时创建订阅者索引类，该类包含订阅者和订阅者方法信息。EventBus官方建议使用订阅者索引获得最佳性能。

使用：
```java
android {
    defaultConfig {
        ...
        javaCompileOptions {
            annotationProcessorOptions {
                arguments = [eventBusIndex: 'com.sunny.demo.eventbus.EventBusIndex']
            }
        }
    }
    ...
}

dependencies {
    ...
    implementation 'org.greenrobot:eventbus:3.1.1'
    annotationProcessor 'org.greenrobot:eventbus-annotation-processor:3.1.1'
}
```

然后重新build工程，然后EventBus注解处理器就会在`eventBusIndex`指定的路径下创建索引类`EventBusIndex`：
```java
/** This class is generated by EventBus, do not edit. */
public class EventBusIndex implements SubscriberInfoIndex {
    private static final Map<Class<?>, SubscriberInfo> SUBSCRIBER_INDEX;

    static {
        SUBSCRIBER_INDEX = new HashMap<Class<?>, SubscriberInfo>();

        putIndex(new SimpleSubscriberInfo(EventBusEntranceActivity.class, true, new SubscriberMethodInfo[] {
            new SubscriberMethodInfo("onPostingMessageEvent", MessageEvent.class),
            new SubscriberMethodInfo("onMainMessageEvent", MessageEvent.class, ThreadMode.MAIN),
            new SubscriberMethodInfo("onMainOrderedMessageEvent", MessageEvent.class, ThreadMode.MAIN_ORDERED),
            new SubscriberMethodInfo("onBackgroundMessageEvent", MessageEvent.class, ThreadMode.BACKGROUND),
            new SubscriberMethodInfo("onAsyncMessageEvent", MessageEvent.class, ThreadMode.ASYNC),
            new SubscriberMethodInfo("onMainMessageEvent0", MessageEvent.class),
            new SubscriberMethodInfo("onMainMessageEvent1", MessageEvent.class, ThreadMode.POSTING, 1, false),
            new SubscriberMethodInfo("onMainMessageEvent2", MessageEvent.class, ThreadMode.POSTING, 2, false),
            new SubscriberMethodInfo("onMainMessageEvent3", MessageEvent.class, ThreadMode.POSTING, 3, false),
        }));

        putIndex(new SimpleSubscriberInfo(EventBusStickyActivity.class, true, new SubscriberMethodInfo[] {
            new SubscriberMethodInfo("onMessageEvent", MessageEvent.class, ThreadMode.MAIN, 0, true),
        }));

    }

    private static void putIndex(SubscriberInfo info) {
        SUBSCRIBER_INDEX.put(info.getSubscriberClass(), info);
    }

    @Override
    public SubscriberInfo getSubscriberInfo(Class<?> subscriberClass) {
        SubscriberInfo info = SUBSCRIBER_INDEX.get(subscriberClass);
        if (info != null) {
            return info;
        } else {
            return null;
        }
    }
}
```

最后我们在应用自定义Application的onCreate()方法中将订阅者索引类添加到EventBus中，并将EventBus设置成默认的的EventBus：
```java
public class App extends Application {

    @Override
    public void onCreate() {
        super.onCreate();
        EventBus.builder().addIndex(new EventBusIndex()).installDefaultEventBus();
    }
}
```

### 总结

EventBus从使用上来说是非常简单的，使用过程中我们要注意五种线程模式的合理使用以及EventBus注册和反注册需成对出现，最后就是最好使用官方建议的添加订阅者索引来优化性能。