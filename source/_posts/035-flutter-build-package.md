---
title: flutter应用打包、修改图标和app名字
date: 2020-11-17 11:38
category: Flutter
---

# 打包 flutter app

## 创建密钥

如果您已有密钥库，请跳到下一步。 如果没有，请在命令行中运行以下命令来创建一个：

在 Mac / Linux 上，使用以下命令：

```bash
keytool -genkey -v -keystore ~/key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias key
```

在 Windows 上，使用以下命令：

```bash
keytool -genkey -v -keystore c:\Users\USER_NAME\key.jks -storetype JKS -keyalg RSA -keysize 2048 -validity 10000 -alias key
```

此命令将`key.jks`文件存储在主目录中。 如果要将其存储在其他位置，请更改传递给`-keystore`参数的参数。 但是，请将密钥库文件保持私有状态； 不要将其检入公共源代码控制中！

> **注意**：
>
> - `keytool`命令可能不在您的路径中，它是 Java 的一部分，而 Java 是作为 Android Studio 的一部分安装的。 对于具体路径，请运行`flutter doctor -v`并找到在'Java binary at:'之后打印的路径。 然后使用该标准路径将 Java（最后）替换为 keytool。 如果您的路径包含以空格分隔的名称（例如“Program Files”），请使用平台专用的名称表示法。 例如，在 Mac / Linux 上，使用 Program \ Files，在 Windows 上，使用“ Program Files”。
> - 只有 Java 9 或更高版本才需要`-storetype JKS`标记。 从 Java 9 版本开始，密钥库类型默认为`PKS12`。

## 应用程序中引用密钥库

创建一个名为`<app dir>/android/key.properties`的文件，其中包含对密钥库的引用：

```
storePassword=<password from previous step>
keyPassword=<password from previous step>
keyAlias=key
storeFile=<location of the key store file, such as /Users/<user name>/key.jks>
```

## 配置 Gradle 登录

通过编辑`<app dir>/android/app/build.gradle`文件为您的应用配置签名。

1. 在 android 块之前添加代码：

   ```
      android {
         ...
      }
   ```

   使用属性文件中的密钥库信息：

   ```
      def keystoreProperties = new Properties()
      def keystorePropertiesFile = rootProject.file('key.properties')
      if (keystorePropertiesFile.exists()) {
          keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
      }

      android {
            ...
      }
   ```

   将`key.properties`文件加载到`keystoreProperties`对象中。

2. 在 buildTypes 块之前添加代码：

   ```
      buildTypes {
          release {
              // TODO: Add your own signing config for the release build.
              // Signing with the debug keys for now,
              // so `flutter run --release` works.
              signingConfig signingConfigs.debug
          }
      }
   ```

   使用签名配置信息：

   ```
      signingConfigs {
          release {
              keyAlias keystoreProperties['keyAlias']
              keyPassword keystoreProperties['keyPassword']
              storeFile keystoreProperties['storeFile'] ? file(keystoreProperties['storeFile']) : null
              storePassword keystoreProperties['storePassword']
          }
      }
      buildTypes {
          release {
              signingConfig signingConfigs.release
          }
      }
   ```

   在模块的`build.gradle`文件中配置`signingConfigs`块。

您的应用的发布版本现在将自动签名。

> **注意**：更改 gradle 文件后，您可能需要运行`Flutter Clean`。 这样可以防止缓存的版本影响签名过程。

## 用 R8 缩小代码

R8 是 Google 推出的新代码缩减器，在您构建发行版 APK 或 AAB 时默认启用。 要禁用 R8，请传递`--no-shrink`标志在使用`flutter build apk`或`flutter build appbundle`。

> **注意**：混淆和缩小会大大延长 Android 应用程序的编译时间。

## 查看应用清单

查看位于`<app dir>/android/app/src/main`中的默认`App Manifest`文件`AndroidManifest.xml`，并验证值是否正确，尤其是以下内容：

**_应用_**

在 application 标记中编辑`android：label`，以反映应用的最终名称。

**_权限_**

如果您的应用程序代码需要 Internet 访问，请添加`android.permission.INTERNET`权限。 标准模板不包含此标签，但允许在开发程中访问 Internet 以启用 Flutter 工具与正在运行的应用程序之间的通信

## 查看构建配置

查看位于`<app dir>/android/app`中的默认 Gradle 构建文件文件`build.gradle`，并验证值是否正确，尤其是`defaultConfig`块中的以下值：

**_applicationId_**

指定最终的唯一（应用程序 ID）`appid`

**versionCode & versionName**

指定内部应用程序的版本号，以及版本号显示字符串。 您可以通过在`pubspec.yaml`文件中设置`version`属性来执行此操作。 请查阅版本文档中的版本信息指南。

**_minSdkVersion，compilesdkVersion & targetSdkVersion_**

指定最低 API 级别，在其上编译应用程序的 API 级别以及在其上设计运行该应用程序的最大 API 级别。 有关详细信息，请查阅版本文档中的“ API 级别”部分。

指定您的应用使用的 Android SDK 构建工具的版本。 或者，您可以使用 Android Studio 中的[Android Gradle 插件]，它将自动为您的应用程序导入所需的最低构建工具，而无需使用此属性。

## 构建要发布的应用

发布到 Play 商店时，您有两种可能的发行格式。

- App bundle（首选）
- APK

### 创建 app bundle

如果您完成了签名步骤，则将对应用程序捆绑包进行签名。 此时，您可能会考虑混淆 Dart 代码，以使反向工程更加困难。 混淆代码涉及在构建命令中添加几个标志，并维护其他文件以消除对堆栈跟踪的混淆。

在命令行中：

1. 进入 `cd <app dir>`

   （将`<app dir>`替换为应用程序的目录。）

2. 运行`flutter build appbundle`

   （运行`flutter build`默认为发布版本。）

在`<app dir>/build/app/outputs/bundle/release/app.aab`中创建了您的应用程序的发行包。

默认情况下，应用程序捆绑包包含 Dart 代码和 Flutter 运行时，它们针对 armeabi-v7a（ARM 32 位），arm64-v8a（ARM 64 位）和 x86-64（x86 64 位）编译。

### 创建 APK

尽管应用捆绑包的优先级高于 APK，但有些商店尚不支持应用捆绑包。 在这种情况下，请为每个目标 ABI（应用程序二进制接口）构建一个发行版 APK。

如果您完成了签名步骤，则 APK 将被签名。 此时，您可能会考虑混淆 Dart 代码，以使反向工程更加困难。 混淆代码涉及在构建命令中添加几个标志。

在命令行中：

1. 进入 `cd <app dir>`

   （将`<app dir>`替换为应用程序的目录。）

2. 运行` flutter build apk --split-per-abi`

   （运行`flutter build`默认为`--release`。）

此命令产生三个 APK 文件：

- `<app dir>/build/app/outputs/apk/release/app-armeabi-v7a-release.apk`
- `<app dir>/build/app/outputs/apk/release/app-arm64-v8a-release.apk`
- `<app dir>/build/app/outputs/apk/release/app-x86_64-release.apk`

删除`--split-per-abi`标志会生成一个 fat APK，其中包含为所有目标 ABI 编译的代码。 此类 APK 的大小要大于拆分后的对应文件，导致用户下载不适用于其设备架构的本机二进制文件。

### 在设备上安装 APK

请按照以下步骤在连接的 Android 设备上安装 APK。

在命令行中：https://flutter.dev/docs/deployment/android

1. 使用 USB 电缆将 Android 设备连接到计算机。
2. 使用`cd <app dir>`，其中`<app dir>`是您的应用程序目录。
3. 运行`flutter install`。

[https://flutter.dev/docs/deployment/android](https://flutter.dev/docs/deployment/android)

# 修改图标和 app 名字

## Android

1. 打开`<app dir>/android/app/src/main/AndroidMainifest.xml`，找到配置位置：

   ```xml
   	    <application
   	        android:name="io.flutter.app.FlutterApplication"
   	        android:label="flutter-app"
   	        android:icon="@mipmap/ic_luncher_logo">
   	        <activity
   ```

   `label`为 app 名字，`icon`为 app 图标。

2. 图标存放位置在`<app dir>/android/app/src/main/res`下，
   > **注意**:
   >
   > - 有好几个 mipmap 文件夹，这和 ios 是一样的，这几个是适配不同分辨率的手机。只需让美工出相应尺寸的图标放到相应的文件夹就可以了。
   > - 注意图片需要是 png 格式。

## IOS

1. 打开`<app dir>/ios/Runner/info/plist`，找到配置位置：

   ```xml
   <dict>
   	<key>CFBundleDevelopmentRegion</key>
   	<string>$(DEVELOPMENT_LANGUAGE)</string>
   	<key>CFBundleExecutable</key>
   	<string>$(EXECUTABLE_NAME)</string>
   	<key>CFBundleIdentifier</key>
   	<string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
   	<key>CFBundleInfoDictionaryVersion</key>
   	<string>6.0</string>
   	<key>CFBundleName</key>
   	<string>flutter-app</string>
   ```

   `CFBundleName`下面的字符串就是 app 的名称。

2. 图标放到`<app dir>/ios/Runner/Assets.xcassets/AppIcon.appiconset`下，然后在`Contents.json`配置。

   ```json
   {
     "images" : [
       {
         "size" : "20x20",
         "idiom" : "iphone",
         "filename" : "Icon-App-20x20@2x.png",
         "scale" : "2x"
       },
       {
         "size" : "20x20",
         "idiom" : "iphone",
         "filename" : "Icon-App-20x20@3x.png",
         "scale" : "3x"
       },
       {
         "size" : "29x29",
         "idiom" : "iphone",
         "filename" : "Icon-App-29x29@1x.png",
         "scale" : "1x"
       },
       {
         "size" : "29x29",
         "idiom" : "iphone",
         "filename" : "Icon-App-29x29@2x.png",
         "scale" : "2x"
       },

   ```

过
