# webpack学前必备

[webpack中文网](https://www.webpackjs.com/)

[webpack官网](https://webpack.js.org/)

## 1. Webpack 介绍 

#### Webpack 是什么?? (面试)  

- 前端模块化打包工具 
- WebPack可以看做是**模块打包机**：它做的事情是，分析你的项目结构，找到JavaScript模块、其它的一些浏览器不能直接运行的拓展语言（Sss，less等）以及新语法，并将其转换和打包为合适的格式供浏览器使用。 



#### 为什么要使用WebPack?? (面试)

- 浏览器不识别 SASS、Less ==> 需要对Less/SASS 进行预编译转为CSS ,  供浏览器使用
- 项目中的模块化以及互相之间引用依赖包造成文件分散 ==> 需要把各个分散的模块集中打包成大文件，减少HTTP的链接的请求次数 
- 打包成了大文件,体积就变大了 ==> 代码压缩 
- 部分ES6语法有兼容问题 => ES5/ES3 => 浏览器使用
- .........
- 以上这些操作以前都是需要我们手动处理,这是非常繁琐的, 这个时候webpack就可以上场了,以上的这些操作,
- 在webpack里,只要配置好,一下就可以都搞定了



#### 使用说明

> 一般来说，以后开发都是在webpack的环境下进行开发（node环境）
>
> 并且我们写完的项目并不会直接上线。 而是会经过webpack进行打包。 最终把打包过的文件进行上线。



## 2. Webpack 四个核心概念 (学前了解)：

>  **入口(entry)**、**出口(output)**、**加载器(loader)**、**插件(plugins)**

- 入口 : 要打包哪个文件
- 出口 : 要打包到哪里
- 加载器 : 加载除了js文件其他文件的功能 (css less png)
- 插件 : 处理加载器完成不了的功能, 使用插件



## 3. npm 使用回顾

#### 3.1 全局安装

- 格式 : `npm i xxx -g`
- 作用 :  全局安装的包, 是当成一个工具来使用的
- 比如 :`npm i http-server -g`, `npm i live-server -g` , `npm i json-server -g `, `npm i yarn -g` 等等 



#### 3.2 本地安装1 

- 格式 : `npm i xxx`
- 其他版本 :`npm i xx  --save`   和 `npm i xx -S`
- 作用 :  本地安装的包,  `发布上线阶段` 要用到的库 
- 依赖位置 :  把包的依赖添加到 `dependencies`中
- 比如 : `npm i axios`



#### 3.3 本地安装2  (配置webpack用的最多)

- 格式 : `npm i xxx -D`
- 其他版本 : `npm i xxx --save dev`   //development
- 作用 :  本地安装的包,  只在`开发阶段`都要用到的库 
- 依赖位置 :  把包的依赖添加到 `devdependencies`中
- 比如 : `npm i webpack -D` (就今天使用,因今天就是打包)



#### 3.4 yarn 包管理器

快速、可靠、安全的依赖管理工具。和 npm 类似, 都是包管理工具, 可以用于下载包 

```
npm i jquery
yarn add jquery
```

下载地址: https://yarn.bootcss.com/docs/install/#windows-stable 

windows本  **推荐通过软件包**  安装 (教学资料中)

mac本通过homebrew进行安装

基本命令: 

```text
1. 初始化
	yarn init -y

2. 添加依赖
	yarn add xxxx 
	yarn add xxxx@[version]

3. 移除包
	yarn remove xxxx
             
4. 安装项目全部依赖            
	yarn 
```



## 4. package.json 的介绍

> yarn init -y 

```js
{
  // 包名/项目名  要求:小写字母, 不能是大写或者叫webpack
  "name": "webpack-demo",
  // 版本号  
  "version": "1.0.0",
  // 介绍
  "description": "",
  // 默认入口文件index.js  可以自己指定  
  "main": "index.js",
  // 脚本 (★★★)  
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  // 关键字  
  "keywords": [],
  // 作者
  "author": "",
  // 许可证   
  "license": "ISC"
}
```

- 配置自定义脚本
  - 创建一个 `index.js` =>`console.log('测试启动脚本')`
  - 执行 : `node index.js`
  - 也可以通过运行脚本来执行命令 (优势 如果配置太多 手动就不好处理了)
- **常见脚本**

```js
"scripts": {
  "build": "node index.js",
  "dev": "node index.js",
  "serve": "node index.js",
}
// 像以上这几种运行脚本 :  
- yarn build
- yarn dev
- yarn serve
```



# webpack配置

## 1. webpack打包的基本使用

[本地安装](https://www.webpackjs.com/guides/installation/#本地安装)

1. 创建项目名称 `webacpk-demo` : `不能是中文 不能是大写 不能是webpack`
2. 使用`yarn init -y`生成`package.json`  :
3. 创建两个文件夹 `src/`(源代码文件夹 入口)  和  `dist/`(最终打包输出的文件夹 出口)
4. 在 `src` 里面创建一个 `index.js` 文件  (准备要被打包的入口文件)

```js
console.log('我就要被打包了,哦也');
```

5. 安装依赖包 :`yarn add webpack webpack-cli -D`

```js
webpack  - 核心包
webpack-cli - 使用 webpack 4+版本需要安装
```

6. 添加脚本 : `build`

```js
"scripts": {
    // "build" : "webpack 入口文件"
    "build" : "webpack ./src/index.js"
  },
```

7. 打包运行命令 :`yarn build`

## 2. mode 配置

```js
The 'mode' option has not been set, webpack will fallback to 'production' for this value.
Set 'mode' option to 'development' or 'production' to enable defaults for each environment.
You can also set it to 'none' to disable any default behavior. Learn more: https://webpack.js.org/configuratiuration/mode/
```

- `mode`配置项
  - `development` : 开发阶段  (不压缩)
  - `production` :  发布阶段 (压缩)
- 配置

```json
mode : 'development'
```



## 3. 指定配置文件 (★★★)

[使用一个配置文件](https://www.webpackjs.com/guides/getting-started/#使用一个配置文件)

> 随着配置越来越多,在脚本里配置就显得麻烦了 所以我们一般常用配置文件

1. 根目录 创建一个配置文件 : `webpack.config.js`

```js
你也可以创建 webpack.config.dev.js(开发阶段用的)  webpack.config.prod.js(发布阶段用的)
```

2. 添加配置 

```js
// 因为 webpack 是基于 node
// 所以在配置文件里面 我们可以直接使用 node 的语法
const path = require('path')

module.exports = {
  // 入口
  entry: path.join(__dirname, './src/index.js'),

  // 出口
  output: {
    // 目录
    path: path.join(__dirname, './dist'),
    filename: 'bundle.js',
  },
  // mode
  mode: 'development',
}
```

3. 修改脚本

```js
"build" : "webpack --config webpack.config.js"
```



## 4. 隔行变色案例

1. 创建 :  `src/index.html`
2. 隔行案例结构  => `ul>li{我是li $}* 10`
3. 安装 `jquery` :  `yarn add jquery`
4. 引入 jquery 和 设置样式  (`ok`)

```js
// 引入
<script src="../node_modules/jquery/dist/jquery.js"></script>

// 设置样式
$('li:odd').css('background', 'red')
$('li:even').css('background', 'blue')
```

5. 使用 es6 的模块化语法 `import`

```js
// 使用ES6的模块化语法
import $ from 'jquery'      # 优点: 不用沿着路径去找      # 浏览器不支持import语法 报错
....
// 语法报错
```

6. 打包 : `yarn build`
7. 引入 `打包后的文件`

```js
<script src='../dist/bundle.js'></script>
```

8. **code记得保存一份**



## 5. 插件: html-webpack-plugin

[html-webpack-plugin](https://www.webpackjs.com/plugins/html-webpack-plugin/)

> 作用 :
>   1. 能够根据指定的模板文件 (index.html),自动生成一个新的 index.html,并且注入到dist文件夹下
>   2. 能够自动引入js文件

1. 安装 : 

```js
yarn add html-webpack-plugin -D
```

2. 配置 :

```js
第一步: 引入模块
const htmlWebpackPlugin = require('html-webpack-plugin')
第二步: 配置
// 插件
plugins: [
  // 使用插件 指定模板
  new htmlWebpackPlugin({
    template: path.join(__dirname, './src/index.html')
  })
]
```



# webpack 处理 非 js 文件

> webpack 只能处理 js 文件,非 js (css.less.图片.字体等) 处理不了, 借助 loader 加载器

## 1. 处理 css文件

1. 创建一个css文件 : `src/css/index.css`

```css
li {
  line-height: 40px;
  height: 40px;
}
```

2. `index.js` 中引入

```js
import './css/index.css'
```

3.  安装 : `yarn add style-loader css-loader@5.0.2 -D`

>  "css-loader": "^5.0.2",  

3.  配置

```js
// loader
module: {
  rules: [
      //1.处理 css
      // 注意点 use执行loader 顺序 从右往左
      // css-loader  :  读取css文件内容,将其转化为一个模块
      // style-loader : 拿到模块, 创建一个style标签,插入页面中
    { test: /\.css$/, use: ['style-loader', 'css-loader'] }
  ]
}
```

5. 重启 `yarn build`



## 2. 处理 less 文件

1. 创建一个 less 文件 : `src/css/index.less`

```css
ul {
  list-style: none;
  li {
    &:hover {
      color: yellow;
        
    }
  }
}
```

2. `index.js` 中引入

```js
import './css/index.less'
```

3.  安装 : `yarn add less-loader less  style-loader css-loader -D`
4. 配置

```js
// loader
module: {
  rules: [
      //2.处理 less
    { test: /\.less$/, use: ['style-loader', 'css-loader','less-loader'] }
  ]
}
```

5. 重启 `npm run dev`



## 3. 处理图片

1. 创建`src/images/`, 引入两种图片 `01.png 和 1.gif`
2. 在`index.css`中设置背景图片

```js
body {
  padding-top: 200px;
  background: url(../images/01.png) no-repeat;
}
```

3. 安装 : `yarn add url-loader -D `

> url-loader (推荐) 和 file-loader 二选一即可

4. 配置 : 

```js
// 处理图片
  { test : /\.(jpg|png|gif)$/, use : ['url-loader'] },
```



# 开发阶段

## 1. webpack-dev-server 

> 作用 : 为使用 webpack 打包提供一个服务器环境
>
> ​	1 自动为我们的项目创建一个服务器
>
> ​	2 自动打开浏览器
>
> ​	3 自动监视文件变化,自动刷新浏览器...

1. 安装 :`yarn add webpack-dev-server -D`
2. 添加一个脚本 `serve`

```js
"scripts": {
  "serve": "webpack serve  --config webpack.config.js"
},
```

3.  运行脚本 :

```js
运行 : yarn serve

结果 : i ｢wdm｣: Compiled successfully.

查看 : Project is running at http://localhost:8080/
```

4. 作用演示 :

```js
# 自动打开浏览器
 devServer: {
    open: true
}
# 自动监视文件变化 + 自动刷新
$('li:even').css('background', 'blue')  ==> 'yellow' 

# 配置端口
 devServer: {
    open: true,
    port :3001
}
```



## 2. hot 热更新 (待定)

```js
"dev": "webpack serve --config webpack.config.js --hot"
```



##3. serve和 build的使用区别

```js
"scripts": {
  "build": "webpack --config webpack.config.js",
  "serve" : "webpack serve --config webpack.config.js"
},
或者
"scripts": {
  "build": "webpack",
  "serve" : "webpack serve"
},
```

- **yarn serve**

```js
开发模式 :  => 开发项目中使用 , 不会打包出实体文件, 打包到内存中, 这样能够及时监视更新
```

- **yarn  build**

```js
发布上线使用
1 执行 : `npm run build` 对项目进行打包,生成dist文件
2 模拟本地服务器 : 安装 : `npm i -g live-server`
3 把dist文件里的内容放到服务器里即可, 直接运行`live-server`
```



# 处理es6语法

> 现在的项目都是使用 ES6 开发的, 但是这些新的 ES6 语法, 并不是所有的浏览器都支持, 所以就需要有一个工具,帮我们转成 es5 语法, 这个就是: babel

- Babel is a JavaScript compiler. ==> babel 是一个 JavaScript 编译器
- webpack 只能处理 import / export 这个 es6 模块化语法 而其他的 js 新语法,应该使用 babel 来处理

>let wodemage = () => console.log('我是马哥')
>
>wodemage()
>
>yarn build 打一个实体包 查看是否转化为function,如果不转,后面会可能不兼容其他浏览器

[babel 英文网](https://babeljs.io/)

[babel中文网](https://www.babeljs.cn/)

[webpack-babel-loader](https://webpack.js.org/loaders/babel-loader/)

1. 安装 : 

```js
yarn add  babel-loader @babel/core @babel/preset-env -D

babel-loader 处理js
@babel/core 核心包
@babel/preset-env  处理es6 789....
```

2. 配置

```js
module: {
  rules: [
    {
      test: /\.js$/,
      exclude: /node_modules/,
      use: {
        loader: 'babel-loader',
        options: {
          presets: ['@babel/preset-env']   // 处理es6-最新   
        }
      }
    }
  ]
}
```

3. 或者 创建 `.babelrc`

```js
{
  "presets": ["env"]
}
```


