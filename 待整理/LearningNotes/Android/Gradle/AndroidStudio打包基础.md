## AndroidStudio打包基础

### AndroidStudio在没有配置打包flavor的时候一般只能打四种类型的apk

1.未签名debug apk
未签名debug apk不需要签名配置，直接使用assembleDebug就能生成app-debug.apk包：
<img src="https://upload-images.jianshu.io/upload_images/5231076-0f3ee31101c4d7f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" width = 50% height = 50% />
s
2.未签名release apk
经过实践，在android studio没有配置过签名文件时，如果通过assembleRelease去打release包是不会成功的，只有配置了签名文件，才能打出app-release-unsigned.apk包，配置签名文件的步骤：

![image.png](https://upload-images.jianshu.io/upload_images/5231076-ac8a16cc2d99a515.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/5231076-9770a67cfe269863.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/5231076-28990523697b7cd4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
这里已经填了一些信息是我已经配置了KeyStore，如果没配置的话，这里的输入框都是空的，我们直接点击create new...下一步就行。

![image.png](https://upload-images.jianshu.io/upload_images/5231076-0312283b71400184.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
到这里我们就按图片上的标注来填对应信息就行了，最后点ok生成我们的keystore，到此，我们就有了apk打包签名所需的秘钥了。

这个时候，我们再去点击assembleRelease去生成未签名的release apk就能够成功了，生成的apk位于主module下(一般为app module)的build文件夹下，具体见图：
![image.png](https://upload-images.jianshu.io/upload_images/5231076-e6ca8d859835cb90.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
unsigned表示的是未签名。

3.签名debug apk
一般的我们都不会去生成签名的debug apk(又不拿debug包上架，没有什么卵用...)，但我们还是简单介绍下，想要生成带有签名的debug apk，需要我们配置一下gradle文件，配置gradle文件有两种方式，一种是我们手动在build.gradle文件中添加，一种是通过android studio提供的图形化界面配置，然后studio帮我们在build.gradle文件中生成，个人建议使用图形化界面(别手动敲错了，虽然没有敲代码装逼，但是妥妥的啊，放心...)，这里我们两种都说一下，其实最终的效果都是一样的。先来说图形化界面配置：
![image.png](https://upload-images.jianshu.io/upload_images/5231076-658275492cddfc01.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
在`Build`工具栏中打开`Edit Build Types...`

![image.png](https://upload-images.jianshu.io/upload_images/5231076-483831492d0b0008.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
点击加号按钮填上我们已有的KeyStore的相关信息，然后跳转到`Build Types`页面配置debug版本的配置信息：

![image.png](https://upload-images.jianshu.io/upload_images/5231076-2771d4ed5d845927.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
在`Signing Config`中配置上我们刚才在`signing`中配置的config，点击ok完事，等着studio帮你自动生成好就行了。

细心的人应该就能发现在app下的build.gralde文件发生了一点点变化：
原来这里是这样的
![image.png](https://upload-images.jianshu.io/upload_images/5231076-befc9761e6c0ce0b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
现在是这样的
![image.png](https://upload-images.jianshu.io/upload_images/5231076-71fcb121e5e9fb11.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
没错，我们如果使用手动添加build.gradle配置的话也是这些内容，现在我们使用assembleDebug生成的debug包就是带签名的。

4.签名release apk
带签名的release apk配置和带签名的debug apk基本相似，只不过它需要在release中配置：
![image.png](https://upload-images.jianshu.io/upload_images/5231076-0296e96429edc2ba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
这样通过assembleRelease就可以生成app-release.apk包了，生成的包位于app模块下的build/outputs/apk/release/文件夹下。
