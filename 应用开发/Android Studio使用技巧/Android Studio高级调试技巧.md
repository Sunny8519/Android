#### 断点分类
- Java Line Breakpoint(行断点)
- Java Method Breakpoint(方法断点)
- Java Field Breakpoint(字段断点)
- Java Exception Breakpoint(异常断点，支持官方异常)
- Exception Breakpoint(异常断点，支持自定义异常)

#### Java Line Breakpoint(行断点)
行断点是我们平常开发最常见的断点，这里不再介绍。

#### Java Field Breakpoint(字段断点)
字段断点在变量值被修改或者访问的时候被断下，但是不会卡在具体某一行上，仅是卡在类名上，要观察在哪儿被修改，需要查看调用栈。

![Breakpoints_6](https://upload-images.jianshu.io/upload_images/5231076-2059b37b832f70ea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Breakpoints_7](https://upload-images.jianshu.io/upload_images/5231076-69cc50b33690ee27.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里修改confirm字段值的代码就是在onClick中，DialogStyleActivity类的第45行。

#### Java Method Breakpoint(方法断点)
方法断点，顾名思义就是断在方法上的，当触发方法时会被断到该方法的第一行代码。

![Breakpoints_8](https://upload-images.jianshu.io/upload_images/5231076-93cb7f6c660e5db3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### Java Exception Breakpoints（Java异常断点）
Java异常断点；这种断点能够在程序发生异常的那行代码停住，并显示出异常的相关信息，比如异常栈，线程等。

如何设置Java异常断点？
Mac版Android Studio可以通过shift+command+f8可以调出Breakpoints面板，如下图：

![Breakpoints](https://upload-images.jianshu.io/upload_images/5231076-f2bd0affd9d16106.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

红框部分显示的就是Java异常断点，Java异常断点并不依赖于具体的某一行代码(也就是我们不需要像下面这样找到某一行代码再打断点)：

![断点](https://upload-images.jianshu.io/upload_images/5231076-553f35a003009d72.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

只要程序进入Debug模式，选中想要断下的Java异常断点，那么程序在发生异常时会被停在发生异常的那行代码上，这对于我们来说是很方便的。

**应用场景**
典型的应用场景就是我们现在有一块代码被try{}catch(Exception e){}住了，但是try中的代码行数非常多，而且发生的异常就是在这其中，如果我们使用普通断点就得一步步跟下去，直到某一行代码抛出异常我们才知道具体抛出异常的代码在什么地方，但是如果我们使用Java异常断点，就可以直接定位到发生异常的代码行数，效率要高很多。

**注意点**
如果我们在Breakpoints面板中对Java异常断点只勾选了Uncaught exception的话会达不到我们预想中的效果，android studio不会帮我们定位到具体抛出异常的代码。

![Breakpoints_1](https://upload-images.jianshu.io/upload_images/5231076-7f278d8a3ef56857.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

断点效果：

![try...catch...](https://upload-images.jianshu.io/upload_images/5231076-91b6a1f7b7941b29.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这种已经被捕获的异常不会被android studio断下，因此我们看不到任何效果显示这儿发生了异常，没有被try...catch...捕获的异常虽然会被android studio断下，但是也仅仅只是在Debugger面板中看到发生异常的类型，线程等一些信息，具体发生异常的代码并不清楚。

这种是非常明显的未被捕获的异常代码(当前已经处于debug模式且代码已被触发，我们没有看到studio帮我们定位到`person.getAge()`那行代码)：

![Exception](https://upload-images.jianshu.io/upload_images/5231076-5728831d9ea91ef5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

只是在Debugger面板中看到少量信息：

![断点信息](https://upload-images.jianshu.io/upload_images/5231076-d0da5b0e5fe3acea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在我看来这些信息对于我们定位问题是远远不够的。

**但是我们如果勾选了catch exception，效果就如我们预期的了，我们来看下具体效果**

![Breakpoints_2](https://upload-images.jianshu.io/upload_images/5231076-5b8001ab3511ff27.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

或者

![Breakpoints_3](https://upload-images.jianshu.io/upload_images/5231076-dbf784b15eab3ea5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

现在进入debug模式，还是原来的代码，我们看看效果：

![断点2](https://upload-images.jianshu.io/upload_images/5231076-fd1110e098ea4d74.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![断点3](https://upload-images.jianshu.io/upload_images/5231076-de0ca6e9147cf9a7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Breadkpoints_4](https://upload-images.jianshu.io/upload_images/5231076-a98d9d9e157168df.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

well!果然如我们所想...这样就能直接定位到问题所在点。

#### 给断点添加条件
所有断点都能添加断点条件，即符合条件会被断下，不符合的正常执行。

![Breakpoints_5](https://upload-images.jianshu.io/upload_images/5231076-1c440eb3e3ed2665.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**应用场景**
当某个变量的值有多个的时候，在调试时不想被其他值干扰，我们就可以根据我们想要断下的变量值设置一个条件断点，这样只有当变量值符合我们设置的条件时，android studio才会为我们断下这个断点，比如上面的confirm会有两个值，可能为true，可能为false，现在我们只想断下值为true情况下的断点，我们可以在图中1处设置`confirm == true`，这样2处的代码在confirm值为true时会被studio断下，false时则不会。

我们在for循环中遍历一组数组，我们也可以用条件断点来断下我们想要看的值。

在多种类型Item的ListView或者RecyclerView的Adapter中我们只想看某种类型的Item时就可以利用条件断点断下，这样在滑动的过程中不会被不相关的Item干扰。

**注意点**
- 任何种类的断点都可以添加条件；
- 添加条件时一定要保证返回值是boolean类型，不能把"i == 40"写成"i = 40"，这样就变成给变量赋值了；
- 添加条件时要保证条件中的变量到断点处已经被定义了，否则条件表达式是不成立的(PS(技巧):发现条件中的变量为红色就需要检查一下这个变量到断点处是否存在)；

#### 断点时给变量设置自定义值

![Breakpoints_9](https://upload-images.jianshu.io/upload_images/5231076-5c549aec961c1a3d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在Mac中给变量设置自定义值的快捷键是F2，利用设置自定义值我们可以在调试过程中判断不同值的代码是否正常运行。

#### Watches的使用

![Breakpoints_10](https://upload-images.jianshu.io/upload_images/5231076-ad4ce5ae426ea0e9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

有时候我们在代码调试过程中发现图中1处的变量区域有很多变量，有些变量不是我们需要关心的，我们就可以使用图中2处的Watches把我们关心的变量添加到Watches中，调试时一目了然。