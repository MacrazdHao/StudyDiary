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

## 坑5 不支持CSS选择器或伪元素

就是不支持，暂时没找到对应方案

## 坑6 Image组件不设置宽高无法显示

Image组件默认宽高为0，并且只设宽/高其中一个，另一个不会自动调整。

本地图片宽高：

```javascript
import React from 'react';
import { Image } from 'react-native';
export default function TestComponent(props) {
    const img = require('../assets/images/test.jpg');
    // 按比例缩放时，就按比例取值即可
    let imgWidth = Image.resolveAssetSource(img).height;
    let imgHeight = Image.resolveAssetSource(img).width;
    return <Image source={img} style={{width: imgWidth, height: imgHeight}} />
}
```

网络图片宽高：

```javascript
import React, { useState } from 'react';
import { Image } from 'react-native';
export default function TestComponent(props) {
    const img = 'http://tupianurl.com/test.jpg';
    let [width, setWidth] = useState(0);
    let [height, setHeight] = useState(0);
    Image.getSize(item.item, (w, h) => {
      // 按比例缩放时，就按比例取值即可
      setWidth(w);
      setHeight(h);
    }, (e) => {
      console.log(e)
    });
    return <Image source={img} style={{width: width, height: height}} />
}
```

## 坑7 不支持CSS的clac()函数

正在寻找方法解决

方法一：

使用webpack Loader，引入React-Native可用的less

https://www.jianshu.com/p/ac9aa35e77e5

这个方法未做试验，有兴趣的自行尝试。

方法二：

目前正在尝试使用的方法，无设置width，而是使用margin进行定宽。

## 坑8 Windows下无法构建IOS应用

准确来说，这不是react-native的坑，而是垃圾苹果的坑，开发IOS应用的权限仅限于在MacOS进行开发。

若条件不允许，只能够在Windows环境下开发，目前发现的唯一方式就是装虚拟机跑黑苹果了。

附：黑苹果虚拟机的安装方法在本项目的另一篇日志中

## 坑9 两种脚手架均有不足

### react-native-cli（原生脚手架）

* 优势：无需使用expo服务，可本地直接生成现成apk，无需排队，速度较快（虽然本身还是慢）；

* 缺点：无法使用expo服务；生成apk需要手动生成签名秘钥。

附：手动生成签名秘钥方法 https://www.cnblogs.com/weschen/p/8358952.html

### create-react-native-app

* 优势：可以使用expo服务生成apk，全自动，甚至可以让其帮你完成签名秘钥的生成过程；

* 缺点：使用expo服务生成apk需要排队，耗时长，说不准要花上半个小时；生成后的apk并非生成在本地，而是在其服务器上，不知是不是因为在国内的缘故，下载速度也是极慢，耗时+1。

## 坑10 部分第三方组件兼容性差（组件黑名单列表）

1. react-native-snap-carousel：一个第三方的轮播组件，动效之类的都很nice，但只要用了，生成的app在部分机器上会导致闪退（如一加7pro）；

## 坑11 expo调试应用react-navigation无法动态更新

注：react-native-cli的调试没有试过，待确认

在组件内部设置navigationOptions的属性之后，不会自动更新出来，需要关掉调试的app重新启动调试才能刷新，一不小心就以为是因为代码出现了问题

## 坑12 子节点不能继承父节点style属性

在HTML中，父节点有一部分样式是能够被子节点继承的，然而在react-native中，子节点无法继承父节点的style样式，这就会造成一些麻烦

## 坑13 View没有onPress属性

react-native的组件属性不像HTML一样自由，所有基础组件的属性都会被react-native严格规定，超出范围的是无效的

比如View不像HTML中的div标签，可以添加onclick属性，在View组件中，添加onPress组件是无效的，一开始还以为我自己的写法有问题

解决方法：

在View的外层添加一层TouchableOpacity组件即可

## react-navigation的正反传参

这应该不算是坑，只是使用技巧问题，官方文档上貌似只有正向传参的方法，而没有反向的（没仔细看，懒得找了）。

稍作记录一下

正向传参

```javascript

// 页面1
props.navigation.push('screenName', {
  p1: 'a',
  p2: function(){}
});

// 页面2
// 获取params方式1
props.navigation.state.params.p1;
// 获取params方式2
props.navigation.getParam('p1', 'default Value without passing value');

```

正向传参就不用再详细说了，官方文档直接就有demo

而反向传参就有必要特别说一下，反向传参其实是利用了正向传参的原理，把函数传过去调用而已，看代码：

```javascript
    
// 反向传参
    // 页面1
    const [test, setTest] = useState('test');
    props.navigation.push('TeleCode', {
      returnTest: (value) => {
        setTest(value);
      }
    });
    
    // 页面2
    // 调用反向传参函数方式1
    props.navigation.getParam('returnTest')('a test value');
    // 调用反向传参函数方式2
    props.navigation.state.params.returnTest('a test value');
    props.navigation.goBack();

```
