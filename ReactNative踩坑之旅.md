# 为开发公司App不得不开新坑
## 坑1 - 运行环境

参考文章：https://blog.csdn.net/u013551952/article/details/103227165

运行react-native start(开启Metro Bundler)时出现如下错误：
```
error Invalid regular expression: /(.*\\__fixtures__\\.*|node_modules[\\\]react[\\\]dist[\\\].*|website\\node_modules\\.*|heapCapture\\bundle\.js|.*\\__tests__\\.*)$)$/: Unterminated character class. Run CLI with --verbose flag for more details.
SyntaxError: Invalid regular expression: /(.*\\__fixtures__\\.*|node_modules[\\\]react[\\\]dist[\\\].*|website\\node_modules\\.*|heapCapture\\bundle\.js|.*\\__tests_s__\\.*)$/: Unterminated character class
    at new RegExp (<anonymous>)
    at blacklist (D:\WORKER\mytest\ReactNative\AwesomeProject\node_modules\metro-config\src\defaults\blacklist.js:34:10)
    at getBlacklistRE (D:\WORKER\mytest\ReactNative\AwesomeProject\node_modules\@react-native-community\cli\build\tools\loadMetroConfig.js:66:59)
    at getDefaultConfig (D:\WORKER\mytest\ReactNative\AwesomeProject\node_modules\@react-native-community\cli\build\tools\loadMetroConfig.js:82:20)
    at load (D:\WORKER\mytest\ReactNative\AwesomeProject\node_modules\@react-native-community\cli\build\tools\loadMetroConfig.js:118:25)
    at Object.runServer [as func] (D:\WORKER\mytest\ReactNative\AwesomeProject\node_modules\@react-native-community\cli\build\commands\server\runServer.js:82:58)    
    at Command.handleAction (D:\WORKER\mytest\ReactNative\AwesomeProject\node_modules\@react-native-community\cli\build\index.js:164:23)
    at Command.listener (D:\WORKER\mytest\ReactNative\AwesomeProject\node_modules\commander\index.js:315:8)
    at Command.emit (events.js:210:5)
    at Command.parseArgs (D:\WORKER\mytest\ReactNative\AwesomeProject\node_modules\commander\index.js:651:12
```
在终端中打开cmd，输入node -v检查node.js版本，如果超过12.10，请卸载你的node.js，并且下载旧版降级到12.10或以下，问题自动解决。

## 坑2 每个原生组件都要import

这不知道算不算坑，反正就会觉得挺麻烦的，一不留神还以为别的地方出错。

```javascript
import { StyleSheet, Text, View } from 'react-native';
```

## 坑3 获取设备百分比

```javascript
// 获取设备宽度
let MainWidth = Dimensions.get('window').width;
// 设备百分比：乘以具体百分比
let res = MainWidth*0.33;
```

## 坑4 Button没有style属性

react native自带的Button组件不支持通过style来进行自定义样式，只支持最低限度的样式修改，如单独的color属性
