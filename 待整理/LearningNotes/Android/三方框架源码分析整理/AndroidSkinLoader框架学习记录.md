## 一.动态权限申请
为什么这里涉及到了android 6.0之后的动态权限申请呢，因为在加载皮肤包的时候，需要读取磁盘权限才能将皮肤包加载进内存中，但是呢我们加载皮肤包的过程和逻辑应放在一个单例中进行，因为这是一个比较重的操作，没有必要每次都去加载皮肤包，放在单例中就意味着我们可能要在Application中初始化这个单例对象，那么问题来了：**Application中能动态申请权限吗？**

为了了解这个问题，我们有必要重新认识一下android的动态权限申请机制。

### 1.动态权限涉及到的重要方法
```
检查app是否已经拥有指定权限了：
ContextCompat.checkSelfPermission(@NonNull Context context, @NonNull String permission)

申请权限：
ActivityCompat.requestPermissions(final @NonNull Activity activity,final @NonNull String[] permissions, final int requestCode)

检测用户上次对这个指定权限的具体操作，即上次是否拒绝过(没有勾选“不再提醒”)这个权限的申请，如果有这个方法返回true，如果没有返回false
ActivityCompat.shouldShowRequestPermissionRationale(@NonNull Activity activity,@NonNull String permission)

#Activity
@Override
onRequestPermissionsResult(int requestCode, @NonNull String[] permissions,@NonNull int[] grantResults)
```
android 6.0之后的动态权限申请基本上就依靠这四个方法来实现。所有的动态权限申请框架都是在这四个方法之上做了更方便使用者调用的封装。

从上面四个方法来看，我们大概能注意到后面三个方法都是跟Activity有着密切的联系，其实除了`onRequestPermissionsResult()`方法是Activity中的，`requestPermissions()`和`shouldShowRequestPermissionRationale()`在Activity中也有，ActivityCompat的底层还是调用的Activity中对应的方法，所以，从上面这些方法依赖Activity的情况来看，在Application中动态申请权限第一个要解决的问题就是如何拿到一个Activity实例。
拿到Activity的实例的一个解决方案就是通过`Application.ActivityLifecycleCallbacks`接口，通过该接口中的方法我们可以拿到应用程序出现的每个Activity实例。

在Application中动态申请权限一直是困扰我的一个问题，因为在实践插件化换肤的时候就碰到了加载皮肤包需要读取存储卡权限的问题，在封装加载皮肤包逻辑的时候需要把获取存储卡权限封装到加载皮肤包逻辑所在的单例中，单例作为全局唯一的一份放在Application中初始化，这个时候就会碰到在Application中动态申请权限的问题了。但是现在想想其实我的初衷是为了让加载皮肤包的那段逻辑不会得到重复执行，也没必要非得在Application中初始化，我们可以在单例中做一个皮肤包是否已加载的判断条件就完事了，如果已经加载就不再加载，没有加载就执行我们的加载逻辑。


相关开源框架：
[AndPermission](https://github.com/yanzhenjie/AndPermission)
[easypermissions](https://github.com/googlesamples/easypermissions)
[RxPermissions](https://github.com/tbruyelle/RxPermissions)

关于动态权限相关参考文章：
[抽丝剥茧：理解Android权限机制](https://www.cnblogs.com/0xJDchen/p/6806573.html)
[(Android 9.0)动态权限运行机制源码分析](https://blog.csdn.net/lj19851227/article/details/82084227)
[Android 动态权限设计 （权限的申请与处理）](https://www.jianshu.com/p/8e37e9cf20a5)
[Android 动态权限适配方案总结](https://blog.csdn.net/yuguqinglei/article/details/80375702)

[使用AOP的方式动态申请权限](https://www.jianshu.com/p/2324a2bdb3d4)
[一句代码搞定权限请求，从未如此简单](https://www.jianshu.com/p/c69ff8a445ed)
[Android 6.0 运行时权限管理最佳实践](https://juejin.im/post/57d5de3e2e958a00546a7465)
1.android6.0时权限分为了Normal Permissions和Dangerous Permissions：
- Normal Permissions只需要在AndroidManifest.xml文件中声明就可以了，app在安装时就授权了这些权限，并且这些权限是不能撤销的，除非卸载App；
- Dangerous Permissions是以权限组的形式存在的，只要授权了权限组中的某个权限，那么该权限组中的其他权限也会被授权。
- 



[Android 8.0 运行时权限策略变化和适配方案](https://juejin.im/post/5991476f5188254898192ab9)
[巧用Fragment，解耦Android6.0权限申请手记](https://juejin.im/post/59e081f96fb9a0452340e44b)
[EasyAndroid基础集成组件库之：EasyPermissions 动态权限申请库](https://juejin.im/post/5b1a2a326fb9a01e5d32f208)
[国产 Android 权限申请最佳适配方案 —— permissions4m](https://juejin.im/post/59b5e1a76fb9a00a6a7410d6)
[Android权限检查API checkSelfPermission失效问题](https://juejin.im/post/59e01ece51882578c6736db7)


问题记录：
1.ActivityCompat和ContextCompat类的用途？
2.Activity中的finishAffinity()方法有什么用途？

