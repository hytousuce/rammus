# Rammus  ![pub package](https://img.shields.io/pub/v/rammus.svg)

阿里云推送Flutter插件.

> Rammus这个项目是一位网友*付费*开发的。在此特别感谢这位朋友。
> 能将其开源出来，是要感谢这位朋友的。
> 开源不易，时间有限，可能有很多功能还没有完成，需要大家共同维护，请各位朋友理解。
> 有问题可以提，但请嘴下积德，开源不意味着开源作者欠某些人的。
> 欢迎加入QQ群：892398530共同交流。

欢迎各位使用者PR。

## 麻烦先读一下官方文档

[麻烦读下推送官方文档](https://help.aliyun.com/document_detail/51056.html?spm=a2c4g.11186623.6.623.47bf59abvM9j25)

[Flutter使用Rammus实现阿里云推送](https://my.oschina.net/wupeilin/blog/3108695)

## Android上的配置

### 设置appKey,appSecret

在`AndroidManifest.xml`设置appKey,appSecret

```
        <meta-data
            android:name="com.alibaba.app.appkey"
            android:value="" /> <!-- 请填写你自己的- appKey -->
        <meta-data
            android:name="com.alibaba.app.appsecret"
            android:value="" />
```

> 也可以动态设置，具体方式看官方文档

### 初始化SDK

好吧，由于SDK的限制，用户只能在`Application`中的`onCreate`里初始化：

```
        RammusPlugin.initPushService(this)

```

此步骤仅进行初始化，**未进行推送渠道的注册**，有关注册渠道的信息请看下方[使用方式](#使用方式)部分。

### 设置第三方推送通道
在`AndroidManifest.xml`设置第三方推送的相关信息
```
        <!-- 华为 -->
        <meta-data
          android:name="com.huawei.hms.client.appid"
          android:value="appid=华为appid" />
        <!-- 小米 -->
        <meta-data
          android:name="com.xiaomi.push.client.app_id"
          android:value=""/>
        <meta-data
          android:name="com.xiaomi.push.client.app_key"
          android:value="" />
        <!-- oppo -->
        <meta-data
          android:name="com.oppo.push.client.app_key"
          android:value="" />
        <meta-data
          android:name="com.oppo.push.client.app_secret"
          android:value="" />
        <!-- meizu -->
        <meta-data
          android:name="com.meizu.push.client.app_id"
          android:value="" />
        <meta-data
          android:name="com.meizu.push.client.app_key"
          android:value="" />
        <!-- vivo -->
        <meta-data
          android:name="com.vivo.push.app_id"
          android:value="" />
        <meta-data
          android:name="com.vivo.push.api_key"
          android:value="" />
        <!-- gcm -->
        <meta-data
          android:name="com.gcm.push.send_id"
          android:value="" />
        <meta-data
          android:name="com.gcm.push.app_id"
          android:value="" />
```
在app build.gradle文件中添加第三方推送依赖
```
        implementation 'com.aliyun.ams:alicloud-android-third-push-huawei:$version'
        implementation 'com.aliyun.ams:alicloud-android-third-push-xiaomi:$version'
        implementation 'com.aliyun.ams:alicloud-android-third-push-oppo:$version'
        implementation 'com.aliyun.ams:alicloud-android-third-push-vivo:$version'
        implementation 'com.aliyun.ams:alicloud-android-third-push-meizu:$version'
        implementation 'com.aliyun.ams:alicloud-android-third-push-fcm:$version'
```
> Application在Android原生项目里。不会创建的自行百度。


## iOS上的配置

稍微有点麻烦。

### 添加一下源

在项目中的`PodFile`前面加上下面的两句话
```ruby
source 'https://github.com/CocoaPods/Specs.git'
source 'https://github.com/aliyun/aliyun-specs.git'
```
### 添加一下info.plist
请参考[这个链接](https://help.aliyun.com/document_detail/30072.html?spm=a2c4g.11186623.6.630.396f40b1t4SLCb)把
把`info.plist`添加到你的项目中。

到此ios配置完了。

### 关于iOS通知栏
如果你想推送通知的时候在通知栏上有显示请确保调用了下面的代码：
```dart
  rammus.configureNotificationPresentationOption();
```

## 使用方式

### 注册推送渠道（Android 端）

> iOS 端目前未将注册渠道与初始化分离，若你的应用目前仅在 iOS 端运行，可以忽略本步骤。

插件提供 `register()` 方法以执行 Android 端 SDK 的 `void register(Context context, CommonCallback callback);`与厂商渠道的 `register` 接口：
```dart
    rammus.register();
```

根据需求可以在用户签署隐私政策后再执行本方法，以防止提前获取到用户敏感信息进而影响应用上架应用市场。

更多请见[阿里云文档](https://help.aliyun.com/document_detail/51056.html)
