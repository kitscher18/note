# npm - Node包管理工具 (★★★★★)

## npm的基本概念

- node package manager
- [npm官网](https://npmjs.com)
- [npm中文文档](https://www.npmjs.com.cn/)

```html
1. npm 是node的包管理工具，
```

- 作用：通过`npm`来快速安装开发中使用的包
- npm不需要安装，只要安装了node，就自带了`npm`

## npm基本使用

### 初始化包配置文件

```javascript
npm init;    //这个命令用于初始化一个包，创建一个package.json文件，我们的项目都应该先执行npm init
             //注意 :  不准使用中文
npm init -y;  //快速的初始化一个包， 
              //注意 : -y:yes
```

### 安装包

```javascript
npm install 包名;  //安装指定的包名的最新版本到项目中
                   //注意 :  包名 一定都是小写的
npm install 包名@版本号;  //安装指定包的指定版本

npm i 包名； //简写  等同于 (npm i 包名 --save 和 npm i 包名 -S )

演示 : npm输入  element-ui 和 vue-quill-editor编辑器 
```

### 卸载包

```javascript
npm uninstall 包名;  //卸载已经安装的包

npm un 包名;  // 简写
```

## package.json文件

package.json文件，包（项目）描述文件，用来管理组织一个包（项目），它是一个纯JSON格式的。

- 作用：描述当前项目（包）的信息，描述当前包（项目）的依赖项
- 如何生成：`npm init`或者`npm init -y`
- 作用
  - 作为一个标准的包，必须要有`package.json`文件进行描述
  - 一个项目的node_modules目录通常都会很大，不用拷贝node_modules目录，可以通过package.json文件配合`npm i`直接安装项目所有的依赖项
- 描述内容

```json
{
  "name": "03-npm",  //描述了包的名字，不能有中文
  "version": "1.0.0",  //描述了包的的版本信息， x.y.z  如果只是修复bug，需要更新Z位。如果是新增了功能，但是向下兼容，需要更新Y位。如果有大变动，向下不兼容，需要更新X位。
  "description": "", //包的描述信息
  "main": "index.js", //入口文件（模块化加载规则的时候详细的讲）
  "scripts": {  //配置一些脚本，在vue的时候会用到，现在体会不到
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],  //关键字（方便搜索）
  "author": "",  //作者的信息
  "license": "ISC",  //许可证，开源协议
  "dependencies": {   //重要，项目的依赖， 方便代码的共享  通过 npm i 可以直接安装所有的依赖项
    "bootstrap": "^3.3.7",
    "jquery": "^3.3.1"
  }
}
```

**注意：一个合法的package.json，必须要有name和version两个属性** 



## 本地安装和全局安装

有两种方式用来安装 npm 包：本地安装和全局安装。选用哪种方式来安装，取决于你如何使用这个包。 

  1. 本地安装
     - 说明 : 想把我们用的包,安装到当前本地项目中使用 
     - 比如 : npm i jquery , npm i mime 
     - 要求 : 执行的安装命令(npm i mime) 位置,必须在当前项目下执行 
     - 包位置 :  本地安装的包 => 当前项目下的 node_modules
     - 使用包 :  `const mime = require('mime')  , const $ = require('jquery')`

  2. 全局安装
     - 说明 : 想把一个包/库,当成一个`工具`来使用, 就采用全局安装 
     - 比如 : `npm i mime -g` (global)  , `npm i  -g live-server `
     - 要求 : 可以在任意地方, 都可以执行命令
     - 包位置 : `C:\Users\用户名\AppData\Roaming\npm`
     - 使用包 :  在`终端命名行`里使用, 不能在代码里     `mime .txt   /  mime.jpg`

```javascript
// 全局安装,会把npm包安装到C:\Users\用户名\AppData\Roaming\npm目录下，作为命令行工具使用
npm i -g 包名;

//本地安装，会把npm包安装到当前项目的node_modules文件中，作为项目的依赖
npm i 包名;  

// 本地
const mime = require('mime')

console.log(mime.getType('.aaa.txt'));
```



##清除缓存 

- 网络问题 ERR! : network 

 ```js
// npm ERR! A complete log of this run can be found in:

// npm ERR!     C:\Users\ma250\AppData\Roaming\npm-cache_logs\2019-03-11T08_54_30_775Z-debug.log
 ```

- 删除缓存(清缓存)
  - 方式1 : `npm cache clean -f` (--force)
  - 方式2 : `C:/users/用户名/AppData/Roaming/npm-cache`  里面可以全部删除掉

 

## npm下载加速

### 设置淘宝镜像

```js
- 查询当前镜像
# npm get registry

- 设置为淘宝镜像
# npm config set registry https://registry.npm.taobao.org/
```

## nodemon 自动重启

> supervisor

- 作用：监视到js文件修改后，自动重启node程序

- 安装：`npm i nodemon -g`

- 使用：`nodemon app.js` 运行node程序

- > 演示 : 没有req和res参数,没有end,没有值,ok => 汉字 => 设置响应头

  

## 其他的包管理工具

- yarn

  - 全局安装 `npm i yarn -g`
  - 使用 : 
    - `yarn add XXX `
    - `yarn global add XXX`
    - `yarn remove XXX`

- cnpm

  - 全局安装 `npm i cnpm -g`
  - 使用 : 
    - `cnpm i XXX -S `
    - `cnpm i XXX -g`
    - `cnpm un XXX`

  