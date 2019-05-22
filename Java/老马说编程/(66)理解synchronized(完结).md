## 理解synchronized

### 多线程共享内存存在的两个问题

1.竞态条件
竞态条件是指多个线程竞争共享资源时出现的数据不可预见的错误，导致程序最终运行的结果不是我们想要的(跟预期不一样)。

2.内存可见性
内存可见性出现的原因有一部分是硬件的锅，为什么这么说呢？因为目前的计算机系统的内存存储速度与CPU的计算速度完全不是一个量级上的，为了解决这样的问题，通常计算机系统都会设计各种各样的寄存器用于内存和CPU之间的中转站，寄存器的读写速度跟CPU是匹配的，这样数据的流向就变成了从内存到寄存器再到CPU，然后从CPU到寄存器再到内存，在多核CPU下使用多线程处理数据，很可能数据的计算结果还存在各自CPU中的寄存器中呢，线程与线程之间数据的操作完全是不透明的，这样计算出来的结果很可能就是错误的。

### synchronized关键字

synchronized关键字可用于实例方法，静态方法和代码块。

被synchronized修饰的实例方法、静态方法和代码块，内部的代码就变成了原子操作。

#### synchronized修饰实例方法

synchronized修饰实例方法的时候保护的是同一个对象的方法调用，**每个对象都有一个锁和一个等待队列**，当一个线程持有一个对象的锁后，如果其他线程也想执行被synchronized修饰的实例方法，其他线程就需要进入等待队列等待当前线程执行完释放锁，此时进入等待队列的线程状态将变成BLOCKED，当前线程执行完释放了锁，如果此时等待队列中有多个线程，那么将随机唤醒某个线程，这是不确定的。

synchronized保护的是当前实例对象而非代码，只要访问的是同一个对象的synchronized实例方法(或代码块)，即使是不同的代码也会同步执行。

#### synchronized修饰静态方法

synchronized修饰实例方法的时候保护的是当前对象，那么当synchronized修饰静态方法的时候保护的也是对象--类对象。

#### synchronized修饰代码块

```
public class Counter {
    private int count;

    public void incr(){
        synchronized(this){
            count ++;    
        }
    }
    
    public int getCount() {
        synchronized(this){
            return count;
        }
    }
}
```

这里的synchronized保护的就是当前对象。

总结：
**理解多线程的同步问题我们可以参照着单线程模型，比如代码的执行在单线程的时候是一种什么状况，放到多线程模型下呢，程序运行的结果还会不会跟单线程的结果一样，如果不一样，我们就得想办法让它和单线程的一样。**

**举个例子，就拿上面Counter类来说，在单线程下，先执行incr()方法然后再执行getCount()方法，我们拿到的结果是1，现在用多线程来执行(没用synchronized修饰)，比如两个线程，一个执行incr()方法，一个执行getCount()方法，由于incr()方法内部的count++不是原子操作，可能只是执行了0+1这个操作，还没给count变量赋值呢，第二个线程就执行了getCount()方法，得到的结果还是0，但是在单线程中，如果调用了incr()方法，再调用getCount()方法得到的结果就一定是1，不可能是0，多线程下怎么实现单线程的效果呢，我们加上synchronized关键字看看，当第一个线程调用了incr()方法后进入到synchronized修饰的代码块中，第二个线程执行了getCount()方法，发现Counter对象的锁被占用了，进入等待队列等待，当第一个线程把synchronized代码块中的代码执行完后释放了锁，第二个线程被唤醒执行getCount()方法，这样获得的结果就一定是1了。**

### 理解synchronized
- 可重入性
- 内存可见性
- 死锁

#### 可重入性
通过synchronized获得锁是可重入的，对于同一个线程在获得了锁之后再调用需要同样锁的代码时可以直接调用，比如在一个synchronized实例方法内可以直接调用另一个synchronized实例方法，不需要跟其他线程去竞争锁的归属权。

**可重入是通过检查锁的持有线程和持有线程数量来实现的。**
大概步骤如下：
1.一个线程在调用被synchronized保护的代码时，会先检查该对象是否被锁；
2.如果没被锁，那么占用该锁，锁的持有线程就是当前线程，持有线程数量加1，变为了1；
3.如果对象已经被锁了，再检查锁的持有线程是否是当前线程，如果不是，则进入等待队列等待，如果是当前线程，则线程持有数量加1；
4.在释放锁时，会减少持有数量，当数量减到0的时候，就释放整个锁。

#### 内存可见性
synchronized除了可以保证代码的原子性，避免出现竞态条件，还能保证内存可见性，比如下面这段代码：
```
public class Switcher {
    private boolean on;

    public boolean isOn() {
        return on;
    }

    public void setOn(boolean on) {
        this.on = on;
    }
}
```

Switcher类中的这两个方法中的代码都是原子操作，当多个线程访问同一个Switcher对象时，一个线程调用isOn()方法，一个线程调用setOn()方法，虽然都是原子操作，但是能保证程序运行的结果是正确的吗？答案是不能，如果一个线程调用setOn()方法后，虽然on变量值变了，但是还在寄存器中，没有写进内存，那么这个时候另个线程去调用isOn()方法的时候，读上来的值还是原来的值，并没有变。

如果我们给isOn()和setOn()方法加上synchronized关键字修饰，就不会出现这种内存可见性问题，**代码在获得锁的时候会从内存中读最新的值，在释放锁的时候会把最新的值写入内存。**

**保证内存可见性还有一种方式是通过关键字volatile，Java在操作volatitle修饰的变量时会插入特殊的指令，保证读写到的变量值都是最新的内存值，而并非是缓存中的值。**

#### 死锁

死锁现象：有a,b两个线程，a线程已持有锁A，等待锁B，b线程已持有锁B，等待锁A，那么他们就陷入了相互等待的循环中，谁也执行不下去，这就是死锁。

死锁Demo：
```java
public class DeadLockDemo {
    private static Object lockA = new Object();
    private static Object lockB = new Object();

    private static void startThreadA() {
        Thread aThread = new Thread() {

            @Override
            public void run() {
                synchronized (lockA) {
                    try {
                        // 线程A已持有锁A，然后睡1秒
                        Thread.sleep(1000);
                    } catch (InterruptedException e) {
                    }
                    // 申请锁B
                    synchronized (lockB) {
                    }
                }
            }
        };
        aThread.start();
    }

    private static void startThreadB() {
        Thread bThread = new Thread() {
            @Override
            public void run() {
                synchronized (lockB) {
                    try {
                        // 线程B已持有锁B，然后睡1秒
                        Thread.sleep(1000);
                    } catch (InterruptedException e) {
                    }
                    // 申请锁A
                    synchronized (lockA) {
                    }
                }
            }
        };
        bThread.start();
    }

    public static void main(String[] args) {
        startThreadA();
        startThreadB();
    }
}
```

死锁解决或者避免的办法：

- 应该避免代码在持有一个锁的同时去申请另一个锁；、
- 如果确实需要申请多个锁，不同的线程应该按照同样的顺序申请锁。
- Java可以通过jstack命令发现死锁

### 同步容器及其注意事项

#### 同步容器

我们平常使用的HashMap，ArrayList这些容器都不是线程安全的，如果想要使用线程安全的同步容器，我们可以使用Collections类提供的一系列静态方法生成同步容器。

对于同步容器，并不是加了synchronized关键字就代表操作安全了，还需要以下问题需要注意：
- 对容器的复合操作；
- 伪同步；
- 迭代。

**复合操作**

```java
public class EnhancedMap<K, V> {
    Map<K, V> map;
    
    public EnhancedMap(Map<K,V> map){
        this.map = Collections.synchronizedMap(map);
    }
    
    public V putIfAbsent(K key, V value){
         V old = map.get(key);
         if(old!=null){
             return old;
         }
         map.put(key, value);
         return null;
     }
    
    public void put(K key, V value){
        map.put(key, value);
    }
    
    //... 其他代码
}
```

上述代码中EnhancedMap是一个包装类，类中的map是从Collections的`synchronizedMap()`方法获取到的同步容器，包装类中增加了`putIfAbsent(K,V)`方法，该方法先检查map中是否有key对应的值，如果没有，存储k-v值，如果有，返回map中已存有的value值，很明显`putIfAbsent()`和`put()`方法在多线程环境下是有问题的，很有可能出现多个线程在`putIfAbsent()`中根据key获取到的value为null，这违反了`putIfAbsent()`方法原本的语义。

给`putIfAbsent()`方法加上synchronized关键字就安全了吗？

**伪同步**

```java
public synchronized V putIfAbsent(K key, V value){
    V old = map.get(key);
    if(old!=null){
        return old;
    }
    map.put(key, value);
    return null;
}
```

这样做还是有问题的，因为这里同步错了对象，`putIfAbsent()`方法加synchronized关键字同步的是EnhancedMap包装类对象，而`put()`方法中同步的是SynchronizedMap对象，所以`putIfAbsent()`和`put()`方法还是存在竞态条件的。`putIfAbsent()`方法应该改为这样：
```java
public V putIfAbsent(K key, V value){
    synchronized(map){
        V old = map.get(key);
        if(old!=null){
            return old;
        }
        map.put(key, value);
        return null;
    }
}
```

**迭代**

创建同步List对象，一个线程修改List，一个线程对List进行遍历：

```java
private static void startModifyThread(final List<String> list) {
    Thread modifyThread = new Thread(new Runnable() {
        @Override
        public void run() {
            for (int i = 0; i < 100; i++) {
                // 修改List，不断往List中增加元素
                list.add("item " + i);
                try {
                    Thread.sleep((int) (Math.random() * 10));
                } catch (InterruptedException e) {
                }
            }
        }
    });
    modifyThread.start();
}

private static void startIteratorThread(final List<String> list) {
    Thread iteratorThread = new Thread(new Runnable() {
        @Override
        public void run() {
            while (true) {
                // 不断遍历List
                for (String str : list) {
                }
            }
        }
    });
    iteratorThread.start();
}

public static void main(String[] args) {
    final List<String> list = Collections
            .synchronizedList(new ArrayList<String>());
    startIteratorThread(list);
    startModifyThread(list);
}
```

运行程序之后会抛出**并发修改异常**：
```java
Exception in thread "Thread-0"   java.util.ConcurrentModificationException
at java.util.ArrayList$Itr.checkForComodification(ArrayList.java:859)
at java.util.ArrayList$Itr.next(ArrayList.java:831)
```

抛出该异常就是因为遍历容器的过程中容器发生了结构性变化，同步容器并没有解决这个问题，如果要避免这个问题，需要在遍历的时候给整个容器对象加锁(保证在遍历的过程中不要增删同步容器中的元素)：

```java
private static void startIteratorThread(final List<String> list) {
    Thread iteratorThread = new Thread(new Runnable() {
        @Override
        public void run() {
            while (true) {
                synchronized(list){
                    for (String str : list) {
                    }    
                }
            }
        }
    });
    iteratorThread.start();
}
```

### 总结

- synchronized是可重入锁，原理是通过检查锁的持有线程以及持有线程数量；

- 内存可见性，线程在获得锁的时候会从内存中读取共享变量的最新值，在释放锁的时候会把共享变量最新值写入内存；volatile也可以解决内存可见性问题，它是通过插入特殊的代码来使每次读写都是从内存拿的最新的。

- 使用synchronized需要注意死锁问题，当多个线程申请多个相同锁时，尽量保持申请锁的顺序一致；

- 同步容器复合操作、伪同步以及迭代的一些问题。
