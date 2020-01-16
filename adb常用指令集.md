# adb常用指令集

安装apk：

```
adb install *.apk
```

卸载app：

```
adb uninstall [包名]
```

获取第三方包名：

```
adb shell pm list package -3
```

获取Activity名：

```
cd /d C:\Program Files\Android_SDK\build-tools\29.0.2
.\aapt.exe dump badging [APK文件] | findstr "activity"
```

关闭APP：

```
adb shell am force-stop [包名]
```

启动APP：

```
adb shell am start [包名]/[Activity名]
如：com.weex.app/com.weex.app.SplashActivity
```
