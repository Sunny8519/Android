## Android

### 1.Intent传递数据大小有限制吗？为什么会有这个限制，谈谈你的理解？

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

### 2.通过Bundle传递的数据是不是都要经历序列化和反序列化过程？

不是所有通过Bundle传递的数据都会经过序列化和反序列化过程，这要分情况：

1.在通过Intent启动Activity时，通过Intent中的Bundle传递的数据会经历序列化和反序列化过程，这是因为启动Activity涉及了跨进程通信，Android又是通过Binder来解决跨进程通信问题，所以数据要想跨进程传递就需经历序列化和反序列化过程(这是传递值的过程，从一个进程的内存进入到另一个进程的内存，如果是传递引用，每个进程的内存是独立互不干涉的，无法相互访问，使用引用访问显然不可行)。

2.Fragment也可以使用Bundle传递数据，但是Fragment不涉及跨进程通信，因为通过Bundle给Fragment传递的数据实际上传的是引用，而不是值，所以也就没有序列化和反序列化一说，之所以通过Bundle传递的数据都要实现Serializable或者Parcelable接口，是因为Bundle要兼容跨进程通信，但是实现了Serializable或者Parcelable接口的数据不一定会经过序列化和反序列化过程，也许只是传递了一个引用。

### 3.Intent传递数据大小有限制，有什么好的方案替代Intent传递数据吗？或者通过Intent传递数据应该注意什么？

替代方案：
替代方案要分两个方面，因为通过Intent传递数据涉及到跨进程通信，所以我们的替代方案应该从这两个方面来说：1.进程内；2.跨进程(比如通过Intent：启动同一个进程中的另一个Activity-进程内，启动其他进程的另一个Activity-跨进程)。

1.进程内：
- EventBus的粘性事件。因为Activity在一些特殊情况下是会销毁重建的，重建的Activity可以通过Intent拿到上一次传递给页面的数据。

- 根据数据标识还原数据，比如要传递一张图片，可以仅传递图片的资源id或者url，到目标页面之后再根据这些标识还原图片数据。

- 把数据持久化再到目标页面还原。

- 把数据存放在静态变量中，到目标页面直接读取，但要注意共享变量线程安全以及变量不用时资源释放的问题。

2.跨进程：
写入临时文件或者数据库，通过FileProvider将该文件或者数据库通过url发送至目标页面。

**通过Intent传递数据应该注意：**
对于大数据，例如长字符串，Bitmap等，不要考虑使用Intent传递。

### 4.Android中Handler的机制和原理？

参考文章：
[android的消息处理机制（图+源码分析）——Looper,Handler,Message](https://www.cnblogs.com/codingmyworld/archive/2011/09/12/2174255.html)
[聊一聊Android的消息机制](https://my.oschina.net/youranhongcha/blog/492591)

### 5.Serializable和Parcelable

#### 说一下Serializable和Parcelable的区别？

参考文章：
[序列化Serializable和Parcelable的理解和区别](https://www.jianshu.com/p/a60b609ec7e7)
[Parcelable原理](https://segmentfault.com/a/1190000012522154)

**Serializable:**
Serializable是一个标识接口，实现了这个接口的对象就能进行序列化和反序列化，从而达到可传输的状态(比如字节流的形式)，为什么没提“可存储”状态呢，个人觉得在能够存储之前，对象就需要达到可传输的状态，不然对象数据如何从内存进入磁盘。对象数据传输可能会用于内存与内存之间，网络之间，内存与磁盘之间等等。

**对象数据的传输为什么需要序列化和反序列化呢？**
我们可以把序列化机制理解为一种协议，对于数据传输总是要有协议的，不然数据如何恢复，我们需要跟传输的两端商定好一种机制，发送端按照这种机制去把对象实例转换为可传输的字节流，接收端再按照这种机制把字节流转换为对象实例，因此可传输的字节流中必然包括了一些描述性数据，比如类名，字段名，字段是什么类型，有没有父类，父类类名，父类有哪些字段，字段名是什么，又是什么类型等等，当然还必须包括字段的实际值。关于序列化的大致原理可参考：[Java序列化机制和原理](https://www.cnblogs.com/redcreen/articles/1955307.html)

**Parcelable**
Android独有，Parcelable是一个接口，它也有标识的作用，设计之初就是用于Android中高性能的IPC传输(只在内存中传输)，但跟Serializable不同的是Parcelable内部是通过Parcel把一个完整的对象拆分成基本数据类型(除了String类型)来进行序列化的，不涉及大量的反射和输入输出流操作，因而性能更高，但是不能进行数据持久化以及网络传输。

注：网络上很多文章说Parcelable不适合进行数据持久化和网络传输，个人觉得表达有误，应该是不能，可以这么来想这个问题：Parcelable类仅存在于`android.os`包下，是android独有的类，在其他平台上是没有的，如果我们使用Parcelable进行序列化，然后把数据通过网络传输到服务端，假设服务端是Java平台，那么JDK如何反序列化该数据？
如果能够把对象通过Parcelable持久化到文件，Java平台拿到这个文件如何反序列化？答案都是不能。

Parcel是一块内存缓存，序列化后的数据会写入Parcel缓存中。Parcel原理(讲解的不透彻，仅供参考)见:
[什么是Parcel](https://blog.csdn.net/niu_gao/article/details/6453391)
[Android中Parcel的分析以及使用](https://www.cnblogs.com/yaozhongxiao/archive/2013/04/30/3052172.html)

#### 为什么在使用Serializable序列化和反序列化时建议添加serialVersionUID？

作用：比较类版本

Serializable在序列化时会根据序列化机制规定的格式把对象转为字节流，字节流中包括对象类的一些信息，其中就有serialVersionUID，如果serialVersionUID没有显示的声明，那么在序列化时序列化机制会根据编译生成的class自动生成一个serialVersionUID包含在字节流中(类似于指纹算法)。反序列化时，序列化机制需要拿当前类的serialVersionUID与字节流中的serialVersionUID比较，不同就会抛出异常，对于没有显示指定serialVersionUID的类来说，默认的serialVersionUID值受多种因素影响(字段名、字段的增加减少、编译器等)，对于编译器不同而导致serialVersionUID不一致的异常是无意义的，所以开发中还是建议显示指定一个serialVersionUID值，这样我们不仅避免了因编译器不同可能导致的异常，同时我们更改字段名或者增加减少字段也不会报错了。

#### 使用Serializable进行序列化和反序列化时在哪些情况下即使指定了serialVersionUID也会反序列化失败？

类发生了结构性的改变，比如类名修改了，某个已存在的属性类型发生了改变。

#### 使用Serializable如何重写序列化过程？

从ObjectOutputStream#writeObject(Object obj)和ObjectOutputStream#readObject(Object obj)源码中可以看出序列化过程中会通过反射拿取被序列化对象中writeObject(ObjectOutputStream)和readObject(ObjectOutputStream)这两个方法的方法对象(Method)，如果被序列化对象中有这两个方法，那么就会通过反射调用这两个方法进行序列化和反序列化，所以我们重写序列化过程的切入点就在这儿，我们可以在被序列化对象中增加如下两个方法：
```java
private void writeObject(ObjectOutputStream oos) throws IOException {
    //调用默认的序列化方法，如里面注释一样，可以把非静态和非transient字段给序列化了
    oos.defaultWriteObject();
    //把isMan也序列化一下(isMan是transient修饰符修饰的字段)
    oos.writeBoolean(isMan);
}

private void readObject(ObjectInputStream ois) throws IOException, ClassNotFoundException {
    //调用默认的反序列化方法，如里面注释一样，可以把非静态和非transient字段给反序列化了
    ois.defaultReadObject();
    //读取序列化的字段
    isMan = ois.readBoolean();
}
```

### 6.Android IPC

#### Android中创建进程的方式

Android中创建进程的方式有两种：
1.android:process属性
2.通过JNI在native层fork进程
第一种方式是android中最正统也是最常用的创建多进程的方式，第二种有点特殊，对开发者的技术要求也更高。

#### Android中是不是只能给四大组件(Activity,Service,Receiver,ContentProvider)指定进程？

是的，Android中我们可以在AndroidManifest.xml文件中通过android:process属性给四大组件指定所在进程，我们无法给一个线程或者一个实体类指定运行时所在的进程。

#### android中查看正在运行的应用进程信息的adb命令是什么？

`adb shell ps`：查看所有正在运行的应用进程信息
`adb shell ps | grep xxxx`：根据包名筛选应用进程

#### android:process指定的进程名中使用`:`分割和不适用`:`分割有什么区别？

以`:`开头的进程名其实是简写，全名应该是`包名:xxx`，比如`com.demo.sunny:remote`，而不以`:`开头的进程名就是当前指定的字符串。

以`:`开头启动的进程是应用私有进程，其他应用的组件不能跑在这个进程中，而不以`:`开头的进程属于全局进程，其他应用的组件可以通过shareUID的方式跑在该进程中。

#### 两个应用中的组件怎样才能跑在同一个进程中？

两个应用需要有相同的ShareUID以及签名，在这两个条件下，即使他们不运行在同一个进程中，两个应用也能够互相访问对方的私有数据，比如data目录，组件信息等，如果运行在同一个进程中，不仅可以可以访问data目录，组件信息，还可以共享内存数据，就像是一个应用一样。

#### 在如下场景中打印出的值是多少？
有个两个Activity，ActivityA运行在默认进程A中，ActivityB运行在进程B中，UserManager类中有一个全局静态变量`public static int a = 1;`，现在先运行ActivityA，在onCreate方法中把UserManager中的静态变量a改为2，然后启动ActivityB，在ActivityB的onCreate方法中打印出a的值，那么该值是多少呢？为什么？

打印出的值应该为1，因为在android中，每个进程都对应着一个独立的虚拟机，这也意味着每个进程有自己独立的内存空间，不能够共享内存数据，每个类在每个进程中都有一份副本，互相独立，在进程A中改了类的静态变量，但是进程B中的静态变量却没有修改。

#### 你所了解的android中多进程会带来哪些问题？

1.静态成员变量和单例模式完全失效；
2.线程同步机制完全失效；
3.SharedPreferences可能变得不可靠了；
4.Application会多次创建。

#### 为什么多进程Application会多次创建？

比较浅显的层次回答：
Android中每启动一个进程都需要为其分配独立的虚拟机，相当于一个启动应用的过程，启动应用自然需要创建Application。运行在同一个进程中的组件是属于同一个虚拟机和Application的，相对应的运行在不同进程中的组件就是属于不同虚拟机和Application。

#### 用过Parcelable吗？使用Parcelable如何实现一个对象的序列化？

1.对象实现Parcelable接口；
2.覆写接口中的两个方法和一个一参构造方法
```java
public int describeContents()
public void writeToParcel(Parcel dest, int flags)

// 访问修饰符可以是任何一种，一般是私有，只用于CREATOR内部实例化对象
private Xxxx(Parcel in)
```
3.实例化一个静态常量CREATOR，变量名是固定的，只能是CREATOR。
```java
public static final Parcelable.Creator<Xxxx> CREATOR = new Parcelable.Creator<Xxxx>(){
    public Xxxx createFromParcel(Parcel in){
        return new Xxxx(in);
    }

    public Xxxx[] newArray(int size){
        return new Xxxx[size];
    }
}
```

#### Parcelable实现序列化的`writeToParcel(Parcel dest,int flags)`方法的第二个参数值有几种？

有三种：0，1，2，1对应于Parcelable中的`PARCELABLE_WRITE_RETURN_VALUE`，2对应于Parcelable中的`PARCELABLE_ELIDE_DUPLICATES`，大多数情况下flags值都是0。

#### `public int describeContents()`方法返回值有几种？

返回值有两种：0和1，1对应于Parcelable中的`CONTENTS_FILE_DESCRIPTOR`，表示当前对象中含有文件描述符(？？)，大多数情况下返回值是0。

#### Parcelable中List和Map对象能进行序列化和反序列化吗？

能进行序列化和反序列化，但前提是这些集合中的元素实现了Parcelable接口。

#### AIDL文件存放规则是什么？

使用aidl进行进程间通信通常涉及三种文件：实现Parcelable接口的实体类/与实体类对应的aidl文件(名字与实体类相同，作用是对实体类的声明)/操作该实体类的接口类aidl文件。

**aidl文件夹的新建规则**
aidl文件都是存放于aidl文件夹下的packageName下，aidl文件夹与java、res、assets等文件夹平级，aidl文件夹下是packageName，也就是java文件夹下的包路径(通常是包名,androidManifest.xml文件中有声明)，这个packageName必须与java文件夹下的packageName相同，这是aidl文件夹的新建规则。

**aidl文件存放规则**
如果在java/packageName/entity/文件夹下有一个实体类Book.java(java/packageName/entity/Book.java)，那么实体类对应的aidl声明文件必须放在aidl/packageName/entity文件夹下(aidl/pacakgeName/entity/Book.aidl)。操作实体类的接口类aidl文件放置就比较随意了，但是必须放在aidl/packageName/文件夹下，可以放在aidl/packageName/entity/文件夹下，也可以放在直接放在aidl/packageName/文件夹下。

文件新建完之后，rebuild项目，会在build/generated/目录下生成与aidl相关的文件夹，该文件夹下会有操作实体类的接口类aidl文件的实现类(java文件)。

#### aidl文件在跨进程通信中是否是必须的？

不是必须的，实现跨进程通信的是Binder机制，而aidl只是一个辅助角色，一个工具，系统能够根据aidl文件帮助开发者自动生成Binder跨进程通信的一些固定格式的代码，这些固定格式的代码开发者完全可以自己去实现(不嫌麻烦的话)。

#### 说一下aidl自动生成的接口大致结构？

```java
/*
 * This file is auto-generated.  DO NOT MODIFY.
 */
package com.sunny.demo;

public interface IBookManager extends android.os.IInterface {
    /**
     * Local-side IPC implementation stub class.
     */
    public static abstract class Stub extends android.os.Binder implements com.sunny.demo.IBookManager {
        private static final java.lang.String DESCRIPTOR = "com.sunny.demo.IBookManager";

        /**
         * Construct the stub at attach it to the interface.
         */
        public Stub() {
            this.attachInterface(this, DESCRIPTOR);
        }

        /**
         * Cast an IBinder object into an com.sunny.demo.IBookManager interface,
         * generating a proxy if needed.
         */
        public static com.sunny.demo.IBookManager asInterface(android.os.IBinder obj) {
            if ((obj == null)) {
                return null;
            }
            android.os.IInterface iin = obj.queryLocalInterface(DESCRIPTOR);
            if (((iin != null) && (iin instanceof com.sunny.demo.IBookManager))) {
                return ((com.sunny.demo.IBookManager) iin);
            }
            return new com.sunny.demo.IBookManager.Stub.Proxy(obj);
        }

        @Override
        public android.os.IBinder asBinder() {
            return this;
        }

        @Override
        public boolean onTransact(int code, android.os.Parcel data, android.os.Parcel reply, int flags) throws android.os.RemoteException {
            java.lang.String descriptor = DESCRIPTOR;
            switch (code) {
                case INTERFACE_TRANSACTION: {
                    reply.writeString(descriptor);
                    return true;
                }
                case TRANSACTION_getBookList: {
                    data.enforceInterface(descriptor);
                    java.util.List<com.sunny.demo.aidl.Book> _result = this.getBookList();
                    reply.writeNoException();
                    reply.writeTypedList(_result);
                    return true;
                }
                case TRANSACTION_addBook: {
                    data.enforceInterface(descriptor);
                    com.sunny.demo.aidl.Book _arg0;
                    if ((0 != data.readInt())) {
                        _arg0 = com.sunny.demo.aidl.Book.CREATOR.createFromParcel(data);
                    } else {
                        _arg0 = null;
                    }
                    this.addBook(_arg0);
                    reply.writeNoException();
                    return true;
                }
                default: {
                    return super.onTransact(code, data, reply, flags);
                }
            }
        }

        private static class Proxy implements com.sunny.demo.IBookManager {
            private android.os.IBinder mRemote;

            Proxy(android.os.IBinder remote) {
                mRemote = remote;
            }

            @Override
            public android.os.IBinder asBinder() {
                return mRemote;
            }

            public java.lang.String getInterfaceDescriptor() {
                return DESCRIPTOR;
            }

            @Override
            public java.util.List<com.sunny.demo.aidl.Book> getBookList() throws android.os.RemoteException {
                android.os.Parcel _data = android.os.Parcel.obtain();
                android.os.Parcel _reply = android.os.Parcel.obtain();
                java.util.List<com.sunny.demo.aidl.Book> _result;
                try {
                    _data.writeInterfaceToken(DESCRIPTOR);
                    mRemote.transact(Stub.TRANSACTION_getBookList, _data, _reply, 0);
                    _reply.readException();
                    _result = _reply.createTypedArrayList(com.sunny.demo.aidl.Book.CREATOR);
                } finally {
                    _reply.recycle();
                    _data.recycle();
                }
                return _result;
            }

            @Override
            public void addBook(com.sunny.demo.aidl.Book book) throws android.os.RemoteException {
                android.os.Parcel _data = android.os.Parcel.obtain();
                android.os.Parcel _reply = android.os.Parcel.obtain();
                try {
                    _data.writeInterfaceToken(DESCRIPTOR);
                    if ((book != null)) {
                        _data.writeInt(1);
                        book.writeToParcel(_data, 0);
                    } else {
                        _data.writeInt(0);
                    }
                    mRemote.transact(Stub.TRANSACTION_addBook, _data, _reply, 0);
                    _reply.readException();
                } finally {
                    _reply.recycle();
                    _data.recycle();
                }
            }
        }

        static final int TRANSACTION_getBookList = (android.os.IBinder.FIRST_CALL_TRANSACTION + 0);
        static final int TRANSACTION_addBook = (android.os.IBinder.FIRST_CALL_TRANSACTION + 1);
    }

    public java.util.List<com.sunny.demo.aidl.Book> getBookList() throws android.os.RemoteException;

    public void addBook(com.sunny.demo.aidl.Book book) throws android.os.RemoteException;
}
```

该接口(我们称它为自己定义的接口A)继承了`android.os.IInterface`接口，`android.os.IInterface`接口中只有一个`public Binder asBinder();`方法，我们自己定义的接口A中就是我们自己定义的操作方法；

Stub类结构：
1.DESCRIPTOR静态常量和N个TRANSACTION_xxx静态常量
2.asBinder()
3.asInterface(IBinder binder)
4.onTransact(int code,Parcel data,Parcel reply,int flags)
5.实现了接口A的代理类Proxy

接着就是接口中的静态抽象内部类Stub，它继承了`android.os.Binder`类，因此它自身就是一个Binder，并且它还实现了我们自己定义的接口A，不过没有实现接口A中我们定义的方法，而是实现了`android.os.IInterface`中的`public Binder asBinder();`方法，返回的就是它本身；Stub中的静态常量有三个，分别是`DESCRIPTOR`,`TRANSACTION_xxxx1`,`TRANSACTION_xxxx2`，后面两个常量是用来标识我们在接口A中定义的接口方法的，`DESCRIPTOR`是Binder的唯一标识，一般用当前Binder类名来表示。

asInterface(IBinder binder)的方法参数是来自服务端的Binder，该方法的作用就是根据客户端和服务端是否在同一个进程返回不同的接口A对象：如果在同一个进程，直接返回服务端的Binder对象，不在同一个进程，返回一个Stub.Proxy代理对象。

onTransact()方法是在服务端的Binder线程池中被调用的，也就是说只有在不同进程中时，该方法才会被调用；当客户端调用接口A中的方法时，系统会判断这是否是一个跨进程调用，如果是跨进程调用，这个远程请求会经过系统底层封装后交由onTransact()方法处理，服务端通过code判断客户端请求的是哪一个方法，接着从data中取出目标参数(如果方法有目标参数的话)，然后执行目标方法，目标方法执行完之后向reply中写入返回值(如果目标方法有返回值的话)，这就是一次完整的跨进程调用过程，需要注意的是onTransact()方法返回true时表示调用成功，返回false表示调用失败，我们可以利用这一点做服务端的权限控制(我们也不希望随便一个进程都能远程调用我们的服务)。

代理类Proxy结构：
`Proxy(android.os.IBinder remote)`一参构造方法传入服务端的Binder对象，所以该代理类Proxy主要工作就是代理服务端Binder，它也实现了接口A，在方法实现中把方法参数写入Parcel，然后调用服务端Binder对象的`transact(int code,Parcel data,Parcel reply,flags)`方法，进而触发服务端Binder的`onTransact()`方法，`transact()`方法执行完后再通过reply获取服务端被调用方法的返回值。

#### Binder死亡代理是指什么？

linkToDeath、unlinkToDeath、DeathRecipient

#### Bundle支持哪些数据的传输？

#### Android中如何在native层fork出一个进程，说出具体的步骤？


### 7.Android中的Service

参考：
[Android Service演义](https://my.oschina.net/youranhongcha/blog/710046)

### 8.自定义Handler时如何避免内存泄漏？

首先为了避免内存泄漏我们应该搞明白为什么Handler可能会导致内存泄漏的发生。

一般Handler内存泄漏指的是Acivity的内存泄漏，如果我们在Activity中创建了一个非静态内部类或者匿名内部类，由于这两种内部类会隐性持有外部类的引用(也就是Activity实例的引用)，当Activity生命周期走完后Handler中如果还存在未处理的message时就会导致Activity一时半会销毁不了，比如发送了一个延时的message：message会持有Handler引用，Handler会隐性持有Activity的引用，message又被MessageQueue持有，而MessageQueue被主线程持有，主线程的生命周期是贯穿整个应用的生命周期的。

避免Handler可能发生的内存泄漏的方法：
1.在Activity的onDestroy()方法中手动把MessageQueue中的消息清空，即调用`removeCallbacksAndMessages(null)`；
2.让Handler不直接持有外部类Activity等组件的引用，也就是让Handler称为静态内部类，如果在Handler实例中要调用外部类的实例方法或属性的话，可以在Handler中持有一个外部类的弱引用；
3.将Handler抽取成通用的独立的Java文件。
```java
// 使用这种方式需要注意:通过构造传入的Callback不能是匿名内部类，否则由于弱引用特性，匿名内部类实例将在下一次GC被回收，Callback的生命周期应该和使用这个Handler的对象生命周期一致
public class WeakRefHandler extends Handler {
    private WeakReference<Callback> mWeakReference;
    
    public WeakRefHandler(Callback callback) {
        mWeakReference = new WeakReference<Handler.Callback>(callback);
    }
    
    public WeakRefHandler(Callback callback, Looper looper) {
        super(looper);
        mWeakReference = new WeakReference<Handler.Callback>(callback);
    }
    
    @Override
    public void handleMessage(Message msg) {
        if (mWeakReference != null && mWeakReference.get() != null) {
            Callback callback = mWeakReference.get();
            callback.handleMessage(msg);
        }
    }
}
```

### 9.onNewIntent()方法的调用时机？

### 10.RecyclerView相比ListView有哪些优势？

### 11.谈一谈Proguard混淆技术

Proguard技术有如下功能:
压缩 --检查并移除代码中无用的类
优化--对字节码的优化，移除无用的字节码
混淆--混淆定义的名称，避免反编译
预监测--在java平台对处理后的代码再次进行检测
代码混淆只在上线时才会用到，debug模式下会关闭，是一种可选的技术。
那么为什么要使用代码混淆呢?
因为Java是一种跨平台的解释性开发语言，而java的源代码会被编译成字节码文件，存储在.class文件中，由于跨平台的需要，java的字节码中包含了很多源代码信息，诸如变量名、方法名等等。并且通过这些名称来访问变量和方法，这些变量很多是无意义的，但是又很容易反编译成java源代码，为了防止这种现象，我们就需要通过proguard来对java的字节码进行混淆，混淆就是对发布的程序进行重新组织和处理，使得处理后的代码与处理前的代码有相同的功能，和不同的代码展示，即使被反编译也很难读懂代码的含义，哪些混淆过的代码仍能按照之前的逻辑执行得到一样的结果。
但是，某些java类是不能被混淆的，比如实现了序列化的java类是不能被混淆的，否则反序列化时会出问题。
下面这类代码混淆的时候要注意保留，不能混淆。
Android系统组件，系统组件有固定的方法被系统调用。
被Android Resource 文件引用到的。名字已经固定，也不能混淆，比如自定义的View 。
Android Parcelable ，需要使用android 序列化的。
其他Anroid 官方建议 不混淆的，如
```
android.app.backup.BackupAgentHelper
android.preference.Preference
com.android.vending.licensing.ILicensingService
```
Java序列化方法，系统序列化需要固定的方法。
枚举 ，系统需要处理枚举的固定方法。
本地方法，不能修改本地方法名
annotations 注释
数据库驱动
有些resource 文件

### 12.ANR出现的场景以及解决方案

在Android中，应用的响应性被活动管理器（Activity Manager）和窗口管理器（Window Manager）这两个系统服务所监视。当用户触发了输入事件（如键盘输入，点击按钮等）,如果应用5秒内没有响应用户的输入事件，那么，Android会认为该应用无响应，便弹出ANR对话框。而弹出ANR异常，也主要是为了提升用户体验。
解决方案是对于耗时的操作，比如访问网络、访问数据库等操作，需要开辟子线程，在子线程处理耗时的操作，主线程主要实现UI的操作

### 13.OOM异常是否可以被try...catch...捕获？

只有在一种情况下，这样做是可行的：在try语句中声明了很大的对象，导致OOM，并且可以确认OOM是由try语句中的对象声明导致的，那么在catch语句中，可以释放掉这些对象，解决OOM的问题，继续执行剩余语句。
但是这通常不是合适的做法。Java中管理内存除了显式地catch OOM之外还有更多有效的方法：比如SoftReference, WeakReference, 硬盘缓存等。在JVM用光内存之前，会多次触发GC，这些GC会降低程序运行的效率。如果OOM的原因不是try语句中的对象（比如内存泄漏），那么在catch语句中会继续抛出OOM。

### 14.SQLite可以执行多线程操作吗?如何保证多线程操作数据库的安全性

每当你需要使用数据库时，你需要使用DatabaseManager的openDatabase()方法来取得数据库，这个方法里面使用了单例模式，保证了数据库对象的唯一性，也就是每次操作数据库时所使用的sqlite对象都是一致得到。其次，我们会使用一个引用计数来判断是否要创建数据库对象。如果引用计数为1，则需要创建一个数据库，如果不为1，说明我们已经创建过了。
在closeDatabase()方法中我们同样通过判断引用计数的值，如果引用计数降为0，则说明我们需要close数据库。
大致的做法就是在多线程访问的情况下需要自己来封装一个DatabaseManager来管理Sqlite数据库的读写，需要同步的同步，需要异步的异步，不要直接操作数据库，这样很容易出现因为锁的问题导致加锁后的操作失败。

### 15.字节流和字符流的区别

字节流操作的基本单元为字节；字符流操作的基本单元为Unicode码元(2个字节)。
字节流默认不使用缓冲区；字符流使用缓冲区。
字节流通常用于处理二进制数据，实际上它可以处理任意类型的数据，但它不支持直接写入或读取Unicode码元；字符流通常处理文本数据，它支持写入及读取Unicode码元。

### 16.android中跨进程通信方式有几种？请分别简单描述一下

1.通过Bundle传递数据，android中的三大组件Activity，Service，Receiver都可以通过Intent来传递Bundle数据，Bundle类实现了Parcelable接口，我们可以很方便的把想要传输的数据附加在Bundle中通过Intent传输。

2.使用文件共享，文件共享就是把数据写入文件中暂时保存，然后在需要的时候取出来使用，不过在跨进程的情景下需要考虑的文件的同步问题。

3.使用Messenger...