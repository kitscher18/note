# 极客园 PC 端项目

## 资料

- PC端仓库：https://gitee.com/zqran/geek-pc
- 短信接收&M端演示：http://geek.itheima.net/
- PC 端接口文档：http://geek.itheima.net/api-pc.html

## 项目介绍

>「极客园」对标`CSDN`、`博客园`等竞品，致力成为更加贴近年轻 IT 从业者（学员）的科技资讯类应用  
> 产品关键词：IT、极客、活力、科技、技术分享、前沿动态、内容社交  
> 用户特点：年轻有活力，对IT领域前言科技信息充满探索欲和学习热情

- 极客园 PC 端项目：个人自媒体管理端
- 项目演示
- 项目功能，包括
  - 登录
  - 首页
  - 内容（文章）管理
    - 文章列表
    - 发布文章
    - 修改文章
- 技术栈：
  - `react` v17.0.2 / `react-dom` v17.0.2
  - ajax请求库：`axios`
  - 路由：`react-router-dom` 以及 `history`
  - 富文本编辑器：`react-quill`
  - CSS 预编译器：`sass`
  - UI 组件库：`antd` v4
  - 项目搭建：React 官方脚手架 `create-react-app`

## 项目搭建

1. 使用 React CLI 搭建项目：`npx create-react-app geek-pc-87`
2. 进入项目根目录：`cd geek-pc-87`
3. 启动项目：`yarn start`
4. 调整项目目录结构：

```tree
/src
  /assets         项目资源文件，比如，图片 等
  /components     通用组件
  /pages          页面
  /utils          工具，比如，token、axios 的封装等
  App.css         根组件样式文件
  App.js          根组件
  index.css       全局样式
  index.js        项目入口
```

- 仓库地址：`https://gitee.com/zqran/geek-pc-87.git`

## 配置基础路由

1. 安装路由：`yarn add react-router-dom`
2. 在 pages 目录中创建两个页面：Login、Layout
3. 在 App 组件中，导入路由组件以及两个页面组件
4. 配置路由规则

```js
// 导入路由
import { BrowserRouter as Router, Route } from 'react-router-dom'

// 导入页面组件
import Login from './pages/Login'
import Layout from './pages/Layout'

// 配置路由规则
function App() {
  return (
    <Router>
      <div className="App">
        <Route path="/home" component={Layout}></Route>
        <Route path="/login" component={Login}></Route>
      </div>
    </Router>
  )
}
```

## 组件库 - antd

[Ant Design](https://ant.design/index-cn)
[antd PC 端组件库文档](https://ant.design/docs/react/introduce-cn)

> `antd` 是基于 Ant Design 设计体系的 React UI 组件库，主要用于研发企业级中后台产品。
> **开箱即用**

1. 安装：`yarn add antd`
2. 使用：

```js
// 1 在 index.js 中导入 antd 的样式文件
import 'antd/dist/antd.css'

// 2 在 Login 页面组件中，使用 antd 的 Button 组件
import { Button } from 'antd'

const Login = () => (
  <div>
    <Button type="primary">Button</Button>
  </div>
)
```

## 项目功能 - 登录

功能如下：

1. 搭建登录页面结构
2. 登录表单校验
3. 登录逻辑
4. 封装处理 token、axios 的工具函数
5. 登录访问控制

### 搭建登录页面结构

```jsx
<div className="login">
  <Card className="login-container">
    <img className="login-logo" src={logo} alt="" />
    <Form> ... </Form>
  </Card>
</div>
```

- SASS 样式

```scss
.login {
  width: 100%;
  height: 100%;
  position: absolute;
  left: 0;
  top: 0;
  background-image: url(../../assets/login.png);
}

.login-container {
  width: 440px;
  height: 360px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  box-shadow: 0 0 50px rgb(0 0 0 / 10%);
}

.login-logo {
  width: 200px;
  height: 60px;
  display: block;
  margin: 0 auto 20px;
}

.login-checkbox-label {
  color: #1890ff;
}
```

### 登录表单校验

```js
<Form.Item
  rules={[
    {
      pattern: /^1[3-9]\d{9}$/,
      message: '手机号码格式不对',
      validateTrigger: 'onBlur'
    },
    { required: true, message: '请输入手机号' }
  ]}
>
</Form.Item>
```

### 登录逻辑

- 安装 axios：`yarn add axios`

1. 导入 axios
2. 创建 login 方法来处理登录逻辑，并将其作为 Form 的 `onFinish` 属性的值
  - 说明1：当表单校验成功时，会触发 onFinish，也就会调用 login 方法
  - 说明2：通过 login 方法的参数就可以获取到表单中所有表单项的值（手机号、验证码）
3. 拿到手机号和验证码，通过 axios 发送请求，进行登录
4. 登陆成功时，将 token 保存到本地缓存中，并且跳转到后台首页
5. 登录失败时，提示错误信息

- 接口的基础路径：`http://geek.itheima.net/`
  - 比如，登录接口地址为 `/v1_0/authorizations`，需要将基础路径和登录功能自己的地址拼接到一块：`http://geek.itheima.net/v1_0/authorizations`

### 封装处理 token、axios 的工具函数

不管是 token 还是 axios 都会被多次使用，因此，封装成可复用的函数来提升开发效率

- 封装 token：
  1. 在 `utils` 目录中创建 token.js 文件
  2. 创建获取 token 的函数
  3. 创建设置 token 的函数
  4. 创建删除 token 的函数

```js
// 使用常量保存操作 localStorage 时用到的 key
// 优势：避免重复多次书写字符串，同时，减少出错的几率
const TOKEN_KEY = 'itcast_geek_pc_87'

// 获取 token
const getToken = () => localStorage.getItem(TOKEN_KEY)
// 设置 token
const setToken = token => localStorage.setItem(TOKEN_KEY, token)
// 删除 token
const removeToken = () => localStorage.removeItem(TOKEN_KEY)

export { getToken, setToken, removeToken }
```

- 封装 axios：
  1. 在 `utils` 目录中创建 http.js 文件
  2. 导入 axios
  3. 创建 axios 实例
  4. 设置接口基础路径

```js
import axios from 'axios'

const http = axios.create({
  baseURL: 'http://geek.itheima.net/v1_0',
  timeout: 5000
})

export { http }
```

- 优化导入
  - 目的：**通过一个 import 语句同时导入 http 或 token 的工具**
  1. 在 `utils` 目录中创建 index.js 文件
  2. 在 index.js 中，导入并导出 token.js 中的所有内容
  3. 在 index.js 中，导入并导出 http.js 中的所有内容

```js
export * from './token'
export * from './http'
```

### 登录访问控制

对于极客园 PC 端项目来说，

- 有的页面*不需要登录*就可以访问，比如，登录页
- 有的页面*需要登录*后才能访问，比如，项目后台首页、内容管理等（除了登录页面，其他页面需要登录才能访问）

因此，就需要对项目进行登录访问控制，让需要登录才能访问的页面，必须在登录后才能访问。
在没有登录时，直接跳转到登录页面，让用户进行登录。

- 如何实现登录访问控制呢？
  - 分析：不管哪个页面都是通过**路由**来访问的，因此，需要从路由角度来进行控制
  - 思路：创建 `AuthRoute` 组件，判断是否登录，1 登录直接显示要访问的页面 2 没有登录跳转到登录页面

- 实现步骤：
  1. 在 `utils/token.js` 文件中添加一个新的函数 `isAuth`
  2. 在 `components` 目录中创建 `AuthRoute` 组件

```js
// utils/token.js 文件：
const isAuth = () => !!getToken()

// components/AuthRoute/index.js 文件：
import { Route } from 'react-router-dom'
import { isAuth } from '../../utils'

// 封装路由的 Route 组件
// 约定： AuthRoute 组件的使用方式与 Route 组件的使用方式完全相同
// 优势： 完全按照 Route 组件来使用 AuthRoute 组件即可，没有增加额外的学习成本
// 使用方式：<AuthRoute path="/home" component={Layout} />
// component 属性表示：要渲染的组件。因为要渲染该组件，所以，我们将该属性单独抽离出来
// ...rest 表示：传递给该组件的所有其他其他属性
const AuthRoute = ({ component: Component, ...rest }) => {
  return (
    <Route
      {...rest}
      render={props => {
        // 判断是否登录
        if (!isAuth()) {
          // 未登录 -> 跳转到登录页
          return props.history.push('/login')
        }
        // 已登录 -> 展示该页面组件
        return <Component {...props} />
      }}
    />
  )
}

export { AuthRoute }
```

---

## 项目功能 - 首页

功能如下：

1. 搭建布局组件结构
2. 左侧菜单
4. 个人信息
5. 退出功能

### 搭建布局组件结构

```scss
.ant-layout {
  height: 100%;
}

.header {
  padding: 0;

  .logo {
    width: 200px;
    height: 60px;
    background: url(../../assets/logo.png) no-repeat center / 160px auto;
  }
}

.site-layout-background {
  background: #fff;
}

.layout-content {
  overflow-y: auto;
}

.user-info {
  position: absolute;
  right: 0;
  top: 0;
  padding-right: 20px;
  color: #fff;

  .user-name {
    margin-right: 20px;
  }

  .user-logout {
    display: inline-block;
    cursor: pointer;
  }
}
```

- 菜单和退出icon： HomeOutlined、DiffOutlined、EditOutlined、LogoutOutlined

### 左侧菜单

- [Menu 配合路由使用时菜单选中高亮问题](https://github.com/ant-design/ant-design/issues/1575#issuecomment-362488383)

菜单高亮的几种情况：

1. 点击菜单时【不需要特殊处理，默认就是点谁谁选中】
2. 刷新页面时
3. 点击按钮跳转路由时【比如，从文章列表页面，跳转到文章编辑页面】

对于第 2、3 两种情况，是需要通过代码来处理才能让菜单正确高亮

- 处理第 2 种情况，也就是刷新页面时，让菜单正确高亮
  - 分析：刷新页面时，path 是什么，就要让 path 对应的菜单高亮 ===> 菜单和 path 之间对应起来即可

- 处理第 3 种情况，也就是点击按钮跳转路由时
  - 分析：路由切换后，会将最新的路由信息传递给 Layout 组件，导致 Layout 组件重新渲染 ===> 也就是执行了 Layout 组件的**更新阶段**

### 个人信息

- 接口地址：`/user/profile` - 获取用户个人资料
- 注意：该接口需要携带 token，否则，无法获取数据
- 使用 `axios 拦截器` 来统一添加：

```js
// utils/http.js 文件：
// 添加 请求拦截器：
http.interceptors.request.use(config => {
  // 添加 token 到请求头中
  config.headers.Authorization = `Bearer ${getToken()}`
  return config
})
```

- 注意：服务端会去判断 token 是否失效，极客园项目中，token 的失效时间为 2 小时
- 因此，我们需要在 token 失效后，引导用户重新登录

```js
// 添加 响应拦截器：
http.interceptors.response.use(
  response => {
    return response
  },
  error => {
    // token 失效时，会走 error，并且 HTTP 状态码为 401
    if (error.response.status === 401) {
      removeToken()
      history.push('/login')
    }
  }
)
```

### 退出功能

1. 弹窗让用户确认是否确定退出，防止用户误操作
2. 移除 token 并返回登录页面即可

## 在React脚手架创建的项目中使用 sass

1. 安装 node-sass：`yarn add node-sass` （ 作用：用来解析 SASS 语法，转化为浏览器能够识别的 CSS ）
2. 如果这个包安装失败，可以通过单独设置 node-sass 包的淘宝镜像来解决：

```cmd
# 全局设置一次即可
npm config set sass_binary_site https://npm.taobao.org/mirrors/node-sass/
```

3. 将原来的 .css 修改为 `.scss`
4. 将 js 中导入的 .css 修改为 `.scss`

- Sass 的语法有两种形式：[SASS 的两种语法参考](https://sass-lang.com/documentation/syntax)
  1. 以 .sass 结尾 ==> 简洁的语法形式，没有 `{}` 没有 `;`
  2. 以 **.scss** 结尾 ==> 类似于 CSS 的语法，带有 `{}` 带有 `;`

