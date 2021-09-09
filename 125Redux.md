# Redux

redux、react-redux、redux-thunk 中间件

1. Redux：React 中最常用的状态管理工具
2. React-Redux：React 和 Redux 之间的桥梁，只有使用该库后，才能在 React 组件中使用 Redux
3. Redux 中间件：Redux 库的扩展机制，用来处理具有副作用(side effect)的功能，比如，异步操作等

## 为什么要用 Redux

[redux 中文文档](http://cn.redux.js.org/)

[redux 英文文档](https://redux.js.org/)

> Redux 是 React 中最常用的状态管理工具（状态容器）

问题：为什么要用 Redux?

![with redux](./images/with-redux.png)

- 主要的区别：**组件之间的通讯问题**
- 不使用 Redux (图左边) ：
  - 只能使用父子组件通讯、状态提升等 React 自带机制 
  - 处理远房亲戚(非父子)关系的组件通讯时乏力
  - 组件之间的数据流混乱，出现 Bug 时难定位
- 使用 Redux (图右边)：
  - **集中式存储和管理应用的状态**
  - 处理组件通讯问题时，**无视组件之间的层级关系** 
  - 简化大型复杂应用中组件之间的通讯问题
  - 数据流清晰，易于定位 Bug

## Redux 开发环境准备

使用 React CLI 来创建项目，并安装 Redux 包即可：

1. 创建 React 项目：`npx create-react-app redux-basic`
2. 启动项目：`yarn start`
3. 安装 Redux 包：`yarn add redux`

## Redux 核心概念

为了让**代码各部分职责清晰、明确**，Redux 代码被分为三个核心概念：action/reducer/store

- action -> reducer -> store
- **action**（动作）：描述要做的事情
- **reducer**（函数）：更新状态
- **store**（仓库）：整合 action 和 reducer

通过例子来理解三个核心概念：

- action：相当于公司中要做的事情，比如软件开发、测试，打扫卫生等
- reducer：相当于公司的员工，负责干活的
- store：相当于公司的老板
- 流程：老板(store)分配公司中要做的事情(action)给员工(reducer)，员工干完活把结果交给老板

### action

- action 行动（名词）
- action：描述要做的事情，项目中的每一个功能都是一个 action
  - 比如，
    - 计数器案例：计数器加1、减1
    - todomvc 案例：添加任务、删除任务等
    - 项目：登录，退出等
- 特点：
  - 只描述做什么
  - JS 对象，必须带有 `type` 属性，用于区分动作的类型
  - 根据功能的不同，可以携带其他数据(比如，`payload` 有效载荷，也就是附带的额外的数据)，配合该数据来完成相应功能

```js
// action

// 计数器案例
{ type： 'increment' } // +1
// 此处的 payload 就表示：计数器减少的数量
// 为什么要传递这个数据？是因为减少的数量是不确定或者由用户指定的，这样，就无法写死一个数量，所以，需要额外的指定该数据
{ type： 'decrement', payload： 2 } // -1

// todomvc 案例
// { type: 'addTodo' }
{ type: 'addTodo', payload: '吃饭' }
// { type: 'removeTodo' }
{ type: 'removeTodo', payload: 3 }
```

### reducer

- reducer 单词的意思：减速器
- 这个名字是参考了 JS 数组中的 `reduce` 这个方法
  - 数组中的 `reduce` 方法，可以来实现累积（比如，累加或者累减）
- reducer：函数，**用来更新状态，是 Redux 状态更新的地方**
- 特点：
  - 函数签名为：`(prevState, action) => newState`
  - 接收上一次的状态和 action 作为参数，根据 action 的类型，执行不同操作，最终返回新的状态
  - 注意：**该函数一定要有返回值**，即使状态没有改变也要返回上一次的状态
  - 约定：**纯函数**，不能包含副作用(比如，不能修改函数外部数据、不能修改函数参数、不能进行异步操作等)
- 补充：纯函数是*函数式编程*中的概念，对于纯函数来说，**相同的输入总是得到相同的输出**

```js
// 伪代码：
// prevState 上一次的状态
// action 当前要执行的动作
const reducer = (prevState, action) => {
  return newState
}

// 示例：
// state 上一次的状态
// action 当前要执行的动作
const reducer = (state, action) => {
  switch (action.type) {
    case 'increment':
      // return 0 + 1
      // 返回新状态
      return state + 1
    default:
      return state
  }
}

// 模拟 reducer 的调用
// 第一次调用 reducer
// reducer(0, { type: 'increment' }) // 本次执行完成后，状态变为：1
// reducer(0, { type: 'increment' }) // 本次执行完成后，状态变为：1

// 基于上一次的状态继续调用 reducer
// reducer(1, { type: 'decrement' }) // 本次因为无法处理该 action，所以，直接返回上一次的状态
```

- 回顾 数组 的 reduce 方法
  - 累积的过程：每个下一次，都是基于上一次的结果

```js
const arr = [1, 3, 5, 9]

// 计算数组中所有元素的和
const count = arr.reduce((prevCount, item) => {
  // prevCount 是上一次的计算结果
  // item 表示数组中的当前值
  return prevCount + item
}, 0)

// 代码执行过程，也就是 累积的过程：
// 第二个参数 0 是：累积的默认值，因为要计算和所以，默认值为：0

// 第一次遍历数组：
// prevCount + item
// 0 + 1
// 返回本次结果，交给下一次的 prevCount
// return 1

// 第二次遍历数组：
// prevCount + item
// 1 + 3
// 返回本次结果，交给下一次的 prevCount
// return 4

// 第三次遍历数组：
// prevCount + item
// 4 + 5
// 返回本次结果，交给下一次的 prevCount
// return 9

// 第四次遍历数组：
// prevCount + item
// 9 + 9
// 返回本次结果，交给下一次的 prevCount
// return 18

// 最终，得到结果为：18
```

- 纯函数的演示
  - 相同的数输入，永远得到相同的输出

```js
// 纯函数：
const add = () => {
  return 123
}

add() // 123
add() // 123

const add = (num1, num2) => {
  return num1 + num2
}

add(1, 3) // 4
add(1, 3) // 4

const add = (obj) => {
  return obj
}

add({ a: '1' }) // { a: '1' }
add({ a: '1' }) // { a: '1' }

// 符合纯函数的概念
// 带有副作用，因为直接修改了变量的值，对外部的数据产生了影响
const add = (obj) => {
	obj.num = 123
  return obj
}

add({ a: '1' }) // { a: '1', num: 123 }
add({ a: '1' }) // { a: '1', num: 123 }

// 不是纯函数：
const add = () => {
  return Math.random()
}

add() // 0.12311293827497123
add() // 0.82239841238741814
```

- 回顾副作用
  - 比如，一个函数来说，对函数外部的环境产生了影响，那么，就说是有副作用的

```js
// 无副作用
const add = (num1, num2) => {
  return num1 + num2
}

add(1, 3)

// 有副作用：
let c = 0
const add = (num1, num2) => {
  // 函数外部的环境产生了影响，所以是有副作用的
  c = 1
  return num1 + num2
}

add(1, 3)

// 符合纯函数的概念
// 带有副作用，因为直接修改了变量的值，对外部的数据产生了影响
const add = (obj) => {
	obj.num = 123
  return obj
}
const o = {}
add(o)
console.log(o) // { num: 123 }
```

### store

- store：仓库，Redux 的核心，整合 action 和 reducer
- 特点：
  - **一个应用只有一个 store**
  - 维护应用的状态，获取状态：`store.getState()`
  - 创建 store 时**接收 reducer 作为参数**：`const store = createStore(reducer)`
  - 发起状态更新时，需要分发 action：`store.dispatch(action)`
- 其他 API，
  — 订阅(监听)状态变化：`const unSubscribe = store.subscribe(() => {}) `
  — 取消订阅状态变化： `unSubscribe()`

```js
import { createStore } from 'redux'

// 创建 store
const store = createStore(reducer)

// 更新状态
// dispatch 派遣，派出。表示：分发一个 action，也就是发起状态更新
store.dispatch(action)

// ---
// 其他 API

// 监听状态变化
const unSubscribe = store.subscribe(() => {
  // 状态改变时，执行相应操作
})

// 取消监听状态变化
unSubscribe()
```

### Redux 获取默认值的执行过程

- 我们发现：只要创建 store，那么，Redux 就会调用一次 reducer
- 这一次调用 reducer 的目的：**获取状态的默认值**
- 如何调用 reducer？
  - Redux 内部第一次调用 reducer： `reducer(undefined, {type: "@@redux/INITv.a.4.t.t.p"})`

- 因为传入的状态值是 undefined ，并且是一个随机的 action type，所以：
  - 状态值因为 undefined，所以，我们设置的默认值就会生效，比如，此处的：10
  - 因为是一个随机的 action type，所以，reducer 中 switch 一定无法处理该 action，那就一定会走 default。也就是直接返回了状态的默认值，也就是：10
- Redux 内部拿到状态值（比如，此处的 10）以后，就用这个状态值，来作为了 store 中状态的最新值
- 因此，将来当我们调用 `store.getState()` 方法来获取 Redux 状态值的时候，拿到的就是 10 了

```js
// 1 导入 createStore
import { createStore } from 'redux'
// 2 创建 store
const store = createStore(reducer)

// action => { type: 'increment' }
function reducer(state = 10, action) {
  console.log('reducer:', state, action)
  switch (action.type) {
    case 'increment':
      return state + 1
    default:
      return state
  }
}

console.log('store 状态值为：', store.getState())
```

### Redux 代码执行过程

1. 创建 store 时，Redux 就会先调用一次 reducer，来获取到默认状态
2. 当你需要更新状态时，就先分发动作 `store.dispatch(action)`
3. Redux 内部，store 就会调用 reducer，传入：上一次的状态（当前示例中就是：`10`）和 action（`{ type: 'increment' }`），计算出新的状态，并返回
4. reducer 执行完毕后，将最新的状态交给 store，store 用最新的状态替换旧状态，状态更新完毕

```js
import { createStore } from 'redux'
const store = createStore(reducer)

function reducer(state = 10, action) {
  console.log('reducer:', state, action)
  switch (action.type) {
    case 'increment':
      return state + 1
    default:
      return state
  }
}

console.log('状态值为：', store.getState()) // 10

// 发起更新状态：
// 参数： action 对象
store.dispatch({ type: 'increment' })
// 相当于： reducer(10, { type: 'increment' })

console.log('更新后：', store.getState()) // 11
```

### Redux 使用总结

- 如何为 Redux 设置状态默认值？？？**为 reducer 的 state 参数设置默认值即可**
- 如何发起状态更新？？？`store.dispatch( action )`
- 使用 redux 就是：1 提供 action      2 提供 reducer 来针对于每一个 action 进行状态更新

```js
// 使用 switch-case 来处理每一个 action
function reducer(state = 10, action) {
  switch (action.type) {
    case 'increment':
      return state + 1
    case 'decrement':
      return state - 1
    default:
      return state
  }
}

// 使用 if、else-if 来处理每一个 action
function reducer(state = 10, action) {
  if (action.type === 'increment') {
    return state + 1
  } else if (action.type === 'decrement') {
		return state - 1
	} else {
    return state
  }
}
```

---

## Redux 在项目中的常用模式

1. Redux 的代码结构
2. action 的常用模式
  - `action creator`：简化多次使用 action 时，重复创建 action 对象
  - `action type`：集中处理 action 类型，保持项目中 action 类型的一致性
3. reducer 的常用模式：reducer 的分离与合并
  - 分离：按功能，使用不同的 reducer 处理不同的状态，此时，会产生多个 reducer
  - 合并：**store 只能接收一个 reducer**，因此，需要将多个 reducer **合并**为一个根 reducer(使用 `combineReducers` 函数)

### Redux 的代码结构

在使用 Redux 进行项目开发时，不会将 action/reducer/store 都放在同一个文件中。 因此，我们需要组织 Redux 的代码(目录)结构：

```html
/store        --- 在 src 目录中创建，用于存放 Redux 相关的代码
  /actions    --- 存放所有的 action
  /reducers   --- 存放所有的 reducer
  index.js    --- redux 的入口文件，用来创建 store
```

### Action Creator

- `Action Creator` 指的是：使用函数创建 action 对象
- 目的：简化多次使用 action 时，重复创建 action 对象

```js
// 不使用 Action Creator
//  需要在每次 dispatch action 时，重复手动创建 action 对象，很繁琐
store.dispatch({ type: 'decrement', payload: 2 })
store.dispatch({ type: 'decrement', payload: 8 })

// ---

// 使用 Action Creator
//  只需要在每次 dispatch action 时，调用 Action Creator 函数即可
const decrement = payload => {
  return { type: 'decrement', payload }
}
// 简化：
const decrement = payload => ({ type: 'decrement', payload })

store.dispatch(decrement(2))
store.dispatch(decrement(8))
```

### Action Type

- Action Type 指的是：action 对象中 type 属性的值
- Redux 项目中会多次使用 action type，比如，action 对象、reducer 函数、dispatch(action) 等
- 目标：**集中处理 action type，保持项目中 action type 的一致性**

处理方式：

1. 在 store 目录中创建 `actionTypes` 目录或者 `constants` 目录，集中处理
2. 使用**常量**来存储 action type
3. action type 的值采用：`'domain/action'(功能/动作)形式`，进行分类处理，比如，
   - 计数器：`'counter/increment'` 表示 Counter 功能中的 increment 动作
   - TodoMVC：`'todos/add'` 表示 TodoMVC 案例中 add 动作等
   - 登录：`login/getCode` 表示登录获取验证码的动作；`login/submit` 表示登录功能
   - 个人信息：`profile/get` 表示获取个人资料；`profile/updateName` 表示修改昵称
4. 将项目中用到 action type 的地方替换为这些常量，从而保持项目中 action type 的一致性

```js
// actionTypes 或 constants 目录：

const increment = 'counter/increment'
const decrement = 'counter/decrement'

export { increment, decrement }

// --

// 使用：
// actions/index.js
import * as types from '../acitonTypes'
const increment = () => ({ type: types.increment })
const decrement = payload => ({ type: types.decrement, payload })

// reducers/index.js
import * as types from '../acitonTypes'
const reducer = (state, action) => {
  switch (action.type) {
    case types.increment:
      return state + 1
    case types.decrement:
      return state - action.payload
    default:
      return state
  }
}
```

- 上课的示例代码：

```js
// actionTypes 目录：
// 创建所有 action 的类型
// 因为类型是固定不变，所以，推荐使用 JS 中的常量来表示
// 变量名称推荐使用纯大写字母来表示

const INCREMENT = 'counter/increment'
const DECREMENT = 'counter/decrement'

export { INCREMENT, DECREMENT }


// actions 目录：

// 使用 action creator 来创建：
// import { INCREMENT, DECREMENT } from '../actionTypes'
import * as ats from '../actionTypes'

// 计数器 增加
const increment = payload => ({ type: ats.INCREMENT, payload })
// const increment = payload => ({ type: INCREMENT, payload })
// 计数器 减少
const decrement = payload => ({ type: ats.DECREMENT, payload })

export { increment, decrement }
```

*注：额外添加 Action Types 会让代码结构变复杂，此操作可省略。但，`domain/action` 命名方式强烈推荐！*

### Reducer 的分离与合并

- 随着项目功能变得越来越复杂，需要 Redux 管理的状态也会越来越多
- 此时，有两种方式来处理状态的更新：
  1. 使用一个 reducer：处理项目中所有状态的更新
  2. 使用多个 reducer：按照项目功能划分，每个功能使用一个 reducer 来处理该功能的状态更新
- 推荐：**使用第二种方案(多个 reducer)**，每个 reducer 处理的状态更单一，职责更明确
- 此时，项目中会有多个 reducer，但是 **store 只能接收一个 reducer**，因此，需要将多个 reducer 合并为一根 reducer，才能传递给 store
- 合并方式：使用 Redux 中的 `combineReducers` 函数
- 注意：**合并后，Redux 的状态会变为一个对象，对象的结构与 combineReducers 函数的参数结构相同**
  - 比如，此时 Redux 状态为：`{ a： aReducer 处理的状态, b： bReducer 处理的状态 }`
- 注意：虽然在使用 `combineReducers` 以后，整个 Redux 应用的状态变为了`对象`，但是，对于每个 reducer 来说，每个 reducer 只负责整个状态中的某一个值。也就是每个 reducer 各司其职，最终，由多个 reducer 合作完成整个应用状态的更新。
  - 也就是：**每个reducer只负责整个应用状态中的某一部分**，每个 reducer 都很自私只关注自己的数据
  - 举个例子：
    - 登录功能：`loginReducer` 处理的状态只应该是跟登录相关的状态
    - 个人资料：`profileReducer` 处理的状态只应该是跟个人资料相关的状态
    - 文章列表、文章详情、文章评论 等

```js
import { combineReducers } from 'redux'

// 计数器案例，状态默认值为：0
const aReducer = (state = 0, action) => {}
// Todos 案例，状态默认值为：[]
const bReducer = (state = [], action) => {}

// 合并多个 reducer 为一个 根reducer
const rootReducer = combineReducers({
  a: aReducer,
  b: bReducer
})
export default rootReducer

// store/index.js
const store = createStore(rootReducer)

// 合并后的 redux 状态： { a: 0, b: [] }
```

