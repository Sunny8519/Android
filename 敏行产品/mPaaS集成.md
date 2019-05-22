### mPaaS集成

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

mpxcx

com.minxing.zzbank


1.在控制台创建应用，根据包名下载配置文件；
2.使用mPaaS插件的mPaaS Project install功能迁移现有项目，肯定会发生冲突；
3.