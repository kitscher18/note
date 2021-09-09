# React-Redux 绑定库

- 问题:为什么要使用 React-Redux 绑定库?
- 回答:React 和 Redux 是两个独立的库，两者之间职责独立。因此，为了实现在 React 中使用 Redux 进行状态管理 ，就需要一种机制，将这两个独立的库关联在一起。这时候就用到 React-Redux 这个绑定库了。
- 作用:**为 React 接入 Redux，实现在 React 中使用 Redux 进行状态管理**。 
- react-redux 库是 Redux 官方提供的 React 绑定库。

![react-redux](./images/react-redux.jpeg)

## 基本使用

React-Redux 的使用分为两部分：

1. 项目整体处理：只需要设置一次即可
2. 组件接入：哪个组件需要接入 Redux 就在哪个组件中使用

### 项目整体处理

1. 安装 react-redux 包:`yarn add react-redux`

2. 在 `src/index.js` 项目入口文件中，导入以下内容
   1. 从 react-redux 包中，导入 `Provider` 组件
   2. 从 store 目录中，导入创建好的 Redux store
   3. 使用 Provider 组件包裹 App(项目的根组件)，表示:整个应用中的任意组件都可以使用 Redux 进行状态管理
   4. 为 Provider 组件提供 store 属性，值为导入的 Redux store

```js
// src/index.js 文件中：

import { Provider } from 'react-redux'
import store from './store'

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.querySelector('#root')
)
```

### 组件接入

- 哪个组件需要接入 Redux 就在哪个组件中使用
- React-Redux 作用：**为 React 接入 Redux，实现在 React 中使用 Redux 进行状态管理**
- 包含两个功能：1 **读取 Redux 状态**   2 **分发 action 以更新状态**

```js
import { useSelector, useDispatch } from 'react-redux'

// 1 组件中读取 Redux 状态
const count = useSelector(state => state.count)

// 2 更新状态
const dispatch = useDispatch()
dispatch(action)
```

## 执行过程

- 只要调用了 `dispatch(action)` 来更新状态，Redux 中的状态就会改变
- 只要 Redux 中的状态改变了，Redux 就会通知所有使用了 `useSelector()` 的组件
- 也就是，所有用到 Redux 状态的组件，都会被重新渲染，这样，组件中就可以拿到最新的状态了

## 演示 Redux 数据流

![Redux 数据流](./images/ReduxDataFlow.gif)

## 总结

- 任何一个组件都可以直接接入 Redux，也就是可以直接：1 修改 Redux 状态 2 接收 Redux 状态
- 并且，只要 Redux 中的状态改变了，所有接收该状态的组件都会收到通知，也就是可以获取到最新的 Redux 状态
- 这样的话，两个组件不管隔得多远，都可以**直接通讯**了

### 使用 Redux 后，组件中的状态放在哪？

1. 可以将所有的状态全部放到 redux 中，由 redux 管理
2. 只将某些数据放在 redux 中，一些其他的数据可以放在组件中
   - 如果一个状态，只在某个组件中使用（比如，表单项的值），推荐：放在组件中
   - 需要放到 redux 中的状态：
     1. 在多个组件中都要使用的数据【涉及组件通讯】
     2. 通过 ajax 请求获取到的接口数据【涉及到请求相关逻辑代码放在哪的问题】

### 使用 Redux 后的优缺点

- 缺点
  1. 麻烦（提升了项目的复杂度）：将原来只在一个组件中书写的功能，分离到了多个文件（action/reducer）

- 优点
  1. 分离出来的各个文件（action/reducer/组件自身），**职责单一且明确**
     - 组件的职责：使用数据渲染UI
     - action 的职责：描述要做什么
     - reducer 的职责：更新状态
  2. 优雅的解决了组件通讯
     - 任何组件都可以直接拿到 redux 状态；也可以直接修改 redux 状态
     - 不管两个组件离得多远，只要一个组件更新了状态，另外一个组件就可以直接拿到最新的状态

- 小项目没必要使用 redux，中型、大型的前端项目才有必要使用 Redux























