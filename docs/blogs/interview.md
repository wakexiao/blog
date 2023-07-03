---
title: 前端面试汇总
date: 2023-4-25
categories:
  - 前端
tags:
  - 面试
sticky: 1
---
<img src="https://s1.ax1x.com/2023/06/30/pCBi8VP.jpg" alt=""/>
<!-- more -->

## 一. HTML + CSS
> 一般 HTML 和 CSS 相关的面试题都问的会比较少；只挑了部分常问的问题做整理。

### 1.1 语义化的理解
- 1. 使页面内容结构化，便于浏览器和搜索引擎解析，利于 seo
- 2. 增加代码可读性，利于维护
- 3. 在没有css样式的情况下，页面也能呈现出很好的内容结构

### 1.2 H5 新增的新特性
- 1. 语义化标签：section、header、footer、nav...
- 2. 智能表单：type = tel | email
- 3. 本地存储新增了 locaStorage 和 sectionStorage

### 1.3 CSS 盒模型
- CSS 中有两种盒模型：标准盒模型、(IE)怪异盒模型;
> 这两种额盒模型都是由 content + padding + border + margin 构成， 但是盒子内容的宽高计算方式会有所不同；标准盒模型：宽高只包含content，怪异盒模型：宽高包含 content + padding + border；可以通过 CSS 的 box-sizing 属性来修改元素的盒模型：
```css
box-sizing: 'border-box'; // 怪异盒模型
box-sizing: 'content-box'; // 标准盒模型(默认值)
```

### 1.4 什么是 BFC
> BFC 全称为：block formatting context，名为 “块级格式化上下文"

`W3C`官方解释为：`BFC`它决定了元素如何对其内容进行定位，以及与其它元素的关系和相互作用，当涉及到可视化布局时，`Block Formatting Context`提供了一个环境，`HTML`在这个环境中按照一定的规则进行布局。

简单来说就是，`BFC`是一个完全独立的空间（布局环境），让空间里的子元素不会影响到外面的布局。

**创建 BFC 的方式**  
1. 浮动元素：float 的值不为 none  
2. 定位元素：position 的值为 absolute 或 fixed  
3. overflow 的值不为 visible
4. display 的值是 inline-block、table-cell、flex

**BFC 能解决什么问题**
1. 解决两个块级元素之间垂直方向 margin 重叠
2. 使用 float 脱离文档流之后造成了高度塌陷，使用在父元素增加 ```overflow: hidden;``` 创建一个 BFC 来解决高度塌陷。

### 1.5 水平垂直居中的实现方式
- 1. 使用 flex
```css
.parent {
  display: flex;
  justify-content: center;
  algin-items: center;
}
```
- 2. 使用 position + margin
```css
.parent {
  position: relative;
}
.children {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  margin: auto;
}
```
- 3. 使用 position + transform
```css
.parent {
  position: relative;
}
.children {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%)
}
```

### 1.6 使用 flex 常用的属性有哪些
- 6 个设置在 flex 容器上的属性：
1. flex-direction 决定主轴的方向：
    - flex-direction: row | column | row-reverse | column-reverse
2. flew-wrap 一条主轴排列不下的时候如何换行
    - flex-wrap: wrap | nowrap | wrap-reverse
3. flex-flow 是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap;
    - flex-flow: column wrap;
4. justify-content 定义了子元素在主轴上的对齐方式
    - justify-content: flex-start | flex-end | center | space-between | space-around
5. algin-items 定义了子元素在交叉轴上如何对齐
    - algin-items: flex-start | flex-end | center | baseline | stretch
6. align-content 定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。
    - align-content: flex-start | flex-end | center | space-between | space-around | stretch
> 前五个属性比较常用

- 6个设置在子元素上的属性：
1. order 定义项目的排列顺序。数值越小，排列越靠前，默认为0。
2. flex-grow 定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。
3. flex-shrink 定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。
4. flex-basis 定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。
5. flex属性是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选。flex: 1 => 1 1 auto
6. align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。

> 第四个 flex 缩写的熟悉用的比较多；flex 详细介绍可以查看[阮一峰flex教程](https://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)

## 二、JS 基础

### 2.1 JS 的数据类型
- JS 中有两种数据类型：基本数据类型(简单数据类型/原始数据) 和 引用数据类型(复杂数据类型)
1. [七种基本数据类型](https://developer.mozilla.org/zh-CN/docs/Glossary/Primitive)：
`string`、`number`、`boolean`、`undefined`、`null`、`symbol`、`bigint`

2. 一种引用数据类型：`Object`
> 在 js 中，array、function、object、Date 等都归类为 Object，通过 typeof 判断类型，这些返回的都是 object 字符串，但是 typeof null 也是 object，这是特例。

### 2.2 typeof 可以判断哪些类型
> typeof 能判断除 null 外的基本数据类型和 function，识别引用数据类型和 null 都返回 object。

### 2.3 原型和原型链
> 每个构造函数都有一个 prototype 指向原型对象，原型对象有一个 constructor 属性指回构造函数，而通过构造函数实例出来的对象有一个 __proto__ 属性指向构造函数的 prototype，在实例自身没有找到属性的时候，会通过原型链一层一层的往上找，最终找到顶层 null。
```js
function Person(name, age){
  this.name = name;
  this.age = age;
};
Person.prototype.field = 'ps';
const person = new Person('jack', 18);
console.log(person); // {name: 'jack', age: 18}
console.log(person.field); // ps
// 通过构造函数创建出来的实例对象会有一个 __proto__ 属性，指向构造函数的原型对象 peototype
// 如果自身没有这个属性则会通过原型链往上层找
console.log(person.__proto__ === Person.prototype);  // true
// 构造函数的 prototype.constructor 指向 构造函数本身
console.log(Person.prototype.constructor === Person);  // true
console.log(Person.prototype.__proto__ === Function.prototype);  // false
// 构造函数原型对象上的 __proto__ 指向 Object 的原型对象，构造函数本身也是一个对象
console.log(Person.prototype.__proto__ === Object.prototype);  // true
// 对象原型上的 __proto__ 属性指向 null，原型链的顶层就是 null 
console.log(Object.prototype.__proto__ === null);  // true
```
- 实现类似一个 instanceof 方法
```js
/**
 * 实现类似 instanceof 方法
 * @param {object} instance 需要判断的对象
 * @param {object} origin 是否属于该原型对象
 * @return boolean
 */
export default function instance_of(instance, origin){
  // 处理 undefined 或 null
  if(instance == null) return false;
  // 处理基本数据类型
  if(typeof instance !== 'object' && typeof instance !== 'function') return false;
  const tempInstance = instance;
  while(tempInstance) {
    if(tempInstance.__proto__ === origin.prototype){
       return true;
    }
    // 未匹配上 往上层继续找
    tempInstance = tempInstance.__proto__;
  }
  // 已经到最顶层 null，上方循环结束
  return false;
}
```

### 2.4 new 操作符做了什么
- 1. 首先创建有一个空对象
- 2. 根据原型链，设置空对象的 `__proto__` 指向构造函数的 `prototype`
- 3. 构造函数的 `this` 指向这个对象并且执行该构造函数(这个构造函数可能通过this挂在了其他属性)
- 4. 把执行之后返回的对象 return 出去(需要判断执行构造函数返回的是否是一个对象，如果是正常返回，不是返回空对象)

```js
function myNew(context) {
  const obj = {}; // 创建一个新对象
  // 将新对象的 __proto__ 指向构造函数的 prototype
  obj.__proto__ = context.prototype;
  // 绑定构造函数的 this 并且执行
  const result = context.apply(obj, [...arguments].slice(1));
  // 把构造函数返回的值 return 出去，如果执行构造函数之后返回的是一个对象，直接返回，如果不是对象返回一个空对象
  return result instanceof Object ? result : obj;
}
```
### 2.5 Object.create 和 new Object 
> Object.create({}) 是先创建一个空对象，然后把空对象的隐式原型 __proto__ 指向这个 create 传入的参数;  
> new Object() 和直接定义 {} 是相等的;
### 2.6 call、apply、bind 的区别和实现
> 可以看我这篇文章，很容易理解：[深入理解 call,apply,bind 的原理和使用](/blogs/call_apply_bind.html)
### 2.7 ```for in``` 和 ```for of``` 有什么区别
> ```for in``` 遍历可枚举数据： 例如：Object,String,Array，遍历得到的是 key，不能遍历 Map,Set 数据
>
> ```for of``` 遍历可迭代数据： 例如：Map,Set,Array,String, 遍历得到的是 value，不能遍历 Object

### 2.8 作用域和this
- js 词法作用域和动态作用域是什么？js 是没有动态作用域的，作用域在函数定义的时候就已经确定了，也就是词法作用域（静态作用域）。
> 自由变量会在函数定义地方的作用域中去一层一层往上层找，而不是执行作用域中往上查找；  

> this 的值是在执行的时候确定的，不是在定义的时候确定的。
### 2.9 事件流
> 捕获 -> 目标阶段 -> 冒泡  
> 事件通过捕获到达目标阶段，再从目标对象冒泡到document对象
- addEventListener 的第三个参数：true 表示该元素在捕获阶段响应事件，false 表示该元素在冒泡阶段响应事件；默认为 false。

### 2.10 js 加法的隐式转换
分为两种情况：  
1. 均为原始类型 -> 有字符串都转为字符串类型，再拼接；没有字符串都转为数字，再做加法，如果有一端为NaN，结果就是NaN  
2. 含有对象类型 -> 对象类型的值先调用 valueOf 方法，如果这时符合第一种情况，就按第一种方法走下去；不符合第一种情况就再调用 toString 方法，如果这时还是不符合就报错，符合就按第一种情况走下去。


## 三. React
### 3.1. vue 和 react 的区别
不同点：
1. 最直观的就是模板渲染方式不同；react 使用的 JSX 语法，通过 babel 编译转换成 React.createElement 语法，而 vue 使用的是一种拓展的 HTML 语法渲染。
2. 数据流的不同；vue 使用的是响应式的数据，使用 Object.defineProperty 双向绑定，而 react 是单向数据流，没有双向绑定。两个框架更新数据的方式也不一样，vue 是直接通过 this 去修改这个组将上的数据，而 react 只能通过 setState 去修改。
3. 两个框架监听数据变化实现的原理也不一样；vue 是通过 Object.defineProperty 方法去劫持数据的 getter 和 setter 方法，能精确知道数据的变化，从而去更新视图，数据是可变的。react 则是通过数据的引用然后再通过 diff 算法去更新视图，需要手动的去 setState，使用新的 state 去替换旧的 state，数据是不可变的；所以如果 react 不优化会造成不必要的渲染开销，以组件来说，当数据改变了，这个组件都会重新渲染（包括子组件），所以可以通过 shouldComponentUpdate 生命周期去判断子组件是否需要重新 render，PureComponent 组件就是通过这个方法去优化的，hooks 可以使用 React.memo 高阶组件。

相同点：
1. 都使用了虚拟 dom。
2. 都是用数据去驱动视图。
3. 都有组件化这个概念。

### 3.2. setState 和 useState 的区别
我们都知道 setState 和 useState 都是异步更新的; 那我们看看这两个方法有什么区别呢？
function 组件使用 useState，用法：
```
const [a, setA] = useState(0);
setA(value | func); 
// 单传 value 为需要更新的值；
// 传 function 的话，function 能接收到当前 a 的值：(a) => a + 1，函数的返回值将会是更新 a 的值。
```
```js
import React, { useState } from 'react';

export default function DemoUseState(props) {
  const [a, setA] = useState(0);

  const handleAddClick = () => {
    // setA(a + 1);
    // setA(a + 1); // 如果直接传值去render的话，这里 set 两次也会只有一次的效果，为什么呢？
    // 因为在这个 function 里面两次 set 获取到 a 的值是上一次的值，所以相当于这两次 set 都set了相同的值

    // 同步 set 会合并在一个方法里去 set，并且会依次执行，但只 render 一次，先 set a + 1 再 set a + 1; 
    setA((a) => a + 1);
    setA((a) => a + 1);
  };
  const handleAsyncSubClick = () => {
    // 异步 set 会依次执行，不会合并去更新，render 两次，每次 set a - 1
    Promise.resolve().then(() => {
      // setA(a - 1); 
      // setA(a - 1); // 这里这样写虽然异步会 render 两次，但是和上面 + 是一样的，a 的值两次都是一样的

      setA((a) => a - 1);
      setA((a) => a - 1);
    });
  };
  console.log('render useState', a);
  return (
    <div>
      <div>a: {a}</div>
      <button onClick={handleAddClick}>同步加</button>
      <button onClick={handleAsyncSubClick}>异步减</button>
    </div>
  );
}

```
class setState 的用法：
```
import React, { Component } from 'react';

export default class DemoSetState extends Component {
  constructor(props) {
    super(props);
    this.state = {
      a: 0,
    };
  }
  handleAddClick = () => {
    // 同步：合并到一个方法里去调用，render 一次
    this.setState(
      (state, props) => ({ a: state.a + 1 }),
      () => { // 这里也可以使用普通函数，react 底层使用 bind 绑定了 this
        // 因为同步的原因，批量处理 setState，这里获取到第一次加 a 的值是 2
        console.log('第一次加之后', this.state.a); // 第一次加之后 2
      }
    );
    this.setState(
      (state, props) => ({ a: state.a + 1 }),
      () => {
        console.log('第二次加之后', this.state.a); // 第二次加之后 2
      }
    );
  };
  handleAsyncSubClick = () => {
    // 异步：render 两次，和 useState 一样
    Promise.resolve().then(() => {
      this.setState(
        (state, props) => ({ a: state.a - 1 }),
        () => {
          // 异步单独处理每一个 setState，这里获取到第一次减 a 的值是 1
          console.log('第一次减之后', this.state.a); // 第一次减之后 1 点击了加按钮之后再点击减按钮
        }
      );
      this.setState(
        (state, props) => ({ a: state.a - 1 }),
        () => {
          console.log('第二次减之后', this.state.a); // 第二次减之后 0
        }
      );
    });
  };
  render() {
    console.log('render setState', this.state.a);
    return (
      <div>
        <div>a: {this.state.a}</div>
        <button onClick={this.handleAddClick}>同步加</button>
        <button onClick={this.handleAsyncSubClick}>异步减</button>
      </div>
    );
  }
}
```
> 总结：
> 1. useState 和 setState 是针对不同组件的，一个是 class 组件，一个是 function 组件；class 组件在 this.state 里设置默认值，通过setState 去修改值；function 通过 useState 方法传入默认参数，返回一个值和设置值的方法  
> 2. set 使用方面两个方法的逻辑处理基本一样，同步 set 会批量处理到一个方法里去调用，只 render 一次；异步更新会依次调每一个 set。
> 3. setState 有第二个 callback 参数，在这个 callback 函数里可以通过 this.state 立马获取到执行当前 setState 之后的值；同步的话因为 setState 会批量处理，所以不管 setState 几次，第二个参数里通过 this 获取到的 state 都是批量处理后 state 的值。
> 4. setState 默认是"异步"更新(会比宏任务先执行)，也会有同步更新的情况；在异步代码中， 不在 react 上下文中触发，比如 SetTimeout、promise.then、自定义 DOM 事件、ajax 回调中触发 setState 就会同步更新，如果是同步代码中执行 setState 会将多个 setState 合并为一个，后者会覆盖前者。setState 在异步代码中同步更新的情况在 react 18 版本中也支持异步更新了，需要把 ReactROM.render 换成 ReactDOM.createRoot。

备注：setState 是"异步"的，但是本质上是同步的，只不过让 React 做成了异步的样子；因为要考虑性能问题，多次 state 修改，只进行一次 DOM 渲染。React 使用的是合成事件，在 React 内部执行函数的时候，可以理解成他遇到 setState 操作的时候，将所有的 setState 操作都放到一个数组中去了，执行完这个函数之后再去执行 setState，这样就可以理解为什么 setState 会比宏任务先执行了。

### 3.3. react 中 useMemo 和 useCallback
推荐文章： [详解 React useCallback & useMemo](https://juejin.cn/post/6844904101445124110)
> react hooks 都是在函数组件中使用的，每当组件重新 render，这个函数也将重新执行，里面声明的数据和function都将重新声明，导致如果声明的数据或者 function 依赖的state没有变化，也会重新声明。并且如果将这些数据或者function当作props传给子组件，即便子组件依赖的props没有发送改变，因为父组件重新render，数据和 function 页重新声明了，所以每次传给子组件都是新的值，子组件也会重新 render，造成了不必要的开销。

> 使用 useMemo 和 useCallback 就是为了当数据或者函数的依赖没有发生改变的时候，将它缓存下来；useMemo 缓存的是传入函数返回的值，useMemo一般用来缓存一些耗时计算的数据，避免重复计算；useCallback 缓存的是传入的函数；useCallback 的功能也可以用 useMemo 来实现，传入的函数 return 出一个函数就是变通的实现 useCallback；这两个 hooks 一般配合 React.mome 包裹子组件来优化子组件，避免子组件不必要的render；React.memo 高阶组件是对组件接收的 props 全部遍历一遍，和之前的props做一次浅比较，和之前的 PurComponent 一样的效果。

> 注意：也并不是所有的数据和 function 都要包裹 useMemo 和 useCallback，可能还会造成反向优化，声明函数和数据占用的开销是微乎其微的；况且使用这两个hook都需要重新声明一个新的函数传入进去，也会造成一定的开销，所以可能会造成反向优化。

### 3.4. React合成事件和原生事件
推荐文章：1.[深入React合成事件机制原理](https://juejin.cn/post/6922444987091124232) 2. [React事件和原生事件关系](https://blog.csdn.net/G_ZZH/article/details/117529660)
-  为什么 react 要使用合成事件  
    1. 为了兼容不同浏览器，抹平浏览器的兼容性差异
    2. 使用 fiber 机制的原因，jsx 中绑定的事件是通过 props 传递下去的，在生成 fiber 节点的时候，他对应的 dom 节点可能还未挂载
    3. 对事件进行归类，可以在事件产生的任务上包含不同的优先级
 - 顶层注册，事件收集，统一触发
 > react 17之前是将事件注册到 document 对象上，17 之后变成了 root 节点。  
 > 事件触发顺序： 原生事件先触发，原生事件按照事件流触发，捕获 -> 目标 -> 冒泡，之后才触发 react 合成事件

### 3.5 说说 redux
推荐文章 [一篇文章总结redux、react-redux、redux-saga](https://juejin.cn/post/6844903846666321934)
-   **Redux 的三大原则**
    - 单一数据源（单向数据流）
    - state 是只读的
    - 使用纯函数来修改 state
> redux 是 react 最常用的一个状态管理库，把很多页面常用的数据统一存放在 store 中，需要修改 store 中的数据唯一的方式就是 dispatch 一个 action，这个action 对应的 reducer 去处理store 中的数据，reducer 是一个纯函数，它不允许修改入参，store 中的数据也是不能直接修改的，通过创建一个新的 state 去替换之前的 state 来更新 store 中的数据；dispath action 只能是同步的，如果需要支持异步action，需要使用 redux-thunk 或者 redux-saga 中间件，原理就是判断 store.dispach 传入进来的是不是一个函数，如果是函数的话就执行这个函数，否则就直接 dispatch；一般 redux 是配合 react-redux 库使用的，否则组件需要手动 subscribe 才会获取到store 中最新的数据，使用 react-redux 中的 connect 高阶组件把 store 中 state 和 action 通过props 传递给组件

## 四. 其他
### 4.1 如何编写高质量的代码
1. 代码规范
2. 功能完整
3. 代码健壮(鲁棒)
### 4.2 code review 相关
1. 代码规范(eslint 不能完全检查，比如 变量命名、代码语意)
2. 重复代码需要抽离、复用
3. 单个函数内容过程，需要拆分
4. 算法复杂度是否可用？是否可用继续优化
5. 是否有安全相关的漏洞
6. 是否和现有的功能重复
7. 组件设计是否合理

### 4.3 TS 中 type 和 interface 区别
1. type 支持定义基本数据类型，interface 不支持。
2. type 是一个赋值操作，只是将自己定义的别名和类型关联起来，不是创建一个新的类型，而是对类型的一个引用；interface 定义的是一个接口类型；我个人使用一般type 多用于联合类型，interface 定义一个对象的结构组成。
3. interface 可以用 extends 实现继承，type 可以通过 & 符号（也叫交叉类型）实现类似继承的效果，交叉类型只适用于表示对象类型的类型别名。
4. interface 支持同名合并，也就是可以使用相同的名字定义 interface，最终同名的 interface 会自动合并，且会影响之前使用的这个 interface，最终的规则是以合并之后的规则。

> 总结： type 可以声明基本数据类型也可以声明对象，interface不支持声明基本数据类型；一般用type声明联合类型，interface用来声明一个对象的结构，interface 可以使用 extends 实现继承，也可以多次声明，多次声明会合并，type 可以用 & 符号实现类似继承的效果，也叫交叉继承。

### 4.4 说说你对 webpack 的理解
推荐文章：1. [「吐血整理」再来一打Webpack面试题](https://juejin.cn/post/6844904094281236487) 2. [# 当面试官问Webpack的时候他想知道什么](https://juejin.cn/post/6943468761575849992)
> webpack 是一个模块化打包工具，有五大核心模块，entry 入口js 文件，loader 模块加载器（loader 实在module 的 rules 里配置的，通过正则匹配文件后缀名来使用不同的 loader，loader 是一个数组的配置，可以配置多个loader，按顺序执行，上一个loader执行返回的内容会成为下一个loader的入参），plugin 插件，output 打包完输出的文件目录，mode 打包模式，分为 development 开发环境打包和 production 生成环境打包

### 4.5 webpack 中的 loader 和 plugin 有什么区别
推荐文章： [吐血整理的webpack入门知识及常用loader和plugin](https://juejin.cn/post/7067051380803895310)
> loader 模块代码转换器；让webpack能够去处理除了JS、JSON之外的其他类型的文件，是针对单个文件的。常用的 laoder 有 babel-lader、ts-loader、css-loader、style-loader、post-css、less-loader 等。

> plugin 扩展插件；用于扩展webpack的功能，处理 laoder 不能处理的事情；是作用于 webpack 本身，可以监听 webpack 的生命周期在合适的时机通过 webapck 提供的 api 改变输出的结果。常用的 plugin 有 clean-webpack-plugin（开始打包之前清理 output 目录 dist 文件夹）、html-webpack-plugin（生产html文件或者使用自定义的html模板，并且可以将打包生成的js文件自动引入到生成的 html 文件中）

### 4.6 从浏览器地址栏输入 url 到请求返回发生了什么
- 1. 通过 DNS 域名解析拿到实际的 IP 地址
- 2. 检查是否有命中缓存（强缓存、协商缓存），html 文件不能被强缓存缓存
- 3. 建立 TCP 连接（三次握手，四次挥手），发起 http 请求
- 4. 收到服务器响应返回的 html 文件, 浏览器开始从上往下解析这个 html 文件
- 5. 构建 dom 树，加载其他资源，例如引入的 css、js、图片 等，遇到 js 文件，会先加载并执行 js 脚本，会阻塞浏览器的渲染，浏览器处理 js 中的事件循环等异步逻辑

### 4.7 script 标签中 defer 和 async 的区别
- 当浏览器遇到 script 标签时会阻塞 HTML 解析，只有 JS 内容加载并执行完成之后才会继续解析 HTML 内容。
- script 标签加上 async 属性表示异步，会异步去请求该 JS 内容，如果请求完之后，HTML 还没有解析完，会阻塞浏览器解析 HTML，先执行请求回来的 JS 文件，执行完后再进行解析；当然，如果在 JS 脚本请求回来之前就已经解析完 HTML，那就正常执行 JS 代码。
- script 标签加上 defer 属性表示延迟，会异步去请求该 JS 内容，和 async 不同的是，如果请求完之后，HTML 还没有解析完，不会暂停解析 HTML，而是等 HTML 解析完之后再执行 JS 代码。
> 总结：1. 什么属性都没有的 script 标签，在 HTML 中按顺序执行，会阻塞 HTML 解析，所以 JS 的 script 标签一般都放在 body 之后；2. 有 async 属性可能会造成 HTML 解析阻塞；有 defer 属性不会造成 HTML 解析阻塞；可以参考这篇文章：[图解 script 标签中的 async 和 defer 属性](https://juejin.cn/post/6894629999215640583)

### 4.8 http1.0、http1.1 和 http2.0 有什么区别
- http1.0  
    1. 最基础的 http 协议，只支持 get、post 方法请求  
- http1.1  
    1. 新增了缓存策略，cache-control、etag  
    2. 支持长连接，connection: keep-alive，一次 TCP 连接多次请求  
    3. 支持断点续传（大文件分片上传），状态码206  
    4. 支持新的方法请求，put、delete 等，支持 Restful API  
- http2.0（http2.0 只支持 https 协议，还需要在 nginx 配置）
    1. 支持压缩 header，减少体积
    2. 多路复用，一次 TCP 连接中可以多个 http 并行请求（http1.1 最大的并发数是 6 个，不同的浏览器也可能不一样）

### 4.9 http 缓存
推荐文章： [你知道304吗？图解强缓存和协商缓存](https://juejin.cn/post/6974529351270268958#heading-15)

- 强缓存： 直接从本地获取资源，状态码 200
> 强缓存是通过服务器响应返回的 responseHeaders 中 cache-control 字段设置的 max-age 值，再次请求该资源会判断cache-control 中 max-age 值的时间是否过期，未过期则直接从本地缓存中拉取资源，过期了就重新请求，例如：`Cache-Control: max-age = 31536000`。cache-control 也可以设置其他值，比如 no-cache，不使用本地缓存。除了 cache-control 字段，很早之前使用的是 expires，也是设置一个时间；如果 cache-control 和 expires 同时存在，cache-control 优先级较高。

总结：强缓存是通过 cache-control 字段中 max-age 设置的时间值来判断的，未过期不会发起请求，直接从本地缓存中拉去资源，状态码是 200。

- 协商缓存： 服务端缓存策略，状态码 304
> 协商缓存是通过服务器响应返回的 responseHeaders 中 last-modified 或者 etag 字段的值，再次请求该资源时，如果服务器返回了 last-modified 字段，会将 last-modified 的值保存下来，在 requestHeaders 的 if-modified-since 字段带上这个值发送给服务器，服务器收到请求会比较 if-modified-since 字段的值和服务器上该资源的 last-modified 的值是否相同，相同则返回304，表示该资源和上一次请求没有变化，不通则返回 200，并返回新的资源和新的 last-modified 值；如果服务器返回的是 etag 字段，会将 etag 的值保存下来，在 requestHeaders 的 if-none-match 字段带上这个值发送给服务器，服务器收到请求会比较 if-none-match 的值和该资源最后一次修改的 etag 值是否相等，相等则返回304，表示该资源和上一次请求没有变化，不同则返回 200，并返回新的资源和新的 etag 值。当 last-modified 和 etag 同时存在时，会优先使用 etag。etag 是该资源生成的唯一hash字符串标识，last-modified 是该资源最后修改的时间，最多精确到秒级，所以 etag 会更精确。

总结：协商缓存是服务端通过 last-modified 或者 etag 值来判断的，状态码是 200；etag 是该资源生成的唯一hash字符串标识，last-modified 是一个时间值，etag 优先级较高也更精确。

### 4.10 你在项目中有做过哪些性能优化吗
- 静态资源使用 cdn 加载
- 路由懒加载、图片懒加载
- 防抖和节流
- 使用 useCallback 配合 React.Memo 减少子组件不必要的 render
- 组件卸载清除定时器和监听事件
- 使用 http2，使用了 http2 之后，减少http请求的优化就意义不大了，因为 http2 支持并发请求
- 统一操作 dom 或者通过修改 class 来修改样式，减少重排和重绘
- 注意代码逻辑，如果循环嵌套三层就需要考虑是否有更优的算法来解决，On(3) 的时间复杂度基本就是不可用的

## 五. 和 HR 谈薪资相关问题

1. 有没有年终奖，是不是固定12薪，每个月几号发工资
2. 试用期几个月，打折吗？转正有没有调薪机会
3. 上下班时间，是不是弹性上班打
4. 加班怎么算，调休还是算工资
5. 有没有其他补贴或者福利，比如：自带电脑、餐补、交通有补贴吗或者每年公司会安排体检吗 等
6. 每年有没有调薪机会
7. 五险一金按什么基数比例交
8. 有没有年假，是多少天
