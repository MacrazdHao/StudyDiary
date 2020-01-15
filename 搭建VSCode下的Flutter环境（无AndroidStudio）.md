# 前言
没有前言，不废话
# 配置环境
Win10系统，无AndroidStudio的环境下，搭建Flutter在VSCode中的运行环境
# 配置资源
* JDK8
```
一定要JDK8，不要，官网下载旧版需要登录，所以预留了一个百度云备份
下载地址：
https://pan.baidu.com/s/1uWcRwUxZzc0m3fYQYTcOQw
提取码: w6us
```
* Android SDK Tools
```
进入页面后找到Command line tools only点击下载
下载地址：
https://developer.android.com/studio/# command-tools
百度云备用链接：
https://pan.baidu.com/s/1FeBX9Ui8WMMIGfyL96HdpQ
提取码: wv11
```
* Flutter
```
建议下载稳定版
下载地址：
https://flutter.dev/docs/development/tools/sdk/releases?tab=windows# windows
由于国内访问Flutter可能会受到限制，Flutter官网给中国开发者提供了临时镜像，我们把下面的系统环境变量添加到用户环境里，就可以获取临时镜像，如果翻墙的话，应该设置不设置都无法所谓。
PUB_HOSTED_URL 值https://pub.flutter-io.cn
FLUTTER_STORAGE_BASE_URL 值https://storage.flutter-io.cn
```
* Dart
```
进入页面后直接下载即可
下载地址：
http://www.gekorm.com/dart-windows/
```
* VSCode
```
自己去官网下载吧
```
* gradle（可选）
```
按照所需版本，在以下页面中选择下载：
https://gradle.org/releases/
```
# JDK环境配置
系统配置：JAVA_HOME,Path,CLASSPATH
不赘述，参考https://www.cnblogs.com/suni/p/8279672.html
# Android SDK的安装及配置
这是个关键的一步，可以避开臃肿得像恶魔一样的AndroidStudio的安装。
1. 将下载来的Android SDK Tools的压缩包解压到某一文件夹，这里以D:/Android_SDK为例，注意，不要把压缩包里面tools文件夹中的文件解压到文件夹，而是直接保留tools文件夹进行解压，解压后即D:/Android_SDK/tools
2. 打开刚才解压的文件夹，进入D:/Android_SDK/tools/bin，找到**SDKManager.bat**
3. 在该目录的窗口空白处按住Shift+鼠标右键，选择打开Power Shell窗口
4. 输入（可输入sdk然后按Tab键自动补全，然后再输入*--list*）
```shell
.\sdkmanager.bat --list
```
5. 正常情况下，会输出一大片的依赖目录列表；
	如果出现以下类似的错误，则可能是你下载的JDK版本没有按照一开始的要求来下载，重申，一定要JDK8，新版都是和Android SDK不兼容的：
```
Exception in thread "main" java.lang.NoClassDefFoundError: javax/xml/bind/annotation/XmlSchema 
    at com.android.repository.api.SchemaModule$SchemaModuleVersion.<init>(SchemaModule.java:156) 
    at com.android.repository.api.SchemaModule.<init>(SchemaModule.java:75) 
    at com.android.sdklib.repository.AndroidSdkHandler.<clinit>(AndroidSdkHandler.java:81) 
    at com.android.sdklib.tool.sdkmanager.SdkManagerCli.main(SdkManagerCli.java:73) 
    at com.android.sdklib.tool.sdkmanager.SdkManagerCli.main(SdkManagerCli.java:48)
Caused by: java.lang.ClassNotFoundException: javax.xml.bind.annotation.XmlSchema 
    at java.base/jdk.internal.loader.BuiltinClassLoader.loadClass(BuiltinClassLoader.java:582) 
    at java.base/jdk.internal.loader.ClassLoaders$AppClassLoader.loadClass(ClassLoaders.java:190) 
    at java.base/java.lang.ClassLoader.loadClass(ClassLoader.java:499)
```
6. 安装所需的依赖
```shell
安装指令：
.\sdkmanager.bat "[DependenceName]"
```
```
上述命令中的[DependenceName]对应刚才输出的依赖目录列表的依赖，在里面里找到以下依赖的名字逐个安装（安装最新的就好了）：
build-tools;[版本号]   平台构建工具
platform-tools   平台工具集
platforms;android-[版本号]   相应版本的APIs，对应构建工具
patcher;v4   补丁包
extras;intel;Hardware_Accelerated_Execution_Manager   PC硬件加速相关(这里就是这么长，没错)
emulator   模拟器，可选。如果真机调试可忽略
system-images;android-[版本号];google_apis_playstore;x86   模拟器系统镜像，可选。如果真机调试可忽略
```
7. 配置系统环境
添加系统变量：ANDROID_HOME、ANDROID_SDK
	变量值都是刚才Android SDK tools解压的路径，依照刚才的示例则是D:/Android_SDK
# Flutter的安装及配置
1. 将下载的Flutter中的flutter文件夹解压到指定目录即可，这里以D:/为例
2. 在解压目录下找到**flutter_console.bat**文件并双击运行，即可运行flutter命令
3. 配置系统环境，在系统变量中Path后面添加刚才的flutter解压路径的bin目录，按照刚才的示例则是D:/flutter/bin
4. 在command输入命令行flutter doctor可以查看当前环境是否已经完成各项依赖的安装，正常来说除了Conected devices之外其他都是[!]或者[√]
	如果出现以下类似的情况，则是因为刚才的Android SDK的安装步骤出现问题了，或者环境变量没有设置好：
```
[✓] Flutter (on Linux, locale en_US.UTF-8, channel master)
[✗] Android toolchain - develop for Android devices 
✗ Unable to locate Android SDK.
Install Android Studio from: https://developer.android.com/studio/index.html 
On first launch it will assist you in installing the Android SDK components.
(or visit https://flutter.io/setup/# android-setup for detailed instructions).
If Android SDK has been installed to a custom location, set $ANDROID_HOME to that location.
[✓] Android Studio (version 3.0.0)
• Android Studio at /home/f/App/android-studio
• Gradle version 3.2
• Java version OpenJDK Runtime Environment (build 1.8.0_112-release-b06)
[✓] Connected devices（1 available）
```
	其他的则按照（仔细阅读英文提示）对应提示安装相关依赖即可
# VSCode的安装及配置
1. 自己去官网下载VSCode
2. 在插件模块搜索flutter，点击安装(install)第一个Flutter
3. 重启VSCode，完成安装
# Flutter项目的新建及运行
1. 头部菜单：查看 - 命令面板（Ctrl+Shift+P）
2. 输入flutter，选择**Flutter: New Project**
3. 输入项目名称，这里以myapp为例，回车确定
4. 选择指定目录建立项目
5. 等待项目创建完成后，点开左侧文件列表的**lib/main.dart**文件
6. 真机设备开启**USB调试**，连接真机设备。如果使用虚拟设备的可以忽略这个步骤，自己琢磨，我这里没用虚拟设备
7. 头部菜单：调试 - 启动调试
8. 真机设备自动安装并运行自动生成的flutter Demo

# 更新Flutter
1. vscode中打开任意一个Flutter项目
2. Ctrl + ~ 调出终端，运行 flutter packages get 命令
3. 等待完成更新
