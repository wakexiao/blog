---
title: 深入理解 call,apply,bind 的原理和使用
date: 2023-4-25
categories:
  - 前端
tags:
  - JavaScript
  - 手写
  - 面试
sticky: 2
---

# 前言
> 最近在准备面试，重新复习了下`call`、`apply` 和 `bind` 的手写实现。让我们来看看这三个方法的区别和如何手写实现吧。

## call、apply、bind 的基本使用和区别
### call 基本使用
```js
const info ={
  name: 'jack',
  age: 18,
}
function fn(desc1, desc2){
  console.log(`我会${desc1}, 还会${desc2}`);
  console.log(this.age);
}
fn('唱歌', '跳舞');
// 我会唱歌, 还会跳舞
// undefined
fn.call(info, '吃饭', '睡觉');
// 我会吃饭, 还会睡觉
// 18
```
### apply 基本使用
```js
const info ={
  name: 'jack',
  age: 18,
}
function fn(desc1, desc2){
  console.log(`我会${desc1}, 还会${desc2}`);
  console.log(this.age);
}
fn('唱歌', '跳舞');
// 我会唱歌, 还会跳舞
// undefined
fn.apply(info, ['吃饭', '睡觉']);
// 我会吃饭, 还会睡觉
// 18
```

#### call 和 apply 的区别
> 从上面两段代码可以看出，`call` 和 `apply` 的区别只有传参是不一样的；`call` 是接手多个参数的，`apply`只接收两个参数；`call` 的传参方式和普通函数调用一样，有多少个参数就在后面传多少个；而`apply`的传参方式是数组，真正调用的时候会解构依次按顺序传入到需要调用的函数里。

### bind 的基本使用
```js
const info ={
  name: 'jack',
  age: 18,
}
function fn(desc1, desc2, desc3){
  console.log(`我会${desc1}, 还会${desc2}, 也会${desc3}`);
  console.log(this.age);
}
const newFn = fn.bind(info, '上班', '摸鱼');
newFn('加班');
// 我会上班, 还会摸鱼, 也会加班
// 18
```
#### bind 和 call 的区别
> `bind` 的使用看起来会比 `call` 和 `apply` 复杂一点，但是也还是比较好理解的；第一个参数是需要绑定的`this`对象，后面就和函数传参一样，需要多少个就传多少个；用法看起来和 `call` 很相似，但是 `call` 会立即执行该函数，`bind`是返回一个新函数，这就是为什么 `react` 中绑定 `this` 指向用的是 `bind` 了。还有一个就是返回的新函数之后调用也是可以传参的，这个参数的传入顺序是接着调用 `bind` 传参之后的。

## 手写实现 call、apply、bind
### 手写 call
> 手写 `call` 之前我们先来看个例子：
```js
// 举例
var obj = {
  name: 'jack',
  age: 18
};
function fn(){
  console.log(this.age);
}
fn();
```
>上面的代码片段毫无疑问，打印的是 `undefined`;因为普通函数的`this`，谁调用就指向谁，`fn` 是在`window`中调用的，所以指向`window`，`window`中没有`age`属性，所以输出`undefined`。

接下来我们改造一下：
```js
var obj = {
  name: 'jack',
  age: 18
};
function fn(){
  console.log(this.age);
}
obj.fn = fn;
obj.fn();// 打印 18
```
>上面的代码经过我们的改造，在 obj 中添加一个 fn 属性，值为 fn 方法，通过 obj.fn 去调用，这时打印的 this.age 就是 18 了，因为是通过 obj 调用的。所以 `call` 实现的思路就来了，绑定 `this` 不就是把需要调用的函数在需要绑定 `this` 的对象身上添加一个属性，值为这个函数，然后再通过这个对象去调用，这个不就OK实现了？这是有杠精就会说，不对，你在 obj 对象上新增了属性，不太行；那我们再通过 `delete` 把 obj 上新增的属性删掉不就好了。
```js
function myCall(context, ...args){
  // 取出传递的参数, 不用上方 ...args 获取函数的剩余方式可以 arguments 获取参数
  // const args = [...arguments].slice(1);
  // 如果没有传需要绑定的对象，就指向 window
  const self = context || window;
  // 当前的 this 就是这个函数， 绑定this的时候是 fn.myCall
  //fn调用的myCall，所以this就是fn
  self.fn = this; // 把这个函数当做 fn 属性放在需要绑定 this 的对象中
  // 然后再通过这个对象去调用 fn 并把参数传递下去
  const result = self.fn(...args);
  // 再通过 delete 把这个属性从这个对象中删除就好了
  delete self.fn;
  // 返回 result 结果，call 是会立即执行该函数的
  return result;
}
Function.prototype.myCall = myCall;

let obj = {
  name: 'jack',
  age: 18,
};
function getName(desc, desc1) {
  console.log(desc, desc1);
  return this.age;
}
console.log(getName('正常调用', 'test'));
// 正常调用 test
// undefined
console.log(getName.myCall(obj, 'myCall调用', 'test'));
// myCall调用 test
// 18
```
### 手写apply
> 会了`call`之后`apply`不就是手到擒来了吗？只需要把传参方式改一下就好了。
```js
function myApply(context, agrArr){
  // 如果没有传需要绑定的对象，就指向 window
  const self = context || window;
  self.fn = this;
  const result = self.fn(...agrArr);
  delete self.fn;
  return result;
}
Function.prototype.myApply = myApply;

let obj = {
  name: 'jack',
  age: 18,
};
function getName(desc, desc1) {
  console.log(desc, desc1);
  return this.age;
}
console.log(getName('正常调用', '参数1'));
// 正常调用 参数1
// undefined
console.log(getName.myCall(obj, 'myApply调用', '参数1'));
// myCall调用 参数1
// 18
```
### 手写bind
```js
function myBind(context) {
  let self = this;
  // arguments 是类数组，不是数组，所以不能直接用 slice 方法
  let bindArgs = Array.prototype.slice.call(arguments, 1);
  // bind 是返回一个函数，所以定义一个函数，然后再在这个函数中通过传入的对象去调用这个函数并返回结果
  let result = function () {
    // 这里的 arguments 是bind之后返回新函数传入的
    const args = Array.prototype.slice.call(arguments);
    return self.call(context, ...bindArgs, ...args);
  };
  return result;
}

Function.prototype.myBind = myBind;

let obj = {
  name: 'jack',
  age: 18,
};
function getAge(desc, desc1, desc2) {
  console.log(desc, desc1, desc2);
  return this.age;
}
let myGetAge = getAge.myBind(obj, 'myBind调用', '参数1');
console.log(getAge('正常调用', '参数1', '参数2'));
// 正常调用 参数1 参数2
// undefined
console.log(myGetAge('参数2'));
// myBind调用 参数1 参数2
// 18
```
> bind 的实现也很简单，只不过是返回一个参数，不过需要注意的是需要把调用 bind 的时候传入的参数和返回的函数调用传入的参数合并依次传入。此时，又要杠精来了，说你用 `call` 来实现 `bind`, 我都能用 `call` 了，还需要你实现 `bind` 吗？那有没有办法能不用 `call` 或者 `apply` 实现 `bind` 呢？答案是有的，我先实现一个 `call` 或者 `apply` 然后在实现 `bind` 的时候调用自己实现的 `call` 或者 `apply` 不就好了么。

```js
function myCall(context, ...args){
  context = context || window;
  context.fn = this;
  const result = context.fn(...args);
  delete context.fn;
  return result;
}

function myBind(context, ...bindArgs) {
  // 保存当前函数，因为bind需要返回一个新的函数，在新的函数里this指向会有问题
  const self = this;
  // 不使用原生的call方法，使用自己写的 myCall 方法
  return function(...args){
    Function.prototype.myCall = myCall;
    return self.myCall(context, ...bindArgs, ...args)
  }
}
Function.prototype.myBind = myBind;

let obj = {
  name: 'jack',
  age: 18,
};
function getAge(desc, desc1, desc2) {
  console.log(desc, desc1, desc2);
  return this.age;
}
let myGetAge = getAge.myBind(obj, 'myBind调用', '参数1');
console.log(getAge('正常调用', '参数1', '参数2'));
// 正常调用 参数1 参数2
// undefined
console.log(myGetAge('参数2'));
// myBind调用 参数1 参数2
// 18
```
> 为什么实现 `bind` 还需要自己先实现 `call` 呢？按照 call 和 apply 的方法实现不行吗？我在实现的过程中发现有一些问题，具体可以看看。
```js
function myBind(context, ...bindArgs) {
    // 如果没有传需要绑定的对象，就指向 window
    const self = context || window;
    self.fn = this;
    const result = function(...args) {
      self.fn(...bindArgs, ...args);
    };
    // delete self.fn;
    return result;
}
Function.prototype.myBind = myBind;
let obj = {
    name: 'jack',
    age: 18,
};
function getName(desc, desc1, desc2) {
    console.log(desc, desc1, desc2);
    return this.age;
}
console.log(getName('正常调用', '参数1', '参数2'));
// 正常调用 参数1 参数2
// undefined
const newGetName = getName.myBind(obj, 'myApply调用', '参数1');
console.log(newGetName('参数2'));
// myApply调用 参数1 参数2
// 18
```
> 上面代码看起来没啥问题，但是，如果把 myBind 函数中的 delete self.fn 放开，那么就会报错了，现在这个方法实现有什么问题呢?

> 我们先打印一下 obj 看就明白了：
```js
console.log(obj);
// {name: 'jack', age: 18, fn: ƒ}
```

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3238bdb138604bfdb19e1fd02b2b2886~tplv-k3u1fbpfcp-watermark.image?)
> 我们在绑定 this 的对象上添加了一个 fn 属性，那么我们把他删掉试试呢？

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/01d8b203c2a74a2089a81207e915036a~tplv-k3u1fbpfcp-watermark.image?)
> 如下报错，为什么呢？因为我们在 `bind` 的时候返回的函数是通过 obj.fn 调用，`return` 函数之后又把 obj 上的 fn 属性删掉了，因为对象是引用地址原因，然后再去调用 `return` 的函数当然就会报错啦。理解了 `call` 和 `apply` 的实现，再写一个 `call` 和 `apply` 也是很简单的啦，所以手写 `bind` 需要在 `return` 的函数中去绑定 `this`，注意这一点就好了。 
