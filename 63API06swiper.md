# swiper插件

>  官网地址:  https://www.swiper.com.cn/ 

1. 查看在线演示 明确自己想要的案例 ( https://www.swiper.com.cn/demo/index.html )
2. 查看swiper的基本使用 ( https://www.swiper.com.cn/usage/index.html )
3. 也可以直接看swiper插件里面的demos,作为参考
4. 配置swiper插件 ( https://www.swiper.com.cn/api/index.html )

## 应用

>  完成JDM的手机轮播图

1. 引包 => 引入对应的css和js文件

   ```html
   <link rel="stylesheet" href="./swiper/swiper-bundle.min.css">
   <script src="./swiper/swiper-bundle.min.js"></script>
   ```

2. 复制固定的结构

   ```jsx
   <!-- 最大的盒子 -->
   <div class="swiper-container">
     <!-- 里面的滚动ul -->
     <div class="swiper-wrapper">
       <!-- 每一个li -->
       <div class="swiper-slide">Slide 1</div>
       <div class="swiper-slide">Slide 2</div>
       <div class="swiper-slide">Slide 3</div>
     </div>
     <!-- 小圆点分页器 -->
     <div class="swiper-pagination"></div>
   
     <!-- 左右导航按钮 -->
     <div class="swiper-button-prev"></div>
     <div class="swiper-button-next"></div>
   </div>
   ```

3. 调用Swiper() , 进行配置

   ```js
   // 第一个参数: 需要进行轮播的模块对应的选择器  => 尽量用自己的类名
   // 第二个参数: 是一个配置对象, 里面可以配置不同的需求
   new Swiper('.jd-carousel', {
       loop: true, // 循环模式选项
       autoplay: true,
       // 如果需要分页器
       pagination: {
           el: '.swiper-pagination'
       }
});
   ```
   
> 完成JDM的滚动反弹效果

```js
官网:
1. 在在线演示中, 找到free模式
2. 在新窗口打开
3. 在新窗口右键打开网页源代码
4. 主要看配置对象里面的配置参数
```



1. 引包 (上面已经引过咯)

2. 复制固定的结构 (由于事先已经写好咯结构和样式,再完全替换较为麻烦, 所以将swiper的类名直接移植到自己的页面上来更方便)

   ```html
   <div class="sec-kill-body swiper-container">
     <ul class="goods-wrap swiper-wrapper">
       <li class="swiper-slide">
         <div class="img-wrap">
           <a href="#"><img src="./images/sec01.jpg" alt="" /></a>
         </div>
         <div class="goods-info">
           <p>&yen; 1499</p>
           <del>&yen; 1999</del>
         </div>
       </li>
       ...
     </ul>
   </div>
   ```

3. 调用swiper进行对象配置 (配置对象可以参考基本模板里面的free模式 => 在新窗口打开 => 打开源代码)

   ```js
   new Swiper('.sec-kill-main', {
     // 是否开启自由移动模式
     freeMode: true,
     slidesPerView: 'auto'
   });
   ```

**注意** 

1. 当一个网页里面需要多个swiper的时候,  new Swiper的第一个参数用自己的类名 以防止冲突
2. li一定要有自己的宽度.

作业: 完成手机端的京东快报新闻切换效果