# Express框架

- **基于 Node.js 平台，快速、开放、极简的 web 开发框架**
- [express 官网](http://expressjs.com/)
- [express 中文网](http://expressjs.com.cn/)
- [express api](http://www.expressjs.com.cn/4x/api.html)

````
js(DOM) => jquery
node   ==> express
````



### 起步 (四步走)

- 安装：`npm i express`

```js
// 1. 导入 express
let  express = require('express')

// 2. 创建 express实例，也就是创建 express服务器
let app = express()

// 3. 启动服务器
app.listen(8001, function () {
  console.log('服务器已启动')
})

// 4. 注册路由
app.get('/', function (req, res) {
  res.send('ok')
})
```



### API说明

- res.send('哈哈') ==> `res.end() + res.setHeader()`

- req : 请求对象 而是在之前req的基础上做的拓展的请求对象
- res : 响应对象,也是在之前res的基础上做的拓展的响应对象



## 注册路由的三种方式

> 安装 : postman 演示

#### 方式1 :  app.METHOD：

-  - 比如：app.get / app.post / app.delete / app.patch
   - 请求方式 : 完全匹配
   - 请求地址 : 完全匹配


```js
app.get('/index', (req, res) => {
  res.send('get')
})
app.post('/index', (req, res) => {
  res.send('post')
})
```



#### 方式2 :  app.all(path, callback) : 

-  请求方式：任意方式
-  请求路径：完全匹配

```js
app.all('/index', (req, res) => {
  res.send('index')
})
```



#### 方式3 :  app.use(path, callback)     更重要的作用是处理中间件

- 请求方式：任意方式
- 请求路径：只要是以path开头的请求地址，都可以被use处理

- 注意：path参数可省略，默认值为：`/`

```js
// 测试
app.use('/index', (req, res) => {
  res.send('index')
})
```



## use 的 其他用法

```js
//4. use
//4.1  普通用法  类型(任意)/路径(开头)   /index/1  /index/2
// app.use('/index', (req, res) => {
//   res.end('index')
// })

//4.2 根路径
// 因为 /index 和 /login  /add 都以/开头的
// app.use('/', (req, res) => {
//   res.end('index')
// })

//4.3 省略
// 如果 路径为 / , 则可以省略
// app.use((req, res) => {
//   res.end('index')
// })

//4.4 把函数提出来
// let test = (req, res) => {
//   res.end('index')
// }
// app.use(test)

//4.5 小例子
let test = (req, res) => {
  res.end('mage')
}
app.use('/mg', test)

//4.6
let expre = function () {
  return (req, res) => {
    res.send('use')
  }
}
app.use(expre())

```



## req : request 常用属性和方法

```js
//1. 获取 GET 请求参数，是一个对象 
req.query  => 获取的 /index?id=3

//2. 获取 POST 请求参数，是一个对象 , 需要配置`body-parser`模块
req.body
```

- 获取`POST`请求参数

```js
// 导入body-parser模块
var bodyParser = require('body-parser');
// 将POST请求参数转化为对象，存储到req.body中
app.use(bodyParser.urlencoded({ extended: true })); // for parsing application/x-www-form-urlencoded

// false : querystring => 解析查询 字符串 => {}
// true :   qs   => 解析查询字符串 => {}

// 此时，就可以获取到POST请求参数了
console.log(req.body)
```



## res : response 常用属性和方法

```js
app.get('/index', (req, res) => {
  // req : request 对象
  // req.query : get的参数对象
  // req.body  : post的参数对象 (下次演示)

  // res : response 对象
  // 1. res.send() 结束响应 res.end() + setHeader()
  // 2. res.sendFile(路径)  读取 + 响应  , 绝对路径
  // res.sendFile(path.join(__dirname, './index.html'))
  // 3. 设置响应码
  // res.sendStatus(300)
  // 4. 设置响应头
  // res.setHeader('content-type', 'text/plain;charset=utf8')
  // res.set('content-type', 'text/plain;charset=utf8')

  //5. 重定向
  res.redirect('/add')

  res.send('哈哈')
})
```



# Express 接口开发

## 1.GET 接口

- 前端

```js
// get 请求
axios
    .get('http://localhost:3000/login', {
    params: {
        username,
        password,
    },
})
    .then(res => {
    console.log('登录:', res.data)
})
```

- 后端

```js
app.get('/login', (req, res) => {

  // 1. 解决跨域  
  res.setHeader('Access-Control-Allow-Origin', '*')
  
  // 2. 处理 get 请求   
  let { username, password } = req.query
  if (username === 'admin' && password === '123456') {
    res.send({
      code: 200,
      msg: '登录成功',
    })
  } else {
    res.send({
      code: 400,
      msg: '登录失败',
    })
  }
})
```





## 2.POST 接口

- 前端

```js
  axios
      .post('http://localhost:3000/login', {
      username,
      password,
  })
      .then(res => {
      console.log('登录:', res.data)
  })
```



- 后端

```js
// 4.解析 req.body 
app.use(express.json())

app.post('/login', (req, res) => {
  console.log('-------------------')

  //1. 解决跨域1 :  允许CORS跨域的域名
  res.setHeader('Access-Control-Allow-Origin', '*')

  console.log('请求了', req.body)

  //3. 处理 post 数据
  let { username, password } = req.body

  if (username === 'admin' && password === '123456') {
    res.send({
      code: 200,
      message: '登录成功',
    })
  } else {
    res.send({
      code: 400,
      message: '登录失败',
    })
  }
})

//2. 解决跨域2
app.options('*', (req, res) => {
  //  允许CORS跨域的域名
  res.setHeader('Access-Control-Allow-Origin', '*')
  // 允许CORS跨域的请求方式 默认 GET POST
  res.setHeader('Access-Control-Allow-Methods', 'GET,POST,PUT,PATCH,DELETE')
  // 允许CORS跨域的请求头
  res.setHeader('Access-Control-Allow-Headers', 'content-type')
  res.send(null)
})
```

- 简单请求和复杂请求

```js
- ajax 是简单请求,  
- axios是非简单请求(发预检请求 options  斥候 先去来个试探请求 看能不能避开跨域请求)
```



## 前端结构部分

```html
<!-- html -->
<input value="admin" class="username" type="text" /> <br />
<input value="123456" class="password" type="text" /> <br />
<button>登录</button>

<!-- 引入 -->
<script src="../../node_modules/axios/dist/axios.js"></script>
<script>
    document.querySelector('button').onclick = function () {
        console.log(123)
        let username = document.querySelector('.username').value
        let password = document.querySelector('.password').value
        console.log(username, password)
    }
</script>
```



# Express中间件

## 基本介绍

### 中间件介绍

- 中间件(Middleware )，特指业务流程的中间处理环节。
- 中间件，是express最大的特色，也是最重要的一个设计
- 很多第三方模块，都可以当做express的中间件，配合express，开发更简单。
- 一个express应用，是由各种各样的中间件组合完成的
- 中间件，本质上就是一个函数

### 中间件原理

为了理解中间件，我们先来看一下我们现实生活中的自来水厂的净水流程。

![image-20200331130641861](.\images\image-20200331130641861.png)

- 在上图中，自来水厂从获取水源到净化处理交给用户，中间经历了一系列的处理环节
- 我们称其中的每一个处理环节就是一个中间件。
- 这样做的目的既提高了生产效率也保证了可维护性。

express中间件原理：

![image-20200331004703510](.\images\image-20200331004703510.png)



## 中间件语法

- 中间件就是一个函数
- 中间件函数中有三个基本参数， req、res、next
  - req就是请求相关的对象
  - res就是响应相关的对象
  - next：它是一个函数，必须调用了next，中间件才会往下传递


+ 语法：

```js
app.use( (req, res, next) => {
  // ....
  next()
})
```



### 中间件的特点

- 每个中间件函数，共享req对象、共享res对象
  - js代码中，所有的req对象是一个对象；所有的res是一个对象
- 不调用`next()`，则程序执行到当前中间件函数后，不再向后执行
  - 注意中间件的顺序，因为有可能因为顺序原因，你的中间件函数不会执行
  - 为了防止代码逻辑混乱，调用 next() 函数后不要再写额外的代码
- 客户端发送过来的请求，可能连续调用多个中间件进行处理
- 使用`app.use()`注册的中间件，GET和POST请求都可以触发

### 中间件初体验

为所有的请求，统一设置响应头

```js
app.use((req, res, next) => {
  req.aa = '超人'

  next()
})
app.use((req, res, next) => {
  req.bb = '孙悟空'

  next()
})

app.get('/login', (req, res) => {
  console.log('结果:', req.aa, req.bb)

  res.send('login')
})
```

中间件的语法只需要 了解即可，真正开发中都会使用内置的或者第三方中间件。



## express内置的中间件

### 1.static

- 什么叫做静态资源
  - css文件
  - 图片文件
  - js文件
  - 等等
- 什么叫做开放静态资源
  - 开放，即允许客户端来访问
- 具体做法
  - 比如，允许客户端访问public文件夹里面的文件
  - `app.use(express.static('public'))`

```js
// 把 www/ 文件当成一个公共资源文件
app.use(express.static('www'))

// 把 public/ 文件当成一个公共资源文件
app.use(express.static('public'))
```



### 2.urlencoded

> postman 演示

用于处理`application/x-www-form-urlencoded`请求的数据

```js
app.use(express.urlencoded({extended: false}));
```

- ajax

```js
// ajax
$.ajax({
    url: 'http://localhost:3000/login',
    method: 'post',
    data: {
        username,
        password,
    },
    success: res => {
        console.log('登录:', res)
    },
})
```



### 3.json

用于处理`content-type`为`application/json`的中间件

```js
app.use(express.json());
```

- axios

```js
axios
    .post('http://localhost:3000/login', {
    username,
    password,
})
    .then(res => {
    console.log('登录:', res.data)
})
```





## 第三方中间件

### cors

使用方法：

- 下载安装cors   `npm i cors`
- `const cors = require('cors');`  --- 加载模块
- `app.use(cors());`  --  注册中间件即可

