---
typora-copy-images-to: mdImg
---

# 学习目标

> - [x] 能够完成苏宁rem布局首页
>
> - [x] 能够使用媒体查询完成响应式布局容器
>
>   ```css
>   <!DOCTYPE html>
>   <html lang="en">
>   <head>
>     <meta charset="UTF-8">
>     <meta http-equiv="X-UA-Compatible" content="IE=edge">
>     <meta name="viewport" content="width=device-width, initial-scale=1.0">
>     <title>Document</title>
>     <style>
>       * {
>         margin: 0;
>         padding: 0;
>       }
>   
>       .container {
>         height: 2000px;
>         background-color: pink;
>         margin: 0 auto;
>       }
>   
>       /* 需求： */
>       /* 超小屏幕: 0 - 768px      	版心：100% 	背景颜色：绿色 */
>       @media screen and (min-width:0px) and (max-width:768px) {
>         .container {
>           width: 100%;
>           background-color: green;
>         }
>       }
>   
>       /* 小屏设备: 768px - 992px  	版心：750px 	背景颜色：蓝色 */
>       @media screen and (min-width:768px) and (max-width:992px) {
>         .container {
>           width: 750px;
>           background-color: blue;
>         }
>       }
>   
>       /* 中屏设备: 992px - 1200px 	版心：970px 	背景颜色：黄色 */
>       @media screen and (min-width:992px) and (max-width:1200px) {
>         .container {
>           width: 970px;
>           background-color: yellow;
>         }
>       }
>   
>       /* 大屏设备: 1200 ~  正无穷     版心：1170px   背景颜色：粉色 */
>       @media screen and (min-width:1200px) {
>         .container {
>           width: 1170px;
>           background-color: pink;
>         }
>       }
>     </style>
>   </head>
>   <body>
>     <div class="container"></div>
>   </body>
>   </html>
>   ```
>
>   
>
>
> - [x] 能够使用bootstrap的栅格系统
>
>   ```html
>   栅格系统：
>   .col-取值1-取值2
>   
>   取值1：
>   	1、lg：大屏及以上屏幕生效
>   	2、md：中屏及以上屏幕生效
>   	3、sm：小屏及以上屏幕生效
>   	4、xs：超小屏及以上屏幕生效
>   
>   取值2：份数 1~12
>   ```
>
>   
>
> - [ ] 能够使用bootstrap的基本样式
>
> - [ ] 能够使用bootstrap的响应式工具
>
> - [ ] 能够完成微金所的头部
>
> 。。。。。。



**理解上课的知识点......**



# rem布局的适配方案

> 知道rem布局的原理是：使用rem单位之后，动态根据当前屏幕宽度，自动改变html标签的font-size大小。
>
> 现在我们已经学会了媒体查询的语法了，接下来就要知道到底如何通过rem  + 媒体查询实现具体的rem布局适配



### rem布局适配具体步骤

> rem布局适配的原理说白了，就是根据屏幕的大小，动态的改变html标签的font-size的大小，此时就可以配合媒体查询做到不同屏幕的适配。
>
> 注意点：rem布局进行的是元素**等比例缩放**



**需求：**当在750的设计图（屏幕）中量取div的宽高为400*400，此时要求使用rem适配 `375`、`640`、`750`  的屏幕



#### 适配的步骤

1. **根据设计图尺寸，先把px的单位转换成rem单位（根据设计图屏幕下html的font-size转换）**

   > `标签的px值 = rem单位*html标签的font-size`

   - **rem单位 == 标签的px值 / html标签的font-size**
   - 为了计算方便，一般我们会**针对于设计图的大小**自定义一个html标签的font-size的大小（比如：750设计图自定义50px）

2. **配合媒体查询，当屏幕越小的时候，需要让html标签的font-size的变小（等比例缩放）**

   - 保持比例相同：**比例 == 屏幕宽度 / 当前html标签的font-size**

     > 其实在上一步根据设计图自定义font-size的时候，比例已经确定了，后面直接计算即可

   - 推导：**当前html标签的font-size ==屏幕宽度/比例** 

   

因为要求页面是**等比例缩放**的，所以：

`屏幕1宽度/屏幕1html标签font-size` = `屏幕2宽度/屏幕2html标签font-size`

> 保证屏幕宽度与html标签font-size的比例相同，就可以轻松适配多个屏幕

![rem适配比例关系](mdImg/rem%E9%80%82%E9%85%8D%E6%AF%94%E4%BE%8B%E5%85%B3%E7%B3%BB.png)

```css
/* 1、把px单位通过计算转换成rem单位（除以设计图尺寸屏幕中自定义的font-size：比如50px） */
div {
    width: 8rem;
    height: 8rem;
    background-color: pink;
}

/* ---------------------------------------------------------------------------- */
/* 2、保持比例相同，进行不同屏幕的适配 */
/* 
此时需要适配640px的屏幕的font-size
比例：15
font-size = 屏幕宽度/比例 = 375 / 15 = 25
*/
@media screen and (min-width: 375px) {
    html {
        font-size: 25px;
    }
}

/* 此时需要适配640px的屏幕的font-size
比例：15
屏幕的宽度/font-size =  15
font-size = 屏幕宽度 / 比例 = 640 / 15 = 42.66
*/
@media screen and (min-width:640px) {
    html {
        font-size: 42.66px;
    }
}

/* 此时这是适配750px的屏幕的font-size的设置
比例：屏幕的宽度 / font-size = 750 / 50 = 15
*/
@media screen and (min-width:750px) {
    html {
        font-size: 50px;
    }
}
```

**注意点：为了保证大屏的媒体查询不被覆盖，媒体查询一般从小屏往大了写。**



---

**小结：**

1. 如果设计图大小为 `750`，在设计图中量取的 `px` 尺寸一般如何转换成rem？
   1. `rem = 量取的px / html标签的font-size（50）`
2. 如果设计图大小为 `750` ，此时定义 `html` 标签的 `font-size:50px`，此时等比例缩放的比例为？此时推导出 `640` 屏幕下，定义的 `html` 标签的 `font-size` 为多少？
   1. `比例 = 屏幕宽度 / html标签的font-size = 750 / 50 = 15`
   2. `html标签的font-size = 屏幕宽度 / 比例 = 640 / 15 = 42.66px`

---



# 苏宁移动端首页项目

## 技术选型

- 方案：单独制作移动端页面
- 技术：采用flex + rem布局

## 项目搭建

**项目搭建：**

- 创建 `sn` 目录
- 新建 `index.html` 为网站首页
- 新建 `less/index.less` 为网站首页的less文件
- 复制 `base.less` 文件到 `less` 目录
- 复制 `images` 和 `uploads` 目录到 `jdm` 目录

准备好的目录结构如下：

```
sn
  ├── index.html                # 网站首页
  ├── favicon.ico               # 网站的ico图标
  ├── less
  │   ├── index.less            # 网站首页对应less样式文件
  │   └── base.less             # 网站初始化less样式文件
  ├── images                    # 固定样式类图片
  └── upload                    # 非固定产品类相关图片
```



**注意点：** 一般使用项目组准备好的 `base.less` 文件时，建议阅读文件，防止其他代码对于自己项目的干扰，对项目有整体把控。



## rem布局前注意点

### 视口设置完整

```html
<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
```

### 整体大容器

> 移动端网页如果有最大宽最小宽的限制，会通过整体大容器限制

- 方法一：可以直接把body当做整体大容器

- 方法二：手动设置div盒子作为一个全局容器：container

### 设计图在量取的时候可以直接按照1倍图量取

**之前：**

- 传统浮动+定位+宽度百分比（流式布局）/flex布局中

- 设计图是二倍图，例如：设计图尺寸是750，实际针对的屏幕是375屏幕。

- 不同屏幕下盒子仅是宽度拉长，图片大小都是定死大小不会变化

- 为了保证固定大小的图片始终清晰，我们会/2设置即可。

**现在：**

- rem布局中

- 设计图是二倍图，例如：设计图尺寸是750，实际**针对的屏幕是375屏幕**。

- rem布局的特点是页面中所有元素会跟随屏幕等比例缩放，图片设置的大小会跟随屏幕大小一起缩放。

- 因此针对于**750屏幕设置的1倍**的大小，等屏幕来到针对的**375屏幕下后，图片宽高会自动等比例缩小2倍**，此时同样保证了375屏幕下是缩小2倍的高清大小。



## rem布局首先进行适配工作

**适配的步骤：**

1. 先适配设计图的屏幕大小（比如：750px），并且根据设计图屏幕大小定义一个html标签的font-size的大小（比如：50px），此时屏幕大小与font-size的比例为15
2. 因为是等比例缩放的，所以每一个适配的屏幕大小与font-size的比例都是相同的，所以各个屏幕大小除以比例就能得出font-size的大小

```less
// 苏宁官网中适配了：750 720 540 480 424 414 400 384 375 360 320
.adapter(@width) {
  @media screen and (min-width:@width) {
    html {
      // round(数值)：让这个数值四舍五入
      // round(数值，保留几位小数)
      font-size: round((@width/15),2);
    }
  }
}

.adapter(320px);
.adapter(360px);
.adapter(375px);
.adapter(384px);
.adapter(400px);
.adapter(414px);
.adapter(424px);
.adapter(480px);
.adapter(540px);
.adapter(720px);
.adapter(750px);

// 适配750的屏幕
// 定义设计图屏幕大小html的font-size值为50px，
// 比例为15
// @media screen and (min-width:750px) {
//   html {
//     font-size: 50px;
//   }
// }
```

## px2rem插件的使用

> 在写项目中，每一次都需要手动写式子把px转换成rem比较麻烦，此时可以使用vscode的插件，完成对应的效果

1. 安装插件 **px2rem** 

   ![px2rem的安装](mdImg/px2rem%E7%9A%84%E5%AE%89%E8%A3%85.gif)

2. 每次写数字px之后，会有提示转换成rem，按下键回车即可

3. 插件中默认html的font-size的大小为16px，所以默认是除以16的如果需要更改，需要进行配置（如：设计如是750，html标签的font-size的大小为50px，此时应该除以50）

   > 在设置中搜索px2rem，然后直接修改设置即可，**注意设置完了之后需要重启才能生效！！**

   ![px2rem的设置](mdImg/px2rem%E7%9A%84%E8%AE%BE%E7%BD%AE.gif)



**目前同学们使用rem布局需要做的事情：**

1. 做rem布局的时候：只需要把老师写好的base.less文件引入即可
2. 按照之前学过的布局方式照常去写布局，**注意点：**把之前设置的px单位转换成rem单位。只需要用插件：量取出来的值/50





# 响应式布局（了解）

> 响应式布局（respond layout）是Ethan Marcotte在2010年5月份提出的一个概念，简而言之，就是**一个网页能够兼容多个终端（pc、手机、平板）**

## 什么是响应式布局

**单独制作移动端页面方案（主流）：** 

> 同一个页面需要开发两套不同设备的版本

- pc端需要开发写一套页面，专门给pc端看
- 移动端再开发一套页面，专门给移动端看
  - 传统：浮动+定位+宽度百分比（流式布局）
  - flex布局
  - rem布局

**响应式布局方案（较少）：**

> 同一个页面只需要开发一套网页

- 只需要写一套代码，同时给pc端和移动端看



##### ヾ(๑╹◡╹)ﾉ"  看看响应式网页，分析特点~



## 响应式开发的布局原理

***通过媒体查询，针对于当前不同的屏幕宽度，自动改变页面中盒子的宽度、盒子的显示或隐藏等样式***



## 响应式开发的优缺点

**优点：** 

- 一套网页可以适配多个终端。只需要开发一套网页即可

**缺点： **

- 一个页面需要兼容多个终端，考虑的情况多种，开发效率较慢
- 代码会更多，网页的加载速度较慢



## 响应式开发的现状

> 在国内并不是很流行，国外较多

- 比较**简单的网页**，可以考虑使用响应式，但是复杂的网页考虑的情况会很多，一般不用。

- 如果已经有了一套pc端网页，此时直接再写一套移动端网页即可，此时不需要使用响应式布局（比如：京东、淘宝）

- 如果开发一套网页都没有，是**新建的项目**，此时可以考虑使用响应式，写一套即可兼容多个设备。

  

# 响应式开发的屏幕分类

> 在响应式开发中需要动态根据屏幕的宽度改变样式，但是不可能每变化1px就改变样式。
>
> 在响应式开发中，把各种屏幕宽度分为四大类，开发中只需要考虑这四种屏幕的情况即可

## 设备屏幕的分类

|  分类名称  |    宽度范围     |
| :--------: | :-------------: |
| 超小屏设备 |    0 ~ 768px    |
|  小屏设备  |  768px ~ 992px  |
|  中屏设备  | 992px ~ 1200px  |
|  大屏设备  | 1200px ~ 正无穷 |



![1](mdImg/123-1584795062109.png)



## 使用媒体查询完成响应式布局容器

> 可以通过媒体查询实现不同终端的布局和样式的切换，完成响应式布局。



**需求：**手动设置全局容器.container的响应式适配四种屏幕。

```css
/* 需求： */
/* 超小屏幕: 0 - 768px      	版心：100% 	背景颜色：绿色 */
/* 小屏设备: 768px - 992px  	版心：750px 	背景颜色：蓝色 */
/* 中屏设备: 992px - 1200px 	版心：970px 	背景颜色：黄色 */
/* 大屏设备: 1200 ~  正无穷     版心：1170px   背景颜色：粉色 */
```



**弊端：现在只有一个div，要做一套响应式布局，就需要如此多的代码，非常的麻烦。真正开发中我们会借助一些响应式的框架，比如bootstrap。**



# bootstrap框架

> 在之前使用媒体查询的方式能手动写出响应式的简单效果，但是非常的麻烦，代码非常的多。在实际开发过程中，一般咱们会借助框架来完成响应式的开发，提高效率。
>
> 框架：顾名思义就是一套架构，它有一套比较完整的网页功能解决方案，而且控制权在框架本身，有预设的样式库、组件、插件等等。使用者要按照框架所规定的方式进行开发。
>
> Bootstrap，来自 Twitter，是目前很受欢迎的前端框架。Bootstrap 是基于 HTML、CSS、JAVASCRIPT 的，它简洁灵活，使得 Web 开发更加快捷。



## bootstrap的初体验

>  bootstrap框架现阶段可以简单的当做别人写好的代码。想要用，先去下载下来
>
>  [bootstrap中文网](http://www.bootcss.com/) 

**版本：**

- 2.x.x 停止维护

  > 做了很多兼容性处理，但是代码不够简洁，功能不够完善

- 3.x.x **目前使用较多**

  > 偏向于响应式开发布局，稳定。但是放弃了IE67的兼容，对IE8支持但是界面效果不友好

- 4.x.x 阶段 最新版

**注意点：** Bootstrap中的  **js功能效果依赖于 jQuery** （ 第三方库, 后面有专门的课程讲解）



### 简单使用

> 说白了，bootstrap框架就是别人写好的代码，类似于：需要使用别人写好的css文件，只需要直接引入即可

- **引入文件**：`<link rel="stylesheet" href="bootstrap/css/bootstrap.css"> `

- bootstrap样式文件中有一些简单的**样式初始化**，所以引入之后不用再引入 `base.css` 文件

  > 但是bootstrap并没有将所有样式都重置，所有之后还需要自己手动写代码重置

- **学习bootstrap框架说白了就是学习类（学习每一个类的含义）**

  > 比如：fl——》左浮动、fr——》右浮动、clearfix——》清除浮动

  **以部分按钮为例：**

  |     类名     |             含义             |
  | :----------: | :--------------------------: |
  |     .btn     | 按钮的基础类（按钮必加的类） |
  | .btn-success |           绿色按钮           |
  | .btn-danger  |           红色按钮           |
  | .btn-primary |           深蓝按钮           |
  | .btn-default |           白色按钮           |



## bootstrap的布局容器

>之前给一个盒子设置响应式布局（不同屏幕下版心不同，移动端宽度100%），代码写了很多很麻烦
>
>但是使用bootstrap框架之后就非常方便，框架中响应式的框架已经写好的，使用的之后直接给标签加类即可

### 响应式布局容器（.container）

> **比较常用**

![container容器](mdImg/container容器.gif)

- 设置了该类的盒子，在不同屏幕下有不同的版心，到了移动端宽度为100%（之前写的效果一样）

  > 底层原理：就是之前写的媒体查询

- 设置了该类的盒子，左右有默认15px的padding

  > 写框架的作者觉得内容直接贴两边不好看，就设置了左右15px的padding

### 流式布局容器（.container-fluid）（了解）

> **了解即可**

![container-fluid容器](mdImg/container-fluid容器.gif)

- 设置了该类的盒子，宽度自适应
- 设置了该类的盒子，左右也有默认15px的padding

### 抵消父元素padding的类（.row）（了解）

> bootstrap中的布局容器默认都设置了左右15px的padding。
>
> 如果不需要这个效果，除了**可以通过选择器padding:0;直接覆盖**，还**可以通过.row类去掉**

- 设置了该类的子盒子，会抵消父元素左右15px的padding

  > 底层原理：通过margin为负值实现



## 栅格系统（重点）

> 在bootstrap中会把一行分成12列，通过对应的类名实现每个盒子宽度的动态变化

### 栅格系统的模拟

**需求：** 响应式容器中有两个盒子，只在大屏设备中宽度各占一半一行中显示，其他屏幕占满一行

- 使用之前的方法：浮动 + 宽度百分比 + 媒体查询 可以实现

---

- 其实在bootstrap中，也可以通过类完成以上效果（如：给两个盒子设置`.col-lg-6`）

  > 底层原理也是通过：浮动 + 宽度百分比 + 媒体查询 做到的。



### 栅格系统的介绍

> bootstrap中将一行分成了12份（12份更容易分配盒子的空间）

**底层原理：** **浮动（一行中显示） + 百分比（宽度均分） + 媒体查询（不同屏幕时才生效）**

**比如： **` .col-lg-6` 表示在大屏及以上屏幕生效，盒子宽度为一行的6/12——》50%；浮动在一行中显示

**语法：**

##### `.col-取值1-取值2`

| 取值1 |                 效果                 |
| :---: | :----------------------------------: |
|  lg   |         大屏及以上屏幕时生效         |
|  md   |         中屏及以上屏幕时生效         |
|  sm   |         小屏及以上屏幕时生效         |
|  xs   | 超小屏及以上屏幕生效（所有屏幕生效） |

**取值2：** 份数（0~12）

> 表示在一行中的宽度占几份

### 栅格系统的练习

```html
<!-- 
    需求:  响应式布局容器中一共有 12 个div
      如果是大屏幕设备, 每行放 6 个 div,  共两行
      如果是中屏设备,   每行放 4 个 div,  共三行
      如果是小屏设备,   每行放 3 个 div,  共四行
      如果是超小屏设备, 每行放 2 个 div,  共六行

    .col-取值1-取值2
      lg：大屏及以上生效
      md：中屏及以上生效
      sm：小屏及以上生效
      xs：所有屏幕都生效
  -->
```

##### ヾ(๑╹◡╹)ﾉ" 写微金所新手体验模块的响应式效果



## bootstrap全局样式阅读（了解）

> [bootstrap全局样式](https://v3.bootcss.com/css/)

**排版：对齐**

> 底层原理：就是一个text-align：left/center/right

|     类名     |     效果     |
| :----------: | :----------: |
|  .text-left  |  文本左对齐  |
| .text-center | 文本居中对齐 |
| .text-right  |  文本右对齐  |

**表格：基本（了解）**

|      类名      |                  效果                  |
| :------------: | :------------------------------------: |
|     .table     | 表格的基本样式（配合thead和tbody使用） |
| .table-striped |                隔行变色                |

**按钮：颜色**

> 按钮需要加上基本类 `.btn`

|     类名     |   效果   |
| :----------: | :------: |
| .btn-danger  | 红色按钮 |
| .btn-success | 绿色按钮 |
| .btn-primary | 深蓝按钮 |
| .btn-default | 白色按钮 |
|  .btn-info   | 浅蓝按钮 |
| .btn-warning | 黄色按钮 |
|  .btn-link   | 链接按钮 |

**按钮：尺寸**

> 按钮默认是中按钮

|  类名   |   效果   |
| :-----: | :------: |
| .btn-lg |  大按钮  |
| .btn-sm |  小按钮  |
| .btn-xs | 超小按钮 |

## 响应式工具介绍

> 在响应式布局中，有时候会设置不同屏幕下元素的显示或者隐藏

**需求：** 一个盒子大屏、中屏显示，小屏、超小屏隐藏

- 自己通过媒体查询实现

---

- 使用bootstrap中预定的.hidden相关类实现

**代码：**

> bootstrap中预定了一些类，可以控制盒子的显示或者隐藏

|    类名    |      效果      |
| :--------: | :------------: |
|  .hidden   | 所有屏幕都隐藏 |
| .hidden-xs | 只在超小屏隐藏 |
| .hidden-sm |  只在小屏隐藏  |
| .hidden-md |  只在中屏隐藏  |
| .hidden-lg |  只在大屏隐藏  |

## 组件介绍（了解）

> 组件比全局样式会多出一些功能出来，但是注意这些功能需要配合js文件一起使用

**组件：字体图标**

> 在bootstrap内部，内置了字体图标，只需要直接复制粘贴类名即可

**比如：**

```html
<span class="glyphicon glyphicon-heart"></span>
```

**组件：导航条**

> bootstrap中已经写好导航条的代码，使用的时候直接复制粘贴即可

```html
<nav class="navbar navbar-default">
  <div class="container-fluid">
    <!-- Brand and toggle get grouped for better mobile display -->
    <div class="navbar-header">
      <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
        <span class="sr-only">Toggle navigation</span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
      </button>
      <a class="navbar-brand" href="#">Brand</a>
    </div>

    <!-- Collect the nav links, forms, and other content for toggling -->
    <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
      <ul class="nav navbar-nav">
        <li class="active"><a href="#">Link <span class="sr-only">(current)</span></a></li>
        <li><a href="#">Link</a></li>
        <li class="dropdown">
          <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false">Dropdown <span class="caret"></span></a>
          <ul class="dropdown-menu">
            <li><a href="#">Action</a></li>
            <li><a href="#">Another action</a></li>
            <li><a href="#">Something else here</a></li>
            <li role="separator" class="divider"></li>
            <li><a href="#">Separated link</a></li>
            <li role="separator" class="divider"></li>
            <li><a href="#">One more separated link</a></li>
          </ul>
        </li>
      </ul>
      <form class="navbar-form navbar-left">
        <div class="form-group">
          <input type="text" class="form-control" placeholder="Search">
        </div>
        <button type="submit" class="btn btn-default">Submit</button>
      </form>
      <ul class="nav navbar-nav navbar-right">
        <li><a href="#">Link</a></li>
        <li class="dropdown">
          <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false">Dropdown <span class="caret"></span></a>
          <ul class="dropdown-menu">
            <li><a href="#">Action</a></li>
            <li><a href="#">Another action</a></li>
            <li><a href="#">Something else here</a></li>
            <li role="separator" class="divider"></li>
            <li><a href="#">Separated link</a></li>
          </ul>
        </li>
      </ul>
    </div><!-- /.navbar-collapse -->
  </div><!-- /.container-fluid -->
</nav>
```

**注意点：**

- 如果需要实现组件中如导航条的功能，需要引入bootstrap中的js文件才行
- bootstrap中的js需要依赖与jquery这个js文件的，所以需要一起引入jquery这个js文件才能生效！
- js文件通过script的src属性引入

```html
<script src="./jquery/jquery-1.12.4.js"></script>
<script src="./bootstrap/js/bootstrap.js"></script>
```



# 微金所项目

## 技术选型

- 方案：响应式布局
- 技术：自己的css + 配合bootstrap框架

## 项目搭建

**项目搭建：**

- 创建 `wjs` 目录
- 新建 `index.html` 为网站首页
- 新建 `css/index.css` 为网站首页的css文件

准备好的目录结构如下：

```
wjs
  ├── index.html                # 网站首页
  ├── favicon.ico               # 网站的ico图标
  ├── css
  │   └── index.css             # 网站首页对应样式文件
  ├── images                    # 固定样式类图片
  ├── fonts                     # 字体图标类文件
  └── lib                       # 需要使用到的其他代码库、框架类代码
 			 ├── jquery               # 后期要学习的js库
 			 └── bootstrap            # 项目中需要使用的框架
 			 
```



## 微金所模块划分

![](mdImg/微金所模块划分.png)



## 响应式项目的思路

1. 先看有没有响应式版心（版心会不会跟随屏幕大小变化而变化）

   > 如果有响应式版心，设置 `.container` 即可

2. 在响应式版心中分配每一个盒子的空间

   > 利用栅格系统进行空间的分配

3. 设置整体大模块是否有显示与隐藏，直接给大模块设置hidden相关属性即可



## bootstrap导航条的改写（不做要求）

> 在bootstrap文档的组件中，有写好的导航条的代码，使用的时候直接复制改写即可。
>
> 因为现在还没学js等相关内容，所以对于以下改写的内容同学们可以直接复制改好的代码，现在学习的重点在于之后样式的覆盖（如果能理解是一种进阶）

**改写前先把不需要的属性和标签删除：**

> 以下属性和标签，在改写过程中可以直接删除，因为是给盲人设备使用的，删除之后便于理解结构（不删也可以）

- `aria-` 开头的属性，该属性可以直接删除
- `role` 属性，该属性可以直接删除
- 有 `sr-only` 类的标签，该标签可以直接删除 

```html
<nav class="navbar navbar-default">
    <!-- 有版心的布局容器 -->
    <div class="container">
        <!-- 小屏设备显示的左边大标题和右边切换按钮的组合 -->
        <div class="navbar-header">
            <!-- 小屏下右边的按钮 -->
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse"
                    data-target="#bs-example-navbar-collapse-1">
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <!-- 左边的大标题 -->
            <a class="navbar-brand" href="#">左边的大标题</a>
        </div>
<!--------------------------------------------------------------------------------------->
        <!-- 用于切换显示内容的结构 -->
        <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
            <!-- 左侧的导航 -->
            <ul class="nav navbar-nav">
                <li class="active"><a href="#">我要投资</a></li>
                <li><a href="#">我要借贷</a></li>
                <li><a href="#">平台介绍</a></li>
                <li><a href="#">新手专区</a></li>
                <li><a href="#">最新动态</a></li>
                <li><a href="#">微平台</a></li>
            </ul>
            <!-- 右侧的导航 -->
            <ul class="nav navbar-nav navbar-right">
                <li><a href="#">个人中心</a></li>
            </ul>
        </div>
    </div>
</nav>
```