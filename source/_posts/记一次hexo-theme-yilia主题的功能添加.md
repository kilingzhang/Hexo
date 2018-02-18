---
title: 记一次hexo-theme-yilia主题的功能添加
date: 2017-08-21 22:17:28
tags: [hexo, hexo-theme-yilia]
---

## 前言
今天翻看大大们的博客，无意间看到了一个很有意思的小功能。[传送门](http://bj.uiv5.com/)
![](http://markdown-1252847423.file.myqcloud.com/lovetime.png)

虽然被喂了一嘴狗粮，但是像我这种有仇必报的人，怎么可能不怼回去呢。我也要搞一个恋爱计时器。嗯，让那些误闯误撞进我博客的人们也感受下狗粮的深深恶意。说做就做，开工！！！

<!--more-->

## 准备

作为一个phper来说，之前我也没有搞过hexo的主题修改啊。完全不懂主题的文件该怎么改。怎么办？看看人家官方的wiki吧！[传送门](https://github.com/litten/hexo-theme-yilia/wiki)
官方给的目录结构是这样的
![](http://markdown-1252847423.file.myqcloud.com/yilia-wiki.png)
看到， `source` 主要是Hexo加载主题资源的主目录，需要编译生成。也就是放我们一些资源文件。`source-src` 放的就是一些 `js` `css`文件。 `layout` 放的就是我们要修改的文件了。
我首先先在浏览器中寻找我要修改的`html`代码位置
![](http://markdown-1252847423.file.myqcloud.com/cuthtmllovetime.png)
我们需要在标记2中添加如图的效果，所以对应的div结构在标记1的`left-col`class中。所以我在文件夹`layout`下寻找可能存在此代码的文件。我打开了所有下属文件，一个都没有跟此class有关系的代码块，好吧，去下一层`_partial`找找。很幸运，竟然有个名字就叫`left-col。ejs`的文件。还找什么，就是你了。
简单浏览一遍代码后，感觉这语法还是很好理解的。虽然不是标准的html代码。但是跟后端的模板很像。我就找了个想添加功能的位置开始写起了恋爱计时功能。
![](http://markdown-1252847423.file.myqcloud.com/nav-lovetime-code.png)

## 代码
### 配置项
```
love_time:
  mname: '学长大大'
  wname: 'xy宝宝'
  starttime: '2016-09-21 00:00:00'
```
### html
```
<nav class="header-lovetime">
    <% if(theme.love_time['mname']){ %>
        <div class="lovetime">
            <section class="love-time">
                <p class="love-people">
                    <%= theme.love_time['mname'] %> 和
                        <%= theme.love_time['wname'] %> 走过了：</p>
                <p id="love-time-count"></p>
            </section>
        </div>
        <style>
            /* 计时器 */
            .header-lovetime{
                animation: fade-in;
                -webkit-animation: fade-in 1s;
            }
            .love-time {
                z-index: 999;
                bottom: 90px;
                left: 30px;
                margin-bottom: 12px;
                text-align: center;
                font-size: 15px;
                color: #9a8c8c;
                border-radius: 5px;
            }

            .love-people {
                font-size: 13px;
            }
            .love-time-count,
            .love-time-asume {
                font-size: 10px;
            }
        </style>
        <script>
            var starttime = "<%= theme.love_time['starttime'] %>";
            setInterval(function () { interval(starttime); }, 100);
            function interval(e) {
                var date1 = new Date(e);  //开始时间
                var date2 = new Date();    //结束时间
                var date3 = date2.getTime() - date1.getTime()  //时间差的毫秒数

                //计算出相差天数
                var days = Math.floor(date3 / (24 * 3600 * 1000))

                //计算出小时数

                var leave1 = date3 % (24 * 3600 * 1000)    //计算天数后剩余的毫秒数
                var hours = Math.floor(leave1 / (3600 * 1000))
                //计算相差分钟数
                var leave2 = leave1 % (3600 * 1000)        //计算小时数后剩余的毫秒数
                var minutes = Math.floor(leave2 / (60 * 1000))


                //计算相差秒数
                var leave3 = leave2 % (60 * 1000)      //计算分钟数后剩余的毫秒数
                var seconds = Math.round(leave3 / 1000)

                var countDays = document.getElementById("love-time-count")
                countDays.innerHTML = days + "天 " + hours + "小时 " + minutes + " 分钟" + seconds + " 秒"


            }
        </script>
        <% } %>
</nav>
```

## 效果图

![](http://markdown-1252847423.costj.myqcloud.com/love-time.gif)


## 后记

大功告成！！！！哈哈哈， 以你为这就完了吗~~~  不，这才是个开始。 看到截图中那个头像没。 我的[首页](http://www.kilingzhang.com)
![](http://markdown-1252847423.costj.myqcloud.com/avatar-eg.gif)
其实是可以鼠标悬停转动的。虽然没什么用，但是我还是想把这个主题的头像也改成相同的效果。/\*可能是强迫症吧\*/当时我想，既然头像也都是在这个`left-col`class中，那么也必定在`left-col.ejs`文件里。果然被我猜中了！！！！！！！！

好吧，是我之前浏览文件的时候看到了。代码如下：
### html
```
<img  src="<%=theme.avatar%>" class="js-avatar avatar" >
```
### css
```
    .avatar{
        transition-duration: 1s;
        animation: fade-in;
        -webkit-animation: fade-in 1s;
    }
    .avatar:hover {
        transition-duration: 2s;									
        transform: rotate(180deg);
        -ms-transform: rotate(180deg);
        /* IE 9 */
        -webkit-transform: rotate(180deg);
        /* Safari and Chrome */
    }

    @keyframes fade-in {
        0% {
            opacity: 0
        }
        40% {
            opacity: 0
        }
        100% {
            opacity: 1
        }
    }

    @-webkit-keyframes fade-in {
        0% {
            opacity: 0
        }
        40% {
            opacity: 0
        }
        100% {
            opacity: 1
        }
    }

```

### 效果图？？？
![](http://markdown-1252847423.costj.myqcloud.com/avatar-failed.gif)
咦，有没有发现，好像两个效果不太一样啊。怎么动画那么快？？我明明设置的是相同的时间呀！！我把动画时间增长好像也不行。就好像时间选项没有设置一样。是什么原因呢？？？我想了一下，既然最终效果是后来动态生成的，可能是后来引入的css样式文件使现在的设置的样式失效掉。应该是**css样式的引入优先级**的问题。可我又不能因为这一个小功能去翻遍完整的css文件吧。于是，我取了个巧妙的方法。以其人之道还治其人之身。
**利用css样式的优先级**，我把html代码改为如下：
```
<img  src="<%=theme.avatar%>" class="js-avatar avatar"  style="transition-duration: 1s;">
```

### 效果图
![](http://markdown-1252847423.costj.myqcloud.com/avatar.gif)

## 结束语
额，没啥技术含量，就算是记录我代码路上的一件趣事吧~~~ 最后跟看到此博客的各位说一声，逛博需谨慎，狗粮请拿走。
