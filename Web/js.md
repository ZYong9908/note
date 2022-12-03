# 语法

## call、apply、bind

- call、apply、bind都是改变this指向的方法

### fn.call()

- 当前实例(函数fn)通过原型链的查找机制，找到`function.prototype`上的`call`方法，`function call(){[native code]}`

> 把找到的call方法执行
> 当call方法执行的时候，内部处理了一些事情
> 1.首先把要操作的函数中的this关键字变为call方法第一个传递的实参
> 2.把call方法第二个及之后的实参获取到
> 3.把要操作的函数执行，并且把第二个以后传递进来的实参传递给函数

```javascript
fn.call(target, arg1, arg2)
---------------------------------
// 非严格模式如果不传参数，或者第一个参数是null或nudefined，this都指向window
let fn = function(a,b){
    console.log(this,a,b)
}
let obj = {name:"obj"}
fn.call(obj,1,2);    // this:obj    a:1         b:2
fn.call(1,2);        // this:1      a:2         b:undefined
fn.call();           // this:window a:undefined b:undefined
fn.call(null);       // this=window a=undefined b=undefined
fn.call(undefined);  // this=window a=undefined b=undefined
// 严格模式第一个参数是谁，this就指向谁，包括null和undefined，如果不传参数this就是undefined
"use strict"
let fn = function(a,b){
    console.log(this,a,b);
}
let obj = {name:"obj"};
fn.call(obj,1,2);   // this:obj        a:1          b:2
fn.call(1,2);       // this:1          a:2          b=undefined
fn.call();          // this:undefined  a:undefined  b:undefined
fn.call(null);      // this:null       a:undefined  b:undefined
fn.call(undefined); // this:undefined  a:undefined  b:undefined
```

### fn.apply()

- apply：和call基本上一致，唯一区别在于传参方式

> apply把需要传递给fn的参数放到一个数组（或者类数组）中传递进去，虽然写的是一个数组，但是也相当于给fn一个个的传递

```javascript
fn.apply(target, [arg1, arg2])
```

### fn.bind()

- bind：语法和call一模一样，区别在于立即执行还是等待执行，bind不兼容IE6~8

```javascript
fn.bind(target, arg1, arg2); // 改变fn中的this，fn并不执行
```

# 常用函数

## 数字

### 设置浮点数精度

- .toFixed()

```javascript
var a=1.234
console.log('a:', a.toFixed(2));
// a:1.23
```



## 字符串

## 数组

### 查找下标

- .findIndex
  - 返回符合条件的第一项的下标


```js
const arr = [1, 2, 3, 4, 5, 3, 3, 2, 4, 5 ]

const index = arr.findIndex(item => {
    return item > 2
})
console.log(index) // 2
--------------------
const index = arr.findIndex(item => item > 2)
console.log(index) // 2
```



### 查找某一项

- .find
  - 查找满足条件的项，返回对应项的值

```js
const arr = [
    {
        name: '张三',
        age: 18
      },
      {
        name: '李四',
        age: 20
      },
      {
        name: '王五',
          age: 22
      }
]

const val = arr.find(item => {
    return item.age === 20
})
----------------
const val = arr.find(item => item.age === 20)
console.log(val)
// {
//     name: '李四',
//         age: 20
// }
```

