# React 基础

## 内容介绍

1. React 概述
2. JSX
3. React 组件基础
4. React 组件进阶
5. React 路由

---

## React 概述

> 目标1：能使用 React 脚手架创建项目
> 目标2：能够使用 React 创建 h1 标题并渲染到页面中

- React 介绍
- React 特点
- React 脚手架（CLI）
- React 的基本使用

### React 介绍

![React Logo](./images/react_logo.jpeg)

- [React 官网](https://reactjs.org/)
- [React 中文翻译](https://zh-hans.reactjs.org/)

React 是一个用于构建用户界面（UI，对咱们前端来说，简单理解为：HTML 页面）的 JavaScript 库  
React 起源于 Facebook 内部项目（News Feed，2011），后又用来架设 Instagram 的网站（2012），并于 2013 年 5 月开源  
React 是最流行的前端开发框架之一，其他：Vue、Angular、Svelte、SolidJS、jQuery 等等

### React 特点

- 声明式  => 关注 what 而不是 how（对应的 命令式）
- 基于组件  => 组件化
- 学习一次，随处使用  => 跨平台

从你的角度看 React 特点：

- 工资高、大厂必备（阿里在用）
- 工资高、大厂必备（字节跳动在用）
- 工资高、大厂必备（百度、腾讯、京东、蚂蚁金服、拼多多、美团、外企、银行等都在用）

### React 脚手架（CLI）

- React 脚手架的介绍
- 使用 React 脚手架创建项目
- 项目目录结构调整

#### React 脚手架的介绍

- 脚手架：为了保证各施工过程顺利进行而搭设的工作平台
- 对于前端项目开发来说，脚手架是为了保证前端项目开发过程顺利进行而搭设的开发平台
- 脚手架的意义：
  - 现代的前端开发日趋成熟，需要依赖于各种工具，比如，webpack、babel、eslint、sass/less/postcss等
  - 工具配置繁琐、重复，各项目之间的配置大同小异
  - 开发阶段、项目发布，配置不同
    - 项目开始前，帮你搭好架子，省去繁琐的 webpack 配置
    - 项目开发时，热更新、格式化代码、git 提交时自动校验代码格式等
    - 项目发布时，一键自动打包，包括：代码压缩、优化、按需加载等

#### 使用 React 脚手架创建项目

- 命令：`npx create-react-app react-basic`
  - npx create-react-app 是固定命令，`create-react-app` 是 React 脚手架的名称
  - react-basic 表示项目名称，可以修改
- 启动项目：`yarn start` or `npm start`
- `npx` 是 npm v5.2 版本新添加的命令，用来简化 npm 中工具包的使用
  - 原始：1 全局安装npm i -g create-react-app 2 在通过脚手架的命令来创建 React 项目
  - 现在：npx 调用最新的 create-react-app 直接创建 React 项目

#### 项目目录结构说明和调整

- 说明：
  - `src` 目录是我们写代码进行项目开发的目录
  - 查看 `package.json` 两个核心库：`react`、`react-dom`（脚手架已经帮我们安装好，我们直接用即可）
- 调整：
  1. 删除 src 目录下的所有文件
  2. 创建 index.js 文件作为项目的入口文件，在这个文件中写 React 代码即可

### React 的基本使用

1. 导入 ReactDOM
2. 创建 h1 标题，展示内容为：Hello React
3. 渲染标题到页面中

```js
import ReactDOM from 'react-dom'

// 把此处的 h1 标签看做 HTML 即可
const title = <h1>Hello React</h1>

ReactDOM.render(title, document.querySelector('#root'))
```

---

## JSX

> 目标：能用 JSX 创建任意 HTML 结构

- JSX 介绍
- JSX 中使用 JavaScript 表达式
- JSX 列表渲染
- JSX 条件渲染
- JSX 样式处理

### JSX 介绍

- JSX：是 JavaScript XML（HTML）的缩写，表示在 JS 代码中书写 HTML 结构
- 作用：在 React 中创建 UI（HTML）结构
- 优势：
  - 采用类似于 HTML 的语法，降低了学习成本，会 HTML 就会 JSX
  - 充分利用 JS 自身的能力来创建 HTML 结构。比如，利用 JS 数组的 map 方法创建列表结构

- 注意：HTML 属性 class -> `className`

注意：*JSX 不是标准的 JS 语法，是 JS 的语法扩展。脚手架中内置的 [@babel/plugin-transform-react-jsx](@babel/plugin-transform-react-jsx) 包，用来解析该语法。*

![JSX 声明式vs命令式](./images/JSX 声明式vs命令式.png)

### JSX 中使用 JavaScript 表达式

- 为什么在 JSX 中使用 JS 表达式？
  - 将 JS 中的数据展示在 HTML 结构中
  - 充分利用 JS 自身的能力来创建 HTML 结构

- 语法：`{ JS 表达式 }`
  - 比如，<h1>你好，我叫 {name}</h1>

- JS 表达式：数据类型和运算符的组合（可以单独出现数据类型，也可以数据类型+运算符的组合）

  - 特点：==有值== 或者说 能够计算出一个值
  - 字符串、数值、布尔值、null、undefined、object（ [] / {} ）
  - 1 + 2、'abc'.split('')、['a', 'b'].join('-')
  - function fn() {}、 fn()
  - 验证是不是 JS 表达式的技巧：看内容能不能作为方法的参数，比如，`console.log( 表达式 )`
  
  ```js
  // 函数声明 - 语句
  function fn() {}
  
  // 此时，等号右边的内容就是表达式（函数表达式）
  const foo = function fn() {}
  ```
  
  - 语句：if 语句/ switch-case 语句/ 变量声明语句，这些都不能出现在 `{}` 中！！！

### JSX 列表渲染

- 作用：重复生成相同的 HTML 结构，比如，歌曲列表、商品列表等
- 实现：使用数组的 `map` 方法
- 注意：需要为遍历项添加 `key` 属性
  - key 在 HTML 结构中是看不到的，是 React 内部用来进行性能优化时使用的
  - key 在当前列表中要唯一
  - 如果列表中有像 id 这种的唯一值，就用 id 来作为 key 值
  - 如果列表中没有像 id 这种的唯一值，就可以使用 index（下标）来作为 key 值

```js
const songs = [
  { id: 1, name: '痴心绝对' }, 
  { id: 2, name: '像我这样的人' }, 
  { id: 3, name: '南山南' }
]
const list = (
  <ul>
    {songs.map(item => <li key={item.id}>{item.name}</li>)}
  </ul>
)
```

### JSX 条件渲染

- 作用：根据是否满足条件生成 HTML 结构，比如，loading 效果
- 实现：可以使用`if/else`或`三元运算符`或`逻辑与(&&)运算符`

```js
const loadData = (isLoading) => {
  if (isLoading) {
    return <div>数据加载中，请稍后...</div> 
  }
  return (
    <div>数据加载完成，此处显示加载后的数据</div>
  )
}
const el = (
  <div>
    {loadData(true)}
    {loadData(false)}
  </div>
)
```

### JSX 样式处理

- 两种方式：
  1. 行内样式 - style
     1. 像 width/height 等属性，可以省略 px，直接使用 数值 即可
     2. 如果是需要使用百分比的单位，此时，继续使用字符串的值即可（比如，"60%"）
  2. 类名 - className【推荐】

```js
// 行内样式
<h1 style={ {color: 'red', backgroundColor: 'skyblue'} }>
  我想红，我是红
</h1>

// 类名
<h1 className="title">
  我想红，我是红
</h1>
```

- 使用 className：

```js
import './index.css'

// 类名
<h1 className="title">
  我想红，我是红
</h1>
```

### JSX 总结

- React 使用 JSX 来编写 UI（HTML）
- React 完全利用 JS 语言自身的能力来增强 UI 的编写 - 能用 JS 来实现的功能绝不会增加一个新的 API
- 现在，就可以使用 React 来编写任意 UI 结构了

---
