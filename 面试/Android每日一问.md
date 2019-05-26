## Android

### Intent传递数据大小有限制吗？为什么会有这个限制，谈谈你的理解？

这里有对TransactionTooLargeException异常的解释：https://developer.android.com/reference/android/os/TransactionTooLargeException.html

从Activity#startActivity(Intent)方法的实现来看，代码最终会调用到`ActivityManager.getService().startActivity(...)`这行代码，`ActivityManager.getService()`返回的是系统进程中的AMS在app进程中的binder代理，也就是说app进程调用系统进程中的方法了，所以这里涉及跨进程通信。

穿插一点AMS知识：
简单一点说系统进程中的AMS管理了所有进程中的Activity，因此app进程与系统进程是一个双向通信的过程。启动一个Activity在宏观上的流程是这样的：在app中的进程A启动了一个Activity，会调用到系统进程中AMS的相关实现，待AMS中的实现调用完后会回调app进程中的相关方法进行Activity生命周期的回调，大致呈U字形的调用逻辑。

那么Intent中的数据传递也是呈U字形，先从app进程通过binder进入到系统进程，然后再从系统进程进入到被启动的Activity所在进程，为什么这里要强调**被启动的Activity所在进程**，因为我们在AndroidManifest.xml中可以通过android:process属性为Activity或者Service指定单独进程。**所以startActivity()方法原生就支持跨进程通信。**

现在再来看看TransactionTooLargeException文档上的解释：
在远程调用过程中，传递的参数和返回值会作为Parcel对象存储在Binder事务缓冲区中，如果参数或者返回值太大了导致缓冲区放不下就会抛出TransactionTooLargeException异常。
Binder事务缓冲区有一个限定的固定大小，当前为1M(注：实际比1M要小)，并且这1M的大小是被当前进程所有正在进行的事务所共享的。因此，即使大多数单个事务的大小适中，当有许多事务正在进行时，就有可能会抛出异常。

普通由Zygote进程孵化出来的用户进程所映射的Binder内存大小不足1M，准确的值是`1024*1024-4096 *2`，这个限制在`frameworks/native/libs/binder/processState.cpp`中定义。可以这么简单理解：每个进程系统都会为其分配一个大小不足1M的Binder内存空间。

所以总结来说startActivity携带的数据会经过Binder内核再传递到目标Activity，由于Binder内存大小限制，所以startActivity携带的数据大小也就有了限制。

由于Binder内存大小本身就不足1M，再加上启动Activity需要携带的必要数据，实际上留给我们携带的数据大小不足512K，一般超过512K，就有可能发生TransactionTooLargeException异常。

**为什么Binder会有这个限制？？**
简单理解：Binder的设计就是为了频繁而灵活的进程间通信，而不是为了复制copy大量的数据，一旦涉及到大量数据的传递必然会有性能方面的问题。

### 通过Bundle传递的数据是不是都要经历序列化和反序列化过程？

不是所有通过Bundle传递的数据都会经过序列化和反序列化过程，这要分情况：

1.在通过Intent启动Activity时，通过Intent中的Bundle传递的数据会经历序列化和反序列化过程，这是因为启动Activity涉及了跨进程通信，Android又是通过Binder来解决跨进程通信问题，所以数据要想跨进程传递就需经历序列化和反序列化过程(这是传递值的过程，从一个进程的内存进入到另一个进程的内存，如果是传递引用，每个进程的内存是独立互不干涉的，无法相互访问，使用引用访问显然不可行)。

2.Fragment也可以使用Bundle传递数据，但是Fragment不涉及跨进程通信，因为通过Bundle给Fragment传递的数据实际上传的是引用，而不是值，所以也就没有序列化和反序列化一说，之所以通过Bundle传递的数据都要实现Serializable或者Parcelable接口，是因为Bundle要兼容跨进程通信，但是实现了Serializable或者Parcelable接口的数据不一定会经过序列化和反序列化过程，也许只是传递了一个引用。

### Intent传递数据大小有限制，有什么好的方案替代Intent传递数据吗？或者通过Intent传递数据应该注意什么？

替代方案：
替代方案要分两个方面，因为通过Intent传递数据涉及到跨进程通信，所以我们的替代方案应该从这两个方面来说：1.进程内；2.跨进程。

1.进程内：
- EventBus的粘性事件。因为Activity在一些特殊情况下是会销毁重建的，重建的Activity可以通过Intent拿到上一次传递给页面的数据。

- 根据数据标识还原数据，比如要传递一张图片，可以仅传递图片的资源id或者url，到目标页面之后再根据这些标识还原图片数据。

- 把数据持久化再到目标页面还原。

- 把数据存放在静态变量中，到目标页面直接读取，但要注意共享变量线程安全以及变量不用时资源释放的问题。

2.跨进程：
写入临时文件或者数据库，通过FileProvider将该文件或者数据库通过url发送至目标页面。

**通过Intent传递数据应该注意：**
对于大数据，例如长字符串，Bitmap等，不要考虑使用Intent传递。