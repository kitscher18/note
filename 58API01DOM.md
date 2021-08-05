# WebApi基本概念

## 为什么要学习WebApi?

> 什么是webApi?

```
就是JS提供的一套操作页面和浏览器的一套方法 
	
web: 网页 
api: 应用程序接口 (输入输出 => 参数和返回值 Math.max())
```

> 为什么学习webApi呢?

```
之前学习JS都是学习语法, 没有做用JS作出真正好看的效果 (99乘法表)
学了webApi之后 => 可以操作网页上的html和css (轮播图, 下拉菜单等等用用户的交互效果)
```

> webApi的组成

```
DOM和BOM

DOM => Document Object Model   操作页面的一套工具 (对象)
BOM => Browser Object Model   操作浏览器的一套工具 (对象)
```

问:

1. webApi的概念 ? 作用 ?
2. webApi的组成 ?

# DOM-文档对象模型(重要)

## DOM基本概念

问: 如何通过JS 修改 html呢 ? 

```jsx
<!--下面的div是否可以抽象成一个JS对象 ? -->
<div class="box" id="box" style="width: 200px; height: 200px">
  我是一个盒子
</div>

// 使用JS描述html标签
let obj = {
  tagName: 'div',
  className: 'box',
  id: 'box',
  style: {
    width: '200px',
    height: '200px',
    backgroundColor: 'red'
  },
  innerText: '我是一个盒子'
}
// 改语法
obj.style.width = '500px'
```

> DOM（Document Object Model）文档对象模型，是`W3C组织`推荐的一套用于处理`HTML`的标准`编程接口`。
>

tip: JS本质上并不是直接操作html, 而是浏览器会将网页当中的标签抽象成一个个的JS对象, 标签的一些属性和内容都可以在这个对象里面找到, 通过修改这个对象达到操作html的目的, 因为这个对象一旦修改,与之对应的标签也会修改



![](imgs/1.png)

>  术语介绍

```
document => 代表整个网页
element => 代表网页的一个个的标签  
```

问:

1. 什么是DOM对象 ? 标签和DOM对象之间有什么关系 ?
2. 修改DOM对象是否会影响到标签呢 ?

## DOM初体验

> 需求:  通过JS将图片换掉

> 思路:  修改img的src的取值即可

```javascript
1. 先获取到对应的img对象 (在DOM中要操作一个dom对象,首先要获取到这个dom对象 类似于css,要操作一个元素,首先需要选择它)
2. 修改该对象的src属性的取值即可
```

> API介绍

```html
语法: document.getElementById(id名称) => 对象里面的一个函数 可以通过id去获取到页面里面的id元素
作用: 通过id获取到对应的DOM对象
参数: id
返回值: 一个DOM对象
说明: 了解即可. 实际工作中很少使用
```

> 小知识点说明

1. 通过`console.log()`打印DOM对象 会以标签的形式展示出来
2. 通过`console.dir()`打印DOM对象 会以对象的形式展示出来
3. html标签里面的属性在DOM对象里面都能找到

问题:

1. 如何通过id获取元素 ?
2. 如何打印出DOM对象 ? 而不是标签 ?

#  JS获取元素(DOM对象)的方式

> 操作任何标签的第一件事情就是获取到这个标签对应的DOM对象! 使用id获取元素的方式过于单一, 所以JS引入了类似于css选择元素的方式 分为两种

```js
document.querySelector('css选择器') // 获取满足条件的第一个 => 返回的直接是DOM对象
document.querySelectorAll('css选择器'); // 获取满足条件的所有 => 返回的是一个伪数组
```

### document.querySelector('css选择器')

> 获取的结果永远只有一个值(第一个满足条件的值)

```js
// 参数: css选择器  标签/类/id/后代/子代/属性选择器/结构伪类选择器
// 返回值: 满足条件的第一个元素  个数永远是一个 可以直接操作
document.querySelector('css选择器')

// 标签选择器
let box = document.querySelector('div');
// 类选择器
let box = document.querySelector('.box');
// id选择器
let box = document.querySelector('#box');
// 后代选择器
let box = document.querySelector('body #box');
// 子代选择器
let box = document.querySelector('body > #box');
// 属性选择器
let ipt = document.querySelector('input[type=text]');
// 结构伪类选择器
let box = document.querySelector('.box:nth-child(2)');
console.log(box);
```

**特点**: **获取的结果永远只有一个值(第一个满足条件的值)**, 可以直接进行操作,弊端在于**无法实现批量操作**

提问:

1. 什么时候需要使用 document.querySelector ?
2. document.querySelector方法的参数可以是什么 ?
3. document.querySelector返回的是什么 ? 是否可以直接操作 ?

### document.querySelectorAll('css选择器')

> 但凡是涉及到批量操作的, 需要使用到下面这个API => 见物料2

```jsx
// 参数: css选择器   标签/类/id/后代/子代/属性选择器
// 返回值: 满足条件的伪数组集合 即使只有一个也是集合 必须通过下标操作
document.querySelectorAll('css选择器');
```

**特点**: 获取的结果一定是一个**伪数组集合**, 即使只有一个值也是数组集合. **所以不能直接操作,需要使用下标单独操作** 

===重要的话说三遍===: document.querySelectorAll返回的是数组集合,必须通过下标操作里面的DOM对象

简单练习: 将所有的div的title全部替换 ?

问题:

1. 什么时候需要使用document.querySelectorAll ?
2. document.querySelectorAll方法和document.querySelector的区别 ?
3. document.querySelectorAll返回的是什么 ? 是否可以直接操作 ?

> 选择元素的练习

```html
<div class="the_div">
  <p>我是p标签</p>
  <h3>我是h3</h3>
  <h3>我是h3</h3>
  <h3>我是h3</h3>
  <a id="link" href="#">我是一个a标签</a>
  <ul>
    <li>北京</li>
    <li>天津</li>
    <li>东京</li>
  </ul>
  <ul class="my_li">
    <li title="li元素">小明</li>
    <li title="li元素">小红</li>
    <li title="li元素">小蓝</li>
  </ul>
</div>
```

需求:

1. 练习1: 通过id获取到a标签 (可以自己给a添加id)
2. 练习2: 修改a标签的href属性为 'http://www.itcast.cn'
3. 练习3: 通过标签名 获取到所有h3标签
4. 练习4: 通过querySelectorAll() 获取所有的li标签
5. 练习5: 通过querySelectorAll() 获取所有小字辈开头的名字的li标签
6. 练习6: 将小字辈li的title属性都修改成 "小字辈的li元素"

> 小知识补充

`body`,`html`,`head` 等特殊标签可以直接通过`document`获取到

# 注册事件

> 需求:  让用户 "点击" 一下按钮,然后完成换图片的交互效果 (点击按钮换属性)

提取模型 => 当用户在`某个元素`上触发了`某种行为`之后在`执行的事情`

>  事件的三要素

1. 事件源: 用户在`某个元素` (按钮)
2. 事件类型: `某种行为` (click)
3. 事件处理函数: 触发事件`执行的事情` (修改图片的属性)

> 基本语法

```js
// on... 在什么的时候  在点击的时候
事件源.on事件名 = 事件处理函数
```

> 基本思路

1. 获取事件源
2. 给事件源绑定点击事件
3. 在事件处理函数里面将图片的src属性替换掉

> 细节说明

1. 事件处理函数不会主动执行,是当用户点击了才会执行,用户点击一次就会重新执行一次
2. JS解析器会将点击事件交给浏览器去监听, 并且自己直接执行下面的代码 (后面的代码先执行),等用户执行点击的操作的时候浏览器会通知JS解析器,然后JS解析器再去执行事件函数里面的代码

提问:

1. 什么时候会使用到事件 ?
2. 事件的三要素分别是什么 ?
3. 事件处理函数什么时候执行 ? 正常来说, 事件后面的代码先于事件执行还是按顺序执行 ?

> 练习 => 见物料4

```html
<ul class="my-ul">
    <li>我是1</li>
    <li>我是2</li>
    <li>我是3</li>
    <li>我是4</li>
    <li>我是5</li>
    <li>我是6</li>
    <li>我是7</li>
    <li>我是8</li>
    <li>我是9</li>
    <li>我是10</li>
</ul>
```

1. 点击第一个li标签, 弹出你好

2. 点击一组li，点击哪个li都可以弹出你好

3. 给一组li添加点击事件，点击哪个li打印哪个li的索引值

**易错点:** for循环里面的循环变量初始值必须使用let声明, 否则无法在事件处理函数中获取到正确的索引值

# 元素属性操作

> 如果可以修改标签的属性,就可以实现很多操作,比如换图片,换类名,换表单的取值,或者换文本内容等

> 操作标签元素的属性,本质上就是操作DOM对象的属性, DOM对象的属性一旦发生变化, 会自动反应到标签上

```
获取属性值: DOM对象.属性
设置属性值: DOM对象.属性 = 新值
```

> 常用属性介绍 => 见物料5

```
href、title、src、className(标签class写法)、innerText 、innerHTML
```

> 注意点

1. class是ES6的一个关键字,所以早期为了避免冲突, 使用className代替class
   1. className 是采用新值换旧值的做法, 所以实现增加或者删除类名比较麻烦
   2. ES6: classList对象 => add(新增的类名) , remove(删除的类名)	
2. 修改元素的内容有两个属性可以完成

  1. innerHTML

  2. innerText	


```text
共同点: 都可以获取和修改标签内容
不同点: 当内容含有标签的时候
 	innerText: 不会去识别内部的标签, 获取:只会获取内容 设置:不会解析标签,将标签原样显示在页面上
 	innerHTML: 会去识别内容里面的标签, 获取:获取标签 + 内容 设置: 会将标签解析显示在页面上
```

**使用结论**: 实际工作中对于从用户那边得到的数据都使用innerText, 因为更加安全, 对于innerHTML, 如果能够明确数据的来源,使用较为方便.

提问:

1. 什么情况下需要修改标签属性 ?
2. 修改DOM对象的属性是否会影响到标签 ?
3. innerText和innerHTML的区别 ? 

> 练习

1. 点击p标签, 修改p标签里的内容
2. 点击按钮, 修改img标签上的图片
3. 点击按钮, 修改div的class值, class(将矩形变成圆形)



# 综合案例

## 美女相册案例

```
思路：
	1. 获取标签
	2. 批量注册点击事件
	3. 设计图片数组
	4. 图片数组的下标和点击的元素的下标吻合进行关联
```

## 美女画廊(作业)

```
思路:
	1. 获取标签
	2. 批量注册点击事件
	3. 设计图片和文本数据数组
	4. 使用点击的下标和数组的下标吻合进行关联
```

# 表单属性

> 表单的常用属性: `type`,` value`,`disabled`, `checked`,`selected`

细节说明:

1. 如`type`,`value`这些属性与正常的标签属性操作没有什么不同, 都是可以通过点语法直接获取或者赋值的.
2. 但是`disabled`, `checked`, `selected` 属性稍微有些不同, 这些属性在html上只要添加了就有效果,没添加就没有效果类似于这样的属性.那么在DOM对象中的控制使用布尔类型控制.`true`则表示设置了,`false`则表示取消了	

> 练习

1. 点击全选按钮, 来控制一个多选框, 选中和未选中切换

2. 在输入框输入你的名字, 然后点击按钮, 弹出你输入的名字

3. 有3个按钮, 上面分别有名字, 点击以后, 把名字赋予到一个输入框中显示  

   => 在事件处理函数中, 可以使用`this`代表当前的事件源

## 综合练习

### 表单全选

> 需求1: 点击全选按钮 让tbody的input和全选按钮的checked一样

> 需求2: 给tbody的input批量绑定点击事件,当里面所有的input的checked结果都是true的情况下,让全选按钮的checked结果也是true 否则就是false

###  购物车

```
需求: 点击+, 增加数量, 点击-, 减少数量, 当数量为1时, 禁用-号
```



