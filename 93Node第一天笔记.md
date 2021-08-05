# 课程安排

1. node 基本模块
2. npm (★)
3. 模块化 (★) + express
4. Promise (★)
5. webpack





# nodejs 基本介绍

## 1. 为什么要学习nodejs

1. 全栈工程师的必经之路

```js
语法 + 思想
前端 (js html css) ==>  node( 后端语法(js) + 后端思想 ) ==>  后端( 后端语法  + 后端思想 )   
```

2. NodeJS 是前端项目的基础设施，前端项目中用到的大量包和库,都要通过npm去安装 (node中含有npm)

3. 对于前端工程师，面试时对于nodejs有一定的要求

- [web前端10-20k](https://www.zhipin.com/job_detail/d35a590c832162ed1XB42Ny-Flo~.html?ka=search_list_7)

  

## 2. node.js 是什么？

node.js，也叫作node，或者nodejs，指的都是一个东西。

1. [node.js官方网站](https://nodejs.org/)
2. [node.js中文网](http://nodejs.cn/)
3. [node.js 中文社区](https://cnodejs.org/)

Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境，nodejs允许javascript代码运行在服务端

```
node 不是一个编程语言, node是一个基于Chrome V8 引擎的 js运行环境
1. Chrome V8 引擎   => 解析js
2. node是js运行环境 , 可以运行js的一个环境
    php  ==> apache 服务器 
    java ==> jvm
    js   ==> node(服务器) 和 浏览器(前端)
```

**nodejs的本质：不是一门新的编程语言，nodejs是javascript运行在服务端的运行环境，编程语言还是javascript**



## 3. nodejs与浏览器的区别

相同点：nodejs与浏览器都是运行环境，都能够解析js程序。对于ECMAScript语法来说，在nodejs和浏览器中都能运行。

不同点：nodejs无法使用DOM和BOM的操作，浏览器无法执行nodejs中的文件操作等功能

![](.\images\node和浏览器的不同点.png)

思考：

1. 在浏览器端，可以使用javascript操作文件么？
2. 在nodejs端，可以使用BOM和DOM的方法么？
3. 我们学习nodejs，学习什么内容？ 

## 4. nodejs可以干什么？

1. 开发服务端程序
2. 开发命令行工具（CLI），比如mime
3. 开发桌面应用程序（借助 node-webkit、electron 等框架实现）



# node 使用准备

## 1. 安装nodejs

下载地址 : https://nodejs.org/en/

官网术语解释

- LTS 版本：Long-term Support 版本，长期支持版，即**稳定版 ( 推荐 )** 。
- Current 版本：Latest Features 版本，最新版本，新特性会在该版本中最先加入。 

查看node版本

```bash
node -v
```

打开终端

- cmd : 地址栏输入cmd
- powershell : shift + 右键



```js
// 问题1 : window7 不能使用  14.xx 版本的 => 换了12版本 ???  ==> ok 
// 问题2 : 但是不能再 powershell 里面使用, 只能在cmd 里面使用  ===> 重启 => ok
```





## 2. 环境变量 [window系统]

> 为什么哪里都可以使用node ? 而不是哪里都可以是 FeiQ.exe ????
>
> node 回车

当要求系统运行一个**程序** 而没有告诉它程序所在的完整路径时，

1. 首先在**当前目录**中查找和该字符串匹配的可执行文件
2. 进入系统 path 环境变量查找完整路径

配置环境变量：

```javas
找到环境变量：此电脑--右键--> 属性 --> 高级系统设置 -->  环境变量 
```

直接将可执行程序所在目录配置到PATH中

```javascript
//如果是window7操作系统，注意要用分号;隔开，不要覆盖原来的内容
// 每个电脑的路径不一样注意哦
C:\Users\mawenxing\Desktop\软件快捷方式
```

- 演示

```js
文件里面 : ./FEIQ.EXE
桌面 : FEIQ.EXE
```





## 3. 运行nodejs程序

#### 方式一：REPL介绍 (一般偶尔演示用)

1. REPL 全称: Read-Eval-Print-Loop（交互式解释器）
   - R 读取 - 读取用户输入，解析输入了Javascript 数据结构并存储在内存中。
   - E 执行 - 执行输入的数据结构
   - P 打印 - 输出结果
   - L 循环 - 循环操作以上步骤直到用户两次按下 ctrl-c 按钮退出。
2. 在REPL中编写程序 （类似于浏览器开发人员工具中的控制台功能）
   - 直接在vscode控制台输入 `node` 命令进入 REPL 环境
3. 按两次 Control + C 退出REPL界面 或者 输入 `.exit` 退出 REPL 界面
   - 按住 control 键不要放开, 然后按两下 c 键



#### 方式二：使用node执行js文件 (常用)

- 创建js文件 `helloworld.js`

- 写nodejs的内容：`console.log('hello nodejs')`
- 打开命令窗口 `cmd`
  - shift加右键打开命令窗口，执行 `node 文件名.js`即可
  - 直接在vscode中执行
    - 右键在终端中打开
    - ctr + `
- 执行命令：`node helloworld.js`

注意：在nodejs中是无法使用DOM和BOM的内容的，因此`document, window`等内容是无法使用的。



## 4. 关于代码规范的问题

[JavaScript 语句后应该加分号么？](https://www.zhihu.com/question/20298345)

- Prettier



# 模块学习

> http://nodejs.cn/api/

## 1. global模块

```js
/**
  * 之前 : window.console.log('XXX')  是BOM 的, 
  * 因为这里的console,不是那里的console  是node里面有个全局模块,有个console
  * http://nodejs.cn/api/fs.html
  * 
  * window 顶层对象 全局变量
  * global 全局模块 里面的变量 , 都是全局变量 
  * 注意 : node里面使用 global里面的变量,不需要引入 , 说明其他的是需要引入的
  */
  
  //1. console.log()  打印

  //2. setTimeout 延时器
  setTimeout(() => {
    console.log('123');
  }, 1000);

  //3. __dirname 当前文件夹的绝对路径
  // C:\Users\ma250\Desktop\44-Node\day02\02-代码\Demos
   console.log(__dirname);
 
  //4. require 引入模块 
  // 在node里是通过require 来引入模块的
  // 除了global其他模块都是需要引入的
  const fs = require('fs') 

```



## 2. fs模块

> fs模块是nodejs中最常用的一个模块，因此掌握fs模块非常的有必要，fs模块的方法非常多,用到了哪个查哪个即可。
>
> 文档地址：http://nodejs.cn/api/fs.html

  在nodejs中，提供了fs模块，这是node的核心模块

  注意：

1. 除了global模块中的内容可以直接使用，其他模块都是需要加载的。
2. fs模块不是全局的，不能直接使用。因此需要导入才能使用。

```javascript
let fs = require("fs");
```

#### 2.1 读取文件

> 涛亮学猫叫

> 语法：fs.readFile(path[, options], callback)

方式一：不传编码参数

```javascript
//参数1： 文件的名字
//参数2： 读取文件的回调函数
  //参数1：错误对象，如果读取失败，err会包含错误信息，如果读取成功，err是null
  //参数2：读取成功后的数据（是一个Buffer对象）
fs.readFile("data.txt", function(err, data){
  if(err) return   console.log(err);
  console.log(data);
});
```

方式二：传编码参数

```javascript
//参数1： 文件的路径
//参数2： 编码，如果设置了，返回一个字符串，如果没有设置，会返回一个buffer对象
//参数3： 回调函数
fs.readFile("data.txt", "utf8",function(err, data){
  console.log(err);
  console.log(data);
});
```

关于Buffer对象

```javascript
1. Buffer对象是Nodejs用于处理二进制数据的。
2. 其实任意的数据在计算机底层都是二进制数据，因为计算机只认识二进制。
3. 所以读取任意的文件，返回的结果都是二进制数据，即Buffer对象
4. Buffer对象可以调用toString()方法转换成字符串。
```

#### 2.2 写文件

> 语法：fs.writeFile(file, data[, options], callback)

```javascript
//参数1：写入的文件名(如果文件不存在，会自动创建)
//参数2：写入的文件内容（注意：写入的内容会覆盖以前的内容）
//参数3：写文件后的回调函数
fs.writeFile("2.txt", "hello world, 我是一个中国人", function(err){
  if(err) {
    return console.log("写入文件失败", err);
  }
  console.log("写入文件成功");
});
```

注意：

1. 写文件的时候，会把原来的内容给覆盖掉



#### 2.3 练习1 : 使用 读取 和 写入完成 追加的功能

```js
// 1 获取
fs.readFile('./a.txt', 'utf8', (err, data) => {
  if (err) {
    return console.log('错误哦')
  }
  //2. 拼接
  data += '和男人'

  //3. 写入  
  fs.writeFile('./a.txt', data, err => {
    if (err) {
      return console.log('错误')
    }

    console.log('追加成功')
  })
})
```



#### 2.4  练习2 : 写成绩

```js
// 原来 : 小红=99 小白=100 小黄=70 小黑=66 小绿=88  <= 成绩.txt
// 现在 : 成绩-ok.txt
小红:99
小白:100
小黄:70
小黑:66
小绿:88


// 代码
// 1. 读取到成绩.txt文件的内容
// 2. 对读取到的内容进行处理
// 3. 写入到 成绩-ok.txt 里面
let fs = require('fs')

fs.readFile('./成绩.txt', 'utf8', (err, data) => {
  if (err)  return console.log('错误')

  let arr = data.split(' ').map(v => v.replace('=', ':'))
  console.log(arr.join('\r\n'))
  data = arr.join('\r\n')

  fs.writeFile('./成绩-ok.txt', data, err => {
    console.log('写入成')
  })
})

```



#### 2.5 追加文件

> 语法：fs.appendFile(path, data[, options], callback)

```javascript
//参数1：追加的文件名(如果文件不存在，会自动创建)
//参数2：追加的文件内容（注意：写入的内容会覆盖以前的内容）
//参数3：追加文件后的回调函数
fs.appendFile("2.txt", "我是追加的内容", function(err){
  if(err) {
    return console.log("追加文件内容失败");
  }
  console.log("追加文件内容成功");
})
```

思考：如果没有appendFile，通过readFile与writeFile应该怎么实现？





#### 2.6 文件同步与异步的说明

> fs中所有的文件操作，都提供了异步和同步两种方式

异步方式：不会阻塞代码的执行

```javascript
//异步方式
var fs = require("fs");

console.log(111);
fs.readFile("2.txt", "utf8", function(err, data){
  if(err) {
    return console.log("读取文件失败", err);
  }
  console.log(data);
});
console.log("222");
```

同步方式：会阻塞代码的执行

```javascript
//同步方式
console.log(111);
var result = fs.readFileSync("2.txt", "utf-8");
console.log(result);
console.log(222);
```

总结：同步操作使用虽然简单，但是会影响性能，因此尽量使用异步方法，尤其是在工作过程中。



## 3. path模块 

- 演示一个问题 : 
- 在读写文件的时候，文件路径可以写相对路径或者绝对路径

```javascript
//data.txt是相对路径，读取当前目录下的data.txt, 相对路径相对的是指向node命令的路径
//如果node命令不是在当前目录下执行就会报错， 在当前执行node命令的目录下查找data.txt，找不到
fs.readFile("data.txt", "utf8", function(err, data) {
  if(err) {
    console.log("读取文件失败", err);
  }

  console.log(data);
});
```

相对路径：相对于执行node命令的路径

绝对路径：`__dirname`: 当前文件的目录，`__filename`: 当前文件的目录，包含文件名

```js
//C:\Users\ma250\Desktop\50-node\day02\02-代码\a\a.txt
// C:\Users\ma250\Desktop\50-node\day02\02-代码\a + a.txt
// C:\Users\ma250\Desktop\50-node\day02\02-代码\a + b.txt

// 绝对路径01   太死板了
 let path1 = __dirname + '/a.txt'
// 绝对路径02
let path2 = path.join(__dirname, './a.txt')
console.log(path2)

//2. 异步读取文件
fs.readFile(path2, 'utf8', (err, data) => {
  if (err) {
    return console.log('读取文件失败:', err)
  }
  console.log(data)
})

/**
 * 现象 :  02-代码 / Demos  / 13-path模块.js
 *    Demos => node 13... => ok
 *    02-代码 => node Demos/13.. => 报错了
 * 原因 : 路径错了
 * 解析 :  /a.txt 浏览器里面,相对的是当前文件
 *  但是,在node里面,  相对路径 相对的 `node执行目录`
 * 解决办法 : 使用绝对路径
 */
```



### 3.1 path模块的常用方法

```javascript
path.join();//拼接路径
// 好處
const path2 = path.join(__dirname,'data.txt')   ok
const path2 = path.join(__dirname,'/data.txt')  ok
const path2 = path.join(__dirname,'./data.txt') ok
```

#### 改造 拼接文件 的 路径问题代码



### 3.2 练习 : 歌词播放

- 准备

```js
1. 准备 : 爱歌词 + 网易云音乐
```

- 代码

```js
/**
 * 爱歌词 + 网易云音乐
 * 1. 读取歌词文件
 * 2. 提取出所有的 时间和歌词
 * 3. 配合延时器 setTimeout 打印歌词
 * 4. 需要把提取到的时间转成毫秒值
 */
// [01:28.76]  => 毫秒 =>  01 * 60 * 1000 + 28 * 1000 + 0.78 * 1000
//             => 毫秒 =>  01 * 60 * 1000 + 28 * 1000 +   78 * 10

function toNumber(time) {
  let reg = /(\d{2}):(\d{2})\.(\d{2})/
  let res = reg.exec(time)
  let [, m, s, ms] = res
  return m * 60 * 1000 + s * 1000 + ms * 10
}
// console.log(toNumber('01:28.76'))

let fs = require('fs')
let path = require('path')

fs.readFile(path.join(__dirname, './演示-薛之谦.lrc'), 'utf8', (err, data) => {
  if (err) console.log('错误')

  const reg = /\[(\d{2}:\d{2}\.\d{2})\](.*)\r\n/g

  // console.log(reg.exec(data)[0])
  // console.log(reg.exec(data)[0])
  // console.log(reg.exec(data)[0])

  let result
  while (true) {
    result = reg.exec(data)
    if (!result) {
      break
    }

    let [, time, text] = result

    setTimeout(() => {
      console.log(text)
    }, toNumber(time))
  }
})
```





## 4. http模块

#### 4.1 创建服务器基本步骤

```javascript
//1. 导入http模块，http模块是node的核心模块，作用是用来创建http服务器的。
var http = require("http");

//2. 创建服务器
var server = http.createServer();

//3. 启动服务器，监听某个端口
server.listen(3001, function(){
  console.log("服务器启动成功了, 请访问： http://localhost:3001");
});

//4. 服务器处理请求
server.on("request", function() {
  console.log("我接收到请求了");
});
```

详细说明

1. 给服务器注册request事件，只要服务器接收到了客户端的请求，就会触发request事件
2. request事件有两个参数，request表示请求对象，可以获取所有与请求相关的信息，response是响应对象，可以获取所有与响应相关的信息。
3. 服务器监听的端口范围为：1-65535之间，推荐使用3000以上的端口，因为3000以下的端口一般留给系统使用

- 演示
  - 重复开启一个端口服务器问题 错误码
  - 不安全的接口问题  [坑人的6666端口](https://blog.csdn.net/hi_pig2003/article/details/52995528)



#### 4.2 request对象详解

文档地址：http://nodejs.cn/api/http.html  ==> 【[http.IncomingMessage 类](http://nodejs.cn/api/http.html#http_class_http_incomingmessage) 】

常见属性：

```js
# headers: 所有的请求头信息
# method： 请求的方式
# url： 请求的地址
```

注意：在发送请求的时候，可能会出现两次请求的情况，这是因为谷歌浏览器会自动增加一个`favicon.ico`的请求。

**小结：**request对象中，常用的就是method和url两个参数



#### 4.3 response对象详解

文档地址：http://nodejs.cn/api/http.html  ==> 【[http.ServerResponse 类](http://nodejs.cn/api/http.html#http_class_http_serverresponse) 】

常见的属性和方法：

```javascript
res.write(data): 给浏览器发送请求体，可以调用多次，从而提供连续的请求体
res.end();   通知服务器，所有响应头和响应主体都已被发送，即服务器将其视为已完成。
res.end(data); 结束请求，并且响应一段内容，相当于res.write(data) + res.end()
res.statusCode: 响应的的状态码 200 404 500
res.statusMessage: 响应的状态信息， OK Not Found ,会根据statusCode自动设置。
res.setHeader(name, value); 设置响应头信息， 比如content-type
```

**注意：必须先设置响应头，才能设置响应。** 

```js
//1. res.write() 响应给浏览器的数据
// res.write('777')

//2. res.end() 结束响应

//3. res.setHeader  设置响应头
// 告诉浏览器,解析数据要以UTF8
// res.setHeader('content-type','text/plain;charset=utf8')

//4. 设置状态码 
// res.statusCode = 202
// res.statusMessage = 'langtao'

// 结束响应
res.end('踢鹏还在一直叫')
```



## 5. 根据不同请求输出不同响应数据

- [request.url](http://nodejs.cn/api/http.html#http_message_url)
- `req.url`：获取请求路径
  - 例如：请求`http://127.0.0.1:3000/index` 获取到的是：`/index`
  - 例如：请求`http://127.0.0.1:3000/` 获取到的是：`/`
  - 例如：请求`http://127.0.0.1:3000` 获取到的是：`/`

```js
//3. 处理请求
server.on('request', (req, res) => {
  if (req.url === '/index.html' || req.url === '/') {
    res.end('<h1>Index</h1>')
  } else if (req.url === '/login.html') {
    res.end('<h1>Login</h1>')
  } else {
    res.end('404')
  }
})
```



## 6. 根据不同请求输出不同响应文件    pages/

- 注意：浏览器中输入的URL地址，仅仅是一个标识，不与服务器中的目录一致。也就是说：返回什么内容是由服务端的逻辑决定

```js
server.on('request', (req, res) => {
  if (req.url === '/' || req.url === '/index.html') {
    let path1 = path.join(__dirname, './pages/index.html')
    fs.readFile(path1, (err, data) => {
      res.end(data)
    })
  } else {
    let path1 = path.join(__dirname, './pages/404.html')
    fs.readFile(path1, (err, data) => {
      res.end(data)
    })
  }
})
```



## 7.  时钟案例   clock/

```js
server.on('request', (req, res) => {
  let url = req.url
  console.log('url:', url)

  if (url == '/') {
    let indexPath = path.join(__dirname, './cloak/index.html')

    fs.readFile(indexPath, (err, data) => {
      res.end(data)
    })
  } else if (url == '/index.css') {
    let csspath = path.join(__dirname, './cloak/index.css')
    fs.readFile(csspath, (err, data) => {
      res.end(data)
    })
  } else if (url == '/index.js') {
    let jspath = path.join(__dirname, './cloak/index.js')
    fs.readFile(jspath, (err, data) => {
      res.end(data)
    })
  } else {
    res.end('404')
  }
})
```





## 8. 模拟Apache服务器     pages-apache/

- 一套代码不用if..else...判断  根据 `req.url` 读取不同的页面内容，返回给浏览器, 不然的话 100个页面就麻烦了
- 演示 : `live-server` 学生可不演示, 
- 静态文件夹 : `pages-apache`  => `index.html`

```js
//4. 监听
server.on('request', (req, res) => {
  console.log(req.url)
  // index.html
  //fs.readFile(path.join(__dirname, './pages-apache', 'index.html'), (err, data) => {
  fs.readFile(path.join(__dirname, './pages-apache', req.url), (err, data) => {
    res.end(data)
  })
})
```



## MIME类型

- MIME(Multipurpose Internet Mail Extensions)多用途Internet邮件扩展类型 是一种表示文档性质和格式的标准化方式
- 浏览器通常使用MIME类型（而不是文件扩展名）来确定如何处理文档；因此服务器将正确的MIME类型附加到响应对象的头部是非常重要的
- [MIME 类型](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/MIME_Types)

### mime模块

> Resource interpreted as Stylesheet but transferred with MIME type text/plain: "http://localhost:3001/anoceanofsky.css".
>
>  \* .index.html => text/html
>
>  \* .XXX.css  ==> text/css
>
>  \* .jpg  ===> image/jpeg

- 作用：获取文件的MIME类型
- 安装：`npm i mime`

```js
var mime = require('mime')

// 获取路径对应的MIME类型
mime.getType('txt')                    // ⇨ 'text/plain'
// 根据MIME获取到文件后缀名
mime.getExtension('text/plain')        // ⇨ 'txt'
```

- 代码

```js
server.on('request', (req, res) => {
  //1. 设置 mime 类型
  res.setHeader('content-type', mime.getType(req.url))
 
  // index.html
  //2. 读取
  fs.readFile(path.join(__dirname, './pages-apache', req.url), (err, data) => {
    res.end(data)
  })
})
```



