## Java基础面试题汇总

### 什么是长连接？什么是短连接？长连接和短连接的区别是什么？

https://blog.csdn.net/ideality_hunter/article/details/77712242

### 什么是Session？谈谈你对Session的理解？

### 1.java中==和equals和hashCode的区别

基本数据类型的`==`比较的值相等。
类的`==`比较的内存的地址，即是否是同一个对象，在不覆盖equals的情况下，同比较内存地址，原实现也为`==`，如String等重写了equals方法。
hashCode也是Object类的一个方法。返回一个离散的int型整数。在集合类操作中使用，为了提高查询速度。（HashMap，HashSet等比较是否为同一个）。
如果两个对象equals，Java运行时环境会认为他们的hashcode一定相等。
如果两个对象不equals，他们的hashcode有可能相等。
如果两个对象hashcode相等，他们不一定equals。
如果两个对象hashcode不相等，他们一定不equals。

### 2.int与integer的区别

int 基本类型
integer 对象 int的封装类

### 3.String、StringBuffer、StringBuilder区别

String:字符串常量 不适用于经常要改变值得情况，每次改变相当于生成一个新的对象。
StringBuffer:字符串变量 （线程安全）
StringBuilder:字符串变量（线程不安全） 确保单线程下可用，效率略高于StringBuffer。

### 4.什么是内部类？内部类的作用

内部类可直接访问外部类的属性
Java中内部类主要分为成员内部类、局部内部类(嵌套在方法和作用域内)、匿名内部类（没构造方法）、静态内部类（static修饰的类，不能使用任何外围类的非static成员变量和方法， 不依赖外围类）

### 5.进程和线程的区别

进程是cpu资源分配的最小单位，线程是cpu调度的最小单位。
进程之间不能共享资源，而线程共享所在进程的地址空间和其它资源。
一个进程内可拥有多个线程，进程可开启进程，也可开启线程。
一个线程只能属于一个进程，线程可直接使用同进程的资源,线程依赖于进程而存在。

### 6.final，finally，finalize的区别

final:修饰类、成员变量和成员方法，类不可被继承，成员变量不可变，成员方法不可重写
finally:与try...catch...共同使用，确保无论是否出现异常都能被调用到
finalize:类的方法,垃圾回收之前会调用此方法,子类可以重写finalize()方法实现对资源的回收

### 7.静态属性和静态方法是否可以被继承？是否可以被重写？以及原因？

可继承 不可重写 而是被隐藏
如果子类里面定义了静态方法和属性，那么这时候父类的静态方法或属性称之为"隐藏"。如果你想要调用父类的静态方法和属性，直接通过父类名.方法或变量名完成。

### 8.成员内部类、静态内部类、局部内部类和匿名内部类的理解，以及项目中的应用

Java中内部类主要分为成员内部类、局部内部类(嵌套在方法和作用域内)、匿名内部类（没构造方法）、静态内部类（static修饰的类，不能使用任何外围类的非static成员变量和方法， 不依赖外围类）
使用内部类最吸引人的原因是：每个内部类都能独立地继承一个（接口的）实现，所以无论外围类是否已经继承了某个（接口的）实现，对于内部类都没有影响。
因为Java不支持多继承，支持实现多个接口。但有时候会存在一些使用接口很难解决的问题，这个时候我们可以利用内部类提供的、可以继承多个具体的或者抽象的类的能力来解决这些程序设计问题。可以这样说，接口只是解决了部分问题，而内部类使得多重继承的解决方案变得更加完整。

### 9.String转换成Integer的方式及原理

String->integer Intrger.parseInt(string);
Integer->string Integer.toString();

### 10.哪些情况下的对象会被垃圾回收机制处理掉？

1.所有实例都没有活动线程访问。
2.没有被其他任何实例访问的循环引用实例。
3.Java 中有不同的引用类型。判断实例是否符合垃圾收集的条件都依赖于它的引用类型。
要判断怎样的对象是没用的对象。这里有2种方法：
1.采用标记计数的方法：
给内存中的对象给打上标记，对象被引用一次，计数就加1，引用被释放了，计数就减一，当这个计数为0的时候，这个对象就可以被回收了。当然，这也就引发了一个问题：循环引用的对象是无法被识别出来并且被回收的。所以就有了第二种方法：
2.采用根搜索算法：
从一个根出发，搜索所有的可达对象，这样剩下的那些对象就是需要被回收的

### 11.静态代理和动态代理的区别，什么场景使用？

静态代理类：
由程序员创建或由特定工具自动生成源代码，再对其编译。在程序运行前，代理类的.class文件就已经存在了。
动态代理类：在程序运行时，运用反射机制动态创建而成。

### 14.Java中实现多态的机制是什么？
答：方法的重写Overriding和重载Overloading是Java多态性的不同表现
重写Overriding是父类与子类之间多态性的一种表现
重载Overloading是一个类中多态性的一种表现.
### 16.说说你对Java反射的理解
JAVA反射机制是在运行状态中, 对于任意一个类, 都能够知道这个类的所有属性和方法; 对于任意一个对象, 都能够调用它的任意一个方法和属性。 从对象出发，通过反射（Class类）可以取得取得类的完整信息（类名 Class类型，所在包、具有的所有方法 Method[]类型、某个方法的完整信息（包括修饰符、返回值类型、异常、参数类型）、所有属性 Field[]、某个属性的完整信息、构造器 Constructors），调用类的属性或方法自己的总结： 在运行过程中获得类、对象、方法的所有信息。
### 17.说说你对Java注解的理解
元注解
元注解的作用就是负责注解其他注解。java5.0的时候，定义了4个标准的meta-annotation类型，它们用来提供对其他注解的类型作说明。
1.@Target
2.@Retention
3.@Documented
4.@Inherited
### 18.Java中String的了解
在源码中string是用final 进行修饰，它是不可更改，不可继承的常量。
### 19.String为什么要设计成不可变的？
1、字符串池的需求
字符串池是方法区（Method Area）中的一块特殊的存储区域。当一个字符串已经被创建并且该字符串在 池 中，该字符串的引用会立即返回给变量，而不是重新创建一个字符串再将引用返回给变量。如果字符串不是不可变的，那么改变一个引用（如: string2）的字符串将会导致另一个引用（如: string1）出现脏数据。
2、允许字符串缓存哈希码
在java中常常会用到字符串的哈希码，例如： HashMap 。String的不变性保证哈希码始终一，因此，他可以不用担心变化的出现。 这种方法意味着不必每次使用时都重新计算一次哈希码——这样，效率会高很多。
3、安全
String广泛的用于java 类中的参数，如：网络连接（Network connetion），打开文件（opening files ）等等。如果String不是不可变的，网络连接、文件将会被改变——这将会导致一系列的安全威胁。操作的方法本以为连接上了一台机器，但实际上却不是。由于反射中的参数都是字符串，同样，也会引起一系列的安全问题。
### 20、Object类的equal和hashCode方法重写，为什么？
首先equals与hashcode间的关系是这样的：
1、如果两个对象相同（即用equals比较返回true），那么它们的hashCode值一定要相同；
2、如果两个对象的hashCode相同，它们并不一定相同(即用equals比较返回false)
由于为了提高程序的效率才实现了hashcode方法，先进行hashcode的比较，如果不同，那没就不必在进行equals的比较了，这样就大大减少了equals比较的次数，这对比需要比较的数量很大的效率提高是很明显的
### 21、List,Set,Map的区别
Set是最简单的一种集合。集合中的对象不按特定的方式排序，并且没有重复对象。 Set接口主要实现了两个实现类：HashSet： HashSet类按照哈希算法来存取集合中的对象，存取速度比较快
TreeSet ：TreeSet类实现了SortedSet接口，能够对集合中的对象进行排序。
List的特征是其元素以线性方式存储，集合中可以存放重复对象。
ArrayList() : 代表长度可以改变得数组。可以对元素进行随机的访问，向ArrayList()中插入与删除元素的速度慢。
LinkedList(): 在实现中采用链表数据结构。插入和删除速度快，访问速度慢。
Map 是一种把键对象和值对象映射的集合，它的每一个元素都包含一对键对象和值对象。 Map没有继承于Collection接口 从Map集合中检索元素时，只要给出键对象，就会返回对应的值对象。
HashMap：Map基于散列表的实现。插入和查询“键值对”的开销是固定的。可以通过构造器设置容量capacity和负载因子load factor，以调整容器的性能。
LinkedHashMap： 类似于HashMap，但是迭代遍历它时，取得“键值对”的顺序是其插入次序，或者是最近最少使用(LRU)的次序。只比HashMap慢一点。而在迭代访问时发而更快，因为它使用链表维护内部次序。
TreeMap ： 基于红黑树数据结构的实现。查看“键”或“键值对”时，它们会被排序(次序由Comparabel或Comparator决定)。TreeMap的特点在 于，你得到的结果是经过排序的。TreeMap是唯一的带有subMap()方法的Map，它可以返回一个子树。
WeakHashMao ：弱键(weak key)Map，Map中使用的对象也被允许释放: 这是为解决特殊问题设计的。如果没有map之外的引用指向某个“键”，则此“键”可以被垃圾收集器回收。
### 26、ArrayMap和HashMap的对比
1、存储方式不同
HashMap内部有一个HashMapEntry<K, V>[]对象，每一个键值对都存储在这个对象里，当使用put方法添加键值对时，就会new一个HashMapEntry对象，
2、添加数据时扩容时的处理不一样，进行了new操作，重新创建对象，开销很大。ArrayMap用的是copy数据，所以效率相对要高。
3、ArrayMap提供了数组收缩的功能，在clear或remove后，会重新收缩数组，是否空间
4、ArrayMap采用二分法查找；
### 29、HashMap和HashTable的区别
1 HashMap不是线程安全的，效率高一点、方法不是Synchronize的要提供外同步，有containsvalue和containsKey方法。
hashtable是，线程安全，不允许有null的键和值，效率稍低，方法是是Synchronize的。有contains方法方法。Hashtable 继承于Dictionary 类
### 30、HashMap与HashSet的区别
hashMap:HashMap实现了Map接口,HashMap储存键值对,使用put()方法将元素放入map中,HashMap中使用键对象来计算hashcode值,HashMap比较快，因为是使用唯一的键来获取对象。
HashSet实现了Set接口，HashSet仅仅存储对象，使用add()方法将元素放入set中，HashSet使用成员对象来计算hashcode值，对于两个对象来说hashcode可能相同，所以equals()方法用来判断对象的相等性，如果两个对象不同的话，那么返回false。HashSet较HashMap来说比较慢。
### 31、HashSet与HashMap怎么判断集合元素重复？
HashSet不能添加重复的元素，当调用add（Object）方法时候，
首先会调用Object的hashCode方法判hashCode是否已经存在，如不存在则直接插入元素；如果已存在则调用Object对象的equals方法判断是否返回true，如果为true则说明元素已经存在，如为false则插入元素。
### 33、ArrayList和LinkedList的区别，以及应用场景
ArrayList是基于数组实现的，ArrayList线程不安全。
LinkedList是基于双链表实现的：
使用场景：
（1）如果应用程序对各个索引位置的元素进行大量的存取或删除操作，ArrayList对象要远优于LinkedList对象；
( 2 ) 如果应用程序主要是对列表进行循环，并且循环时候进行插入或者删除操作，LinkedList对象要远优于ArrayList对象；
### 34、数组和链表的区别
数组：是将元素在内存中连续存储的；它的优点：因为数据是连续存储的，内存地址连续，所以在查找数据的时候效率比较高；它的缺点：在存储之前，我们需要申请一块连续的内存空间，并且在编译的时候就必须确定好它的空间的大小。在运行的时候空间的大小是无法随着你的需要进行增加和减少而改变的，当数据两比较大的时候，有可能会出现越界的情况，数据比较小的时候，又有可能会浪费掉内存空间。在改变数据个数时，增加、插入、删除数据效率比较低。
链表：是动态申请内存空间，不需要像数组需要提前申请好内存的大小，链表只需在用的时候申请就可以，根据需要来动态申请或者删除内存空间，对于数据增加和删除以及插入比数组灵活。还有就是链表中数据在内存中可以在任意的位置，通过应用来关联数据（就是通过存在元素的指针来联系）
### 35、开启线程的三种方式？
Java有三种创建线程的方式，分别是继承Thread类、实现Runable接口和使用线程池
### 36、线程和进程的区别？
线程是进程的子集，一个进程可以有很多线程，每条线程并行执行不同的任务。不同的进程使用不同的内存空间，而所有的线程共享一片相同的内存空间。别把它和栈内存搞混，每个线程都拥有单独的栈内存用来存储本地数据。
### 38、run()和start()方法区别
这个问题经常被问到，但还是能从此区分出面试者对Java线程模型的理解程度。start()方法被用来启动新创建的线程，而且start()内部调用了run()方法，这和直接调用run()方法的效果不一样。当你调用run()方法的时候，只会是在原来的线程中调用，没有新的线程启动，start()方法才会启动新线程。
### 39、如何控制某个方法允许并发访问线程的个数？
semaphore.acquire() 请求一个信号量，这时候的信号量个数-1（一旦没有可使用的信号量，也即信号量个数变为负数时，再次请求的时候就会阻塞，直到其他线程释放了信号量）
semaphore.release() 释放一个信号量，此时信号量个数+1
### 40、在Java中wait和seelp方法的不同；
Java程序中wait 和 sleep都会造成某种形式的暂停，它们可以满足不同的需要。wait()方法用于线程间通信，如果等待条件为真且其它线程被唤醒时它会释放锁，而sleep()方法仅仅释放CPU资源或者让当前线程停止执行一段时间，但不会释放锁。
### 41、谈谈wait/notify关键字的理解
等待对象的同步锁,需要获得该对象的同步锁才可以调用这个方法,否则编译可以通过，但运行时会收到一个异常：IllegalMonitorStateException。
调用任意对象的 wait() 方法导致该线程阻塞，该线程不可继续执行，并且该对象上的锁被释放。
唤醒在等待该对象同步锁的线程(只唤醒一个,如果有多个在等待),注意的是在调用此方法的时候，并不能确切的唤醒某一个等待状态的线程，而是由JVM确定唤醒哪个线程，而且不是按优先级。
调用任意对象的notify()方法则导致因调用该对象的 wait()方法而阻塞的线程中随机选择的一个解除阻塞（但要等到获得锁后才真正可执行）。
### 42、什么导致线程阻塞？线程如何关闭？
阻塞式方法是指程序会一直等待该方法完成期间不做其他事情，ServerSocket的accept()方法就是一直等待客户端连接。这里的阻塞是指调用结果返回之前，当前线程会被挂起，直到得到结果之后才会返回。此外，还有异步和非阻塞式方法在任务完成前就返回。
一种是调用它里面的stop()方法
另一种就是你自己设置一个停止线程的标记 （推荐这种）
### 43、如何保证线程安全？
1.synchronized；
2.Object方法中的wait,notify；
3.ThreadLocal机制 来实现的。
### 44、如何实现线程同步？
1、synchronized关键字修改的方法。2、synchronized关键字修饰的语句块3、使用特殊域变量（volatile）实现线程同步
### 45、线程间操作List
List list = Collections.synchronizedList(new ArrayList());
### 46、谈谈对Synchronized关键字，类锁，方法锁，重入锁的理解
java的对象锁和类锁：java的对象锁和类锁在锁的概念上基本上和内置锁是一致的，但是，两个锁实际是有很大的区别的，对象锁是用于对象实例方法，或者一个对象实例上的，类锁是用于类的静态方法或者一个类的class对象上的。我们知道，类的对象实例可以有很多个，但是每个类只有一个class对象，所以不同对象实例的对象锁是互不干扰的，但是每个类只有一个类锁。但是有一点必须注意的是，其实类锁只是一个概念上的东西，并不是真实存在的，它只是用来帮助我们理解锁定实例方法和静态方法的区别的
### 49、synchronized 和volatile 关键字的区别
1.volatile本质是在告诉jvm当前变量在寄存器（工作内存）中的值是不确定的，需要从主存中读取；synchronized则是锁定当前变量，只有当前线程可以访问该变量，其他线程被阻塞住。
2.volatile仅能使用在变量级别；synchronized则可以使用在变量、方法、和类级别的
3.volatile仅能实现变量的修改可见性，不能保证原子性；而synchronized则可以保证变量的修改可见性和原子性
4.volatile不会造成线程的阻塞；synchronized可能会造成线程的阻塞。
5.volatile标记的变量不会被编译器优化；synchronized标记的变量可以被编译器优化
### 51、ReentrantLock 、synchronized和volatile比较
ava在过去很长一段时间只能通过synchronized关键字来实现互斥，它有一些缺点。比如你不能扩展锁之外的方法或者块边界，尝试获取锁时不能中途取消等。Java 5 通过Lock接口提供了更复杂的控制来解决这些问题。 ReentrantLock 类实现了 Lock，它拥有与 synchronized 相同的并发性和内存语义且它还具有可扩展性。
### 53、死锁的四个必要条件？
死锁产生的原因
1. 系统资源的竞争
系统资源的竞争导致系统资源不足，以及资源分配不当，导致死锁。
2. 进程运行推进顺序不合适
互斥条件：一个资源每次只能被一个进程使用，即在一段时间内某 资源仅为一个进程所占有。此时若有其他进程请求该资源，则请求进程只能等待。
请求与保持条件：进程已经保持了至少一个资源，但又提出了新的资源请求，而该资源 已被其他进程占有，此时请求进程被阻塞，但对自己已获得的资源保持不放。
不可剥夺条件:进程所获得的资源在未使用完毕之前，不能被其他进程强行夺走，即只能 由获得该资源的进程自己来释放（只能是主动释放)。
循环等待条件: 若干进程间形成首尾相接循环等待资源的关系
这四个条件是死锁的必要条件，只要系统发生死锁，这些条件必然成立，而只要上述条件之一不满足，就不会发生死锁。
死锁的避免与预防：
死锁避免的基本思想:
系统对进程发出每一个系统能够满足的资源申请进行动态检查,并根据检查结果决定是否分配资源,如果分配后系统可能发生死锁,则不予分配,否则予以分配。这是一种保证系统不进入死锁状态的动态策略。
理解了死锁的原因，尤其是产生死锁的四个必要条件，就可以最大可能地避免、预防和解除死锁。所以，在系统设计、进程调度等方面注意如何让这四个必要条件不成立，如何确定资源的合理分配算法，避免进程永久占据系统资源。此外，也要防止进程在处于等待状态的情况下占用资源。因此，对资源的分配要给予合理的规划。
死锁避免和死锁预防的区别：
死锁预防是设法至少破坏产生死锁的四个必要条件之一,严格的防止死锁的出现,而死锁避免则不那么严格的限制产生死锁的必要条件的存在,因为即使死锁的必要条件存在,也不一定发生死锁。死锁避免是在系统运行过程中注意避免死锁的最终发生。
### 56、什么是线程池，如何使用?
创建线程要花费昂贵的资源和时间，如果任务来了才创建线程那么响应时间会变长，而且一个进程能创建的线程数有限。为了避免这些问题，在程序启动的时候就创建若干线程来响应处理，它们被称为线程池，里面的线程叫工作线程。从JDK1.5开始，Java API提供了Executor框架让你可以创建不同的线程池。比如单线程池，每次处理一个任务；数目固定的线程池或者是缓存线程池（一个适合很多生存期短的任务的程序的可扩展线程池）。
### 57、Java中堆和栈有什么不同？
为什么把这个问题归类在多线程和并发面试题里？因为栈是一块和线程紧密相关的内存区域。每个线程都有自己的栈内存，用于存储本地变量，方法参数和栈调用，一个线程中存储的变量对其它线程是不可见的。而堆是所有线程共享的一片公用内存区域。对象都在堆里创建，为了提升效率线程会从堆中弄一个缓存到自己的栈，如果多个线程使用该变量就可能引发问题，这时volatile 变量就可以发挥作用了，它要求线程从主存中读取变量的值。
### 58、有三个线程T1，T2，T3，怎么确保它们按顺序执行？
在多线程中有多种方法让线程按特定顺序执行，你可以用线程类的join()方法在一个线程中启动另一个线程，另外一个线程完成该线程继续执行。为了确保三个线程的顺序你应该先启动最后一个(T3调用T2，T2调用T1)，这样T1就会先完成而T3最后完成。
线程间通信
我们知道线程是CPU调度的最小单位。在Android中主线程是不能够做耗时操作的，子线程是不能够更新UI的。而线程间通信的方式有很多，比如广播，Eventbus，接口回掉，在Android中主要是使用handler。handler通过调用sendmessage方法，将保存消息的Message发送到Messagequeue中，而looper对象不断的调用loop方法，从messageueue中取出message，交给handler处理，从而完成线程间通信。


2019Android设计模式总结
1.设计模式六大原则
a.单一职责原则：就一个类来说，应该只有一个引起它变化的原因
一个类做一件事情，避免职责过多。比如这种情况是不太好的，在一个Activity中既有bean文件，又有http请求，还有adapter等等，这就导致我们需要修改任何一个东西的时候都会导致Activity的改变，这样一来就有多个引起它变化的原因，不符合单一职责原则
b.开放封闭原则：类，模块，函数应该是可以扩展的，但是不可以修改
对于扩展是开放的，对于修改是封闭的。尽量做到面对需求的改变时，我们的代码能保持相对稳定，通过扩展的方式应对变化，而不是修改原有代码实现
c.里氏替换原则：所有引用基类的地方，必须可以透明的时候其子类的对象
里氏替换原则是实现开放封闭原则的重要方式之一，我们知道，使用基类的地方都可以使用子类去实现，因为子类拥有基类的所有方法，所以在程序设计中尽量使用基类类型对对象进行定义，在运行时确定子类类型。
d.依赖倒置原则：高层模块不应该依赖于底层模块，两者都应该依赖于抽象，抽象不应该依赖于细节，细节应该依赖于抽象
依赖倒置原则针对的是模块之间的依赖关系，高层模块指调用端，底层模块指具体的实现类，抽象指接口或抽象类，细节就是实现类。该原则的具体表现就是模块间的依赖通过抽象发生，直线类之间不发生直接依赖关系，依赖通过接口或抽象类产生，降低耦合，比如MVP模式下，View层和P层通过接口产生依赖关系
e.迪米特原则（最少知识原则）：一个软件实体应该尽可能少的与其他实体发生相互作用
迪米特原则要求我们在设计系统时，尽量减少对象之间的交互
f.接口隔离原则：一个类对另一个类的依赖应该建立在最小的接口上
接口隔离原则的关键是接口以及这个接口要小，如何小呢，也就是我们要为专门的类创建专门的接口，这个接口只对它有效，不要试图让一个接口包罗万象，要建立最小的依赖关系
2.设计模式的分类
设计模式分为三类
创建型设计模式
与对象创建有关包括单例模式，工厂方法模式，抽象工厂模式，建造者模式，原型模式
结构型设计模式
结构性设计模式是从程序的结构上解决模块之间的耦合问题，包括适配器模式，代理模式，装饰模式，外观模式，桥接模式，组合模式和享元模式
行为型设计模式
主要处理类或对象如何交互及如何分配职责，包括策略模式，模板方法模式，观察者模式，迭代器模式，责任链模式，命令模式，备忘录模式，状态模式，访问者模式，中介模式，解析器模式

1.请列举出在 JDK 中几个常用的设计模式?
单例模式(Singleton pattern)用于 Runtime，Calendar 和其他的一些类中。工厂模式 (Factory pattern)被用于各种不可变的类如 Boolean，像 Boolean.valueOf，观察者模式 (Observer pattern)被用于 Swing 和很多的事件监听中。装饰器设计模式(Decorator design pattern)被用于多个 Java IO 类中。
2.什么是设计模式?你是否在你的代码里面使用过任何设计模式? 设计模式是世界上各种各样程序员用来解决特定设计问题的尝试和测试的方法。设计模式
是代码可用性的延伸
3.Java 中什么叫单例设计模式?请用 Java 写出线程安全的单例模式
单例模式重点在于在整个系统上共享一些创建时较耗资源的对象。整个应用中只维护一个 特定类实例，它被所有组件共同使用。Java.lang.Runtime 是单例模式的经典例子。从 Java 5 开始你可以使用枚举(enum)来实现线程安全的单例。
4.在 Java 中，什么叫观察者设计模式(observer design pattern)?
观察者模式是基于对象的状态变化和观察者的通讯，以便他们作出相应的操作。简单的例
子就是一个天气系统，当天气变化时必须在展示给公众的视图中进行反映。这个视图对象
是一个主体，而不同的视图是观察者。
5.使用工厂模式最主要的好处是什么?在哪里使用?
工厂模式的最大好处是增加了创建对象时的封装层次。如果你使用工厂来创建对象，之后
你可以使用更高级和更高性能的实现来替换原始的产品实现或类，这不需要在调用层做任
何修改。
6.举一个用 Java 实现的装饰模式(decorator design pattern)?它是作用于对象层次还是类 层次?
装饰模式增加强了单个对象的能力。Java IO 到处都使用了装饰模式，典型例子就是 Buffered 系列类如 BufferedReader 和 BufferedWriter，它们增强了 Reader 和 Writer 对象， 以实现提升性能的 Buffer 层次的读取和写入。
7.在 Java 中，为什么不允许从静态方法中访问非静态变量?
Java 中不能从静态上下文访问非静态数据只是因为非静态变量是跟具体的对象实例关联
的，而静态的却没有和任何实例关联。
8.设计一个 ATM 机，请说出你的设计思路?

比如设计金融系统来说，必须知道它们应该在任何情况下都能够正常工作。不管是断电还 是其他情况，ATM 应该保持正确的状态(事务) , 想想 加锁(locking)、事务 (transaction)、错误条件(error condition)、边界条件(boundary condition)等等。尽管 你不能想到具体的设计，但如果你可以指出非功能性需求，提出一些问题，想到关于边界 条件，这些都会是很好的。
9.在 Java 中，什么时候用重载，什么时候用重写?
如果你看到一个类的不同实现有着不同的方式来做同一件事，那么就应该用重写 (overriding)，而重载(overloading)是用不同的输入做同一件事。在 Java 中，重载的方 法签名不同，而重写并不是。
10.举例说明什么情况下会更倾向于使用抽象类而不是接口? 接口和抽象类都遵循”面向接口而不是实现编码”设计原则，它可以增加代码的灵活性，
可以适应不断变化的需求。下面有几个点可以帮助你回答这个问题:
在 Java 中，你只能继承一个类，但可以实现多个接口。所以一旦你继承了一个类，你就 失去了继承其他类的机会了。 接口通常被用来表示附属描述或行为如:Runnable、Clonable、Serializable 等等，因此当你 使用抽象类来表示行为时，你的类就不能同时是 Runnable 和 Clonable(注:这里的意思是指 如果把Runnable等实现为抽象类的情况)，因为在 Java 中你不能继承两个类，但当你使用 接口时，你的类就可以同时拥有多个不同的行为。 在一些对时间要求比较高的应用中，倾向于使用抽象类，它会比接口稍快一点。 如果希望把一系列行为都规范在类继承层次内，并且可以更好地在同一个地方进行编码， 那么抽象类是一个更好的选择。有时，接口和抽象类可以一起使用，接口中定义函数，而 在抽象类中定义默认的实现。


数据结构面试专题

1、常用数据结构简介
数据结构是指相互之间存在着一种或多种关系的数据元素的集合和该集合中数据元素间的关系组成。常用的数据有：数组、栈、队列、链表、树、图、堆、散列表。

1）数组：在内存中连续存储多个元素的结构。数组元素通过下标访问，下标从0开始。优点：访问速度快；缺点：数组大小固定后无法扩容，只能存储一种类型的数据，添加删除操作慢。适用场景：适用于需频繁查找，对存储空间要求不高，很少添加删除。

2）栈：一种特殊的线性表，只可以在栈顶操作，先进后出，从栈顶放入元素叫入栈，从栈顶取出元素叫出栈。应用场景：用于实现递归功能，如斐波那契数列。

3）队列：一种线性表，在列表一端添加元素，另一端取出，先进先出。使用场景：多线程阻塞队列管理中。

4）链表：物理存储单元上非连续、非顺序的存储结构，数据元素的逻辑顺序是通过链表的指针地址实现，每个元素包含两个结点，一个是存储元素的数据域，一个是指向下一个结点地址的指针域。有单链表、双向链表、循环链表。优点：可以任意加减元素，不需要初始化容量，添加删除元素只需改变前后两个元素结点的指针域即可。缺点：因为含有大量指针域，固占用空间大，查找耗时。适用场景：数据量小，需频繁增加删除操作。

5）树：由n个有限节点组成一种具有层次关系的集合。二叉树（每个结点最多有两个子树，结点的度最大为2，左子树和右子树有顺序）、红黑树（HashMap底层源码）、B+树（mysql的数据库索引结构）

6）散列表（哈希表）：根据键值对来存储访问。

7）堆：堆中某个节点的值总是不大于或不小于其父节点的值，堆总是一棵完全二叉树。

8）图：由结点的有穷集合V和边的集合E组成。

2、并发集合了解哪些？
1）并发List，包括Vector和CopyOnWriteArrayList是两个线程安全的List，Vector读写操作都用了同步，CopyOnWriteArrayList在写的时候会复制一个副本，对副本写，写完用副本替换原值，读时不需要同步。

2）并发Set，CopyOnWriteArraySet基于CopyOnWriteArrayList来实现的，不允许存在重复的对象。

3）并发Map，ConcurrentHashMap，内部实现了锁分离，get操作是无锁的。

4）并发Queue，ConcurrentLinkedQueue适用于高并发场景下的队列，通过无锁方式实现。 BlockingQueue阻塞队列，应用场景，生产者-消费者模式，若生产快于消费，生产队列装满时会阻塞，等待消费。

5）并发Deque, LinkedBlockingDueue没有进行读写锁分离，同一时间只能有一个线程对其操作。

6）并发锁重入锁ReentrantLock，互斥锁，一次最多只能一个线程拿到锁。

7）读写锁ReadWriteLock，有读取和写入锁两种，读取允许多个读取线程同时持有，而写入只能有一个线程持有。

3、列举java的集合以及集合之间的继承关系


 

5、容器类介绍以及之间的区别
1）Collection接口：集合框架的根接口，它是集合类框架中最具一般性的顶层接口。

2）Map接口：提供了键值对的映射关系的集合，关键字不能有重复值，每个关键字至多可映射一个值。HashMap(通过散列机制，用于快速访问)，TreeMap（保持key处于排序状态，访问速度不如hashmap）, LinkedHashMap(保持元素的插入顺序)

3）Set接口：可包含重复的元素，LinkedHashSet TreeSet(用红黑树来存储元素) HashSet

4）List接口:可通过索引对元素进行精准的插入和查找，实现类有ArrayList LinkedList

5）Queue接口：继承自Collection接口，LinkedList实现了Queue接口，提供了支持队列的行为。

6）Iterator接口：为了迭代集合

7）Comparable接口：用于比较

6、List,Set,Map的区别
Set是一个无序的集合，不能包含重复的元素；

list是一个有序的集合可以包含重复的元素，提供了按索引访问的方式；

map包含了key-value对，map中key必须唯一，value可以重复。

7、HashMap的实现原理
1）数据结构

jdk1.7及以前，HashMap由数组+链表组成，数组Entry是HashMap的主体，Entry是HashMap中的一个静态内部类，每一个Entry包含一个key-value键值对，链表是为解决哈希冲突而存在。

从jdk1.8起，HashMap是由数组+链表/红黑树组成，当某个bucket位置的链表长度达到阀值8时，这个链表就转变成红黑树。

2）HashMap是线程不安全的，存储比较快，能接受null值，HashMap通过put(key, value)来储存元素，通过get(key)来得到value值，通过hash算法来计算hashcode值，用hashcode标识Entry在bucket中存储的位置。

3）HashMap中为什么要使用加载因子，为什么要进行扩容

加载因子是指当HashMap中存储的元素/最大空间值的阀值，如果超过这个值，就会进行扩容。加载因子是为了让空间得到充分利用，如果加载因子太大，虽对空间利用更充分，但查找效率会降低；如果加载因子太小，表中的数据过于稀疏，很多空间还没用就开始扩容，就会对空间造成浪费。

至于为什么要扩容，如果不扩容，HashMap中数组处的链表会越来越长，这样查找效率就会大大降低。

7.1 HashMap如何put数据（从HashMap源码角度讲解）？
当我们使用put(key, value)存储对象到HashMap中时，具体实现步骤如下：

1）先判断table数组是否为空，为空以默认大小构建table，table默认空间大小为16

2）计算key的hash值，并计算hash&(n-1)值得到在数组中的位置index，如果该位置没值即table[index]为空，则直接将该键值对存放在table[index]处。

3）如果table[index]处不为空，说明发生了hash冲突，判断table[index]处结点是否是TreeNode(红黑树结点)类型数据，如果是则执行putTreeVal方法，按红黑树规则将键值对存入；

4）如果table[index]是链表形式，遍历该链表上的数据，将该键值对放在table[index]处，并将其指向原index处的链表。判断链表上的结点数是否大于链表最大结点限制（默认为8），如果超过了需执行treeifyBin()操作，则要将该链表转换成红黑树结构。

5）判断HashMap中数据个数是否超过了（最大容量*装载因子），如果超过了，还需要对其进行扩容操作。

7.2 HashMap如何get数据？
get(key)方法获取key的hash值，计算hash&(n-1)得到在链表数组中的位置first=table[hash&(n-1)]，先判断first（即数组中的那个）的key是否与参数key相等，不等的话，判断结点是否是TreeNode类型，是则调用getTreeNode(hash, key)从二叉树中查找结点，不是TreeNode类型说明还是链表型，就遍历链表找到相同的key值返回对应的value值即可。

7.3 当两个对象的hashcode相同，即发生碰撞时，HashMap如何处理
当两个对象的hashcode相同，它们的bucket位置相同，hashMap会用链表或是红黑树来存储对象。Entry类里有一个next属性，作用是指向下一个Entry。第一个键值对A进来，通过计算其key的hash得到index，记做Entry[index]=A。一会又进来一个键值对B，通过计算其key的hash也是index，HashMap会将B.next=A, Entry[index]=B.如果又进来C，其key的hash也是index,会将C.next=B, Entry[index]=C.这样bucket为index的地方存放了A\B\C三个键值对，它们能过next属性链在一起。数组中存储的是最后插入的元素，其他元素都在后面的链表里。

7.4 如果两个键的hashcode相同，如何获取值对象？
当调用get方法时，hashmap会使用键对象的hashcode找到bucket位置，找到bucket位置后，会调用key.equals()方法去找到链表中正确的节点，最终找到值对象。

7.5 hashMap如何扩容
HashMap默认负载因为是0.75，当一个map填满了75%的bucket时，和其他集合类一样，将会创建原来HashMap大小两倍的bucket数组，来重新调整HashMap的大小，并将原来的对象放入新的bucket数组中。

在jdk1.7及以前，多线程扩容可能出现死循环。因为在调整大小过程中，存储在某个bucket位置中的链表元素次序会反过来，而多线程情况下可能某个线程翻转完链表，另外一个线程又开始翻转，条件竞争发生了，那么就死循环了。

而在jdk1.8中，会将原来链表结构保存至节点e中，将原来数组中的位置设为null，然后依次遍历e，根据hash&n是否为0分成两条支链，保存在新数组中。如果多线程情况可能会取到null值造成数据丢失。

8、ConcurrentHashMap的实现原理
1）jdk1.7及以前：一个ConcurrentHashMap由一个segment数组和多个HashEntry组成，每一个segment都包含一个HashEntry数组, Segment继承ReentrantLock用来充当锁角色，每一个segment包含了对自己的HashEntry的操作，如get\put\replace操作，这些操作发生时，对自己的HashEntry进行锁定。由于每一个segment写操作只锁定自己的HashEntry，可以存在多个线程同时写的情况。

jdk1.8以后：ConcurrentHashMap取消了segments字段，采用transient volatile HashEntry<K, V> table保存数据，采用table数组元素作为锁，实现对每一个数组数据进行加锁，进一小减少并发冲突概率。ConcurrentHashMap是用Node数组+链表+红黑树数据结构来实现的，并发制定用synchronized和CAS操作。

2）Segment实现了ReentrantLock重入锁，当执行put操作，会进行第一次key的hash来定位Segment的位置，若该Segment还没有初始化，会通过CAS操作进行赋值，再进行第二次hash操作，找到相应的HashEntry位置。

9、ArrayMap和HashMap的对比
1)存储方式不一样，HashMap内部有一个Node<K,V>[]对象，每个键值对都会存储到这个对象里，当用put方法添加键值对时，会new一个Node对象，tab[i] = newNode(hash, key, value, next);

ArrayMap存储则是由两个数组来维护，int[] mHashes; Object[] mArray; mHashes数组中保存的是每一项的HashCode值，mArray存的是键值对，每两个元素代表一个键值对，前面保存key，后面保存value。mHashes[index]=hash; mArray[index<<1]=key; mArray[(index<<1)+1]=value;

ArrayMap相对于HashMap，无需为每个键值对创建Node对象，且在数组中连续存放，更省空间。

2）添加数据时扩容处理不一样，进行了new操作，重新创建对象，开销很大；而ArrayMap用的是copy数据，所有效率相对高些；

3）ArrayMap提供了数组收缩功能，在clear或remove后，会重新收缩数组，释放空间；

4）ArrayMap采用二分法查找，mHashes中的hash值是按照从小到大的顺序连续存放的，通过二分查找来获取对应hash下标index，去mArray中查找键值对。mHashes中的index*2是mArray中的key下标，index*2+1为value的下标，由于存在hash碰撞情况，二分查找到的下标可能是多个连续相同的hash值中的任意一个，此时需要用equals比对命中的key对象是否相等，不相等，应当从当前index先向后再向前遍历所有相同hash值。

5）sparseArray比ArrayMap进一步优化空间，SparseArray专门对基本类型做了优化，Key只能是可排序的基本类型，如int\long，对value，除了泛型Value，还对每种基本类型有单独实现，如SparseBooleanArray\SparseLongArray等。无需包装，直接使用基本类型值，无需hash，直接使用基本类型值索引和判断相等，无碰撞，无需调用hashCode方法，无需equals比较。SparseArray延迟删除。

10、HashTable实现原理
Hashtable中的无参构造方法Hashtable()中调用了this(11, 0.75f)，说明它默认容量是11，加载因子是0.75,在构造方法上会new HashtableEntry<?, ?>[initialCapacity]; 会新建一个容量是初始容量的HashtableEntry数组。HashtableEntry数组中包含hash\Key\Value\next变量，链表形式，重写了hashCode和equals方法。Hashtable所有public方法都在方法体上加上了synchronized锁操作，说明它是线程安全的。它还实现了Serializable接口中的writeObject和readObject方法，分别实现了逐行读取和写入的功能，并且加了synchronized锁操作。

（1） put(Key, Value)方法

1）先判断value是否为空，为空抛出空指针异常；

2）根据key的hashCode()值，计算table表中的位置索引(hash&0x7FFFFFFF)%tab.length值index，如果该索引处有值，再判断该索引处链表中是否包含相同的key，如果key值相同则替换旧值。

3）如果没有相同的key值，调用addEntry方法，在addEntry中判断count大小是否超过了最大容量限制，如果超过了需要重新rehash()，容量变成原来容量*2+1，将原表中的值都重新计算hash值放入新表中。再构造一个HashtableEntry对象放入相应的table表头，如果原索引处有值，则将table[index].next指向原索引处的链表。

（2）get方法

根所key.hashCode()，计算它在table表中的位置，(hash&0x7FFFFFFF)%tab.length，遍历该索引处表的位置中是否有值，是否存在链表，再判断是key值和hash值是否相等，相等则返回对应的value值。

11、HashMap和HashTable的区别
1）Hashtable是个线程安全的类，在对外方法都添加了synchronized方法，序列化方法上也添加了synchronized同步锁方法，而HashMap非线程安全。这也导致Hashtable的读写等操作比HashMap慢。

2）Hashtable不允许值和键为空，若为空会抛出空指针。而HashMap允许键和值为空；

3）Hashtable根据key值的hashCode计算索引，(hash&0x7FFFFFFF)%tab.length，保证hash值始终为正数且不超过表的长度。而HashMap中计算索引值是通过hash(key)&(tab.length-1)，是通过与操作，计算出在表中的位置会比Hashtable快。

4）Hashtable容量能为任意大于等于1的正数，而HashMap的容量必须为2^n，Hashtable默认容量为11，HashMap初始容量为16

5）Hashtable每次扩容，新容量为旧容量的2倍+1，而HashMap为旧容量的2倍。

12、HashMap与HashSet的区别
HashSet底层实现是HashMap,内部包含一个HashMap<E, Ojbect> map变量

private transient HashMap<E,Object> map;
一个Object PRESENT变量（当成插入map中的value值）

private static final Object PRESENT = new Object();
HashSet中元素都存到HashMap键值对的Key上面。具体可以查看HashSet的add方法，直接调用了HashMap的put方法，将值作为HashMap的键，值用一个固定的PRESENT值。

public boolean add(E e) {
        return map.put(e, PRESENT)==null;
    }
HashSet没有单独的get方法，用的是HashMap的。HashSet实现了Set接口，不允许集合中出现重复元素，将对象存储进HashSet前，要先确保对象重写了hashCode()和equals方法，以保证放入set对象是唯一的。

13、HashSet与HashMap怎么判断集合元素重复？
HashMap在放入key-value键值对是，先通过key计算其hashCode()值，再与tab.length-1做与操作，确定下标index处是否有值，如果有值，再调用key对象的equals方法，对象不同则插入到表头，相同则覆盖；

HashSet是将数据存放到HashMap的key中，HashMap是key-value形式的数据结构，它的key是唯一的，HashSet利用此原理保证放入的对象唯一性。

14、集合Set实现Hash怎么防止碰撞
HashSet底层实现是HashMap，HashMap如果两个不同Key对象的hashCode()值相等，会用链表存储，HashSet也一样。

15、ArrayList和LinkedList的区别，以及应用场景
ArrayList底层是用数组实现的，随着元素添加，其大小是动态增大的；在内存中是连续存放的；如果在集合末尾添加或删除元素，所用时间是一致的，如果在列表中间添加或删除元素，所用时间会大大增加。通过索引查找元素速度很快。适合场合：查询比较多的场景

LinkedList底层是通过双向链表实现的，LinkedList和ArrayList相比，增删速度快，但查询和修改值速度慢。在内存中不是连续内存。场景：增删操作比较多的场景。

-二叉树的深度优先遍历和广度优先遍历的具体实现
-堆的结构
-堆和树的区别
-堆和栈在内存中的区别是什么(解答提示：可以从数据结构方面以及实际实现方面两个方面去回答)？
-什么是深拷贝和浅拷贝
-手写链表逆序代码
-讲一下对树，B+树的理解
-讲一下对图的理解
-判断单链表成环与否？
-链表翻转（即：翻转一个单项链表）
-合并多个单有序链表（假设都是递增的）

线程、多线程和线程池面试专题
1、开启线程的三种方式？
1）继承Thread类，重写run()方法，在run()方法体中编写要完成的任务 new Thread().start();

2）实现Runnable接口，实现run()方法 new Thread(new MyRunnable()).start();

3）实现Callable接口MyCallable类，实现call()方法，使用FutureTask类来包装Callable对象，使用FutureTask对象作为Thread对象的target创建并启动线程；调用FutureTask对象的get()方法来获得子线程执行结束后的返回值。

FutureTask<Integer> ft = new FutureTask<Integer>(new MyCallable());

new Thread(ft).start();

2、run()和start()方法区别
run()方法只是线程的主体方法，和普通方法一样，不会创建新的线程。只有调用start()方法，才会启动一个新的线程，新线程才会调用run()方法，线程才会开始执行。

3、如何控制某个方法允许并发访问线程的个数？
创建Semaphore变量，Semaphore semaphore = new Semaphore(5, true); 当方法进入时，请求一个信号，如果信号被用完则等待，方法运行完，释放一个信号，释放的信号新的线程就可以使用。

4、在Java中wait和seelp方法的不同
wait()方法属于Object类，调用该方法时，线程会放弃对象锁，只有该对象调用notify()方法后本线程才进入对象锁定池准备获取对象锁进入运行状态。

sleep()方法属于Thread类，sleep()导致程序暂停执行指定的时间，让出CPU，但它的监控状态依然保存着，当指定时间到了又会回到运行状态，sleep()方法中线程不会释放对象锁。

5、谈谈wait/notify关键字的理解
notify: 唤醒在此对象监视器上等待的单个线程

notifyAll(): 通知所有等待该竞争资源的线程

wait: 释放obj的锁，导致当前的线程等待，直接其他线程调用此对象的notify()或notifyAll()方法

当要调用wait()或notify()/notifyAll()方法时，一定要对竞争资源进行加锁，一般放到synchronized(obj)代码中。当调用obj.notify/notifyAll后，调用线程依旧持有obj锁，因此等待线程虽被唤醒，但仍无法获得obj锁，直到调用线程退出synchronized块，释放obj锁后，其他等待线程才有机会获得锁继续执行。

6、什么导致线程阻塞？
（1）一般线程阻塞

1）线程执行了Thread.sleep(int millsecond)方法，放弃CPU，睡眠一段时间，一段时间过后恢复执行；

2）线程执行一段同步代码，但无法获得相关的同步锁，只能进入阻塞状态，等到获取到同步锁，才能恢复执行；

3）线程执行了一个对象的wait()方法，直接进入阻塞态，等待其他线程执行notify()/notifyAll()操作；

4）线程执行某些IO操作，因为等待相关资源而进入了阻塞态，如System.in，但没有收到键盘的输入，则进入阻塞态。

5）线程礼让，Thread.yield()方法，暂停当前正在执行的线程对象，把执行机会让给相同或更高优先级的线程，但并不会使线程进入阻塞态，线程仍处于可执行态，随时可能再次分得CPU时间。线程自闭，join()方法，在当前线程调用另一个线程的join()方法，则当前线程进入阻塞态，直到另一个线程运行结束，当前线程再由阻塞转为就绪态。

6）线程执行suspend()使线程进入阻塞态，必须resume()方法被调用，才能使线程重新进入可执行状态。

7､线程如何关闭？
1) 使用标志位

2）使用stop()方法，但该方法就像关掉电脑电源一样，可能会发生预料不到的问题

3）使用中断interrupt()

public class Thread {
    // 中断当前线程
    public void interrupt();
    // 判断当前线程是否被中断
    public boolen isInterrupt();
    // 清除当前线程的中断状态，并返回之前的值
    public static boolen interrupted();   
}
但调用interrupt()方法只是传递中断请求消息，并不代表要立马停止目标线程。

8、讲一下java中的同步的方法
之所以需要同步，因为在多线程并发控制，当多个线程同时操作一个可共享的资源时，如果没有采取同步机制，将会导致数据不准确，因此需要加入同步锁，确保在该线程没有完成操作前被其他线程调用，从而保证该变量的唯一一性和准确性。

1）synchronized修饰同步代码块或方法

由于java的每个对象都有一个内置锁，用此关键字修饰方法时，内置锁会保护整个方法。在调用该方法前，需获得内置锁，否则就处于阴塞状态。

2）volatile修饰变量

保证变量在线程间的可见性，每次线程要访问volatile修饰的变量时都从内存中读取，而不缓存中，这样每个线程访问到的变量都是一样的。且使用内存屏障。

3）ReentrantLock重入锁，它常用的方法有ReentrantLock()：创建一个ReentrantLock实例

lock()获得锁 unlock()释放锁

4）使用局部变量ThreadLocal实现线程同步，每个线程都会保存一份该变量的副本，副本之间相互独立，这样每个线程都可以随意修改自己的副本，而不影响其他线程。常用方法ThreadLocal()创建一个线程本地变量；get()返回此线程局部的当前线程副本变量；initialValue()返回此线程局部变量的当前线程的初始值；set(T value)将此线程变量的当前线程副本中的值设置为value

5) 使用原子变量，如AtomicInteger，常用方法AtomicInteger(int value)创建个有给定初始值的AtomicInteger整数；addAndGet(int data)以原子方式将给定值与当前值相加

6）使用阻塞队列实现线程同步LinkedBlockingQueue<E>

9、如何保证线程安全？
线程安全性体现在三方法：

1）原子性：提供互斥访问，同一时刻只能有一个线和至数据进行操作。

JDK中提供了很多atomic类，如AtomicInteger\AtomicBoolean\AtomicLong，它们是通过CAS完成原子性。JDK提供锁分为两种：synchronized依赖JVM实现锁，该关键字作用对象的作用范围内同一时刻只能有一个线程进行操作。另一种是LOCK,是JDK提供的代码层面的锁，依赖CPU指令，代表性是ReentrantLock。

2）可见性：一个线程对主内存的修改及时被其他线程看到。

JVM提供了synchronized和volatile，volatile的可见性是通过内存屏障和禁止重排序实现的，volatile会在写操作时，在写操作后加一条store屏障指令，将本地内存中的共享变量值刷新到主内存；会在读操作时，在读操作前加一条load指令，从内存中读取共享变量。

3）有序性：指令没有被编译器重排序。

可通过volatile、synchronized、Lock保证有序性。

10、两个进程同时要求写或者读，能不能实现？如何防止进程的同步？
我认为可以实现，比如两个进程都读取日历进程数据是没有问题，但同时写，应该会有冲突。

可以使用共享内存实现进程间数据共享。

11、线程间操作List
 

12、Java中对象的生命周期
1）创建阶段（Created）：为对象分配存储空间，开始构造对象，从超类到子类对static成员初始化；超类成员变量按顺序初始化，递归调用超类的构造方法，子类成员变量按顺序初始化，子类构造方法调用。

2）应用阶段（In Use）：对象至少被一个强引用持有着。

3）不可见阶段（Invisible）：程序运行已超出对象作用域

4）不可达阶段（Unreachable）：该对象不再被强引用所持有

5）收集阶段（Collected）：假设该对象重写了finalize()方法且未执行过，会去执行该方法。

6）终结阶段（Finalized）：对象运行完finalize()方法仍处于不可达状态，等待垃圾回收器对该对象空间进行回收。

7）对象空间重新分配阶段（De-allocated）：垃圾回收器对该对象所占用的内存空间进行回收或再分配，该对象彻底消失。

13、static synchronized 方法的多线程访问和作用
static synchronized控制的是类的所有实例访问，不管new了多少对象，只有一份，所以对该类的所有对象都加了锁。限制多线程中该类的所有实例同时访问JVM中该类对应的代码。

14、同一个类里面两个synchronized方法，两个线程同时访问的问题
如果synchronized修饰的是静态方法，锁的是当前类的class对象，进入同步代码前要获得当前类对象的锁；

普通方法，锁的是当前实例对象，进入同步代码前要获得的是当前实例的锁；

同步代码块，锁的是括号里面的对象，对给定的对象加锁，进入同步代码块库前要获得给定对象锁；

如果两个线程访问同一个对象的synchronized方法，会出现竞争，如果是不同对象，则不会相互影响。

15、volatile的原理
有volatile变量修饰的共享变量进行写操作的时候会多一条汇编代码，lock addl $0x0，lock前缀的指令在多核处理器下会将当前处理器缓存行的数据会写回到系统内存，这个写回内存的操作会引起在其他CPU里缓存了该内存地址的数据无效。同时lock前缀也相当于一个内存屏障，对内存操作顺序进行了限制。

16、synchronized原理
synchronized通过对象的对象头（markword)来实现锁机制，java每个对象都有对象头，都可以为synchronized实现提供基础，都可以作为锁对象，在字节码层面synchronized块是通过插入monitorenter monitorexit完成同步的。持有monitor对象，通过进入、退出这个Monitor对象来实现锁机制。

17、谈谈NIO的理解
NIO( New Input/ Output) 引入了一种基于通道和缓冲区的 I/O 方式，它可以使用 Native 函数库直接分配堆外内存，然后通过一个存储在 Java 堆的 DirectByteBuffer 对象作为这块内存的引用进行操作，避免了在 Java 堆和 Native 堆中来回复制数据。  NIO 是一种同步非阻塞的 IO 模型。同步是指线程不断轮询 IO 事件是否就绪，非阻塞是指线程在等待 IO 的时候，可以同时做其他任务。同步的核心就是 Selector，Selector 代替了线程本身轮询 IO 事件，避免了阻塞同时减少了不必要的线程消耗；非阻塞的核心就是通道和缓冲区，当 IO 事件就绪时，可以通过写道缓冲区，保证 IO 的成功，而无需线程阻塞式地等待。

-synchronized 和volatile 关键字的区别
-synchronized与Lock的区别
-ReentrantLock 、synchronized和volatile比较
1）volatile：解决变量在多个线程间的可见性，但不能保证原子性，只能用于修饰变量，不会发生阻塞。volatile能屏蔽编译指令重排，不会把其后面的指令排到内存屏障之前的位置，也不会把前面的指令排到内存屏障的后面。多用于并行计算的单例模式。volatile规定CPU每次都必须从内存读取数据，不能从CPU缓存中读取，保证了多线程在多CPU计算中永远拿到的都是最新的值。

2）synchronized：互斥锁，操作互斥，并发线程过来，串行获得锁，串行执行代码。解决的是多个线程间访问共享资源的同步性，可保证原子性，也可间接保证可见性，因为它会将私有内存和公有内存中的数据做同步。可用来修饰方法、代码块。会出现阻塞。synchronized发生异常时，会自动释放线程占有的锁，因此不会导致死锁现象发生。非公平锁，每次都是相互争抢资源。

3）lock是一个接口，而synchronized是java中的关键字，synchronized是内置语言的实现。lock可以让等待锁的线程响应中断。在发生异常时，如果没有主动通过unLock()去释放锁，则可能造成死锁现象，因此使用Lock时需要在finally块中释放锁。

4）ReentrantLock可重入锁，锁的分配机制是基于线程的分配，而不是基于方法调用的分配。ReentrantLock有tryLock方法，如果锁被其他线程持有，返回false，可避免形成死锁。对代码加锁的颗粒会更小，更节省资源，提高代码性能。ReentrantLock可实现公平锁和非公平锁，公平锁就是先来的先获取资源。ReentrantReadWriteLock用于读多写少的场合，且读不需要互斥场景。

-ReentrantLock的内部实现
-lock原理
-死锁的四个必要条件？
-怎么避免死锁？
-对象锁和类锁是否会互相影响？
-什么是线程池，如何使用?
-Java的并发、多线程、线程模型
-谈谈对多线程的理解
-多线程有什么要注意的问题？
-谈谈你对并发编程的理解并举例说明
-谈谈你对多线程同步机制的理解？
-如何保证多线程读写文件的安全？
-多线程断点续传原理
-断点续传的实现
（五）并发编程有关知识点（这个是一般Android开发用的少的，所以建议多去看看）：

平时Android开发中对并发编程可以做得比较少，Thread这个类经常会用到，但是我们想提升自己的话，一定不能停留在表面，,我们也应该去了解一下java的关于线程相关的源码级别的东西。


Java基础知识点面试专题
1、java中==和equals和hashCode的区别
1）==若是基本数据类型比较，是比较值，若是引用类型，则比较的是他们在内存中的存放地址。对象是存放在堆中，栈中存放的对象的引用，所以==是对栈中的值进行比较，若返回true代表变量的内存地址相等；

2）equals是Object类中的方法，Object类的equals方法用于判断对象的内存地址引用是不是同一个地址（是不是同一个对象）。若是类中覆盖了equals方法，就要根据具体代码来确定，一般覆盖后都是通过对象的内容是否相等来判断对象是否相等。

3）hashCode()计算出对象实例的哈希码，在对象进行散列时作为key存入。之所以有hashCode方法，因为在批量的对象比较中，hashCode比较要比equals快。在添加新元素时，先调用这个元素的hashCode方法，一下子能定位到它应该旋转的物理位置，若该位置没有元素，可直接存储；若该位置有元素，就调用它的equals方法与新元素进行比较，若相同则不存，不相同，就放到该位置的链表末端。

4）equals与hashCode方法关系：

hashCode()是一个本地方法，实现是根据本地机器上关的。equals()相等的对象，hashCode()也一定相等；hashCode()不等，equals()一定也不等；hashCode()相等，equals()可能相等，也可能不等。

所以在重写equals(Object obj)方法，有必要重写hashCode()方法，确保通过equals(Object obj)方法判断结果为true的两个对象具备相等的hashCode()返回值。

5）equals与==的关系：

Integer b1 = 127;在java编译时被编译成Integer b1 = Integer.valueOf(127);对于-128到127之间的Integer值，用的是原生数据类型int，会在内存里供重用，也就是这之间的Integer值进行==比较时，只是进行int原生数据类型的数值进行比较。而超出-128〜127的范围，进行==比较时是进行地址及数值比较。

 

2、int、char、long各占多少字节数
int\float占用4个字节，short\char占用2个字节，long占用8个字节，byte/boolean占用1个字节

基本数据类型存放在栈里，包装类栈里存放的是对象的引用，即值的地址，而值存放在堆里。

3、int与integer的区别
Integer是int的包装类，int则是java的一种基本数据类型，Integer变量必须实例化才能使用，当new一个Integer时，实际是生成一个指向此对象的引用，而int是直接存储数据的值，Integer默认值是null，而int默认值是0

4、谈谈对java多态的理解
同一个消息可以根据发送对象的不同而采用多种不同的行为方式，在执行期间判断所引用的对象的实际类型，根据其实际的类型调用其相应的方法。

作用：消除类型之间的耦合关系。实现多态的必要条件：继承、重写（因为必须调用父类中存在的方法）、父类引用指向子类对象

5、String、StringBuffer、StringBuilder区别
都是字符串类，String类中使用字符数组保存字符串，因有final修饰符，String对象是不可变的，每次对String操作都会生成新的String对象，这样效率低，且浪费内存空间。但线程安全。

StringBuilder和StringBuffer也是使用字符数组保存字符，但这两种对象都是可变的，即对字符串进行append操作，不会产生新的对象。它们的区别是：StringBuffer对方法加了同步锁，是线程安全的，StringBuilder非线程安全。

6、什么是内部类？内部类的作用
内部类指在类的内部再定义另一个类。

内部类的作用：1）实现多重继承，因为java中类的继承只能单继承，使用内部类可达到多重继承；2）内部类可以很好的实现隐藏，一般非内部类，不允许有private或protected权限的，但内部类可以；3）减少了类文件编译后产生的字节码文件大小；

内部类在编译完后也会产生.class文件，但文件名称是：外部类名称$内部类名称.class。分为以下几种：

1）成员内部类，作为外部类的一个成员存在，与外部类的属性、方法并列，成员内部类持有外部类的引用，成员内部类不能定义static变量和方法。应用场合：每一个外部类都需要一个内部类实例，内部类离不开外部类存在。

2）静态内部类，内部类以static声明，其他类可通过外部类.内部类来访问。特点：不会持有外部类的引用，可以访问外部类的静态变量，若要访问成员变量须通过外部类的实例访问。应用场合：内部类不需要外部类的实例，仅为外部类提供或逻辑上属于外部类，逻辑上可单独存在。设计的意义：加强了类的封装性（静态内部类是外部类的子行为或子属性，两者保持着一定关系），提高了代码的可读性（相关联的代码放在一起）。

3）匿名内部类，在整个操作中只使用一次，没有名字，使用new创建，没有具体位置。

4）局部内部类，在方法内或是代码块中定义类，

7、抽象类和接口区别
抽象类在类前面须用abstract关键字修饰，一般至少包含一个抽象方法，抽象方法指只有声明，用关键字abstract修饰，没有具体的实现的方法。因抽象类中含有无具体实现的方法，固不能用抽象类创建对象。当然如果只是用abstract修饰类而无具体实现，也是抽象类。抽象类也可以有成员变量和普通的成员方法。抽象方法必须为public或protected（若为private，不能被子类继承，子类无法实现该方法）。若一个类继承一个抽象类，则必须实现父类中所有的抽象方法，若子类没有实现父类的抽象方法，则也应该定义为抽象类。

接口用关键字interface修饰，接口也可以含有变量和方法，接口中的变量会被隐式指定为public static final变量。方法会被隐式的指定为public abstract，接口中的所有方法均不能有具体的实现，即接口中的方法都必须为抽象方法。若一个非抽象类实现某个接口，必须实现该接口中所有的方法。

区别：
1）抽象类可以提供成员方法实现的细节，而接口只能存在抽象方法；

2）抽象类的成员变量可以是各种类型，而接口中成员变量只能是public static final类型；

3）接口中不能含有静态方法及静态代码块，而抽象类可以有静态方法和静态代码块；

4）一个类只能继承一个抽象类，用extends来继承，却可以实现多个接口，用implements来实现接口。

7.1、抽象类的意义
抽象类是用来提供子类的通用性，用来创建继承层级里子类的模板，减少代码编写，有利于代码规范化。

7.2、抽象类与接口的应用场景
抽象类的应用场景：1）规范了一组公共的方法，与状态无关，可以共享的，无需子类分别实现；而另一些方法却需要各个子类根据自己特定状态来实现特定功能；

2）定义一组接口，但不强迫每个实现类都必须实现所有的方法，可用抽象类定义一组方法体可以是空方法体，由子类选择自己感兴趣的方法来覆盖；

7.3、抽象类是否可以没有方法和属性？
可以

7.4、接口的意义
1）有利于代码的规范，对于大型项目，对一些接口进行定义，可以给开发人员一个清晰的指示，防止开发人员随意命名和代码混乱，影响开发效率。

2）有利于代码维护和扩展，当前类不能满足要求时，不需要重新设计类，只需要重新写了个类实现对应的方法。

3）解耦作用，全局变量的定义，当发生需求变化时，只需改变接口中的值即可。

4）直接看接口，就可以清楚知道具体实现类间的关系，代码交给别人看，别人也能立马明白。

8、泛型中extends和super的区别
<? extends T>限定参数类型的上界，参数类型必须是T或T的子类型，但对于List<? extends T>，不能通过add()来加入元素，因为不知道<? extends T>是T的哪一种子类；

<? super T>限定参数类型的下界，参数类型必须是T或T的父类型，不能能过get()获取元素，因为不知道哪个超类；

9、父类的静态方法能否被子类重写？静态属性和静态方法是否可以被继承？
父类的静态方法和属性不能被子类重写，但子类可以继承父类静态方法和属性，如父类和子类都有同名同参同返回值的静态方法show()，声明的实例Father father = new Son(); (Son extends Father)，会调用father对象的静态方法。静态是指在编译时就会分配内存且一直存在，跟对象实例无关。

10、进程和线程的区别
进程：具有一定独立功能的程序，是系统进行资源分配和调度运行的基本单位。

线程：进程的一个实体，是CPU调度的苯单位，也是进程中执行运算的最小单位，即执行处理机调度的基本单位，如果把进程理解为逻辑上操作系统所完成的任务，线程则表示完成该任务的许多可能的子任务之一。

关系：一个进程可有多个线程，至少一个；一个线程只能属于一个进程。同一进程的所有线程共享该进程的所有资源。不同进程的线程间要利用消息通信方式实现同步。

区别：进程有独立的地址空间，而多个线程共享内存；进程具有一个独立功能的程序，线程不能独立运行，必须依存于应用程序中；

11、final，finally，finalize的区别
final：变量、类、方法的修饰符，被final修饰的类不能被继承，变量或方法被final修饰则不能被修改和重写。

finally：异常处理时提供finally块来执行清除操作，不管有没有异常抛出，此处代码都会被执行。如果try语句块中包含return语句，finally语句块是在return之后运行；

finalize：Object类中定义的方法，若子类覆盖了finalize()方法，在在垃圾收集器将对象从内存中清除前，会执行该方法，确定对象是否会被回收。

12、序列化Serializable 和Parcelable 的区别
序列化：将一个对象转换成可存储或可传输的状态，序列化后的对象可以在网络上传输，也可以存储到本地，或实现跨进程传输；

为什么要进行序列化：开发过程中，我们需要将对象的引用传给其他activity或fragment使用时，需要将这些对象放到一个Intent或Bundle中，再进行传递，而Intent或Bundle只能识别基本数据类型和被序列化的类型。

Serializable：表示将一个对象转换成可存储或可传输的状态。

Parcelable：与Serializable实现的效果相同，也是将一个对象转换成可传输的状态，但它的实现原理是将一个完整的对象进行分解，分解后的每一部分都是Intent所支持的数据类型，这样实现传递对象的功能。

Parcelable实现序列化的重要方法：序列化功能是由writeToParcel完成，通过Parcel中的write方法来完成；反序列化由CREATOR完成，内部标明了如何创建序列化对象及数级，通过Parcel的read方法完成；内容描述功能由describeContents方法完成，一般直接返回0。

区别：Serializable在序列化时会产生大量临时变量，引起频繁GC。Serializable本质上使用了反射，序列化过程慢。Parcelable不能将数据存储在磁盘上，在外界变化时，它不能很好的保证数据的持续性。

选择原则：若仅在内存中使用，如activity\service间传递对象，优先使用Parcelable，它性能高。若是持久化操作，优先使用Serializable

注意：静态成员变量属于类，不属于对象，固不会参与序列化的过程；用transient关键字编辑的成员变量不会参与序列化过程；可以通过重写writeObject()和readObject()方法来重写系统默认的序列化和反序列化。

13、谈谈对kotlin的理解
特点：1）代码量少且代码末尾没有分号；2）空类型安全（编译期处理了各种null情况，避免执行时异常）；3）函数式的，可使用lambda表达式；4）可扩展方法（可扩展任意类的的属性）；5）互操作性强，可以在一个项目中使用kotlin和java两种语言混合开发；

14、string 转换成 integer的方式及原理
1）parseInt(String s)内部调用parseInt(s, 10)默认为10进制 。
2）正常判断null\进制范围，length等。
3）判断第一个字符是否是符号位。
4）循环遍历确定每个字符的十进制值。
5）通过*=和-=进行计算拼接。
6）判断是否为负值返回结果。


java深入源码级的面试题

1、哪些情况下的对象会被垃圾回收机制处理掉？
利用可达性分析算法，虚拟机会将一些对象定义为GC Roots，从GC Roots出发沿着引用链向下寻找，如果某个对象不能通过GC Roots寻找到，虚拟机就认为该对象可以被回收掉。

1.1 哪些对象可以被看做是GC Roots呢？
1）虚拟机栈（栈帧中的本地变量表）中引用的对象；

2）方法区中的类静态属性引用的对象，常量引用的对象；

3）本地方法栈中JNI(Native方法）引用的对象；

1.2 对象不可达，一定会被垃圾收集器回收么？
即使不可达，对象也不一定会被垃圾收集器回收，1）先判断对象是否有必要执行finalize()方法，对象必须重写finalize()方法且没有被运行过。2）若有必要执行，会把对象放到一个队列中，JVM会开一个线程去回收它们，这是对象最后一次可以逃逸清理的机会。

2、讲一下常见编码方式？
编码的意义：计算机中存储的最小单元是一个字节即8bit，所能表示的字符范围是255个，而人类要表示的符号太多，无法用一个字节来完全表示，固需要将符号编码，将各种语言翻译成计算机能懂的语言。

1）ASCII码：总共128个，用一个字节的低7位表示，0〜31控制字符如换回车删除等；32~126是打印字符，可通过键盘输入并显示出来；

2）ISO-8859-1,用来扩展ASCII编码，256个字符，涵盖了大多数西欧语言字符。

3）GB2312:双字节编码，总编码范围是A1-A7,A1-A9是符号区，包含682个字符，B0-B7是汉字区，包含6763个汉字；

4）GBK为了扩展GB2312,加入了更多的汉字，编码范围是8140~FEFE，有23940个码位，能表示21003个汉字。

5）UTF-16: ISO试图想创建一个全新的超语言字典，世界上所有语言都可通过这本字典Unicode来相互翻译，而UTF-16定义了Unicode字符在计算机中存取方法，用两个字节来表示Unicode转化格式。不论什么字符都可用两字节表示，即16bit，固叫UTF-16。

6）UTF-8：UTF-16统一采用两字节表示一个字符，但有些字符只用一个字节就可表示，浪费存储空间，而UTF-8采用一种变长技术，每个编码区域有不同的字码长度。  不同类型的字符可以由1~6个字节组成。                                                                                                                                                                                                                       

3、utf-8编码中的中文占几个字节；int型几个字节？
utf-8是一种变长编码技术，utf-8编码中的中文占用的字节不确定，可能2个、3个、4个，int型占4个字节。

4、静态代理和动态代理的区别，什么场景使用？
代理是一种常用的设计模式，目的是：为其他对象提供一个代理以控制对某个对象的访问，将两个类的关系解耦。代理类和委托类都要实现相同的接口，因为代理真正调用的是委托类的方法。

区别：
1）静态代理：由程序员创建或是由特定工具生成，在代码编译时就确定了被代理的类是哪一个是静态代理。静态代理通常只代理一个类；

2）动态代理：在代码运行期间，运用反射机制动态创建生成。动态代理代理的是一个接口下的多个实现类；

实现步骤：a.实现InvocationHandler接口创建自己的调用处理器；b.给Proxy类提供ClassLoader和代理接口类型数组创建动态代理类；c.利用反射机制得到动态代理类的构造函数；d.利用动态代理类的构造函数创建动态代理类对象；

使用场景：Retrofit中直接调用接口的方法；Spring的AOP机制；

5、Java的异常体系
Java中Throwable是所有异常和错误的超类，两个直接子类是Error（错误）和Exception（异常）：

1）Error是程序无法处理的错误，由JVM产生和抛出，如OOM、ThreadDeath等。这些异常发生时，JVM一般会选择终止程序。

2）Exception是程序本身可以处理的异常，又分为运行时异常(RuntimeException)(也叫Checked Eception)和非运行时异常(不检查异常Unchecked Exception)。运行时异常有NullPointerException\IndexOutOfBoundsException等，这些异常一般是由程序逻辑错误引起的，应尽可能避免。非运行时异常有IOException\SQLException\FileNotFoundException以及由用户自定义的Exception异常等。

6、谈谈你对解析与分派的认识。
解析指方法在运行前，即编译期间就可知的，有一个确定的版本，运行期间也不会改变。解析是静态的，在类加载的解析阶段就可将符号引用转变成直接引用。

分派可分为静态分派和动态分派，重载属于静态分派，覆盖属于动态分派。静态分派是指在重载时通过参数的静态类型而非实际类型作为判断依据，在编译阶段，编译器可根据参数的静态类型决定使用哪一个重载版本。动态分派则需要根据实际类型来调用相应的方法。

7、修改对象A的equals方法的签名，那么使用HashMap存放这个对象实例的时候，会调用哪个equals方法？
会调用对象的equals方法，如果对象的equals方法没有被重写，equals方法和==都是比较栈内局部变量表中指向堆内存地址值是否相等。

8、Java中实现多态的机制是什么？
多态是指程序中定义的引用变量所指向的具体类型和通过该引用变量发出的方法调用在编译时不确定，在运行期间才确定，一个引用变量到底会指向哪个类的实例。这样就可以不用修改源程序，就可以让引用变量绑定到各种不同的类实现上。Java实现多态有三个必要条件：继承、重定、向上转型，在多态中需要将子类的引用赋值给父类对象，只有这样该引用才能够具备调用父类方法和子类的方法。

9、如何将一个Java对象序列化到文件里？
ObjectOutputStream.writeObject()负责将指定的流写入，ObjectInputStream.readObject()从指定流读取序列化数据。

//写入
try {
    ObjectOutputStream os = new ObjectOutputStream(new FileOutputStream("D:/student.txt"));
    os.writeObject(studentList);
    os.close();
} catch(FileNotFoundException e) {
    e.printStackTrace();
} catch(IOException e) {
    e.printStackTrace();
}
10、说说你对Java反射的理解
在运行状态中，对任意一个类，都能知道这个类的所有属性和方法，对任意一个对象，都能调用它的任意一个方法和属性。这种能动态获取信息及动态调用对象方法的功能称为java语言的反射机制。

反射的作用：开发过程中，经常会遇到某个类的某个成员变量、方法或属性是私有的，或只对系统应用开放，这里就可以利用java的反射机制通过反射来获取所需的私有成员或是方法。

1) 获取类的Class对象实例 Class clz = Class.forName("com.zhenai.api.Apple");

2) 根据Class对象实例获取Constructor对象  Constructor appConstructor = clz.getConstructor();

3) 使用Constructor对象的newInstance方法获取反射类对象 Object appleObj = appConstructor.newInstance();

4) 获取方法的Method对象  Method setPriceMethod = clz.getMethod("setPrice", int.class);

5) 利用invoke方法调用方法  setPriceMethod.invoke(appleObj, 14);

6) 通过getFields()可以获取Class类的属性，但无法获取私有属性，而getDeclaredFields()可以获取到包括私有属性在内的所有属性。带有Declared修饰的方法可以反射到私有的方法，没有Declared修饰的只能用来反射公有的方法，其他如Annotation\Field\Constructor也是如此。

11、说说你对Java注解的理解
注解是通过@interface关键字来进行定义的，形式和接口差不多，只是前面多了一个@

public @interface TestAnnotation {

}

使用时@TestAnnotation来引用，要使注解能正常工作，还需要使用元注解，它是可以注解到注解上的注解。元标签有@Retention @Documented @Target @Inherited @Repeatable五种

@Retention说明注解的存活时间，取值有RetentionPolicy.SOURCE 注解只在源码阶段保留，在编译器进行编译时被丢弃；RetentionPolicy.CLASS 注解只保留到编译进行的时候，并不会被加载到JVM中。RetentionPolicy.RUNTIME可以留到程序运行的时候，它会被加载进入到JVM中，所以在程序运行时可以获取到它们。

@Documented 注解中的元素包含到javadoc中去

@Target  限定注解的应用场景，ElementType.FIELD给属性进行注解；ElementType.LOCAL_VARIABLE可以给局部变量进行注解；ElementType.METHOD可以给方法进行注解；ElementType.PACKAGE可以给一个包进行注解 ElementType.TYPE可以给一个类型进行注解，如类、接口、枚举

@Inherited 若一个超类被@Inherited注解过的注解进行注解，它的子类没有被任何注解应用的话，该子类就可继承超类的注解；

注解的作用：

1）提供信息给编译器：编译器可利用注解来探测错误和警告信息

2）编译阶段：软件工具可以利用注解信息来生成代码、html文档或做其它相应处理；

3）运行阶段：程序运行时可利用注解提取代码

注解是通过反射获取的，可以通过Class对象的isAnnotationPresent()方法判断它是否应用了某个注解，再通过getAnnotation()方法获取Annotation对象

12、说一下泛型原理，并举例说明
泛型就是将类型变成参数传入，使得可以使用的类型多样化，从而实现解耦。Java泛型是在Java1.5以后出现的，为保持对以前版本的兼容，使用了擦除的方法实现泛型。擦除是指在一定程度无视类型参数T，直接从T所在的类开始向上T的父类去擦除，如调用泛型方法，传入类型参数T进入方法内部，若没在声明时做类似public T methodName(T extends Father t){}，Java就进行了向上类型的擦除，直接把参数t当做Object类来处理，而不是传进去的T。即在有泛型的任何类和方法内部，它都无法知道自己的泛型参数，擦除和转型都是在边界上发生，即传进去的参在进入类或方法时被擦除掉，但传出来的时候又被转成了我们设置的T。在泛型类或方法内，任何涉及到具体类型（即擦除后的类型的子类）操作都不能进行，如new T()，或者T.play()（play为某子类的方法而不是擦除后的类的方法）

13、Java中String的了解
1）String类是final型，固String类不能被继承，它的成员方法也都默认为final方法。String对象一旦创建就固定不变了，对String对象的任何改变都不影响到原对象，相关的任何改变操作都会生成新的String对象。

2）String类是通过char数组来保存字符串的，String对equals方法进行了重定，比较的是值相等。

String a = "test"; String b = "test"; String c = new String("test");

a、b和字面上的test都是指向JVM字符串常量池中的"test"对象，他们指向同一个对象。而new关键字一定会产生一个对象test，该对象存储在堆中。所以new String("test")产生了两个对象，保存在栈中的c和保存在堆中的test。而在java中根本就不存在两个完全一模一样的字符串对象，故在堆中的test应该是引用字符串常量池中的test。

例：

String str1 = "abc"; //栈中开辟一块空间存放引用str1，str1指向池中String常量"abc"
String str2 = "def"; //栈中开辟一块空间存放引用str2，str2指向池中String常量"def"
String str3 = str1 + str2;//栈中开辟一块空间存放引用str3
//str1+str2通过StringBuilder的最后一步toString()方法返回一个新的String对象"abcdef"
//会在堆中开辟一块空间存放此对象，引用str3指向堆中的(str1+str2)所返回的新String对象。
System.out.println(str3 == "abcdef");//返回false
因为str3指向堆中的"abcdef"对象，而"abcdef"是字符池中的对象，所以结果为false。JVM对String str="abc"对象放在常量池是在编译时做的，而String str3=str1+str2是在运行时才知道的，new对象也是在运行时才做的。
14、String为什么要设计成不可变的？
1）字符串常量池需要String不可变。因为String设计成不可变，当创建一个String对象时，若此字符串值已经存在于常量池中，则不会创建一个新的对象，而是引用已经存在的对象。如果字符串变量允许必变，会导致各种逻辑错误，如改变一个对象会影响到另一个独立对象。

2）String对象可以缓存hashCode。字符串的不可变性保证了hash码的唯一性，因此可以缓存String的hashCode，这样不用每次去重新计算哈希码。在进行字符串比较时，可以直接比较hashCode，提高了比较性能；

3）安全性。String被许多java类用来当作参数，如url地址，文件path路径，反射机制所需的Strign参数等，若String可变，将会引起各种安全隐患。


