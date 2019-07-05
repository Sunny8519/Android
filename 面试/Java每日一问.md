## Java

### 1.Java中的四种引用类型

参考：[Java的四种引用方式](https://www.cnblogs.com/huajiezh/p/5835618.html)

Java提供四种引用类型的目的：
为了让程序员在某些场景下能够控制对象的生命周期。

**强引用**

当一个对象有强引用指向时，该对象永远不会被垃圾回收器回收，即使OOM。

创建一个对象并把这个对象赋值给一个引用变量属于强引用：
```java
Object obj = new Object();
String str = "你好";
```

强引用不分局部变量和全局变量，即使是在一个方法中这样定义一个变量它也是强引用：
```java
public void fun() {
    Object obj = new Object();
    ...
}
```
只不过方法中的`obj`引用变量生命周期有限，方法返回，`obj`引用变量也就被销毁了。

**软引用**

如果一个对象**仅有**软引用指向时，该对象会在内存发生OOM之前被回收，如果内存空间一直足够，垃圾回收器就不会回收它。

注意到**仅有**两个字了吗？没错，当一个对象有强引用和软引用指向时，内存即使发生OOM，该对象也不会被回收，但是当对象的强引用消失仅剩软引用时，内存不足就会回收该对象。

Java中的软引用表示是使用`SoftReference`类，它有两种构造方法：
```java
public SoftReference(Object referent)
public SoftReference(Object referent, ReferenceQueue<? super Object> q)
```

基本使用：
```java
Object obj = new Object();
SoftReference<Object> softObj = new SoftReference<>(obj);
obj = null;
```
如上面示例，当obj引用变量不再指向`new Object()`这个对象时，内存不足就会回收`new Object()`这个对象，通过`SoftReference#get()`方法获得的引用就为null。

结合`ReferenceQueue`使用：
```java
Object obj = new Object();
ReferenceQueue rq = new ReferenceQueue();
SoftReference<Object> srf = new SoftReference<>(obj,rq);
obj = null;
```
如上示例，结合`ReferenceQueue`使用到了`SoftReference`的第二个构造方法，把`ReferenceQueue`对象通过构造参数传递给了`SoftReference`对象，这样当内存不足，`new Object()`对象被回收时，`new SoftReference<>()`这个对象会被加入到`ReferenceQueue`队列里，这就让我们有机会通过`ReferenceQueue`知道哪些对象被回收了，进而清理这些没有利用价值的`new SoftReference<>()`对象。

小结：
1.当对象仅有软引用指向时，我们叫这个对象为软可及对象。
2.垃圾回收器回收软可及对象有一套算法：最近创建或者刚使用过的软可及对象会被虚拟机尽可能保留，长时间闲置不用的软可及对象会被优先回收。
3.创建软引用的同时也会生成很多`SoftReference`对象，当软引用的对象被回收后，应该想办法销毁同时生成的`SoftReference`对象。

**弱引用**

