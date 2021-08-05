# BOM

> BOM（Browser Object Model）：浏览器对象模型，提供了一套操作浏览器功能的工具。

 ![1592535955541](images\1592535955541.png)



BOM包含的内容很多，但是很多东西都不太常用，在BOM中需要大家掌握的就一个东西，那就是***定时器*** 。

## window对象

```
1. window对象是一个全局对象，也可以说是JavaScript中的顶级对象
2. 像document、alert()、console.log()这些都是window的属性，基本BOM的属性和方法都是window的。
3. 所有定义在全局作用域中的变量、函数都会变成window对象的属性和方法
4. window对象下的属性和方法调用的时候可以省略window
```

### window.onload（掌握）

> window.onload事件会在窗体加载完成后执行，通常我们称之为入口函数。

```javascript
window.onload = function(){
	//里面的代码会在窗体加载完成后执行。
	//窗体加载完成包括文档树的加载、还有图片、文件的加载完成。
}
```

思考：一个页面能不能写两个window.onload?

## 延时器与定时器

### setTimeout

> 延迟一段时间之后再去执行对应的函数，只会执行一次

>  设置延时器

```javascript
// 语法：setTimeOut(callback, time);
// 参数1：回调函数，时间到了就会执行。
// 参数2：延时的时间, 以毫秒计数
// 返回：定时器的id，用于清除
// 示例：
let timerId = setTimeOut(function(){
	//1秒后将执行的代码。
}, 1000);
// 注意: 每一个定时器调用都会产生一个不同的定时器id
```

> 清除延时器

```javascript
// 语法：clearTimeOut(timerId)
// 参数：定时器id
// 示例：
clearTimeOut(timerId); // 清除上面定义的定时器
```

### setInterval

> 每间隔一段时间就去执行对应的函数
>

```
对比延时器: 最大的不同在执行次数上不同  
延时器: 一次  
定时器: 一直执行 除非清除该定时器
```

>  设置定时器

```javascript
// 语法：let intervalID = setInterval(func, delay);
// 参数1：重复执行的函数
// 参数2：每次间隔的毫秒数
// 返回：定时器的id，用于清除
// 示例：
let timer = setInterval(function(){
	//重复执行的代码。
}, 1000);
```

>  清除定时器

```javascript
//语法：clearInterval(intervalID)
//参数：定时器id
//示例：
clearInterval(timer);//清除上面定义的定时器
```

> 暴力解决开启定时器问题

```
当用户暴力点击的时候,会出现开启多个定时器的问题!!

解决: 
	方式1: 触发了定时器之后, 立即禁用对应的触发按钮
	方式2: 在每一次开启定时器之前, 将上一次的定时器清除掉即可	 (注意: 装定时器的变量需要是全局变量) 
```

### 案例

【倒计时】

【短信验证码案例.html】

## location对象

> location对象也是window的一个属性，location其实对应的就是浏览器中的地址栏。

### 常用属性和方法

```javascript
location.href => 可以设置和获取页面的地址
location.reload() => 重载页面
```

【案例：定时跳转.html】

## navigator对象

> window.navigator的一些属性可以获取客户端的一些信息

```javascript
//navigator.userAgent：浏览器版本
```

## history对象

history对象表示页面的历史

```javascript
//后退：
history.back();
history.go(-1);
//前进：
history.forward();
history.go(1);
```





