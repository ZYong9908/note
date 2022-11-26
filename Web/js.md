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

