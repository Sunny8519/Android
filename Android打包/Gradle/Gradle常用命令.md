## Gradle常用命令
```
./gradlew -v 版本号
./gradlew clean 删除build文件
./gradlew build 检查依赖并编译打包
./gradlew assembleDebug 编译并打debug包
./gradlew assembleRelease 编译并打Release包
./gradlew installRelease release模式打包并安装
./gradlew uninstallRelease 卸载release包
```

### release和debug设置全局变量

比如设置全局debug开关
```
buildTypes {
    release {
        ...
        buildConfigField "boolean", "isDebug", "false"
        ...
    }
    debug {
    ...
        buildConfigField "boolean", "isDebug", "true"
    ...
    }
}
```