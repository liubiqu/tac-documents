## 准备工作

- 您首先需要一个 Android 工程，这个工程可以是您现有的工程，也可以是您新建的一个空的工程。
- 其次您需要 [配置后台服务器](https://github.com/tencentyun/tac-documents/blob/master/%E4%BD%BF%E7%94%A8%E6%96%87%E6%A1%A3/%E6%94%AF%E4%BB%98%20Payment%20%E9%9B%86%E6%88%90%E6%8C%87%E5%8D%97/%E5%8F%91%E8%B4%A7%E6%9C%8D%E5%8A%A1%E5%99%A8%E9%85%8D%E7%BD%AE.md)。
- 最后您需要申请到相关支付渠道（[如何自行申请渠道](https://github.com/tencentyun/tac-documents/blob/master/%E4%BD%BF%E7%94%A8%E6%96%87%E6%A1%A3/%E6%94%AF%E4%BB%98%20Payment%20%E9%9B%86%E6%88%90%E6%8C%87%E5%8D%97/%E6%94%AF%E4%BB%98%E6%B8%A0%E9%81%93%E7%94%B3%E8%AF%B7%E6%8C%87%E5%BC%95.md)）。

## 第一步：创建项目和应用（已完成请跳过）

在使用我们的服务前，您必须先在 MobileLine 控制台上创建项目和应用。

## 第二步：添加配置文件（已完成请跳过）

在您创建好的应用上单击【下载配置】按钮来下载该应用的配置文件的压缩包：

![](http://tacimg-1253960454.file.myqcloud.com/guides/project/downloadConfig.gif)

解压该压缩包，您会得到 `tac_service_configurations.json` 和 `tac_service_configurations_unpackage.json` 两个文件，请您如图所示添加到您自己的工程中去。

![](https://main.qcloudimg.com/raw/2098031bcf22b6a32ac87066ed8a3278.jpg)

>**注意：**
>请您按照图示来添加配置文件，`tac_service_configurations_unpackage.json` 文件中包含了敏感信息，请不要打包到 APK 文件中，MobileLine SDK 也会对此进行检查，防止由于您误打包造成的敏感信息泄露。

## 第三步：集成 SDK

您需要在工程级 build.gradle 文件中添加 SDK 插件的依赖：

```
buildscript {
	...
    dependencies {
        classpath 'com.android.tools.build:gradle:3.0.1'
        // 添加这行
        classpath 'com.tencent.tac:tac-services-plugin:1.3.+'
    }
}
```

在您应用级 build.gradle 文件（通常是 app/build.gradle）中添加 payment 服务依赖，并使用插件：

```
dependencies {
    // 增加这两行
    compile 'com.tencent.tac:tac-core:1.3.+'
    compile 'com.tencent.tac:tac-payment:1.3.+'
}
...

// 在文件最后使用插件
apply plugin: 'com.tencent.tac.services'
```

到此您已经成功接入了 MobileLine 移动付费服务。


## Proguard 配置

如果您的代码开启了混淆，为了 SDK 可以正常工作，请在 `proguard-rules.pro`文件中添加如下配置：

```
# MobileLine Core

-keep class com.tencent.qcloud.core.** { *;}
-keep class bolts.** { *;}
-keep class com.tencent.tac.** { *;}
-keep class com.tencent.stat.*{*;}
-keep class com.tencent.mid.*{*;}
-dontwarn okhttp3.**
-dontwarn okio.**
-dontwarn javax.annotation.**
-dontwarn org.conscrypt.**

# MobileLine Payment

-keep class com.tencent.midas.** { *;}
-keep class com.tencent.openmidas.** { *;}
```
