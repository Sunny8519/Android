## mPaaS集成

配置gradle环境变量：
```java
open -e .bash_profile
```

.bash_profile文件中加入下内容：
```java
GRADLE_HOME=/Users/sunny/gradle/gradle-4.4
export GRADLE_HOME
export PATH=$PATH:$GRADLE_HOME/bin
```

保存更新.bash_profile文件
```java
source ~/.bash_profile
```

验证gradle版本
```java
gradle -version
```

[MAC JDK环境配置](https://www.cnblogs.com/dingzhijie/p/7016397.html)

[android studio keystore生成](http://www.cnblogs.com/jeffen/p/6824722.html)

### mPaaS管理端登录名和密码
dinglb@dehuinet.com 
yammersns_123

### 现有项目集成mPaaS步骤
1、拉取client和kit的代码

2、准备mPaaS的开发环境：https://tech.antfin.com/docs/2/99034

3、在控制台创建应用，根据包名下载配置文件；

4、迁移：https://tech.antfin.com/docs/2/109312

5、mPaaS project install。conver肯定会失败，然后提示看：https://tech.antfin.com/docs/2/109312

6、mPaaS插件在mxclient_android/MXAndroid/MXClient/src下生成了一个main文件夹。由于我们的代码结构和现在标准的android工程文件结构不一致，所以需要我们手动将main文件夹下的各文件转移到相应的位置，同时删除原来的文件。

7、MXApplication原先继承com.minxing.kit.MXApplication，改为继承com.alipay.mobile.quinox.LauncherApplication

8、MXClient中的build.gradle中删除implementation 'com.android.support:multidex:1.0.3'，MXApplication中删除MultiDex.install(this);

9、在MXClient中的build.gradle中，mpaascomponents里面加上：
```groovy
excludeDependencies=["com.alipay.android.phone.mobilecommon:lbs-build"]
excludeDependencies=["com.alipay.android.phone.thirdparty:androidsupport-build"]
```

10、生成的MockLauncherActivityAgent的postInit中，添加自己的启动类LoadingActivity

11、AndroidManifest中移除： 
```xml
<service android:name="com.amap.api.location.APSService" />
```

12、MXClient的build.gradle中的dependencies中增加：
`implementation 'com.android.support.constraint:constraint-layout:1.1.3'`(这一步视情况添加)

13、SwipeToLoadLayout中的STYLE改为STYLES，修改相关代码

14、删除`libpatcher.so`和`libstlport_shared.so`库