## 第1章 Gradle入门

### 1.1 配置Gradle环境

### 1.2 初体验

```java
task hello{
    doLast{
        println 'hello world'
    }
}
```

在文件夹android-gradle-test-code下新建build.gradle文件，在文件中输入上述代码，在控制台进入android-gradle-test-code文件夹下，执行`gradle -q hello`命令，得到运行结果如下：

```
SunnydeMBP:android-gradle-test-code sunny$ gradle -q hello
hello world
```

对于`build.gradle`文件和`gradle -q hello`命令的解释：
- build.gradle是Gradle默认的构建脚本文件，执行Gradle命令的时候会默认加载当前文件夹下的build.gradle脚本文件，当然我们也可以通过`-b`参数指定需要加载的执行文件，就像下面这样：
```
SunnydeMBP:Desktop sunny$ gradle -b android-gradle-test-code/build.gradle -q hello
hello world
```
`android-gradle-test-code`是Desktop下的一个文件夹，`build.gradle`就位于`android-gradle-test-code`文件夹下。上面这句命令的意思就是执行`android-gradle-test-code`文件夹下的`build.gradle`脚本文件中的`hello`任务。`-q`指定了gradle日志输出级别。

- `build.gradle`中定义了一个名为`hello`的任务，并且给任务`hello`添加了一个动作(Action)-`doLast`，其实就是Groovy语言实现的闭包，闭包的理解：业务代码逻辑或者回调实现。`doLast`就意味着在Task执行完毕之后会回调`doLast`这部分闭包的代码实现。

- `println 'hello world'`其实调用的是一个输出字符串的方法`println()`，不过在Groovy中方法的调用可以省略括号，中间以空格分开就好。

- 在Groovy中单引号和双引号都表示字符串

### 1.3 Gradle Wrapper

Wrapper，顾名思义就是对Gradle的包装，Gradle Wrapper的意义在于它便于团队在开发过程中统一Gradle构建版本，无需1.1中配置Gradle开发环境，Wrapper在Windows下是一个批处理脚本，在Linux下是一个shell脚本，当使用Wrapper启动Gradle的时候，Wrapper会先检查相应Gradle版本有没有被下载关联，如果没有会从配置的地址进行下载，然后运行构建。这对开发人员是非常方便的，只要运行Wrapper命令，它就会帮我们搞定一切。

#### 1.3.1 生成Wrapper

Gradle内置了wrapper task帮我们自动生成wrapper所需的目录文件，进入项目的根目录执行如下命令即可生成：
```
SunnydeMBP:android-gradle-test-code sunny$ gradle wrapper

BUILD SUCCESSFUL in 0s
1 actionable task: 1 executed
```

生成的文件结构如下：
```
|--gradle
    |--wrapper
        |--gradle-wrapper.jar
        |--gradle-wrapper.properties
|--gradlew
|--gradlew.bat
```

gradlew和gradlew.bat分别是Linux和Windows下的可执行脚本文件，它们的用法与Gradle原生命令一样，Gradle怎么用他们就可以怎么用，比如上面用Gradle原生命令执行任务hello，我们也可以使用gradlew来执行：

```
SunnydeMBP:android-gradle-test-code sunny$ ./gradlew -q hello
hello world
```

说明：./表示当前文件夹下，所以`./gradlew`表示执行当前文件夹下的`gradlew`脚本文件。

`gradle-wrapper.jar`是具体业务逻辑实现的jar包，gradlew最终还是使用Java执行的这个jar包来执行相关Gradle操作，`gradle-wrapper.properties`是配置文件，用于配置使用哪个版本的Gradle。

以上这些生成好的相关wrapper文件，一般都需提交到版本控制系统中(比如Git)，这样其他人就可以使用已经配置好的统一的Gradle进行构建开发。

#### 1.3.2 Wrapper配置

当我们在控制台通过命令行生成Wrapper文件的时候，我们还可以指定一些参数来控制Wrapper的生成，如下：

![屏幕快照 2019-05-02 下午6.56.31.png](https://upload-images.jianshu.io/upload_images/5231076-e5128a79f354f812.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

使用方法如下：
```
$ gradle wrapper --gradle-version 4.4
```
这就意味着我们配置的wrapper使用4.4版本的Gradle，它会影响`gradle-wrapper.properties`配置文件中`distributionUrl`属性的值，该值的规则是：
`http\://services.gradle.org/distributions/gradle-${gradleVersion}-bin.zip`。

如果我们在执行命令：`gradle wrapper`时没有指定任何参数，那么生成的wrapper将使用当前Gradle版本，如果当前我们使用的Gradle环境是4.4版本的，那么我们生成的wrapper
也是4.4版本的。

#### 1.3.3 gradle-wrapper.properties配置文件字段解析

配置字段如下：

![屏幕快照 2019-05-02 下午7.20.43.png](https://upload-images.jianshu.io/upload_images/5231076-db0cf6430e6b9347.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

`zipStoreBase`和`zipStorePath`组合在一起，是存放下载的Gradle zip文件的地方；
`distributionBase`和`distributionPath`是解压Gradle zip之后的文件存放的地方。

`zipStoreBase`和`distributionBase`有两种取值：`GRADLE_USER_HOME`和`PROJECT`

`GRADLE_USER_HOME`表示用户目录。 
在windows下是`%USERPROFILE%/.gradle`，例如`C:\Users\<user_name>\.gradle\` 
在linux下是`$HOME/.gradle`，例如`~/.gradle`

`PROJECT`表示项目的当前目录，即gradlew所在的目录。

```java
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-4.4-bin.zip
```

这里配置文件的意思是从这个地址(https://services.gradle.org/distributions/gradle-4.4-bin.zip)下载Gradle文件到`~/.gradle/wrapper/dists`文件夹下，
该文件夹下还会生成二级目录，比如这里下载的是`gradle-4.4-bin.zip`，那么Gradle zip文件存放的全路径是`~/.gradle/wrapper/dists/gradle-4.4-bin/<url-hash>/`
这里的`<url-hash>`是根据distributionUrl路径字符串计算md5值得来的。同样的，下载完Gradle zip文件之后，需要把zip文件解压到
`~/.gradle/wrapper/dists/gradle-4.4-bin/<url-hash>/`文件夹下。

#### 1.3.4 自定义Wrapper Task

`gradle-wrapper.properties`是`gradle wrapper`生成的配置文件，当中的`distributionUrl`可以通过`--gradle-version`指定Gradle版本，还有一种方法可以指定
`distributionUrl`的Gradle版本，就是自定义Wrapper Task，在自定义的Wrapper Task中指定对应Gradle版本，如下：

```groovy
task wrapper(type: Wrapper){
    gradleVersion = '2.4'
    archiveBase = 'GRADLE_USER_HOME'
    archivePath = 'wrapper/dists'
    distributionBase = 'GRADLE_USER_HOME'
    distributionPath = 'wrapper/dists'
    distributionUrl = 'https\://services.gradle.org/distributions/gradle-2.4-bin.zip'
}
```

### 1.4 Gradle日志

#### 1.4.1 Gradle日志级别及使用

![屏幕快照 2019-05-02 下午8.09.44.png](https://upload-images.jianshu.io/upload_images/5231076-1be3e623933d5029.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

使用：
```
# 输出任务tasks中QUIET级别及以上日志
$ gradle -q tasks
# 输出任务tasks中INFO级别及以上日志
$ gradle -i tasks
```

![屏幕快照 2019-05-02 下午8.13.10.png](https://upload-images.jianshu.io/upload_images/5231076-39870b9e90a4a739.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 1.4.2 输出错误堆栈信息

![屏幕快照 2019-05-02 下午8.15.57.png](https://upload-images.jianshu.io/upload_images/5231076-1c64311a0e2acd68.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

使用方法跟日志差不多，也是通过开关来控制，通常情况下我们会使用`-s`，因为输出的错误堆栈比较简练易看，而且也能帮助我们排查大多数问题。


#### 1.4.3 自己使用日志调试

我们在编译Gradle脚本的时候，有时候需要打印出一些变量的值出来看是否符合我们的预期，这和我们写Java代码一样，这时就需要我们调用日志相关的方法打印我们想要的信息，我们可以使用
`println`，它会被定向为QUIET级别的日志；我们还可以使用内置的logger来灵活的打印不同级别的日志：

![屏幕快照 2019-05-02 下午8.21.24.png](https://upload-images.jianshu.io/upload_images/5231076-ade77e05816e01da.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 1.5 Gradle命令行

#### 1.5.1 tasks任务

使用：
```
$ ./graldew tasks
```

该命令会打印出所有的任务。

#### 1.5.2 help任务

使用：
```
$ ./gradlew help --task tasks
```

打印出任务tasks帮助信息

比如我们通过help任务打印出tasks任务的帮助信息：

```
SunnydeMBP:android-gradle-test-code sunny$ ./gradlew help --task tasks

> Task :help 
Detailed task information for tasks

Path
     :tasks

Type
     TaskReportTask (org.gradle.api.tasks.diagnostics.TaskReportTask)

Options
     --all     Show additional tasks and detail.

Description
     Displays the tasks runnable from root project 'android-gradle-test-code'.

Group
     help


BUILD SUCCESSFUL in 0s
```

从帮助信息中我们可以看到Options中有`--all`参数，然后我们在执行tasks任务时就可以带上`--all`参数来打印出其他任务(包括非内置任务):

```
SunnydeMBP:android-gradle-test-code sunny$ ./gradlew tasks --all

> Task :tasks 

------------------------------------------------------------
All tasks runnable from root project
------------------------------------------------------------

Build Setup tasks
-----------------
init - Initializes a new Gradle build.
wrapper - Generates Gradle wrapper files.

Help tasks
----------
buildEnvironment - Displays all buildscript dependencies declared in root project 'android-gradle-test-code'.
components - Displays the components produced by root project 'android-gradle-test-code'. [incubating]
dependencies - Displays all dependencies declared in root project 'android-gradle-test-code'.
dependencyInsight - Displays the insight into a specific dependency in root project 'android-gradle-test-code'.
dependentComponents - Displays the dependent components of components in root project 'android-gradle-test-code'. [incubating]
help - Displays a help message.
model - Displays the configuration model of root project 'android-gradle-test-code'. [incubating]
projects - Displays the sub-projects of root project 'android-gradle-test-code'.
properties - Displays the properties of root project 'android-gradle-test-code'.
tasks - Displays the tasks runnable from root project 'android-gradle-test-code'.

Other tasks
-----------
hello


BUILD SUCCESSFUL in 0s
```

我们会发现比正常执行`./gradlew tasks`多了一些东西(任务以分组形式列出)，就是：

```
Other tasks
-----------
hello
```

这是我们自定义的任务，也在这里显示出来了。

#### 1.5.3 强制刷新依赖

我们开发的功能不可避免的要使用到一些第三方依赖，但是Gradle对于第三方依赖是有缓存的，缓存的目的在于不用每次编译都去下载依赖，一般情况下像Maven这些工具都会自动
管理缓存的更新，不用我们手动操作，但是也有例外的情况，比如Version里面的代码改了，或者联调测试时使用的snapshot版本，这时我们就需要强制刷新依赖，如下命令：
```
./gradlew --refresh-dependencies assemble
```

#### 1.5.4 多任务调用

有时候我们需要同时运行多个任务，比如在jar之前先通过clean任务把之前的class文件清理掉，我们可以使用这样的命令执行：
```
./gradlew clean jar
```
意思为先执行clean再执行jar生成jar包。

#### 1.5.5 通过任务名字缩写执行任务

有时候我们的任务名字很长，要全部打出来还是要废不少时间的，Gradle提供了基于驼峰命名法的缩写调用，比如任务connectCheck，我们可以这样执行：`./gradlew cc`。