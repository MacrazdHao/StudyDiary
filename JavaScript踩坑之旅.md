# JavaScript踩坑之旅

## 前言

作为一名初出毛犊的前端切图仔，正努力往高处飞~

虽已正式工作一年，但记录的也基本仅限于一些面经总结和学习日志。

正式工作中实战的踩坑之旅至今开始记录，但也为时不晚。

## 坑1 ES6中Array新特性Array.prototype.fill()

*MDN文档：https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/fill*

由于公司项目的原因，做某个通用组件时需要用到这个函数的功能，接下来讲一下心路历程：

首先为了编写自定义组件，有了动态设置数组大小的需求，而初始数组的元素为相同值；

于是考虑用for，但貌似并不是那么好用，决定弃坑另寻出路；

找到了ES6的这个fill()方法；

使用fill()后成功创建了动态大小的数组，且每个初始值相同：

```javascript

const vcode = (new Array(4)).fill(useState(null));

```

*踩雷* 发现fill()创建的数组中，所有相同的初始元素实际上是浅拷贝的结果，意味着改变任何一个，其他也会随之改变；

于是疯狂查找相关资料，终于找到相关解决方式：（其中我所用的是第二个方法）

```javascript

// 参考自：https://segmentfault.com/q/1010000019827464/

// 方式1：

var arr = new Array(3).fill(JSON.stringify({
    name: 'deng',
    age: 18,
    job: 'software'
}))
arr=arr.map(item=>JSON.parse(item))
console.log(arr)

arr[0].name = 'dengbbbb'

// 方式2：

var arr = Array.apply(null, { length: 3 }).map(() => ({
    name: 'deng',
    age: 18,
    job: 'software'
}));
arr[0].name = 'dengbbbb';

```

最终以第二个方式解决了问题。
