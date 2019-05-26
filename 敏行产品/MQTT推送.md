## MQTT

> MQTT官网：http://mqtt.org/
MQTT介绍：http://www.ibm.com
MQTT Android github：https://github.com/eclipse/paho.mqtt.android
MQTT API：http://www.eclipse.org/paho/files/javadoc/index.html
MQTT Android API： http://www.eclipse.org/paho/files/android-javadoc/index.html

### MQTT特性

- 发布/订阅协议
- 一对多消息分发
- 基于TCP/IP
- 消息传送提供了三种服务质量：
    - “至多一次”，根据低层因特网协议尽最大努力传输消息，可能会丢失消息；
    - “至少一次”，保证消息抵达，但可能出现重复；
    - “刚好一次”，确保只收到一条消息，但可能会出现丢失消息或重复消息。
- 能最大限度的减少网络流量
- 具有“遗嘱”功能

### 基本概念

- topic：在MQTT中订阅了同一topic的客户端会同时收到消息推送，实现“群聊”功能
- clientid：客户端身份唯一标识
- retained：是否要保留最后的断开连接信息
- subscribe()：订阅某个topic
- publish()：向某个topic发送消息，之后服务端会推送给所有订阅了此topic的客户端
- username：连接到MQTT服务器的用户名
- password：连接到MQTT服务器的密码

### MQTT基本使用步骤

**服务器搭建：**
https://blog.csdn.net/ytangdigl/article/details/77740100
https://blog.csdn.net/qq_28535319/article/details/80064942

**Android端使用：**

```xml
// gradle依赖
repositories {
    maven {
        url "https://repo.eclipse.org/content/repositories/paho-releases/"
    }
}

dependencies {
    implementation 'org.eclipse.paho:org.eclipse.paho.client.mqttv3:1.1.0'
    implementation 'org.eclipse.paho:org.eclipse.paho.android.service:1.1.0'
}
// 或者直接下载jar包
// https://repo.eclipse.org/content/repositories/paho-releases/org/eclipse/paho/org.eclipse.paho.client.mqttv3/

<!--添加权限-->
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.WAKE_LOCK" />

<!--注册Service-->
<service android:name="org.eclipse.paho.android.service.MqttService" />
```

1.创建`MqttAndroidClient`和`MqttConnectOptions`，对于`MqttAndroidClient`最主要的是对于`MqttAsyncClient`的封装，它的主要职责就是连接服务端，`MqttConnectOptions`用来配置MQTT参数。
```java
MqttAndroidClient mqttAndroidClient = new MqttAndroidClient(getApplicationContext(), serverUri, clientId);

MqttConnectOptions mqttConnectOptions = new MqttConnectOptions();
```

2.设置`MqttConnectOptions`常用参数
```java
// 配置MQTT连接
mqttConnectOptions.setCleanSession(false);//是否清除缓存
mqttConnectOptions.setUserName(null);//用户名
mqttConnectOptions.setPassword(null);//密码
mqttConnectOptions.setConnectionTimeout(30);  //超时时间
mqttConnectOptions.setKeepAliveInterval(60); //心跳时间,单位秒
mqttConnectOptions.setAutomaticReconnect(true);//自动重连

String message = "{\"terminal_uid\":\"" + clientId + "\"}";
String topic = myTopic;
Integer qos = 0;
Boolean retained = false;
if ((!message.equals("")) || (!topic.equals(""))) {
    try {
        // 最后的遗嘱
        mqttConnectOptions.setWill(topic, message.getBytes(), qos.intValue(), retained.booleanValue());
    } catch (Exception e) {
        // 根据该标志位进一步判断是否进行连接操作
        doConnect = false;
        iMqttActionListener.onFailure(null, e);
    }
}
```

详细参数请见：
[Class MqttConnectOptions API](https://www.ibm.com/support/knowledgecenter/SSFKSJ_7.5.0/com.ibm.mq.javadoc.doc/WMQMQxrClasses/org/eclipse/paho/client/mqttv3/MqttConnectOptions.html)

3.设置监听
```java
mqttAndroidClient.setCallback(new MqttCallbackExtended() {
            @Override
            public void connectComplete(boolean reconnect, String serverURI) {
                // 连接成功后订阅topic
                subscribeToTopic();
            }

            @Override
            public void connectionLost(Throwable cause) {
                // MQTT连接丢失回调，这里可以做重连操作
            }

            @Override
            public void messageArrived(String topic, MqttMessage message) throws Exception {
                // 监听消息，有消息过来会回调该方法
            }

            @Override
            public void deliveryComplete(IMqttDeliveryToken token) {
                // 数据传输完成会回调该方法
            }
        });
```

4.订阅topic
```java
final String topic = "exampleAndroidTopic";
private void subscribeToTopic() {
    try {
        mqttAndroidClient.subscribe(topic, 0, null, new IMqttActionListener() {
            @Override
            public void onSuccess(IMqttToken asyncActionToken){
                // 订阅成功回调
            }

            @Override
            public void onFailure(IMqttToken asyncActionToken, Throwable exception) {
                // 订阅失败回调
            }
        });
    } catch (MqttException e) {
        e.printStackTrace();
    }
}
```

5.连接服务端
```java
// connect()方法有多种重载形式，我们也可以使用connect(MqttConnectOptions,Object,IMqttActionListener)的方式连接服务端
// 这样我们订阅topic的过程就可以放在IMqttActionListener的回调里
mqttAndroidClient.connect(mqttConnectOptions);

// 主动断开连接，一般在组件生命周期销毁时调用
mqttAndroidClient.disconnect();

// 判断client是否已经连接
mqttAndroidClient.isConnected();
```

6.如果客户端需要发布消息，可以这样使用：
```java
String topic = myTopic;
Integer qos = 0;
Boolean retained = false;
try {
    mqttAndroidClient.publish(topic, msg.getBytes(), qos.intValue(), retained.booleanValue());
} catch (MqttException e) {
    e.printStackTrace();
}
```
如果服务端收到消息，会把消息分发给所有订阅了该topic的客户端。

### 参考

https://blog.csdn.net/qq_17250009/article/details/52774472
https://blog.csdn.net/github_33304260/article/details/73692589

### 敏行MQTT实践
MXKit#loginMXKit    ->
MXKit#initComplete  ->
MXKit#startPushTask ->
MXMQTTStartHelper#startPushTask ->
PushConnectService#startServiceToStartMqtt   ->
PushConnectService#connectClient


1.MXKit#startPushTask方法中`create(Constant.MXKIT_PUSH_LAUNCH);`启动的是什么服务？

