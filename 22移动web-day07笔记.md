---
typora-copy-images-to: mdImg
---

# 学习目标

> - [x] 能够使用bootstrap的基本样式
>
>   ```css
>   响应式布局容器：.container
>   
>   栅格系统：.col-取值1-取值2
>   取值1：
>   	1、lg：大屏及以上屏幕生效
>   	2、md：中屏及以上屏幕生效
>   	3、sm：小屏及以上屏幕生效
>   	4、xs：超小屏及以上屏幕生效
>   
>   按钮的基础类：.btn
>   
>   按钮的颜色类：
>   	1、红色：.btn-danger
>   	2、绿色：.btn-success
>   	...
>   
>   按钮的大小类：
>   	1、大按钮：.btn-lg
>   	2、中按钮：默认就是中按钮
>   	3、小按钮：.btn-sm
>   	4、超小按钮：.btn-xs
>   ```
>
>   
>
>
> - [x] 能够使用bootstrap的响应式工具
>
>   ```css
>   hidden相关的类：
>   
>   .hidden：所有屏幕都隐藏
>   .hidden-lg：只在大屏中隐藏
>   .hidden-md：只在中屏中隐藏
>   .hidden-sm：只在小屏中隐藏
>   .hidden-xs：只在超小屏中隐藏
>   ```
>
>   
>
> - [x] 能够完成微金所的头部
>
> 。。。。。。



**理解上课的知识点......**



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

![container容器](mdImg/container%E5%AE%B9%E5%99%A8.gif)

- 设置了该类的盒子，在不同屏幕下有不同的版心，到了移动端宽度为100%（之前写的效果一样）

  > 底层原理：就是之前写的媒体查询

- 设置了该类的盒子，左右有默认15px的padding

  > 写框架的作者觉得内容直接贴两边不好看，就设置了左右15px的padding

### 流式布局容器（.container-fluid）（了解）

> **了解即可**

![container-fluid容器](mdImg/container-fluid%E5%AE%B9%E5%99%A8.gif)

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



## 修改框架样式的方法

1. 由外往内，先找当前效果和设计图的不同
2. 用调试工具摸需要修改的元素
3. 摸到对应的元素之后，把调试工具中显示的选择器复制到代码中，前面加上当前模块的类选择器增加权重
4. 在选择器设置需要的样式进行覆盖即可



# 拓展：vw单位

## vw单位的介绍

如果需要让元素跟随屏幕进行等比例缩放：

**之前：rem单位**

- 把元素的px单位转换成rem单位

  > rem代表的px值 === html标签的font-size变化

- html标签的font-size不会自己变化，程序员需要手动通过媒体查询等方法设置不同屏幕下：html标签的font-size

  > 适配的代码比较麻烦，但也是可以复用的

  ```css
  @media screen and (min-width: 320px) {
    html {
      font-size: 21.33333333px;
    }
  }
  @media screen and (min-width: 360px) {
    html {
      font-size: 24px;
    }
  }
  @media screen and (min-width: 375px) {
    html {
      font-size: 25px;
    }
  }
  @media screen and (min-width: 384px) {
    html {
      font-size: 25.6px;
    }
  }
  @media screen and (min-width: 400px) {
    html {
      font-size: 26.66666667px;
    }
  }
  @media screen and (min-width: 414px) {
    html {
      font-size: 27.6px;
    }
  }
  @media screen and (min-width: 424px) {
    html {
      font-size: 28.26666667px;
    }
  }
  @media screen and (min-width: 480px) {
    html {
      font-size: 32px;
    }
  }
  @media screen and (min-width: 540px) {
    html {
      font-size: 36px;
    }
  }
  @media screen and (min-width: 640px) {
    html {
      font-size: 42.66666667px;
    }
  }
  @media screen and (min-width: 720px) {
    html {
      font-size: 48px;
    }
  }
  @media screen and (min-width: 750px) {
    html {
      font-size: 50px;
    }
  }
  ```

  

**现在：vw单位**

- 把元素的px单位转换成vw单位

- 完事：浏览器会自动改变vw所代表的px值

  > 不用写适配的代码



**特点：**

**vw：**1vw等于视口宽度的1%。

**vh：**1vh等于视口高度的1%



## vw单位在项目中使用的逻辑

**用rem布局的逻辑理解vw单位：以750px设计图为例**

- 把量取的px值转换成vw单位

  - **之前：rem**

    - 把px转换成rem

      ```html
      px = rem * html标签的font-size
      --------------------------------
      rem = px / html标签的font-size（自定义）
          = px / 50
      ```

  - **现在：vw：表示视口宽度的1%**

    - 把px转换成vw

      ```html
      px = vw * 视口宽度的1%
      ------------------------
      vw = px / 视口宽度的1%
         = px / 7.5
      ```

- **比rem的优点：**

  - 不用写适配的代码了
  - 用插件写起来非常方便



## vw在项目中使用的步骤（贼方便）

> 只需要转换了单位即可，不需要写媒体查询写适配的代码，浏览器会自动完成

**需求：**当在750的设计图（屏幕）中量取div的宽高为450*450，此时要求使用vw适配 `375`、`640`、`750`  的屏幕

1. 安装 `px2vw`插件

   ![image-20201210233404677](mdImg/image-20201210233404677.png)

2. 选择vscode中的设置，搜索 `px2vw`  ，设置当前量取的设计图的尺寸（默认是常见的情况750）

   ![image-20201210233547112](mdImg/image-20201210233547112.png)

3. 写尺寸的时候，使用插件把px转换成 `xx vw` 即可

   ![image-20201210234055312](mdImg/image-20201210234055312.png)





# 移动web注意点总复习

> 网页布局需要练习才能提高熟练度，除此之外，对于基础知识点的学习需要扎实！

- 转换有哪些转换效果？取值分别是什么？

  - 缩放：`scale`
  - 平移：`translate`
  - 旋转：`rotate`

- 转换中 `transform:translateX(100%);`  中的百分比相对于谁？

  - 平移的百分比相对于自己的宽度

- `perspective`  和 `transform-style: preserve-3d` 的区别？

  - `perspective` 
    - 视距：让子元素有近大远小的视觉效果，本身默认还是2d平面元素
    - 给谁设置？父元素设置
  - `transform-style: preserve-3d`
    - 让当前元素变成一个真正的3d立体元素
    - 给谁设置？谁需要变成真正的3d立体元素，就给谁设置！

- 使用动画的步骤分几步？

  ```css
  1、声明、定义动画
  @keyframes 动画名 {
      from {
          开始状态的样式
      }
      
      to {
          结束状态的样式
      }
  }
  
  2、调用动画
  animation: 动画名 动画的持续时间;
  ```

  

- flex布局的相关属性有哪些？

  ```css
  给父元素设置的相关样式----------------------------------------------------
  1、弹性盒子：display:flex
  
  2、主轴方向：flex-direction
  	取值：
  		1、row：水平向右
  		2、row-reverse：水平向左
  		3、column：垂直向下
  		4、column-reverse：垂直向上
  
  3、主轴对齐方式：justify-content
  	取值：
          1、flex-start：向开始方向对齐
          2、flex-end：向结束方向对齐
          3、center：居中对齐
          4、space-around：空白环绕
          5、space-between：空白只在盒子之间显示
          6、space-evenly：空白均分
  
  4、单行侧轴对齐方式：align-items
  	取值：
          1、flex-start：向开始方向对齐
          2、flex-end：向结束方向对齐
          3、center：居中对齐
  		4、stretch：拉伸显示（此时需要保证侧轴方向没有设置宽度/高度）
  
  5、多行侧轴对齐方式：align-content
  	取值：
  		1、flex-start：向开始方向对齐
          2、flex-end：向结束方向对齐
          3、center：居中对齐
  		4、stretch：拉伸显示（此时需要保证侧轴方向没有设置宽度/高度）
  		5、space-around：空白环绕
          6、space-between：空白只在盒子之间显示
          7、space-evenly：空白均分
  
  6、设置换行：flex-wrap
  	取值：
  		1、nowrap：不换行
  		2、wrap：当一行放不下时，此时才会换行
  
  
  给子元素设置的相关样式----------------------------------------------------
  
  1、分配子元素的空间：flex
  	取值：份数
  
  2、子元素的排序：order
  	取值：数字，数字越小，排名越靠前，默认是0
  
  3、单个子元素的侧轴对齐方式：align-self
  	取值：
  		1、flex-start：向开始方向对齐
          2、flex-end：向结束方向对齐
          3、center：居中对齐
  		4、stretch：拉伸显示（此时需要保证侧轴方向没有设置宽度/高度）	
  ```

  

- 圣杯布局用flex布局如何实现？

  - 左宽度固定：`width：xxxpx`
  - 中间宽度自适应：`flex：1`
  - 右宽度固定：`width：xxxpx`

- less中声明变量和函数的语法分别是什么？

  ```less
  声明变量：
  @变量名:变量值;
  
  声明函数：
  .函数名() { 重复的代码 }
  ```

- `em` 和 `rem` 的区别是什么？

  - `em`：1em = 当前标签的font-size
  - `rem`：1rem = html标签的font-size

- 媒体查询的语法和条件分别是什么？

  ```css
  媒体查询的语法：
  @media screen and (条件) {
      选择器
  }
  
  条件：
  	1、min-width：样式生效的最小宽——》当屏幕宽度大于或等于该宽度时，此时样式生效
  	2、max-width：样式生效的最大宽——》当屏幕宽度小于或等于该宽度时，此时样式生效
  	3、width：样式生效的宽度——》当屏幕宽度正好等于该宽度时，此时样式生效
  ```

  

- bootstrap中布局容器有哪些？

  ```css
  1、响应式布局容器：.container
  	根据屏幕宽度不同，设置不同的版心，到了移动端此时宽度占满一行
  
  2、流式布局容器：.container-fluid
  	始终占满一行
  ```

  

- bootstrap中栅格系统如何使用？

  ```css
  栅格系统，一行等分成12分
  
  语法：.col-取值1-取值2
  取值1：
  	1、lg：大屏及以上屏幕生效
  	2、md：中屏及以上屏幕生效
  	3、sm：小屏及以上屏幕生效
  	4、xs：超小屏及以上屏幕生效
  
  取值2：份数
  ```

  

- 注意连写的覆盖问题

  ```css
  div {
    /* 该元素会往右平移同时旋转360deg？ */
    transform: translateX(800px);
    transform: rotate(360deg);
  }
  
  div {
    /* 改背景图片的大小被设置和当前盒子一样大？ */
    background-size: 100% 100%;
    background: url('./1.jpg') no-repeat center center;
  }
  ......
  ```

  

  