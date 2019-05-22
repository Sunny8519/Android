### JNI入门实践

本次实践采用android studio3.2.1

#### 准备
创建一个新项目，如图，勾选上`include C++ support`
![创建项目](https://upload-images.jianshu.io/upload_images/5231076-5fa689c6124cedf9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果以前没有开发过JNI，那么需要下载NDK，CMake等工具，按照AS的提示走就行...

当项目创建好之后，发现app module下的目录结构多了个一个文件夹和一个文件，如图：
![JNI项目目录结构](https://upload-images.jianshu.io/upload_images/5231076-4c4fe8c152749681.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

cpp文件夹下的native-lib.cpp文件是AS为我们创建的一个example，我们可以参考这个文件写c++源码，放在这里的.cpp文件都是可以打入最终的so包的，不过要想打入so包，我们还需要一项配置：
我们点击打开CMakeLists.txt文件，看下大致的文件内容...
```
# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.4.1)

add_library( # Sets the name of the library.
             native-lib

             # Sets the library as a shared library.
             SHARED

             # Provides a relative path to your source file(s).
             src/main/cpp/native-lib.cpp )

find_library( # Sets the name of the path variable.
              log-lib

              # Specifies the name of the NDK library that
              # you want CMake to locate.
              log )

target_link_libraries( # Specifies the target library.
                       native-lib

                       # Links the target library to the log library
                       # included in the NDK.
                       ${log-lib} )
```

除去注释没多少代码...

我们首先来看`cmake_minimum_required(VERSION 3.4.1)`这行代码指定了编译so库所需要的最小的CMake版本(3.4.1)，一般情况下不用我们管，在创建项目的时候AS就帮我们写好了。

再来看看

```
add_library( # Sets the name of the library.
             native-lib

             # Sets the library as a shared library.
             SHARED

             # Provides a relative path to your source file(s).
             src/main/cpp/native-lib.cpp )
```

`add_library`的作用就是配置编译生成的so库名字、是否是动态链接库以及编译生成so库的源码路径，`native-lib`就是编译生成的so库名称，**我们可以对它进行修改，根据功能或者用途命名一个容易意会的名字**，`SHARED`表示编译生成的是一个动态链接库(?【1】)，`src/main/cpp/native-lib.cpp`指的是编译成so库的源码路径，这里可以指定多个文件，比如这样：

```
add_library( # Sets the name of the library.
             native-lib

             # Sets the library as a shared library.
             SHARED

             # Provides a relative path to your source file(s).
             src/main/cpp/native-lib.cpp 
             src/main/cpp/native-lib1.cpp 
             ... )
```
**只有在这里指定了的文件才会被打入so库。**

接下来我们再看看

```
find_library( # Sets the name of the path variable.
              log-lib

              # Specifies the name of the NDK library that
              # you want CMake to locate.
              log )
```

`find_library`是用来指定编译so库的c++源码中所依赖的一些库，`log_lib`是log库的别名，这是为了方便引用，log日志库是我们在c++层面打印日志的，它是NDK提供的。

接下来是`target_link_libraries`

```
target_link_libraries( # Specifies the target library.
                       native-lib

                       # Links the target library to the log library
                       # included in the NDK.
                       ${log-lib} )
```

`target_link_libraries`的作用是把我们即将编译的so库和指定的库(三方库或系统库)相关联，示例里关联的是log日志库(系统库)。

#### 编译多个so库

上面的示例只能编译出一个so库，但是有时候我们想把不同的源文件编译成不同的so库，我们可以按照下面这种方式尝试：
![多so库打包示例-1](https://upload-images.jianshu.io/upload_images/5231076-a552a2f637aed797.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/310)

在main下新建两个文件夹用于存放CMakeLists.txt和.cpp源码文件，每个文件夹下的源码文件表示要打包成一个so库，这样，包括app下的一个CMakeLists.txt，我们一共会打包生成3个so库，光新建文件夹和文件肯定是不能打包成功的，我们还需要一些配置...

先来看看`lib1_res`和`lib2_res`文件夹下的CMakeLists.txt文件，挑一个看就Ok:
```
add_library( # Sets the name of the library.
        lib-1

        # Sets the library as a shared library.
        SHARED

        # Provides a relative path to your source file(s).
        lib1.cpp )


target_link_libraries( # Specifies the target library.
        lib-1

        # Links the target library to the log library
        # included in the NDK.
        ${log-lib} )
```

我们发现只写了`add_library`和`target_link_libraries`，为什么没了`cmake_minimum_required(VERSION 3.4.1)`和`find_library`，我们再来看看app目录下的CMakeLists.txt文件：

```
cmake_minimum_required(VERSION 3.4.1)

add_library( # Sets the name of the library.
             native-lib

             # Sets the library as a shared library.
             SHARED

             # Provides a relative path to your source file(s).
             src/main/cpp/native-lib.cpp )

find_library( # Sets the name of the path variable.
              log-lib

              # Specifies the name of the NDK library that
              # you want CMake to locate.
              log )

target_link_libraries( # Specifies the target library.
                       native-lib

                       # Links the target library to the log library
                       # included in the NDK.
                       ${log-lib} )

ADD_SUBDIRECTORY(src/main/lib1_res)
ADD_SUBDIRECTORY(src/main/lib2_res)
```

多了最后两行代码，这两行代码的意思就是指定子配置文件(CMakeLists.txt)的路径，由此，我们可以猜测CMakeLists.txt文件也是有继承特性的，我们只需要在子配置文件中写上不同于父文件的部分代码即可。

做完这些我们就可以打包尝试一下了...

![build](https://upload-images.jianshu.io/upload_images/5231076-cffc1289dd0ded75.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/310)

最后生成的so库在`app/build/intermediates/cmake/`路径下，包括不同平台的以及不同版本的(debug版和release版)，当然这些应该都是可以配置的。

![so库路径](https://upload-images.jianshu.io/upload_images/5231076-a30ffe41a06bc685.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/310)

从上图红框中我们可以发现每个生成的so库名称前面都会自动加上`lib`前缀。

刚才说某些打包的行为是可以通过配置来改变的，比如：
1.我们可以改变打完包后生成so库所在的文件夹
- 在app目录下的CMakeLists.txt文件中加上下面这行代码：
```
#设置生成的so动态库最后输出的路径
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/src/main/jniLibs/${ANDROID_ABI})
```

经尝试没有成功(【2】)
2.生成不同平台的so库可以通过gradle来配置
- 在app module下的gradle文件中添加如下配置：
```
ndk {
    abiFilters "armeabi-v7a", "arm64-v8a"
}
```
意思是只生成这两个平台的so库。

完整配置如下：
![gradle配置](https://upload-images.jianshu.io/upload_images/5231076-af1cfb45491dc38a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/620)


#### 参考
[AndroidStudio项目CMakeLists解析](https://www.cnblogs.com/chenxibobo/p/7678389.html)

#### 问题
【1】动态链接库是什么？
【2】`set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/src/main/jniLibs/${ANDROID_ABI})`为什没有生效？