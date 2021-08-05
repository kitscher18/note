# Promise

> node ->   js文件演示

## 1. 介绍

- Promise 是 es6 新提出来的一个非常重要的语法

- 作用 :  它是一种(想以编写同步代码的方式) 处理异步的解决方案

- 以前处理异步的方案?  回调
  - 缺点 : 回调地狱

```js
// <!-- 第四个请求 --> 
$.ajax({
    url :'',
    data : {},
    type : '',
    success : res => {
      
  })
```

- 解决方案 : Promise 

> axios.get() .then()

```js
Promise.then(res => {
   // 第一个请求
   }).then(res => {
   // 第二个请求
   }).then(res => {
   // 第三个请求
   })
```



## 2. Promise的基本使用

```js
/**
 * 自己创建一个promise??????????????
 *
 * 1. Promise 是一个构造函数
 * 2. () 参数 是一个回调
 *   - resolve : fn 异步操作 成功的时候
 *   - reject : fn  异步操作 失败的时候
 * 3. Promise内部放一个异步操作
 *
 * 4. 操作成功的时候 => 调  resolve => 走 then
 *    操作失败的时候 => 调  reject => 走 catch
 */

/**
 * 自己使用 promise ?????????????? 
 * 1. p 是一个什么类型 : Promise
 * 2. axios.get() 也是一个Promise类型
 * 3. axios.get().then()
 * ==>  p.then()
 * ==> XXX.then() XXX有可能就是 promise 类型
 两个原型方法
  Promise.prototype.then
  Promise.prototype.catch
 */

const p = new Promise((resolve, reject) => {
  setTimeout(() => {
    console.log('异步操作开始了')
    // 假如异步操作成功  => 调 resolve()  => 走  then
    // resolve('操作成功喽')

    // 假如异步操作失败 => 调 reject   =>  走catch()
    reject('操作失败了,.,,,,,,,')
  }, 500)
})

// 使用
p.then(res => {
  console.log('走then了', res)
}).catch(error => {
  console.log('走catch', error)
})
```



## 3. Promise的链式编程

```js
/**
 * promise 是一个链式编程
 * p : promise 类型  可以调then
 * p.then() 返回的结果还是一个promise
 *
 */
p.then(res1 => {
  console.log('走 then1', res1)   ===> p 里面 resolve的值
})
  .then(res2 => {
    console.log('走 then2', res2) ===> undefined
  })
  .then(res3 => {
    console.log('走 then3', res3) ===> undefined
  })

```

- 返回值问题
- **第一种返回** : `直接返回值`

```js
p.then(res1 => {
  console.log('走 then1', res1)    ===> p 里面 resolve的值
  return 111
})
  .then(res2 => {
    console.log('走 then2', res2)  ===>  111
    return 222
  })
  .then(res3 => {  
    console.log('走 then3', res3)  ===>  222
  })

```

- **第二种返回** : `返回 promise`

```js
p.then(res1 => {
  console.log('走 then1', res1)    ===> p 里面 resolve的值
  return p
})
  .then(res2 => {
    console.log('走 then2', res2)  ===> p 里面 resolve的值
    return p
  })
  .then(res3 => {  
    console.log('走 then3', res3)  ===> p 里面 resolve的值
  })
```



## 4. 封装一个异步读取文件的promise

- 封装之前

```js
//1. 引入模块
import fs from 'fs'

fs.readFile('./a.txt', 'utf8', (err, data) => {
  // 异常截流
  if (err) {
    return console.log('读取失败')
  }
  console.log(data)
})
```

- 封装之后

```js
//1. 引入模块
const fs = require('fs')

//2. 异步读取 的一个promise封装
const p = new Promise((resolve, reject) => {
  fs.readFile('./a1.txt', 'utf8', (err, data) => {
    // 处理异常 异常截流
    if (err) {
      return reject('读取失败')
    }

    // 操作成功
    resolve(data)
  })
})

//3. 使用
p.then(res => {
  console.log('走then了', res)
}).catch(err => {
  console.log('走catch', err)
})
```



##5. 封装一个读取多个文件的promise

```js
//1. 引入模块
const fs = require('fs')

//2. 异步读取 的一个promise封装
function mg_readF(path) {
  //2.1 创建 promise 实例
  const p = new Promise((resolve, reject) => {
    fs.readFile(path, 'utf8', (err, data) => {
      // 处理异常 异常截流
      if (err) {
        return reject('读取失败')
      }

      // 操作成功
      resolve(data)
    })
  })

  //2.2 返回出来
  return p
}

//3. 使用
// mg_readF('./a.txt')  读取a文件的promise实例
// .then() 得到的结果, 由调用这个then() 之前的promise里面的resolve() 决定的
mg_readF('./a.txt')
  .then(res1 => {
    console.log(res1)
    return ml_readF('./b.txt')
  })
  .then(res2 => {
    console.log(res2)
    return ml_readF('./c.txt')
  })
  .then(res3 => {
    console.log(res3)
  })

// ml_readF('./a.txt').then(res => {

// })
// ml_readF('./b.txt').then(res => {

// })
// ml_readF('./c.txt').then(res => {

// })

```



## 6. Promise静态方法 (4个)

#### 介绍 

+ `Promise.all([ promise1, promise2, ... ]).then( ... )`

  Promise.all() 方法会发起并行的 Promise 异步操作，等所有的异步操作全部结束后

  才会执行下一步的 .then操作（**等待机制**）。

+ `Promise.race([ promise1, promise2, ... ]).then( .... )`

  Promise.race() 方法会发起并行的 Promise 异步操作，只要任何一个异步操作完成，

  就立即执行下一步的 .then 操作（**赛跑机制**）

+ `Promise.resolve()`返回一个只会成功的promise对象

+ `Promise.reject()`返回一个只会失败的Promise对象

####举例 : 

- data.json 数据

```js
{
  "one": [11, 12, 13, 14],
  "two": [21, 22, 23, 24],
  "three": [31, 32, 33, 34]
}
json-server data.json
```

- 代码

```js
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Document</title>
  </head>
  <body>
    <script src="./node_modules/axios/dist/axios.js"></script>
    <script>
      // Promise.all().then( )   在all里面所有的异步请求都完成,才会走then
      // Promise.race().then()   在 race 里面只要有一个完成,就可以走then

      // 需求1 : 在三个异步请求`全部完成`之后 ,才执行我要处理的操作

      // axios.get('http://localhost:3000/one').then(res => {
      //   console.log(res)
      // })
      // axios.get('http://localhost:3000/two').then(res => {
      //   console.log(res)
      // })
      // axios.get('http://localhost:3000/three').then(res => {
      //   console.log(res)
      // })

      // console.log('三个异步请求都结束了才执行的我')

      // Promise.all([
      //   axios.get('http://localhost:3000/one'),
      //   axios.get('http://localhost:3000/two'),
      //   axios.get('http://localhost:3000/three')
      // ]).then(res => {
      //   console.log(res)

      //   console.log('三个异步请求都结束了才执行的我')
      // })

      // 需求2 : 只要有 `一个异步操作` 完成, 就可以处理我们的操作
      Promise.race([
        axios.get('http://localhost:3000/one'),
        axios.get('http://localhost:3000/two'),
        axios.get('http://localhost:3000/three'),
      ]).then(res => {
        console.log(res)
        console.log('有一个完成喽')
      })
		
      let p1 =  axios.get('http://localhost:3000/one'),
      let p2 =  axios.get('http://localhost:3000/two'),
      let p3 =  axios.get('http://localhost:3000/three'),
	  Promise.race([
       p1,
          p2,
            p3        
      ]).then(res => {
        console.log(res)
        console.log('有一个完成喽')
      })
    </script>
  </body>
</html>
```



## 7. Promise的三个状态

```js
/**
* Promise的三个状态
*  pending 等待/进行中
*  fulfilled  操作成功
*  rejected   操作失败
*/
// 在 html 中演示
const p = new Promise((resolve, reject) => {
    
    resolve()
     reject()
   
})
```

![image-20210101090212935](.\images\image-20210101090212935.png)



```js
1 : 一个构造函数
2 : 两个原型方法
3 : 三种状态 
4 : 四个静态方法
```



## 场景演示

场景1 : axios.get().then(res => {   })

场景2 :  异步读取文件  

```js
fs : node内置模块 ===> 处理异步 ==> 回调
then-fs : 第三方模块 ==> 处理异步 ==> promise
```



```js
1. 安装 : npm i then-fs
2. 引入
let fs = require('then-fs')
3. 读取
fs.readFile('./a.txt', 'utf8').then(res => {
  console.log('res:', res)
})
```





# async与await

## 基本概念 

> async await号称异步的终极解决方案，async await之后再无回调

async/await 是 ES8 引入的新语法，用来简化 Promise 异步操作。在 async/await 出
现之前，开发者只能通过链式.then() 的方式处理 Promise 异步操作。

+ .then方式的优点：
  + 解决了回调地狱的问题
 + .then方式的缺点
   + 代码冗余、阅读性差、不易理解



## 基本使用 

1. async  和 await 是一对关键字, await 要用在 async 函数中

2. async 用于修饰一个函数,   `async + 函数`
3. await 后面一般会跟一个promise对象,  await会阻塞async函数的执行, 直到等到 promise成功的结果(resolve的结果)    `await + promise`

- 演示1 :  

````jsx
import fs from 'then-fs'

async function fn() {
    
  let res1 = await fs.readFile('./a.txt', 'utf8')
  console.log(res1)
    
   let res2 = await fs.readFile('./b.txt', 'utf8')
   console.log(res2)
    
   let res3 = await fs.readFile('./c.txt', 'utf8')
   console.log(res3)
}

fn()

````

- 演示2 : 

```js
import axios from 'axios'

async function fn1() {
  // 小程序接口
  let url =https://api-hmugo-web.itheima.net/api/public/v1/home/swiperdata'
  let res = await axios.get(url)
  console.log(res.data)
}

fn1()
```



## 注意点 : 

 * 1. async 和 await `成对出现`
      - 缺async : 报错 
      - 缺await : res 就是一个promise,不会它的结果
 * 2. `加不加await`的区别
      1.  let res = axios.get(url)   一看 结果值 是一个promise , 就可以 使用async和await
     2.  let res = await axios.get(url)   加上 await 得到的请求的结果值 也就是之前 .then()里面的res
 * 3. async 和 await 处理异常 借助  `try...catch`
 * 4. async 要添加 await 最近的函数前面  `就近原则`




## 异常处理

await 只会等待 promise 成功的结果, 如果失败了会报错, 需要 try catch

```jsx
import fs from 'then-fs'

async function fn() {

  const data = await fs.readFile('./a.txt', 'utf8')
  console.log(data)
  try {
    const r2 = await fs.readFile('./b.txt', 'utf8')
    console.log(r2)
  } catch (err) {
    console.log('读取文件失败了', err)
  }
  const r3 = await fs.readFile('./c.txt', 'utf8')
  console.log(r3)
}
fn()
console.log(2)
```



## 面试中会用到的其他用法

async  和 await 是一对关键字

1. async用于修饰一个函数, 表示一个函数是异步的

2.  如果async函数内没有await, 那么async没有意义的, 全是同步的内容

3. 只有遇到了await开始往下, 才是异步的开始

```js
console.log(1)
async function fn () {
  console.log(2)
  await 1
  console.log(3)
}
fn()
console.log(4)
```





# 宏任务和微任务

## 基本概念

JavaScript 把任务队列分为**宏任务队列**和**微任务队列** (列举了一些常考部分)

+ 宏任务（macrotask）
  + 整个script标签
  + setTimeout、setInterval、
  + new Promise() 部分
  + 其它宏任务
+ 微任务（microtask）
  + Promise.then() 部分
  + 其它微任务

![image-20201229132251639](.\images\image-20201229132251639.png)

每一个宏任务执行完之后，都会检查是否存在待执行的微任务

如果有，则执行完所有微任务之后，再继续执行下一个宏任务。

```jsx
// 宏任务1
console.log(1)

// 只要看到了加了时间, 那么一定是等时间满足了, 才会加入到队列中
setTimeout(() => {
  console.log(2) // 宏任务2的内容
}, 1000)

setTimeout(() => {
  console.log(4) // 宏任务3的内容
}, 0)

console.log(3) // 宏任务1的内容
```

## 面试题分析

+ 1

```jsx
console.log(1)

setTimeout(function() {
  console.log(2)
}, 0)

const p = new Promise((resolve, reject) => {
  resolve(1000)
})
p.then(data => {
  console.log(data)
})

console.log(3)
```

+ 2

```jsx
console.log(1)
setTimeout(function() {
  console.log(2)
  new Promise(function(resolve) {
    console.log(3)
    resolve()
  }).then(function() {
    console.log(4)
  })
})

new Promise(function(resolve) {
  console.log(5)
  resolve()
}).then(function() {
  console.log(6)
})
setTimeout(function() {
  console.log(7)
  new Promise(function(resolve) {
    console.log(8)
    resolve()
  }).then(function() {
    console.log(9)
  })
})
console.log(10)
```

+ 3

```js
  console.log(1)

  setTimeout(function() {
    console.log(2)
  }, 0)

  const p = new Promise((resolve, reject) => {
    console.log(3)
    resolve(1000) // 标记为成功
    console.log(4)
  })

  p.then(data => {
    console.log(data)
  })

  console.log(5)
```

+ 4

```js
  new Promise((resolve, reject) => {
    resolve(1)

    new Promise((resolve, reject) => {
      resolve(2)
    }).then(data => {
      console.log(data)
    })

  }).then(data => {
    console.log(data)
  })

  console.log(3)
```

+ 5  

```js
setTimeout(() => {
  console.log(1)
}, 0)
new Promise((resolve, reject) => {
  console.log(2)
  resolve('p1')

  new Promise((resolve, reject) => {
    console.log(3)
    setTimeout(() => {
      resolve('setTimeout2')
      console.log(4)
    }, 0)
    resolve('p2')
  }).then(data => {
    console.log(data)
  })

  setTimeout(() => {
    resolve('setTimeout1')
    console.log(5)
  }, 0)
}).then(data => {
  console.log(data)
})
console.log(6)
```

