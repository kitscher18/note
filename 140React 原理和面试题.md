# React 原理和面试题

1. 项目中的内容优化
  - redux Hooks / connect
  - 路由 Hooks / withRouter 高阶组件
2. React 原理
3. 常见、经典的面试题

## 项目中的内容优化

1. Tabs 组件分析
2. keep alive 功能实现
3. 路由优化 - Switch 、 404
4. 反向代理 - 跨域
5. antd-mobile 组件库按需加载

### Tabs 组件分析

- 使用方式：

```js
Tabs 组件的作用：
  1 渲染 Tabs 频道栏中的内容
  2 渲染频道对应的文章列表

<Tabs
  activeKey={channelActiveIndex}
  onTabChange={onTabChange}
  tabs={channels}
>
  {channels.map(item => (
    <ArticleList key={item.id} channelId={item.id} />
  ))}
</Tabs>

// Tabs 组件内部的逻辑：
// React.Children.map() 用来遍历 children（ 此时，children 应该是一个数组 ）
<div className="tabs-content">
  {React.Children.map(children, child => {
    // child 就是 children 数组中的每一项
    // React.cloneELement(React元素) 用来 克隆（拷贝） 一个React元素
    // 此处，使用 React.cloneElement 的目的是：为了给 child 传递额外的属性，比如，此处的 activeId
    return React.cloneElement(child, {
      activeId: tabs[activeIndex]?.id || 0
    })
  })}
</div>
```

### `requestAnimationFrame` - JS 帧动画

- [requestAnimationFrame](https://developer.mozilla.org/zh-CN/docs/Web/API/window/requestAnimationFrame)
- 作用：window.requestAnimationFrame() 告诉浏览器——你希望执行一个动画，并且要求浏览器在下次重绘之前调用指定的回调函数更新动画。
  - 说明：每次浏览器刷新，都会自动调用 requestAnimationFrame 这样，就能保证不会丢帧！
- BOM API 是 window 提供的，用来执行 JS 动画的一个函数
- JS 动画简单来说，就是通过 JS 代码不停的修改某个 style 属性值。此时，我们可能会联想到 定时器
- 类似于 `setInterval(func, wait)` 或 `setTimeout(func, wait)`

```js
const callback = () => {

  if (条件成立) {
    requestAnimationFrame(callback)
  }
}

requestAnimationFrame(callback)
```

- 使用 定时器 来执行 JS 动画的问题：
  - 问题1：指定的**等待时间不准确**，实际上，会在至少等待 wait 毫秒后执行
  - 问题2：**丢帧**，也就是在 一帧 期间没有执行定时器中的操作 或者 在一帧期间执行了多次定时器中的操作
    - 不会丢帧的情况：浏览器刷新一次，操作执行一次

- 浏览器中刷新频率，绝大多数浏览器都是：`60HZ`，说白了就是： 1秒钟刷新 60 次（帧）
  - 每一帧占用的时间为：1000 / 60 ≈ 16.67 毫秒
  - 简单来说也就是：每一帧都是执行 JS 代码，都要执行 浏览器 绘制（渲染）
    - 浏览器中的 JS 线程 和 渲染线程 是互斥，也就是说：如果在执行 JS 就不会执行 渲染；反之亦如此


### keep alive 功能实现

- 效果：从路由 A 切换到路由 B，再返回 路由 A，如果实现了 keep alive 效果，此时，路由 A 对应页面会继续保持原来状态。
  - 此处，要保留的状态，包含：页面滚动位置； Tab 高亮 等

实现方式：

1. 用 redux 存储这些数据：也就是在离开路由 A 的时候，用 redux 保存页面当前的滚动位置；Tab 高亮
  - 将来返回该页面，从 redux 中读取这些数据，以回复原来页面的状态

2. 通过路由来实现
  - 默认路由没有实现该功能，因为只要切换路由页面组件就会卸载，页面内容也就没有了。再返回该页面，页面相当于重新渲染，因此，原来的页面状态都会重置。也就没有实现保留上一次页面状态的效果了。
  - 封装一个自定义路由，来实现即可
  - 实现原理：在切换路由时，让页面不卸载，而是通过 display none 隐藏掉。这样，因为页面没有卸载，所以原来所有的操作都会被保存下来。 将来再返回该页面，只需要 display block 展示即可。这样，就可以恢复原来的内容了

```tsx
// 用法：
// activePath 表示：要让哪个路由存活，也就是在切换到其他路由时，不是销毁该组件而是隐藏
//  注意： activePath 和 path 应该相同
<KeepAlive activePath="/home" exact path="/home">
  <Layout />
</KeepAlive>

const KeepAlive = ({ activePath, children, ...rest }: Props) => {
  return (
    <Route
      {...rest}
      // 注意： children 属性，不管当前路由是否匹配，都会执行 children 函数
      children={props => {
        const {
          location: { pathname }
        } = props
        const isMatch = pathname.startsWith(activePath)

        return (
          <div
            className={styles.root}
            style={{ display: isMatch ? 'block' : 'none' }}
          >
            {children}
          </div>
        )
      }}
    />
  )
}
```

### 路由的 Switch 组件

- 默认情况下，路由匹配时，会对所有的 Route 全部挨个匹配一次
- Switch 组件内部的 Route 只要有一个匹配成功了，就不会再继续往下匹配
- 注意：我们自己封装的 KeepAlive 路由组件，因为要一直匹配，所以，不要将 KeepAlive 放在 Switch 内部

### 处理不存在的路由 - 404

```tsx
<Route
  path="*"
  render={props => {
    // 注意：此处需要将 /home 排除掉，因为 Home 对应的路由没有放在 Switch 中
    //      所以，路由匹配时，会全部匹配一次，当匹配到 Switch 内部时，此处，就会匹配成功
    //      而我们不希望此处的 * 来匹配 Home
    //      因此，此处将 /home 排除掉
    if (props.location.pathname.startsWith('/home')) {
      return null
    }
    return <NotFound />
  }}
></Route>
```

### 反向代理（Proxy） - 解决跨域问题

- [create-react-app 代理](https://create-react-app.dev/docs/proxying-api-requests-in-development)
- [webpack-dev-server 配置](https://webpack.js.org/configuration/dev-server/#devserverproxy)
- 跨域：是由于 浏览器 中的 同源策略 的限制，而产生的！！！
  - 同源策略：1 协议 2 域名 3 端口号
- 服务器端是没有跨域的限制的！

举例说明：

1. 存在跨域问题： 中国人 <- X -> 美国人
  - 由于两个人语言不同，导致两个人无法正常交流。这就好比，浏览器和服务器存在跨域问题，无法直接通讯（数据传递）

2. 使用代理来解决跨域问题： 中国人 <--> 翻译（代理人） <--> 美国人

对于代码来说：

1. 存在跨域问题：         浏览器 <- X -> 服务器
2. 使用代理解决跨域问题：  浏览器 <--> 代理服务器（本地） <--> 服务器
  - 此处，`代理服务器（本地）` 实际上就是指的 webpack 开启的服务器（也就是，webapck-dev-server）

最终，通过脚手架创建的项目，只需要知道如何来配置反向代理，就可以解决跨域问题了：

1. 安装：`yarn add http-proxy-middleware`
2. 在项目中创建 `src/setupProxy.js` 【注意：文件名称和路径都是固定的！！！】
3. 在 `setupProxy.js` 中添加以下配置，即可：

```js
const { createProxyMiddleware } = require('http-proxy-middleware')
const baseURL = process.env.REACT_APP_URL

// https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually
// https://www.npmjs.com/package/http-proxy-middleware

module.exports = function (app) {
  // app 是基于 Express 的，所以，app.use() 就是在配置中间件
  const config = createProxyMiddleware({
    // 标识：真实的接口地址
    target: baseURL,
    // 跟服务端的虚拟主机相关，一般都是设置为 true
    changeOrigin: true,
    // 路径重写
    // 因为我们将 代理的标识 设置为了：/api，所以，每个请求都是以 /api 开头的
    //  比如：/api/user 标识获取用户个人资料
    // 如果没有该配置项： http://geek.itheima.net/v1_0 + /api/user ==> http://geek.itheima.net/v1_0/api/user 
    // 但是，要注意：我们接口路径中，是没有 /api
    // 所以，此处要通过该配置，将 接口路径中的 /api 替换为 ''（空字符）
    // 也就是说，使用该配置后： http://geek.itheima.net/v1_0 + /user ==> http://geek.itheima.net/v1_0/user 
    pathRewrite: {
      '^/api': ''
    }
  })

  // 第一个参数：代理的路径标识，将来在发请求的时候，如果 url 地址是以该标识开头的
  //          那么，这个请求，就会被代理
  //          如果发送请求时，url 不是以 /api 开头的，那么，该请求就不会被代理了
  //          该标识只要是字符串即可，可以是 /api 或 /a 或 /geek
  // 第二个参数：配置项
  app.use('/api', config)
}
```

4. 修改 utils/http.ts

```ts
const http = axios.create({
  // 将原来的接口地址，修改为代理接口的标识即可
  baseURL: '/api',
  timeout: 5000
})
```

4. 重启终端，脚手架就会自动读取该代理配置了，代理就会生效了


### antd-mobile 组件库按需加载

1. 在 craco.config.js 中添加以下配置：

```js
// 配置文档：
// https://www.npmjs.com/package/@craco/craco#configuration
module.exports = {
  // 省略其他配置

  // 添加该配置
  // https://mobile.ant.design/docs/react/introduce-cn#%E6%8C%89%E9%9C%80%E5%8A%A0%E8%BD%BD
  babel: {
    // antd-mobile 按需导入
    plugins: [['import', { libraryName: 'antd-mobile', style: 'css' }]]
  }
}
```

2. 修改 src/index.tsx（项目入口文件）内容

```tsx
// 注释掉该手动导入
// 导入 antd-mobile 的样式文件
// import 'antd-mobile/dist/antd-mobile.css'

import App from './App'

// 将全局样式文件，放在 App 后导入
import './index.scss'
```

3. 重启项目

---

## React 核心原理

1. Virtual DOM（虚拟 DOM）和 Diff 算法
  - 虚拟 DOM 的优势
  - key 属性
2. 虚拟 DOM 的问题
3. React Fiber
4. React 合成事件
   1. 合成事件的变化
5. hooks 原理
   1. useState
   2. useEffect
6. redux
   1. thunk 是什么
   2. connect 用法 - 高阶组件


### 虚拟 DOM 和 Diff 算法

#### 虚拟 DOM 对象

- 为什么使用虚拟DOM？
  1. 真正的 DOM 对象属性很多，处理起来不方便
  2. 性能角度
  3. 虚拟 DOM 的真正价值：跨平台

- Virtual DOM
- 虚拟 DOM 对象：就是一个普通的 JS 对象，用来描述我们希望在页面中看到的 HTML 结构内容

```js
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello JSX!'
  }
}

<div></div>
// { type: 'div' }

<div>123</div>
// { type: 'div', props: { chilren: '123' } }

<div class="tab">123</div>
// { type: 'div', props: { className: 'tab', chilren: '123' } }
```

- 原生 DOM 对象： 也是一个 JS 对象，是浏览器默认提供的
- DOM 对象 和 HTML 元素之间是一一对应的关系

#### React JSX 语法转化的过程

- 转化过程：JSX -> React.createEelement()/_jsx -> 虚拟DOM

```js
// JSX
const el = <div className="abc" onClick={() => {}}>123</div>

// 旧的转化方式：
// React 元素
const el = /*#__PURE__*/React.createElement("div", {
  className: "abc",
  onClick: () => {}
}, "123");

// 新的转化方式：
var _jsxRuntime = require("react/jsx-runtime");
const el = /*#__PURE__*/(0, _jsxRuntime.jsx)("div", {
  className: "abc",
  onClick: () => {},
  children: "123"
});

// 虚拟 DOM
{
  type: 'div',
  props: {
    className: "abc",
    onClick: () => {},
    children: "123"
  }
}
```

#### Diff 算法的说明

- 第一次页面渲染的过程：1 JSX + state => 2 虚拟DOM树（JS对象） => 3 浏览器中看到的HTML结构内容
- 当更新了状态，就会重新渲染组件，也就会重新生成一棵新的 虚拟DOM树
- Diff 算法就会：对比 初始虚拟DOM树 和 更新后的虚拟DOM树，找到不同之处，最终，只将不同的地方更新到页面中

#### 一个组件内部更新机制

- 只要想让状态发生变化，就调用 setState()，只要调用 setState()，就会执行组件的 render 方法。来重新渲染组件
- 注意：render 重新执行，不代表把整个组件重新渲染到页面中。而实际上，React内部会使用 *虚拟DOM* 和 *Diff 算法*来做到 **部分更新**。
  - 部分更新（打补丁）：只将变化的地方重新渲染到页面中，这样，尽量减少了 DOM 操作

#### Diff 算法的说明 - 1

- 如果两棵树的**根元素类型**不同，React 会销毁旧树，创建新树

```js
// 旧树
<div>
  <Counter />
</div>

// 新树
<span>
  <Counter />
</span>

执行过程：destory Counter -> insert Counter
```

#### Diff 算法的说明 - 2

- 对于类型相同的 React DOM 元素，React 会对比两者的属性是否相同，只更新不同的属性
- 当处理完这个 DOM 节点，React 就会递归处理子节点。

```html
// 旧
<div className="before" title="stuff"></div>
// 新
<div className="after" title="stuff"></div>
只更新：className 属性

// 旧
<div style={{color: 'red', fontWeight: 'bold'}}></div>
// 新
<div style={{color: 'green', fontWeight: 'bold'}}></div>
只更新：color属性
```

#### Diff 算法的说明 - 3

- 1 当在子节点的后面添加一个节点，这时候两棵树的转化工作执行的很好

```js
// 旧
<ul>
  <li>first</li>
  <li>second</li>
</ul>

// 新
<ul>
  <li>first</li>
  <li>second</li>
  <li>third</li>
</ul>

执行过程：
React会匹配新旧两个<li>first</li>，匹配两个<li>second</li>，然后添加 <li>third</li> tree
```

- 2 但是如果你在开始位置插入一个元素，那么问题就来了：

```js
// 旧
<ul>
  <li>1</li>
  <li>2</li>
</ul>

// 新
<ul>
  <li>3</li>
  <li>1</li>
  <li>2</li>
</ul>

执行过程：
React将改变每一个子节点，而非保持 <li>Duke</li> 和 <li>Villanova</li> 不变
```

#### key 属性

> 为了解决以上问题，React 提供了一个 key 属性。当子节点带有 key 属性，React 会通过 key 来匹配原始树和后来的树。

```js
// 旧
<ul>
  <li key="2015">1</li>
  <li key="2016">2</li>
</ul>

// 新
<ul>
  <li key="2014">3</li>
  <li key="2015">1</li>
  <li key="2016">2</li>
</ul>

执行过程：
现在 React 知道带有key '2014' 的元素是新的，对于 '2015' 和 '2016' 仅仅移动位置即可
```

- 说明：key 属性在 React 内部使用，但不会传递给你的组件
- 推荐：在遍历数据时，推荐在组件中使用 key 属性：`<li key={item.id}>{item.name}</li>`
- 注意：**key 只需要保持与他的兄弟节点唯一即可，不需要全局唯一**
- 注意：**尽可能的减少数组 index 作为 key，数组中插入元素的等操作时，会使得效率底下**

```js
// 旧
<ul>
  <li key="0">1 <input /></li>
  <li key="1">2 <input /></li>
</ul>

// 新
<ul>
  <li key="0">3 <input /></li>
  <li key="1">1 <input /></li>
  <li key="2">2 <input /></li>
</ul>
```

#### 虚拟 DOM 的真正价值

- 虚拟DOM的真正价值： 虚拟DOM让React代码摆脱了浏览器的限制（束缚），只要能够运行JS的地方，就可以执行 React 代码。
- 所以，使用 React 能够实现跨平台应用。
- 也可以将 React 看做是： 面向虚拟DOM编程

### **key 的技巧**

```tsx
// 利用 key 的特性来实现让 EditInput 组件在打开抽屉时，能够获取到正确的内容
// 原理： React 会比较更新前后的 key 是否相同，如果相同就复用该组件；
//       如果不同，就会卸载原来的EditInput组件，然后，重新渲染EditInput组件
<EditInput
  key={name}
  config={inputDrawer.type === 'name' ? nameConfig : introConfig}
  onUpdate={
    inputDrawer.type === 'name' ? onUpdateName : onUpdateIntro
  }
  onClose={onInputClose}
/>
```

### React 15

- 包含了：虚拟DOM 和 Diff

- 架构
  1. Reconciler：（协调器）负责调用 render 生成虚拟 Dom，进行 Diff，找出变化后的虚拟 Dom
  2. Renderer：（渲染器）负责接收 Reconciler 通知，将变化的组件渲染在当前`宿主环境`
    - 比如浏览器（react-dom），不同的宿主环境会有不同的 Renderer

- 缺陷：
  - 递归同步更新 DOM 树，如果节点非常多，即使只有一次 state 变更，React 也需要进行复杂的递归更新，更新一旦开始，**中途就无法中断**，直到遍历完整颗树，才能释放 `JS 主线程`

- 状态更新
  - 批处理：多次调用 setState() 会合并为一次更新
  - 原理：调用 setState() 并没有立即更新状态，而是存储到 `_pendingStateQueue` 队列中，将需要更新的组件存入 `dirtyComponent` 中。在非 异步代码 中，React 会将 `isBatchingUpdates` 标记设置为 true，表示批量更新；而当 异步代码 执行时，由于 React 已经将内部的 `isBatchingUpdates` 标记设置为 false，所以，异步代码中操作 setState 表现为非批量更新，而是调用一次 setState 就更新一次状态、组件
  - 其实，这是一个 bug，因为在不同情况下 setState 表现不一致
  - React 为了解决这个问题，提供了一个 API 来实现批处理：`ReactDOM.unstable_batchedUpdates()`

### React 16

- [参考：React 各个版本特性的变化](https://juejin.cn/post/7010539227284766751)
- [React 16 实现的动画效果](https://claudiopro.github.io/react-fiber-vs-stack-demo/fiber.html)
- [React 15 实现的动画效果](https://claudiopro.github.io/react-fiber-vs-stack-demo/stack.html)
- 架构：
  - Scheduler（调度器）：调度任务的优先级，高优任务优先进入 Reconciler
  - Reconciler（协调器）：负责找出变化的组件（使用 `Fiber` 重构）
  - Renderer（渲染器）：负责将变化的组件渲染到页面上

1. 大任务拆分小任务

React 的解决思路，就是在浏览器每一帧的时间中预留一些时间给 JS 线程，React 利用这部分时间更新组件。当预留的时间不够用时，React 将线程控制权交还给浏览器让他有时间渲染UI，React 则等待下一帧再继续被中断的工作。

将长任务分拆到每一帧中，每一帧执行一小段任务的操作，就是我们常说的时间切片

2. 任务划分优先级

`Concurrent Mode` 就是为了解决以上两个问题而设计的一套新的架构，重点就是，让组件的渲染 “可中断” 并且具有 “优先级”。

#### requestIdleCallback

1. 跟性能密切相关的数字 16.6

浏览器刷新频率：`60HZ` 也就是每秒刷新 60 次，大概 16.6ms 浏览器刷新一次  
由于 `GUI 渲染线程`和 `JS 线程`是互斥的，所以 JS 脚本执行和浏览器布局、绘制不能同时执行  
**在这 16.6ms 的时间里，浏览器既需要完成 JS 的执行，也需要完成样式的重排和重绘**，如果 JS 执行的时间过长，超出了 16.6ms，这次刷新就没有时间执行样式布局和样式绘制了，于是在页面上就会表现为卡顿。

2. requestIdleCallback
  - [参考](https://juejin.cn/post/6984949525928476703)

```js
// 用法示例
let tasksNum = 500

requestIdleCallback(unImportWork)

function unImportWork(deadline) {
  console.log('---- 开始 ----')
  while (deadline.timeRemaining() && tasksNum > 0) {
    console.log(`执行了${500 - tasksNum + 1}个任务`)
    tasksNum--
  }
  console.log('---- 结束 ----')
  if (tasksNum > 0) {
    // 在未来的帧中继续执行
    requestIdleCallback(unImportWork)
  }
}
```

注意：由于兼容性和刷新帧率的问题，React 并没有直接使用 requestIdleCallback ， 而是使用了 MessageChannel 模拟实现，原理是一样的。

### hooks 原理

1. useState
2. useEffect

- hooks 使用的注意点： 不能在 `if/for/其他的函数中` 来使用 Hooks
- 因为在这些情况下，使用 Hooks 可能会导致 hooks 的执行顺序发生改变。因为 React Hooks 内部是通过 hooks 的调用顺序来记录的。

```js
// 第一次调用
const useState = initialState => {
  // 创建一个变量，来存储初始状态，这样，将来在 useState 内部，修改 变量 而不是修改参数
  let state = hookState[0] || initialState

  // 每次调用 useState 对应的索引
  let currentHookIndex = 0
  // 更新状态的函数
  const setState = newState => {
    hookState[0] = newState
    // 注意：此处需要调用 render 函数，来让组件重新渲染
    render()
  }
  
  hookIndex++
  return [state, setState]
}

// 第二次调用
// 第一次调用
const useState = initialState => {
  // 创建一个变量，来存储初始状态，这样，将来在 useState 内部，修改 变量 而不是修改参数
  let state = hookState[1] || initialState

  // 每次调用 useState 对应的索引
  let currentHookIndex = 1
  // 更新状态的函数
  const setState = newState => {
    hookState[1] = newState
    // 注意：此处需要调用 render 函数，来让组件重新渲染
    render()
  }
  
  hookIndex++
  return [state, setState]
}
```

```tsx
// 创建自己的 useState
let state: number

const useState = (initialState: number) => {
  // 为了能够在组件每次更新时都获取到最新的状态值，需要做两个操作：
  // 1 将 state 的声明放在 useState 的外部。
  //    可以避免每次调用 useState 时重置 state 变量的值
  // 2 判断 state 变量中是否有值，如果没有就用 initialState 默认值
  //    如果有，就用 state 自己的值（ 最新的状态值 ）
  state = state || initialState

  // 修改状态的函数
  const setState = (value: number) => {
    console.log('setState:', value)
    state = value

    // 此处，调用 render 函数，来重新渲染组件
    render()
  }

  // 注意：因为现在用的是 TS，所以，需要为该返回值提供一个 Tuple 类型
  type UseStateTuple = [number, (value: number) => void]
  const ret: UseStateTuple = [state, setState]
  return ret
}

const App = () => {
  console.log('render App')
  const [count, setCount] = useState(0)
  console.log('App count:', count)
  // +1
  const onIncrement = () => {
    setCount(count + 1)
  }

  return (
    <div style={{ backgroundColor: 'pink', padding: 10 }}>
      <h1>计数器：{count}</h1>
      <button onClick={onIncrement}>+1</button>
    </div>
  )
}

function render() {
  ReactDOM.render(<App />, document.getElementById('root'))
}
render()
```

```tsx
// 创建自己的 useState
let state: number[] = []
let hookIndex = 0

const useState = (initialState: number) => {
  state[hookIndex] = state[hookIndex] || initialState

  // 修改状态的函数

  // 第一次调用 useState hookIndex 值为 0
  // 第二次调用 useState hookIndex 值为 1
  let curHookIndex = hookIndex
  const setState = (value: number) => {
    // console.log('hookIndex', hookIndex)
    console.log('curHookIndex', curHookIndex)
    // 注意：此处的 hookIndex 并不是使用 useState 时，对应的那个索引
    // 所以，代码出现了 Bug！
    // state[hookIndex] = value
    state[curHookIndex] = value
    // 重新渲染组件
    render()
  }

  // Tuple 类型
  type UseStateTuple = [number, (value: number) => void]
  const ret: UseStateTuple = [state[hookIndex], setState]

  hookIndex++
  return ret
}

const App = () => {
  // 第一次使用：0
  const [count, setCount] = useState(100)
  // console.log('count', count)
  // 第二次使用：1
  const [age, setAge] = useState(16)
  // console.log('age', age)
  // console.log('state 数组', state)

  // +1
  const onIncrement = () => {
    setCount(count + 1)
  }

  const onAgeIncrement = () => {
    // debugger
    setAge(age + 1)
  }

  return (
    <div style={{ backgroundColor: 'pink', padding: 10 }}>
      <h1>计数器：{count}</h1>
      <button onClick={onIncrement}>+1</button>

      <hr />
      <p>年龄：{age}</p>
      <button onClick={onAgeIncrement}>年龄+1</button>
    </div>
  )
}

function render() {
  ReactDOM.render(<App />, document.getElementById('root'))

  // 重置 hookIndex 的值
  hookIndex = 0
}
render()
```

## 面试题

1. 状态逻辑复用
   1. 高阶组件 HOC - withRouter
   2. render-props 模式
2. 组件生命周期的三个阶段
   1. 不常用钩子函数
   2. 新的钩子函数
3. 性能优化
   1. class 组件
      1. PureComponent
      2. shouldComponentUpdate
   2. Hooks
      1. useCallback
      2. useMemo
4. setState 是同步还是异步？
5. redux 的意义
  1. 解决组件通讯的难题 - （ Context 上下文：实现跨组件传递数据 ）
  2. 【重要】将项目中业务逻辑从组件中抽离出去，这样，组件只负责渲染 UI，业务逻辑交给 redux 来处理

### 状态逻辑复用 - 高阶组件HOC

- High-Order Component
- 高阶组件：是一个函数，接受一个组件，返回一个新组件
- 约定：高阶组件名称以 `with` 开头

```js
// 创建高阶组件
const withMouse = (Comp) => {
  // 该类组件用来提供状态逻辑
  return class HOC extends React.Component {
    state = {
      x: 0,
      y: 0
    }

    handleMousemove = e => {
      const { pageX, pageY } = e

      this.setState({
        x: pageX,
        y: pageY
      })
    }

    componentDidMount() {
      window.addEventListener('mousemove', this.handleMousemove)
    }

    componentWillUnmount() {
      window.removeEventListener('mousemove', this.handleMousemove)
    }

    render() {
      const { x, y } = this.state
      return <Comp x={x} y={y} />
    }
  }
}

// 使用方式：
const 新组件 = withMouse( 指定结构的组件 )

// 参数组件：用来指定组件的结构，以及使用 高阶组件 提供状态
// 返回的新组件： 相当于带有状态逻辑的参数组件，也就是既有状态逻辑，又有组件结构

// 比如，如果是 记录鼠标位置的组件：
const Position = ({ x, y }) => {
  return (
    <div>鼠标当前位置：(x: {x}, y: {y})</div>
  )
}
const PositionWithMouse = withMouse(Position)
<PositionWithMouse />

// 比如，如果是 跟随鼠标移动的猫组件：
const Cat = ({ x, y }) => {
  return (
    <img
      src={catImg}
      style={{
        position: 'absolute',
        top: y,
        left: x
      }}
      alt=""
    />
  )
}
const CatWithMouse = withMouse(Cat)
<CatWithMouse />
```





















## 高阶组件

- 作用： 用来实现组件状态逻辑复用（ 类似于 render-props ）
- 高阶组件 是一个 函数。

## 高阶组件的使用

```js
// 包装后，组件内部就可以通过 props 来获取鼠标位置
const Cat = (props) => {
  // props => { x: 1, y: 1 }
  
  return <img ... />
}

// 增强后的组件具有鼠标位置 = withMouse(被包装的组件)
const CatWithMouse = withMouse(Cat)

// 最终渲染的就是增强后返回的组件
<CatWithMouse />
```

## 高阶组件的封装

```js
const withMouse = (WrappedComponent) => {
  // 因为只有类组件中才有状态，所以，要创建一个类组件并返回
  // 这个类组件实现了 鼠标位置的状态逻辑复用

  class Mouse extends React.Component {
    state = {
      x: 0,
      y: 0
    }
    handleMouseMove = (e) => {
      const { clientX, clientY } = e
      this.setState({
        x: clientX,
        y: clientY
      })
    }
    componentDidMount() {
      window.addEventListener('mousemove', this.handleMouseMove)
    }
    componentWillUnmount() {
      window.removeEventListener('mousemove', this.handleMouseMove)
    }

    render() {
      return <WrappedComponent x={this.state.x} y={this.state.y} />
    }
  }
  return Mouse
}
```

## 高阶组件的注意点

- 1 推荐设置 displayName 属性
  - 作用： 用来在 React 浏览器开发者工具（插件）中展示名称

```js
const withMouse = () => {
  class Mouse ... {}

  // 设置 displayName
  Mouse.displayName = `WithMouse${getDisplayName(WrappedComponent)}`
  function getDisplayName(WrappedComponent) {
    return WrappedComponent.displayName || WrappedComponent.name || 'Component'
  }

  return Mouse
}
```

- 2 推荐传递 props
  - 如果没有传递，那么，props 会丢失
  - 最终的 props 传递路径： 增强后的组件（CatWithMouse） -> 高阶组件函数中的Mouse -> 被包装组件（Cat）

```js
const withMouse = (WrappedComponent) => {
  class Mouse ... {
    render() {

      // {...this.props} 就表示将接收到 props 传递给被包装组件
      return <WrappedComponent {...this.props} />
    }
  }
  return Mouse
}

const CatWithMouse = withMouse(Cat)

// 注意： 如果不传递 props，那么，name属性就丢了
<CatWithMouse name="rose" />
```


## shouldComponentUpdate

- 作用： 阻止组件不必要的更新
- 属于哪个阶段的钩子函数？ 更新阶段
- 触发时机：shouldComponentUpdate -> render
- 返回值：布尔类型，如果返回 false 表示不更新组件； 否则，更新组件

```js
shouldComponentUpdate(nextProps, nextState) {
  return 布尔值
}
```

## 纯组件

```js
// PureComponent
class Hello extends React.PureComponent {}
```

- 可以看做是 shouldComponentUpdate 的简化形式
- 但是，只能代替某些情况下的 shouldComponentUpdate

- 浅对比（shallow compare）
- 1 对于基本数据类型，直接比较 值是否相同

```js
let num1 = 1
let num2 = 1

console.log(num1 === num2)
```

- 2 对于引用（复杂）类型，也就是对象，此时，只会比较对象的引用

```js
let obj1 = {
  name: 'jack'
}
let obj2 = {
  name: 'jack'
}
// 使用浅对比 对比 obj1 和 obj2，结果：不同！！！

let obj3 = {
  name: 'rose'
}
let obj4 = obj3
// 使用浅对比 对比 obj3 和 obj4，结果：相同
```

纯组件虽然使用起来比较方便，但是它的作用是固定，不灵活
所以，某些情况下，还得使用 `shouldComponentUpdate` 自己手动来对比
从而避免不必要的更细

