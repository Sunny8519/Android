## 一. File类

### 1. 创建文件

```java
//File类
public boolean createNewFile() throws IOException
```

该方法只能创建文件，而不能创建文件夹，如果创建文件成功返回true，创建文件失败返回false(比如创建的文件已存在)。创建文件的路径已经在File对象中已经给出。

### 2. 创建文件夹

```java
//File类
public boolean mkdir()
```

创建单层文件夹。

```java
//File类
public boolean mkdirs()
```

创建单层或者多层文件夹，推荐使用该方法创建文件夹。

创建的文件夹路径在File对象中已经给出。

### 3. 删除文件或者文件夹

```java
//File类
public boolean delete()
```

该方法能删除文件或者文件夹，删除成功返回true，删除失败返回false。删除的文件或者文件夹路径已经在File对象中给出。另：使用此方法删除的Windows文件是不会经过回收站的，直接从硬盘删除，使用不小心的话，删除的文件或者文件夹就找不回来了。

### 4. File类的获取功能

```java
//File类
public String[] list()
```

该方法可获取指定路径下的所有文件和文件夹(只有文件和文件夹的名字，不包括父路径)，包括隐藏或者其他一些看不见的文件。

```java
//File类
public File[] listFiles()
```

该方法可获取制定路径下的所有文件和文件夹的File对象，从File对象中可以获得文件或者文件夹的完整路径名。

```java
//File类
public static File[] listRoots()
```

该方法是静态的，获取系统的所有根目录。

### 5. 文件过滤

```java
public File[] listFiles(FileFilter filter)

@FunctionalInterface
public interface FileFilter {

    /**
     * Tests whether or not the specified abstract pathname should be
     * included in a pathname list.
     *
     * @param  pathname  The abstract pathname to be tested
     * @return  <code>true</code> if and only if <code>pathname</code>
     *          should be included
     */
    boolean accept(File pathname);
}
```

FileFilter是一个接口，该接口的功能就是实现文件的筛选条件，因此，我们在调用listFiles(FileFilter)方法的时候需要自己实现一个FileFilter对象。

判断文件是否为文件夹：`File.isDirectory()`

```java
private static void traversalDir(File dir) {
        File[] files = dir.listFiles();
        if (files == null) return;

        for (File file : files) {
            if (file.isDirectory()) {
                traversalDir(file);
            } else {
                System.out.println(file);
            }
        }
    }
```

上面这种写法就是递归遍历所有的文件夹，并打印出所有的文件。

递归调用中一定要有出口，不然肯定会出现死循环，并且递归调用的次数不能过多，过多的话很可能出现StackOverFlowError。

### 6. 利用递归实现阶乘

计算阶乘5!

```java
private int factorial(int n){
    if(n == 1){
        return 1;
    }
    return n * factorial(n - 1);
}

factorial(5);//5!
```

### 7. 斐波那契数列

```java
1 1 2 3 5 8 13
```

类似于上面这种规律的数列就是斐波拉契数列，每项是前面两项之和(除了前两项)。

使用递归实现斐波拉契数列：

```java
private int getFBLQ(int n){
    if(n == 1 || n == 2){
        return 1;
    }
    return getFBLQ(n-1)+getFBLQ(n-2);
}
```

### 8. 利用递归和FileFilter实现过滤Java文件

```java
   private static void traversalDir(File dir) {
        File[] files = dir.listFiles(new CustomFilenameFilter());
        if (files == null) return;

        for (File file : files) {
            if (file.isDirectory()) {
                traversalDir(file);
            } else {
                System.out.println(file);
            }
        }
    }

    static class CustomFilenameFilter implements FilenameFilter {

        @Override
        public boolean accept(File dir, String name) {
            return dir.isDirectory() || name.toLowerCase().endsWith(".java");
        }

    }
```

## 二. IO流

IO流的输入输出是相对于内存来说的，往内存中写叫做输入，往内存以外的地方，比如磁盘，这叫做输出，写入文件的过程其实就是一个往磁盘中写入数据的过程，这也叫做输出；把文件中的数据读取到内存中就叫做输入。

### 1. 字节输出流OutputStream

OutputStream是Java中所有字节输出流的父类，它是一个抽象类。

### 2. FileOutputStream类

文件输出流，该类的作用就是用来把数据写入文件的

## [函数调用的基本原理](https://mp.weixin.qq.com/s/ufjn4U5HAtTuPyXzfSOGnA)

总结：

1.函数在调用过程中是使用栈内存来保存数据的，包括函数的参数，返回地址，以及函数中定义的局部变量；

2.函数的返回值我们可以简单的理解为存放在一个单独的返回值存储器中；

3.函数的参数以及局部变量都只有在函数被调用的时候才会在栈内存中分配内存空间，当函数调用完之后，就会被释放，但是如果参数或者局部变量是数组或者对象的话，当函数调用完之后回收的是栈内存中保存数组和对象堆内存地址的空间。

4.只要是调用函数都会有一定的成本，每一次的调用都需要在栈内存中分配空间去存储参数、局部变量以及返回地址，还要进行额外的入栈和出栈操作。