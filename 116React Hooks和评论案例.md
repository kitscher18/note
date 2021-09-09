# React Hooks

- React Hooks 介绍 
- React Hooks 基础

## React Hooks 介绍

1. Hooks 是什么
2. 为什么要有 Hooks

### Hooks 是什么

- `Hooks`：钩子、钓钩、钩住
- `Hooks` 是 **React v16.8** 中的新增功能
- 作用：为**函数组件**提供状态、生命周期等原本 class 组件中提供的 React 功能
  - 可以理解为通过 Hooks 为函数组件钩入 class 组件的特性
- 注意：**Hooks 只能在函数组件中使用**，自此，函数组件成为 React 的新宠儿

React v16.8 版本前后，组件开发模式的对比：

- React v16.8 以前： class 组件(提供状态) + 函数组件(展示内容)
- React v16.8 及其以后：
  1. class 组件(提供状态) + 函数组件(展示内容)
  2. Hooks(提供状态) + 函数组件(展示内容)
  3. 混用以上两种方式：部分功能用 class 组件，部分功能用 Hooks+函数组件

注意1：虽然有了 Hooks，但 React 官方并没有计划从 React 库中移除 class。
注意2：有了 Hooks 以后，不能再把**函数组件**称为无状态组件了，因为 Hooks 为函数组件提供了状态。

### 为什么要有 Hooks

两个角度：1 组件的状态逻辑复用 2 class 组件自身的问题

1. class 组件自身的问题：
   - 选择：函数组件和 class 组件之间的区别以及使用哪种组件更合适
   - 需要理解 class 中的 this 是如何工作的
   - 相互关联且需要对照修改的代码被拆分到不同生命周期函数中
     - componentDidMount ->  window.addEventListener('resize', this.fn)
     - componentWillUnmount -> window.addEventListener('resize', this.fn)
   - 相比于函数组件来说，不利于代码压缩和优化，也不利于 TS 的类型推导

正是由于 React 原来存在的这些问题，才有了 Hooks 来解决这些问题

### 前面学习的 React 知识是有用的

class 组件相关的 API 不用了，比如：

- `class Hello extends Component`
- `componentDidMount`、`componentDidUpdate`、`componentWillUnmount`
- `this` 相关的用法

原来学习的内容还是要用的，比如：

- JSX：`{}`、`onClick={handleClick}`、条件渲染、列表渲染、样式处理等
- 组件：函数组件、组件通讯
- 路由
- React 开发理念：`单向数据流`、`状态提升` 等
- 解决问题的思路、技巧、常见错误的分析等

## React Hooks 基础

1. 概述
2. useState
3. useEffect

## useState Hook

### 概述

问题：Hook 是什么? 一个 Hook 就是一个特殊的函数，让你在函数组件中获取状态等 React 特性
使用模式：函数组件 + Hooks
特点：从名称上看，Hook 都以 use 开头

### useState Hook 的基本使用

- 使用场景：当你想要在**函数组件中，使用组件状态时**，就要使用 **useState** Hook 了
- 作用：为函数组件提供状态（state）
- 使用步骤：
  1. 导入 `useState` 函数
  2. 调用 `useState` 函数，并传入状态的初始值
  3. 从 `useState` 函数的返回值中，拿到状态和修改状态的函数
  4. 在 JSX 中展示状态
  5. 在按钮的点击事件中调用修改状态的函数，来更新状态

```js
import { useState } from 'react'

const Count = () => {
  // 返回值是一个数组
  const stateArray = useState(0)

  // 状态值 -> 0
  const state = stateArray[0]
  // 修改状态的函数
  const setState = stateArray[1]

  return (
    <div>
      {/* 展示状态值 */}
      <h1>useState Hook -> {state}</h1>
      {/* 点击按钮，让状态值 +1 */}
      <button onClick={() => setState(state + 1)}>+1</button>
    </div>
  )
}
```

- 参数：**状态初始值**。比如，传入 0 表示该状态的初始值为 0
  - 注意：此处的状态可以是任意值（比如，数值、字符串等），而 class 组件中的 state 必须是对象
- 返回值：数组，包含两个值：1 状态值（state） 2 修改该状态的函数（setState）

### 使用数组解构简化

比如，要获取数组中的元素：

1. 原始方式：索引访问

```js
const arr = ['aaa', 'bbb']

const a = arr[0]  // 获取索引为 0 的元素
const b = arr[1]  // 获取索引为 1 的元素
```

2. 简化方式：数组解构
   - 相当于创建了两个变量（可以是任意的变量名称）分别获取到对应索引的数组元素

```js
const arr = ['aaa', 'bbb']

const [a, b] = arr
// a => arr[0]
// b => arr[1]

const [state, setState] = arr
```

- 使用数组解构简化 `useState` 的使用
  - 约定：**修改状态的函数名称以 set 开头，后面跟上状态的名称**

```js
// 解构出来的名称可以是任意名称

const [state, setState] = useState(0)
const [age, setAge] = useState(0)
const [count, setCount] = useState(0)
```

### 状态的读取和修改

状态的使用：1 读取状态 2 修改状态

1. 读取状态：该方式提供的状态，是函数内部的局部变量，可以在函数内的任意位置使用

2. 修改状态：
  - `setCount(newValue)` 是一个函数，参数表示：**新的状态值**
  - 调用该函数后，将**使用新的状态值`替换`旧值**
  - 修改状态后，因为状态发生了改变，所以，该组件会重新渲染

### 组件的更新过程

函数组件使用 **useState** hook 后的执行过程，以及状态值的变化： 

- 组件第一次渲染：
  1. 从头开始执行该组件中的代码逻辑
  2. 调用 `useState(0)` 将传入的参数作为状态初始值，即：0
  3. 渲染组件，此时，获取到的状态 count 值为： 0

- 组件第二次渲染：
  1. 点击按钮，调用 `setCount(count + 1)` 修改状态，因为状态发生改变，所以，该组件会重新渲染
  2. 组件重新渲染时，会再次执行该组件中的代码逻辑
  3. 再次调用 `useState(0)`，此时 **React 内部会拿到最新的状态值而非初始值**，比如，该案例中最新的状态值为 1
  4. 再次渲染组件，此时，获取到的状态 count 值为：1

注意：**useState 的初始值(参数)只会在组件第一次渲染时生效**。 

也就是说，以后的每次渲染，useState 获取到都是最新的状态值。React 组件会记住每次最新的状态值!

### 为函数组件添加多个状态

问题：如果一个函数组件需要多个状态，该如何处理?
回答：调用 `useState` Hook 多次即可，每调用一次 useState Hook 可以提供一个状态。
注意：useState Hook 多次调用返回的 [state, setState] 相互之间，互不影响。

### hooks 的使用规则

注意：**React Hooks 只能直接出现在 函数组件 中，不能嵌套在 if/for/其他函数中**！

否则就会报错：React Hook "useState" is called conditionally. React Hooks must be called in the exact same order in every component render

React 的 useState 这个 Hook 被条件性（放在一个条件判断中）的调用了。

React Hooks 必须要每次组件渲染时，按照**相同的顺序**来调用所有的 Hooks。

- 为什么会有这样的规则？ 因为 React 是按照 Hooks 的调用顺序来识别每一个 Hook，如果每次调用的顺序不同，导致 React 无法知道是哪一个 Hook

---

## useEffect Hook

1. side effect - 副作用
2. useEffect 的基本使用
3. useEffect 的依赖
4. useEffect 发送请求

### side effect - 副作用

使用场景：当你想要在函数组件中，**处理副作用（side effect）时**，就要使用 **useEffect** Hook 了
作用：**处理函数组件中的副作用（side effect）**


问题：副作用（side effect）是什么? 
回答：在计算机科学中，如果一个函数或其他操作修改了其局部环境之外的状态变量值，那么它就被称为有副作用
类比，对于 999 感冒灵感冒药来说：

- （**主**）作用：用于感冒引起的头痛，发热，鼻塞，流涕，咽痛等 
- 副作用：可见困倦、嗜睡、口渴、虚弱感

理解：副作用是相对于主作用来说的，一个功能（比如，函数）除了主作用，其他的作用就是副作用
对于 React 组件来说，**主作用就是根据数据（state/props）渲染 UI**，除此之外都是副作用（比如，手动修改 DOM）

React 组件的公式：**UI = f(state)**

常见的副作用（side effect）：

- 数据（Ajax）请求、手动修改 DOM、localStorage 操作等

```js
// 不带副作用的情况：
// 该函数的（主）作用：计算两个数的和
function fn(a, b) {
  return a + b
}

// 带副作用的情况：
let c = 1
function fn(a, b) {
  // 因为此处修改函数外部的变量值，而这一点不是该函数的主作用，因此，就是：side effect（副作用）
  c = 2
  return a + b
}

// 带副作用的情况：
function fn(a, b) {
  // 因为 console.log 会导致控制台打印内容，所以，也是对外部产生影响，所以，也是：副作用
  console.log(a)
  return a + b
}

// 没有副作用：
function fn(obj) {
  return obj.name
}

// 有副作用：
function fn(obj) {
  // 此处直接修改了参数的值，也是一个副作用
  obj.name = '大飞哥'
  return obj.name
}
const o = { name: '小马哥' }
fn(o)
```

### useEffect 的基本使用

使用场景：当你想要在函数组件中，处理副作用（side effect）时，就要使用 useEffect Hook 了
作用：处理函数组件中的副作用（side effect）
注意：在实际开发中，副作用是不可避免的。因此，react 专门提供了 **useEffect** Hook **来处理函数组件中的副作用**

```js
import { useEffect } from 'react'

useEffect(function effect() {
  document.title = `当前已点击 ${count} 次`
})

useEffect(() => {
  document.title = `当前已点击 ${count} 次`
})
```

解释：

- 参数：回调函数（称为 **effect**），就是**在该函数中写副作用代码**
- 执行时机：该 effect 会在每次组件更新（DOM更新）后执行

### useEffect 的依赖

- 问题：如果组件中有另外一个状态，另一个状态更新时，刚刚的 effect 回调，也会执行 
- 性能优化：**跳过不必要的执行，只在 count 变化时，才执行相应的 effect**

```js
useEffect(() => {
  document.title = `当前已点击 ${count} 次`
}, [count])
```

解释：

- 第二个参数：可选的，可省略；也可以传一个数组，数组中的元素可以成为依赖项（deps） 
- 该示例中表示：只有当 count 改变时，才会重新执行该 effect

### useEffect 的依赖是一个空数组

useEffect 的第二个参数，还可以是一个**空数组（[]）**，表示只在组件第一次渲染后执行 effect
使用场景：1 事件绑定 2 发送请求获取数据 等

```js
useEffect(() => {
  const handleResize = () => {}
  window.addEventListener('resize', handleResize)
}, [])
```

解释：

- 该 effect 只会在组件第一次渲染后执行，因此，可以执行像事件绑定等只需要执行一次的操作
  - 此时，相当于 class 组件的 componentDidMount 钩子函数的作用
- 跟 useState Hook 一样，一个组件中也可以调用 useEffect Hook 多次 
- 推荐：一个 useEffect 只处理一个功能，有多个功能时，使用多次 useEffect

### 总结 useEffect 的使用



```js
// 触发时机：1 第一次渲染会执行 2 每次组件重新渲染都会再次执行
useEffect(() => {})

// 触发时机：只在组件第一次渲染时执行
// 相当于 class 组件钩子函数的 componentDidMount
useEffect(() => {}, [])

// 触发时机：1 第一次渲染会执行 2 当 count 变化时会再次执行
useEffect(() => {}, [count])
```

### 不要对 useEffect 的依赖撒谎！！！

- useEffect 回调函数中用到的数据（比如，count）就是依赖数据，就应该出现在依赖项数组中
- 如果 useEffect 回调函数中用到了某个数据，但是，没有出现在依赖项数组中，就会导致一些 Bug 出现！
- 所以，不要对 useEffect 的依赖撒谎

```js
// useEffect 回调中依赖什么数据，这个数据就一定要出现在 依赖数组中
// 否则，就会出现与我们的预期不同的表现，也就是出 Bug ！！！
// 比如，此处的回调函数中用到了 count，那么，count 就是当前的依赖，
//      就应该在依赖数组中，提供 count，如果不提供，代码就出 Bug 了！！！

// 没有问题：
// useEffect(() => {
//   console.log('effect')
//   // 副作用
//   document.title = `Count：${count}`
// }, [count])

// 有问题：
// 如果安装了 ESLint 插件，在 useEffect 依赖项数组的代码位置就会有黄色的波浪线警告
// 看到该警告，就把鼠标放在 警告 上面，根据提示，来补全依赖！！！
useEffect(() => {
  console.log('effect')
  // 副作用
  document.title = `Count：${count}`
}, []) // 此处的的数组会有一个黄色的波浪线警告
```



---



### useEffect 组件卸载时

问题：如何在组件卸载时，解绑事件？此时，就用到 effect 的返回值了

```js
useEffect(() => {
  const handleResize = () => {}
  window.addEventListener('resize', handleResize)
  
  // 这个返回的函数，会在该组件卸载时来执行
  // 因此，可以去执行一些清理操作，比如，解绑 window 的事件、清理定时器 等
  return () => window.removeEventListener('resize', handleResize)
}, [])
```

解释：

- effect 的返回值也是可选的，可省略。也可以返回一个**清理函数**，用来执行事件解绑等清理操作
- 清理函数的执行时机：1**【空数组没有依赖】组件卸载时**
  - 此时，相当于 class 组件的 `componentWillUnmount` 钩子函数的作用
- 推荐：一个 useEffect 只处理一个功能，有多个功能时，使用多次 useEffect 
- 优势：根据业务逻辑来拆分，相同功能的业务逻辑放在一起，而不是根据生命周期方法名称来拆分代码 
- 编写代码时，关注点集中；而不是上下翻滚来查看代码

### 将事件处理程序放在 useEffect 内部

- 参考：[useEffect 完全使用指南](https://www.jianshu.com/p/ab432b6344f6)

```js
// 1 将 resize 事件处理程序放在 effect 回调中，当前这个代码是没有问题的
useEffect(() => {
  const handleResize = () => {
    console.log('window 窗口大小改变了')
  }
  window.addEventListener('resize', handleResize)

  return () => {
    window.removeEventListener('resize', handleResize)
  }
}, [])

// 2 将 resize 事件处理程序拿到 useEffect 的外部，当前这个代码是没有问题的
const handleResize = () => {
  console.log('window 窗口大小改变了')
}

useEffect(() => {
  window.addEventListener('resize', handleResize)

  return () => {
    window.removeEventListener('resize', handleResize)
  }
}, [])

// 3 有依赖项的情况：
useEffect(() => {
  // resize 事件的处理程序
  const handleResize = () => {
    console.log('window 窗口大小改变了', count)
  }

  window.addEventListener('resize', handleResize)

  return () => {
    window.removeEventListener('resize', handleResize)
  }
}, [count])


// 注意：此处的代码，会给一些警告！！！ 不要按照这种方式写代码！！！
// 4 如果将 handleResize 放到了 useEffect 外部，React 会给以警告：
//   要么将 handleResize 放到 useEffect 中
//   要么使用 useCallback 这个 hook 来包裹 handleResize
// resize 事件的处理程序
const handleResize = () => {
  console.log('window 窗口大小改变了', count)
}
useEffect(() => {
  console.log('useeffect 执行了')
  window.addEventListener('resize', handleResize)

  return () => {
    window.removeEventListener('resize', handleResize)
  }
}, [handleResize])

// 总结以上几种情况，推荐：在给 window 绑定事件时，将 事件处理程序放在 useEffect 内部。
```



### useEffect 发送请求

在组件中，使用 useEffect Hook 发送请求获取数据（side effect）：

```js
useEffect(() => {
  const loadData = async () => {}
  loadData()
}, [])
```

解释：

- 注意：**effect 只能是一个同步函数，不能使用 async**
- 因为 effect 的返回值应该是一个清理函数，React 会在组件卸载或者 effect 的依赖项变化时重新执行 
- 但如果 effect 是 async 的，此时返回值是 Promise 对象。这样的话，就无法保证清理函数被立即调用
- 如果延迟调用清理函数，也就没有机会忽略过时的请求结果或取消请求
- **为了使用 async/await 语法，可以在 effect 内部创建 async 函数，并调用**

```js
// 错误演示：

// 不要给 effect 添加 async
useEffect(async () => {}, [])
```

```js
// https://github.com/facebook/react/issues/14326#issuecomment-441680293

useEffect(() => {
  // 是否取消本次请求
  let didCancel = false

  async function fetchMyAPI() {
    let url = 'http://something/' + productId
    let config = {}
    const response = await myFetch(url)
    // 如果开启其他请求，就忽略本次（过时）的请求结果
    if (!didCancel) {
      console.log(response)
    }
  }

  fetchMyAPI()
  return () => { didCancel = true } // 取消本次请求
}, [productId])
```

## 评论案例

### 渲染列表数据

1. 准备好列表数据（根据页面分析得到）
2. 将准备好的列表数据，添加为组件的状态
3. 将列表数据，传递给 CommentList 子组件
4. CommentList 子组件接收评论列表数据，进行列表渲染

```js
// 评论列表数据的初始值
const comments = [
  {
    id: 1,
    // 评论内容
    content: '一听就懂！👍',
    // 评论创建时间
    createTime: new Date('2021-1-30 21:39:35'),
    // 点赞数
    like: 98,
    // 是否点赞
    isLike: false,
    // 是否踩
    isHate: false
  },
  {
    id: 2,
    // 评论内容
    content: '一听就懂！👍',
    // 评论创建时间
    createTime: new Date('2021-1-30 21:39:35'),
    // 点赞数
    like: 98,
    // 是否点赞
    isLike: false,
    // 是否踩
    isHate: false
  }
]
```

- JSX 渲染对象会报错：

```js
// JSX 中直接在结构中渲染一个对象值的时候，代码会报错：
// 错误信息：
Objects are not valid as a React child (found: Sat Jan 30 2021 21:39:35 GMT+0800 (中国标准时间)). 
If you meant to render a collection of children, use an array instead.

对象不是合法的 React Child => 对象不能在 JSX 中直接渲染！

// 以下代码就会报上面的错误：
const obj = {}
<div>{obj}</div>

// 该代码也会报错，因为 date 也是对象
const date = new Date()
<div>{date}</div>
```

### 在结构中渲染日期

```js
// 因此，对于 日期对象 不能直接渲染的问题，可以将 日期对象 转化为字符串格式，即可
// 此处，我们可以使用 dayjs 第三方库，来帮我们格式化日期

// 1 安装 dayjs： yarn add dayjs
// 2 在 CommentList 组件中导入 dayjs
import dayjs from 'dayjs'
// 3 在 map 遍历数据时，格式化日期
const date = dayjs(createTime).format('YYYY-MM-DD HH:mm:ss')
// 4 在结构中，渲染 date 字符串即可
<span className="time">{date}</span>
```

### 发表评论 - 获取textarea的值

思路：通过受控组件的方式来获取textarea的值

1. 先通过 `useState` hook 来提供一个状态，状态初始值为：`''`
2. 为 textarea 提供 `onChange` 和 `value`
3. `value` 就是我们提供的状态值
4. `onChange` 中，通过事件对象来获取文本框的最新值
5. 传递给修改状态的函数，来更新状态即可

```js
// 提供文本框的值 状态
  const [value, setValue] = useState('')

  // textarea 的事件处理程序
  const onChange = e => {
    setValue(e.target.value)
  }
  
  <textarea
    cols="80"
    rows="5"
    placeholder="发条友善的评论"
    className="ipt-txt"
    value={value}
    onChange={onChange}
  />
```

### 发表评论 - 更新状态

1. 给发表评论按钮绑定点击事件
2. 在点击事件中，拿到 textarea 的值
3. 将 textarea 的值传递给父组件【子 -> 父 组件通讯，父组件提供一个函数，传递给子组件，子组件调用并传递数据】
4. 父组件拿到子组件中传递过来的评论内容
5. 将评论数据添加到 list 数据中，然后更新状态

```js
// Comments 父组件
// 使用 comments 来作为该状态的初始值
// 注意：setList 更新状态的函数，会用新值 替换 旧值
const [list, setList] = useState(comments)

// 发布评论的函数
const onAdd = content => {
  // 创建新的状态
  const id = list.length === 0 ? 1 : list[list.length - 1].id + 1
  const newList = [
    ...list,
    {
      id,
      content,
      // 评论创建时间
      createTime: new Date(),
      // 点赞数
      like: 0,
      // 是否点赞
      isLike: false,
      // 是否踩
      isHate: false
    }
  ]
  // 更新状态
  setList(newList)
}

{/* 发表评论组件 */}
<CommentSend onAdd={onAdd} />


// CommentSend 子组件
const CommentSend = ({ onAdd }) => {
  // 提供文本框的值 状态
  const [value, setValue] = useState('')

  // textarea 的事件处理程序
  const onChange = e => {
    setValue(e.target.value)
  }

  // 发表评论
  const onAddComment = () => {
    if (value.trim() === '') return
    // 将 textarea 的值传递给父组件
    onAdd(value)
    
    setValue('')
  }
  
	render() {
    return (
      // 省略其他结构
    	<div className="textarea-container">
        <textarea
          cols="80"
          rows="5"
          placeholder="发条友善的评论"
          className="ipt-txt"
          value={value}
          onChange={onChange}
        />

        <button className="comment-submit" onClick={onAddComment}>
          发表评论
        </button>
      </div>
			// 省略其他结构
    )
  }
}
```

### 点赞和踩

两种实现方式：

1. 分别单独处理 点赞 和 踩，也就是使用两个事件处理程序
2. 使用一个事件处理程序来处理 点赞 和 踩
   - 使用这种方式，就需要在事件处理程序中，来**区分**点击的是 点赞还是踩

- 因为点击按钮后，也需要修改状态，所以，点赞和踩这个功能也需要用到 【子 -> 父】 的通讯
  - 父组件提供一个函数
  - 将函数传递给子组件
  - 子组件通过 props 接收这个函数

```js
// 事件处理程序
// type 表示：点击了哪个按钮，如果 like 表示：点赞，如果是 hate 表示：踩
// id 表示：表示点击了哪一项
const onLike = (type, id) => {}

// 点赞
<span className={isLike ? 'like liked' : 'like'} onClick={() => onLike('like', id)}>
  <i className="icon" />
    <span>{like !== 0 && like}</span>
</span>

// 踩
<span className={isHate ? 'hate hated' : 'hate'} onClick={() => onLike('hate', id)>
  <i className="icon" />
</span>
```

- onLike 的基础逻辑
  1. 通过 id 先判断是不是当前项
  2. 如果不是当前项，不需要做任何处理，直接返回 item 即可
  3. 如果是当前项，就创建一个新对象，新对象中先拿到原来的数据
  4. 然后，再将修改的数据也放到对象中即可
  5. 如果是当前点击项，还要区分点击的是 👍 还是 踩
     1. 如果 type 是 like，此时，就是点赞
     2. 否则，就是踩

```js
// 点赞和踩
const onLike = (type, id) => {
  const newList = list.map(item => {
    // 1. 通过 id 先判断是不是当前项
    // 2. 如果不是当前项，不需要做任何处理，直接返回 item 即可
    if (item.id !== id) return item

    // 3. 如果是当前项，就创建一个新对象，新对象中先拿到原来的数据
    // 是当前点击项
    // 区分点赞和踩
    if (type === 'like') {
      // 点赞
    } else {
      // 踩
    }

    // 4. 然后，再将修改的数据也放到对象中即可
    return {
      ...item
      // 此处，就是要修改的数据
    }
  })

  setList(newList)
}
```

- 修改数据的操作：

```js
let { id: curId, like, isLike, isHate } = item
// 如果不是当前 id ，此时，不需要处理该数据，直接返回 item 即可
// 不是当前点击项
if (curId !== id) return item

// 是当前点击项
// 区分点赞和踩
if (type === 'like') {
  // 点赞
  
  // 这个操作，仅仅是修改了 isLike 变量的值，并没有修改 item.isLike 的值，所以，这不是直接修改当前状态
  isLike = !isLike
  
  // 不推荐：
  // 这个操作，才是直接修改当前状态的值！！！
  // item.isLike = !item.isLike
} else {
  // 踩
  isHate = !isHate
}

// 如果是当前 id，此时，修改相应的状态值即可
return {
  ...item,
  // 此处，就是要修改的数据
  isLike,
  isHate
}
```

- 点赞和踩的额外的逻辑（1）
  - 因为点赞和踩，只能有一个是选中的
  - 所以，不管是点击 点赞 还是点击 踩，都要判断另外一个是否选中的，如果选中了，就取消选中；否则，不做处理

```js
      if (type === 'like') {
        // 点赞
        isLike = !isLike
        // 点赞的时候，需要额外的判断 踩 有没有选中
        // 如果选中了，就让 踩 不选中
        // 如果没选中，不需要处理或者直接把当前值给它即可
        isHate = isHate ? false : isHate
      } else {
        // 踩
        isHate = !isHate
        // 点击踩的时候，需要额外的判断 点赞 有没有选中
        // 如果选中了，就让 点赞 不选中
        // 如果没有选中，不需要处理或者直接把当前值给它即可
        isLike = isLike ? false : isLike
      }
```

- 点赞和踩功能的完整实现：

```js
// 点赞和踩
  const onLike = (type, id) => {
    const newList = list.map(item => {
      let { id: curId, like, isLike, isHate } = item
      // 如果不是当前 id ，此时，不需要处理该数据，直接返回 item 即可
      // 不是当前点击项
      if (curId !== id) return item

      // 是当前点击项
      // 区分点赞和踩
      if (type === 'like') {
        // 注意：此处拿到的 isLike 应该是当前值，而不是 修改后的值
        //      所以，为了避免错误，需要将 isLike = !isLike 放到后面
        //      也就是，先使用 isLike，后面再修改，这样后面的代码，就不会影响前面了！！！

        // 点赞时，判断当前是否选中，来控制点赞数
        // 如果当前是选中的，再点击，就是不选中，并且，要让点赞数 - 1
        // 如果当前是未选中的，再点击，就是选中，并且，要让点赞数 + 1
        like = isLike ? like - 1 : like + 1
        // 点赞的时候，需要额外的判断 踩 有没有选中
        // 如果选中了，就让 踩 不选中
        // 如果没选中，不需要处理或者直接把当前值给它即可
        isHate = isHate ? false : isHate
        // 点赞
        isLike = !isLike
      } else {
        // 在点击踩的时候，判断点赞是否选中
        // 如果点赞不选中，不做任何处理
        // 如果点赞选中，就让点赞数 - 1，并且取消点赞的选中（选中已经处理过）
        like = isLike ? like - 1 : like

        // 踩
        isHate = !isHate
        // 点击踩的时候，需要额外的判断 点赞 有没有选中
        // 如果选中了，就让 点赞 不选中
        // 如果没有选中，不需要处理或者直接把当前值给它即可
        isLike = isLike ? false : isLike
      }

      // 如果是当前 id，此时，修改相应的状态值即可
      return {
        ...item,
        // 此处，就是要修改的数据
        isLike,
        isHate,
        like
      }
    })

    setList(newList)
  }
```

### 使用本地缓存存储数据

需求：将评论数据，存储到本地缓存（`localStorage`），进入页面时，从本地缓存中，取出上一次存储的数据

通过分析，发现有以下几种情况需要存储数据：

1. 发表评论
2. 点赞和踩

而实际上，不管是发表评论还是点赞和踩，都会修改状态`list`，因此，我们只需要在 list 状态变化时，来将数据存储到本地缓存中即可

```js
// 副作用的代码，都需要放在 useEffect 这个 hook 中
useEffect(() => {
  // 副作用代码
  localStorage.setItem('comments', JSON.stringify(list))
}, [list])
```

在进入页面时，需要从本地缓存中取出上一次存储的数据，

1. 如果本地缓存中有数据，直接使用本地缓存的数据
2. 如果本地缓存中没有数据，就用 comments 默认值

```js
const [list, setList] = useState( JSON.parse(localStorage.getItem('comments')) || comments )
```

## useState 的回调函数参数

useState 的参数可以有两种形式：

1. `useState(普通的数据)` => useState(false) / useState('')
2. `useState(回调函数)` => useState(() => { return 初始值 })
   1. 回调函数的返回值就是状态的初始值
   2. 该回调函数只会触发一次

```js
  // 使用 回调函数 来为 useState 初始化默认值
  // 回调函数的返回值就是状态的初始值！
  // 注意：该回调函数只会触发一次
  const [list, setList] = useState(() => {
    return JSON.parse(localStorage.getItem('comments')) || comments
  })
```

- 该使用哪种形式？
  1. 如果状态就是一个普通的数据（比如，字符串、数字、数组等）都可以直接使用 `useState(普通的数据)`
  2. 如果状态是经过一些计算得到的，此时，推荐使用 `useState(回调函数)`

```js
// 第一种：
const [list, setList] = useState(
  JSON.parse(localStorage.getItem('comments')) || comments
)
// 可以转化为：
// 这种情况下，只要组件更新，此处的 localStorage 等操作就会重复执行
const initList = JSON.parse(localStorage.getItem('comments')) || comments
const [list, setList] = useState(initList)

// 第二种：
// 这种方式，因为回调函数只会执行一次，所以，此处的 localStorage 等操作代码只会执行一次
const [list, setList] = useState(() => {
  return JSON.parse(localStorage.getItem('comments')) || comments
})

// 在这种情况下，推荐使用第二种方式
```















