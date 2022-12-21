# 路由

## router-link

- 可以在不重新加载页面的情况下更改 URL，处理 URL 的生成以及编码

```vue
<router-link to="/about">Go to About</router-link>
```

## router-view

- `router-view` 将显示与 url 对应的组件。可以放在任何地方，以适应布局

```vue
<router-view> <!-- 路由匹配到的组件将渲染在这里 --> </router-view>
```

## $router和$route的区别

- `$router`: 路由操作对象，只写。需要对路由进行操作时使用。如路由跳转
- `$route`: 路由信息对象，只读。获取路由相关信息时使用。如获取当前路由地址

## 获取当前路由地址

```javascript
var url = this.$route.path
```

## 参数

### query(get)

#### 传参

- name 和 path 都能用。用 path 的时候，提供的 path 值必须是相对于**根路径**的相对路径，而不是相对于父路由的相对路径，否则无法成功访问

```javascript
this.$router.push({ path: '/path', query: { id: '123', data: '456' } })
```

#### 获取参数

```js
this.$route.query.xxx
```

### params(post)

#### 传参

- 只能用 name，不能用 path

```javascript
this.$router.push({ name: 'name', params: { id: '123', data: '456' } })
```

#### 获取参数

```js
this.$route.params.xxx
```

