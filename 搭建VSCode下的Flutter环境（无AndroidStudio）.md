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

* ~~Android SDK Tools（已弃用）~~

```
进入页面后找到Command line tools only点击下载
下载地址：
https://developer.android.com/studio/# command-tools
百度云备用链接：
https://pan.baidu.com/s/1FeBX9Ui8WMMIGfyL96HdpQ
提取码: wv11
```

* Android cmdline-tools（SDK Tools改用）

```
进入页面后找到Command line tools only点击下载
下载地址：
https://developer.android.com/studio?hl=en#cmdline-tools
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

# Android SDK的安装及配置（Android Studio内置虚拟机单独安装）

这是个关键的一步，可以避开臃肿得像魔鬼一样的AndroidStudio的安装。

1. 将下载来的~~Android SDK Tools~~ cmdline-tools 的压缩包解压到某一文件夹，这里以D:/Android_SDK为例，解压后即D:/Android_SDK/tools

2. 打开刚才解压的文件夹D:/Android_SDK/cmdline-tools内创建一个名为**latest**的文件夹，并将D:/Android_SDK/cmdline-tools的其余所有文件和文件夹移进去

3. 进入D:/Android_SDK/cmdline-tools/latest/bin/，找到**sdkmanager.bat**

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
system-images;android-[版本号];google_apis_playstore;x86   **模拟器系统镜像**，可选。如果真机调试可忽略
```

7. 配置系统环境

   添加系统变量：ANDROID_HOME、ANDROID_SDK

   变量值都是刚才Android SDK tools解压的路径，依照刚才的示例则是D:\Android_SDK

   附：若你使用的不是Flutter，同时如果需要手动创建emulator虚拟机的话，则按照以下流程进行创建：

8. 使用控制台打开Android_SDK\tools目录

9. 输入一下命令行：

```
android list target
```

10. 你会得到类似以下这样的输出：

```
**************************************************************************
The "android" command is deprecated.
For manual SDK, AVD, and project management, please use Android Studio.
For command-line tools, use tools\bin\sdkmanager.bat
and tools\bin\avdmanager.bat
**************************************************************************

Invoking "C:\Program Files\Android_SDK\tools\bin\avdmanager" list target

Available Android targets:==============] 100% Fetch remote repository...
----------
id: 1 or "android-23"
     Name: Android API 23
     Type: Platform
     API level: 23
     Revision: 3
----------
id: 2 or "android-26"
     Name: Android API 26
     Type: Platform
     API level: 26
     Revision: 2
----------
id: 3 or "android-27"
     Name: Android API 27
     Type: Platform
     API level: 27
     Revision: 3
----------
id: 4 or "android-28"
     Name: Android API 28
     Type: Platform
     API level: 28
     Revision: 6
----------
id: 5 or "android-29"
     Name: Android API 29
     Type: Platform
     API level: 29
     Revision: 3
```

   说明：
   如果上面安装依赖的第6步中下载的镜像文件在这里没有显示，请检查指令是否错误，或SDK版本是否为旧版，如果是旧版，请务必在我提供的地址（官方/百度云）上下载，并重新配置全局变量

11. 这是你已经下载好的安卓系统列表，选好一个后，然后输入以下命令：

```
android create avd -n [虚拟机名称] -k [id]
```

   说明：

   如果无法执行该指令，请检查指令是否错误，或SDK版本是否为旧版，如果是旧版，请务必在我提供的地址（官方/百度云）上下载，并重新配置全局变量
   
   如果你看到类似这种提示，那么把刚才你执行的命令中的id，换成最底下[标签①]描述的那一串，*并且用双引号包住*，重新执行一遍即可
   
   ```
   **************************************************************************
   The "android" command is deprecated.
   For manual SDK, AVD, and project management, please use Android Studio.
   For command-line tools, use tools\bin\sdkmanager.bat
   and tools\bin\avdmanager.bat
   **************************************************************************

   Invoking "E:\Android_SDK\tools\bin\avdmanager" create avd -n testAVD -k android-29

   Error: Package path is not valid. Valid system image paths are:ository...
   system-images;android-29;google_apis_playstore;x86_64    // ①上面说的是这串
   null
   ```
   
   如果是我上面这个例子，则应该是
   
   ```
   .\android.bat create avd -n testAVD -k "system-images;android-29;google_apis_playstore;x86_64"
   ```
   
12. 等待创建完成后，执行以下命令运行虚拟机：

```
emulator -avd [虚拟机名称]
```

13. 运行虚拟机时可能会出现的问题：

* 运行时出错提示：

```
PANIC：Missing emulator engine program for 'x86' CPU
```

   解决方法：

1. 打开 Android_SDK/emulator 目录，并将所有文件复制

2. 打开 Android/tools 目录，并将刚才复制的文件全部粘贴并覆盖即可

* 运行时出错提示：

```
emulator: WARNING: Crash service did not start
emulator: ERROR: x86_ 64 emulation currently requires hardware acce1 erati on!
P1ease ensure Windows Hypervisor P1atform (WHPX) is properly installed and usab1e.
CPU acceleration status: HAXI is not installed on this machine
More info on configuring VM acceleration on Windows:
https:/ / developer. android. com/ studi o/ run/ emul ator一acce1 era ti on#v m-windows
If you are using an Inte1 CPU: p1ease check that virtualization is enab1ed in the BIOS and that HAXK is installed and usable.
Note: if Hyper-V or Credential Guard is enabled, the emulator wi11 not work with HAXK.
See https://gi thub. com/ inte1/haxm/issues/ 105#issuecomment-470927735 for info on how to disable CredentiC的安域传直
isor
If you are using an AMD CPU or need to run alongside Hyper-V-based apps such as Docker, we recommend using
P1 atform Genera1 inf ormation on acce1eration: https:/ / deve1 oper. android. com/ studi 0/ run/ emul ator -acce1 erati on.
```

   解决方法：

   这种情况针对不同CPU是有不同处理方法的。
   
   * 首先是**Intel的CPU**的处理方法：

1. 打开 Android_SDK/tools/bin/ 目录，执行以下命令下载一个安装包：

```
sdkmanager "extras;intel;Hardware_Accelerated_Execution_Manager"
```

2. 此步骤一般可跳过，若下一步无法进行则通过该步骤解决：搜索一下你自己主板的虚拟化支持的选项在哪里就OK

3. 打开 Android_SDK\extras\intel\extras\intel\Hardware_Accelerated_Execution_Manager\ 目录，并执行里面的安装包 intelhaxm-android.exe

4. 安装过程：一直点下一步即可

(以上参考：https://cloud.tencent.com/developer/article/1499938)

   * 其次是**AMD的CPU**的处理方法：

1. 打开 控制面板 —— 程序 —— 程序和功能 —— 启用或关闭Windows功能

2. 找到并勾选 Hyper-V 和 Windows Hypervisor Platform（Windows虚拟机监控程序平台）

3. 重启电脑，完成

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
