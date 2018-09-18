# React Native 开发环境

* iOS
    * 运行环境
    * 模拟器调试
    * 真机调试
* Android
    * 运行环境
    * 模拟器调试
    * 真机调试

## iOS

### 运行环境
* 安装 Xcode

### 模拟器调试
* `react-native run-ios --port 8081`

### 真机调试
* Apple Developer 账户
* 生成开发者证书
* 加入设备
* 生成开发者 Profiles
* 用 Xcode 代开醒目，配置代码签名：
    * TTARGETS -> General -> Signing(Debug)
    * https://reactnative.cn/docs/running-on-device/
* Build 到设备：
    * Xcode toolbar -> Select Device -> Build and run
* 使用 react-native-cli
    * 先安装：`npm install -g ios-deploy`
    * `react-native run-ios --device --configuration Debug`(?)

## Android

### 运行环境
* 安装 JDK 8+
    * http://www.oracle.com/technetwork/java/javase/downloads/index.html
    * http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
* 安装 ADB（Android Debug Bridge）
    * 下载 Android SDK
    * 配置 `.bash_profile`
    * 运行 `source .bash_profile`

```
# .bash_profile 文件
export ANDROID_HOME=/development/android-sdk
-- export ANDROID_NDK_HOME=/development/android-sdk/ndk-bundle

export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/platform-tools
-- export PATH=$PATH:$ANDROID_NDK_HOME/
```

### 模拟器调试
* 安装 Android 模拟器
    * 安装 virtualbox ：https://www.virtualbox.org/
    * 安装 genymotion ：http://www.genymotion.net/
        * 配置 genymotion ADB
* 配置项目文件：local.properties
    * android/local.properties
* 使用 react-native-cli Build 到设备
    * `react-native run-android --port 8081`

```
# 项目/android/local.properties 文件
ndk.dir=/development/android-sdk/ndk-bundle
sdk.dir=/development/android-sdk
```
