# 为开发公司App不得不开新坑

## 前言

如果哪天真有哪个大佬来看到这篇文章，请三思，这只是一名入门菜鸟所写，如有问题，请多担待，提出相关内容并希望能够得到指正，谢谢。

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

这不知道算不算坑，反正就会觉得挺麻烦的，要是漏了引入某个组件，一不留神还以为别的地方出错。

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

2. ant-design：同上

一般伴随以下警告：

```

VM25 common.bundle.js:36926 Warning: componentWillMount has been renamed, and is not recommended for use. See https://fb.me/react-unsafe-component-lifecycles for details.

* Move code with side effects to componentDidMount, and set initial state in the constructor.
* Rename componentWillMount to UNSAFE_componentWillMount to suppress this warning in non-strict mode. In React 17.x, only the UNSAFE_ name will work. To rename all deprecated lifecycles to their new names, you can run `npx react-codemod rename-unsafe-lifecycles` in your project source folder.

Please update the following components: Router, RouterContext

```

引起上述问题的原因：

由于react 16.7版本以上将会移除组件生命周期钩子componentWillMount方法，所以在构建时会出现问题。

解决方法：

打开node_modules中使用到的那些第三方组件的代码，将componentWillMount更改为UNSAFE_componentWillMount即可。

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

## 坑14 react-navigation的正反传参

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

## 坑15 ScrollView，SectionList和FlatList

ScrollView，当渲染数据多起来的时候，加载速度会越来越满，甚至半天打不开页面。

这种需求之下，能够取而代之的是SectionList和FlatList。

目前使用了SectionList，而FlatList尚未用过（已用），先说说SectionList目前遇到的坑吧。

SectionList和ScrollView的滚动函数不同：

ScrollView的是ScrollTo，甚至能直接输入y轴偏移直接滚动；

而SectionList的则是有另一个函数ScrollToLocation，这个函数不接受y轴偏移，而是以itemIndex，sectionIndex，viewOffset，viewPosition作为定位参数。

数据的形式是这样的：[ { title: 'xxx', data: [ ... ] }, { title: 'xxx', data: [ ... ] }, { title: 'xxx', data: [ ... ] }, ... ]

官方对于这几个的参数的解释是这样的：

```
'animated' (boolean) - Whether the list should do an animation while scrolling. Defaults to true.
'itemIndex' (number) - Index within section for the item to scroll to. Required.
'sectionIndex' (number) - Index for section that contains the item to scroll to. Required.
'viewOffset' (number) - 一个以像素为单位，到最终位置偏移距离的固定值，比如为了弥补粘接的header所占据的空间。
'viewPosition' (number) - A value of 0 places the item specified by index at the top, 1 at the bottom, and 0.5 centered in the middle.
```

ScrollToLocation官网详述: https://reactnative.cn/docs/sectionlist/#scrolltolocation

然鹅看不太懂是不是？

我遇到的坑有两个：

1. 我只输入了sectionIndex，这个参数的含义是，需要滚动到的目标section的序号，然鹅我知道是这个意思，但由于没有写itemIndex而出现错误，找了半天原因没找着。

   注意了，只要填了sectionIndex就要填itemIndex，它不会自动给你填充默认值。
   
   而如果你反过来只填写了itemIndex，那没问题，sectionIndex默认为0，这确实令我这个新手感到黑人问号。
   
2. SectionList的scrollToLocation函数无法定位到还未渲染的底下的组(section)或项(item)，这个问题我搜了一下，是有很多相关解答的，目前正尝试，尝试后会回来报告。

不好意思，我不用SectionList了，说说原因（也是坑）：

1. 对于上述描述的SectionList的第2个坑，网上的解决方案是使用getItemLayout的方法，getItemLayout是SectionList的一个属性，用于在其（通过滚动函数）滚动时，节省不必要的计算开支而做的，其提供的参数及需要的返回值为(data, index) => {length: number, offset: number, index: number}。

   这里它本身提供的参数data为我们提供的data本身，index表示当前元素序号（这里有个巨坑，注意了，这里的index，包含了sections（每一组数据的标题），即把sections也算在元素之一内。下列引发的一切血案于此有关），而我们需要提供给它的length表示每个元素的高度，offset表示当前元素的偏移（相对于SectionList内部的y轴偏移），index就是它给的index，原封不动返回。
  
   当然了，这里可以通过自己定义的某个函数来计算相关值并返回给它，可是！！！我一旦在这个函数内调用了当前组件（指调用了SectionList的组件function / Class）内部定义的其他函数或变量，出错了！！完全找不到解释！！但是调用通过props传入的函数/参数没问题！
  
2. 好的，我把一切用到的参数都通过props传入了，那么问题又来了，因为本身section（每一组数据的标题）一般来说不可能和正常的Item同一样式高度，因此要通过特殊计算offset和判断length，设置好了之后，确实也就可以滚动到未渲染的部分。
   
   可是！！！问题再次来了！！无论我怎么计算offset，即使再精准，也莫名其妙定位不出来，甚至乱跳！！我花了大量时间搞这玩意儿，结果还是不行！！！！
   
   按照官方文档来说，SectionList本身没有getItemLayout这个参数（但FlatList），不知是不是这个原因，导致根本无法定位准确。
   
   以下决定使用FlatList了，具体坑请看：
   
使用FlatList的过程，由于踩过了SectionList这个巨坑，因此已经有点看淡了，心态较为平和，也有一定的经验了，看文档和相关资料来说，FlatList和SectionList差距应当并非很大，所以已经做好了心理准备，以及一开始就觉得它和SectionList相差无几，所以不愿意尝试，现在也只能死马当活马医，不得不尝试了。

可是神奇的是，使用的时候却意外的顺利，除了要改一下相关的参数之外，也只有一些小坑，虽然也花了点时间，但最后确确实实稳定地实现了功能。

首先说说遇到的坑：

1. 数据形式，这应该不太算是坑吧，但还是要说说。因为它需要的数据是单一的，而不像SectionList有一个特殊的Section（标题），所以传入的数据形式只能是纯数组，相对于SectionList的标题部分，在FlatList里要改成与其子元素同一层级，具体形式如：[ { value: 'A', title: true }, { value: 'child1', title: false }, { value: 'child2', title: false }, { value: 'B', title: true }, { value: 'child3', title: false } ]

2. 由于数据形式的影响，我又改了一次我的数据。同时我还得在组件内，通过循环将每个元素所属组的标题的序号（比如上述例子的A为组1，则child1和child2两个元素中加入groupIndex为0，对应的B组的标题，和它包括的child3的groupIndex设置为1）、偏移（数值和计算同SectionList，都是指不包含自身的其之上的标题、元素的高度的总和）记录到原数据的每一项之中。当然了，这些数据都是为了计算getItemLayout所用的。

3. 同SectionList，不使用getItemLayout是无法滚动到未渲染部分的，否则直接报错。最重要的是，FlatList的getItemLayout并不会因为调用当前组件（指调用了SectionList的组件function / Class）内部定义的其他函数或变量而出错，随意调用都行，非常方便。

4. 滚动函数和SectionList的scrollToLocation不同，FlatList官方推荐使用的是ScrollToIndex，接受的参数为{ animate: true(default), index, viewOffset, viewPosition }，index当然就是元素序号了，这个直接它本身就会提供给你，直接填入即可，然后就是viewOffset，也是无脑填0即可。

大概就是这样，总的来说FlatList只要计算好getItemLayout相关的数据，一切好办。

## 坑16 获取元素宽高、坐标

因为React Native本身是不能直接通过Dom来获取元素的各种样式的，通过一番搜索，找到了获取的方法。

每个基础组件都会有一个onLayout这个绑定事件，这个绑定事件是在该组件渲染时同步执行的，它会有一个默认参数(event)提供给我们，我们可以读取该参数的数据，即event.nativeEvent.layout，里面就是记录着该组件的宽高、坐标信息。

## 坑17 Status高度（即系统顶部的状态栏）

有的时候会不得不使用这个数据，我一开始也没想到居然会使用到这个？来说说怎么获取。

在需要的组件（页面）中载入StatusBar，然后StatusBar.currentHeight即可获取。

## 坑18 letterIndexTouchMoveHandler事件（基础组件的鼠标滑动绑定事件）

letterIndexTouchMoveHandler提供给我们的event参数中包含的鼠标所处的pageY和pageX是相对于全局（整个手机屏幕）而言的。

因此如果通过这个参数来计算相对于我们App内部界面的y轴坐标，则需要：pageY - StatusBar.currentHeight，如果还需要减去自己App设置的navigationBar（标题栏）的话，自行减去即可。

由于本人还是新手，有没有其他更便捷的方法不知道，只能先以此顶替了，其他则有待发掘，如果有大佬看到这篇文章，并且也有方法的话，希望能多交流。

## 坑19 react-navigation跨栈返回（goBack）

参考：https://blog.csdn.net/sun_DongLiang/article/details/86707388

## 坑20 Text超出部分缩略为省略号

与web端不同，超出部分缩略为省略号无需配置style样式，Text标签内置属性，使用如下：

```html

// numberOfLines表示超出行数缩略为省略号

<Text numberOfLines={3} style={styles.previewText}>{preview}</Text>

```

## 坑21 Image标签不能设置borderRadius

与html不同，无法给Image组件的样式添加borderRadius属性，设置了没有效果。

解决方法：

在Image组件外套一层View，然后给这层View设置borderRadius，并且设置overflow为hidden。

## 坑22 导入组件不存在错误提示

如果出现以下错误提示，则检查是否存在导入路径错误的组件，或导入了不存在的三方依赖组件。

```

The development server returned response error code: 500

```

## 坑23 事件监听及useEffect

这里以Keyboard对象为例，因为我就是在这遇到的问题。

```

  const [keyboardHeight, setKeyboardHeight] = useState(0);
  const [showImgList, setShowImgList] = useState(false);
  let onKeyBoardDidShow = (e) => {
    setKeyboardHeight(e.endCoordinates.height);
    setShowImgList(false);
  };
  let onKeyBoardDidHide = (e) => {
    setKeyboardHeight(0);
    setShowImgList(imgList.length > 0 && true);
  };
  useEffect(() => {
    Keyboard.addListener('keyboardDidShow', onKeyBoardDidShow);
    Keyboard.addListener('keyboardDidHide', onKeyBoardDidHide);
    return () => {
      Keyboard.removeListener('keyboardDidShow', onKeyBoardDidShow);
      Keyboard.removeListener('keyboardDidHide', onKeyBoardDidHide);
    }
  }, []);
  
```

这里在useEffect中addListener之后，要加一个return，返回一个函数，该函数内容为注销监听器(listener)，并且removeListener的第二个参数需要与注册时的事件函数一致，否则无效。

