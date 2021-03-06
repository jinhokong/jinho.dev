---
title: TWIL JS Array 알아보기
date: '2019-06-25T00:00:00.000Z'
layout: post
draft: false
path: '/posts/js-array/'
category: 'Development'
tags:
  - 'Javascript'
  - 'Twil'
description: 'JS Array 에 대해서 깊게 알아 봅시다'
---

## Agenda

- Javascript에서 Array가 어떻게 작동하는지 알아봅니다.
- 위 문제를 해결하는 방법을 알아봅시다.

[만화로 소개하는 ArrayBuffer 와 SharedArrayBuffer ★ Mozilla 웹 기술 블로그](http://hacks.mozilla.or.kr/2017/11/a-cartoon-intro-to-arraybuffers-and-sharedarraybuffers/)

[Diving deep into JavaScript array - evolution & performance](http://voidcanvas.com/javascript-array-evolution-performance/)

# Array in Static Type Langauge

![](https://beginnersbook.com/wp-content/uploads/2014/01/c-arrays.png)

![](https://cdn-images-1.medium.com/max/1600/1*LNVzJWoJlVafWC4qIMJPBw.png)

[Are JavaScript arrays actually linked lists?](https://stackoverflow.com/questions/7069250/are-javascript-arrays-actually-linked-lists)

[Are node.js arrays actually hashmaps?](https://stackoverflow.com/questions/23610105/are-node-js-arrays-actually-hashmaps)

## 그러면 어떻게 해야할까?

[ArrayBuffer](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer)

![](https://hacks.mozilla.org/files/2017/06/02_03-768x580.png)

![](https://hacks.mozilla.org/files/2017/06/02_07-768x403.png)

### How To Use

```js
var buffer = new ArrayBuffer(12);
var dataView = new DataView(buffer);
var int8View = new Int8Array(buffer);
dataView.setInt32(0, 0x1234abcd);
console.log(dataView.getInt32(0).toString(16));
console.log(dataView.getInt8(0).toString(16));
console.log(int8View[0].toString(16));
```

### Performance check

```js
var LIMIT = 10000000;
var arr = new Array(LIMIT);
arr.push({ a: 22 });
for (var i = 0; i < LIMIT; i++) {
  arr[i] = i;
}
var p;
console.time("Array read time");
for (var i = 0; i < LIMIT; i++) {
  //arr[i] = i;
  p = arr[i];
}
console.timeEnd("Array read time");

var LIMIT = 10000000;
var buffer = new ArrayBuffer(LIMIT * 4);
var arr = new Int32Array(buffer);
console.time("ArrayBuffer insertion time");
for (var i = 0; i < LIMIT; i++) {
  arr[i] = i;
}
console.time("ArrayBuffer read time");
for (var i = 0; i < LIMIT; i++) {
  var p = arr[i];
}
console.timeEnd("ArrayBuffer read time");
```
