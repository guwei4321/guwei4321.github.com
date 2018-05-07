title: css3-animation
date: 2017-11-03 10:16:57
tags:
---


{% blockquote W3C https://www.w3.org/TR/css3-animations/ css3-animations %}
This CSS module describes a way for authors to animate the values of CSS properties over time, using keyframes. The behavior of these keyframe animations can be controlled by specifying their duration, number of repeats, and repeating behavior.
{% endblockquote %}

以上是W3C官方解释，翻译过来大概意思就是：animation使用关键帧的方式，并且可以控制动画持续时间、循环次数，过渡类型。
<!--more-->
看了以上解释是不是觉得 animations 能实现的效果貌似用transfrom（过渡）搭配Transition（变形），transition（变形） 搭配 transfrom（过渡）确实是可以完成 animation 的一些效果，但是官网上说了 animation 这个属性是transition属性的扩展，而它比transition复杂的地方就是：keyframes（关键帧），我觉得不仅仅只有关键帧。
<style type="text/css">#sky { width: 500px; height: 500px; position: relative; z-index: 1; overflow: hidden; background-color: #525252; }
    #sky.animate { -webkit-animation:sky  10s ease 1s 1 normal forwards; -webkit-animation-play-state: running; -moz-animation:sky  10s ease 1s 1 normal forwards; -moz-animation-play-state: running; -ms-animation:sky  10s ease 1s 1 normal forwards; -ms-animation-fill-mode: forwards; animation:sky  10s ease 1s 1 normal forwards; animation-fill-mode: forwards; }
    @-webkit-keyframes sky { 0% { background-color: #525252; } 33% { background-color: #6293e5; } 66% { background-color: #6293e5; } 100% { background-color: #525252; } }
    @-moz-keyframes sky { 0% { background-color: #525252; } 33% { background-color: #6293e5; } 66% { background-color: #6293e5; } 100% { background-color: #525252; } }
    @-ms-keyframes sky { 0% { background-color: #525252; } 33% { background-color: #6293e5; } 66% { background-color: #6293e5; } 100% { background-color: #525252; } }
    @keyframes sky { 0% { background-color: #525252; } 33% { background-color: #6293e5; } 66% { background-color: #6293e5; } 100% { background-color: #525252; } }

    #ground { position: absolute; bottom: 0; left: 0; width: 500px; height: 154px; background: #6c5228; z-index: 4; }
    #ground.animate { -webkit-animation:ground 10s ease 1s 1 normal forwards; -webkit-animation-play-state: running; -moz-animation:ground 10s ease 1s 1 normal forwards; -moz-animation-play-state: running; -ms-animation:ground 10s ease 1s 1 normal forwards; -ms-animation-play-state: running; animation:ground 10s ease 1s 1 normal forwards; animation-play-state: running; }
    @-webkit-keyframes ground { 0% { background: #6c5228; } 33% { background: #48a037; } 66% { background: #48a037; } 100% { background: #6c5228; } }
    @-moz-keyframes ground { 0% { background: #6c5228; } 33% { background: #48a037; } 66% { background: #48a037; } 100% { background: #6c5228; } }
    @-ms-keyframes ground { 0% { background: #6c5228; } 33% { background: #48a037; } 66% { background: #48a037; } 100% { background: #6c5228; } }
    @keyframes ground { 0% { background: #6c5228; } 33% { background: #48a037; } 66% { background: #48a037; } 100% { background: #6c5228; } }

    #sun { background: #ffd630; width: 130px; height: 130px; position: absolute; border-radius: 70px; z-index: 2; bottom: 0; left: 340px; }
    #sun.animate { -webkit-animation:sunrise 10s ease 0s 1 normal forwards; -webkit-animation-play-state: running; -moz-animation:sunrise 10s ease 0s 1 normal forwards; -moz-animation-play-state: running; -ms-animation:sunrise 10s ease 0s 1 normal forwards; -ms-animation-play-state: running; animation:sunrise 10s ease 0s 1 normal forwards; animation-play-state: running; }
    @-webkit-keyframes sunrise { 0% { bottom: 0; left: 340px; background: #f00; } 33% { bottom: 340px; left: 340px; background: #ffd630; } 66% { bottom: 340px; left: 40px; background: #ffd630; } 100% { bottom: 0; left: 40px; background: #f00; } }
    @-moz-keyframes sunrise { 0% { bottom: 0; left: 340px; background: #f00; } 33% { bottom: 340px; left: 340px; background: #ffd630; } 66% { bottom: 340px; left: 40px; background: #ffd630; } 100% { bottom: 0; left: 40px; background: #f00; } }
    @-ms-keyframes sunrise { 0% { bottom: 0; left: 340px; background: #f00; } 33% { bottom: 340px; left: 340px; background: #ffd630; } 66% { bottom: 340px; left: 40px; background: #ffd630; } 100% { bottom: 0; left: 40px; background: #f00; } }
    @keyframes sunrise { 0% { bottom: 0; left: 340px; background: #f00; } 33% { bottom: 340px; left: 340px; background: #ffd630; } 66% { bottom: 340px; left: 40px; background: #ffd630; } 100% { bottom: 0; left: 40px; background: #f00; } }

    #cloud { position: relative; top: 50px; left: -100px; opacity: 0; z-index: 3; }
    #cloud.animate { -weblit-animation:cloud 12s ease 0s 1 normal forwards; -webkit-animation-play-state: running; -moz-animation:cloud 12s ease 0s 1 normal forwards; -moz-animation-play-state: running; -ms-animation:cloud 12s ease 0s 1 normal forwards; -ms-animation-play-state: running; animation:cloud 12s ease 0s 1 normal forwards; animation-play-state: running; }
    @-webkit-keyframes cloud { 0% { opacity: 0; left: -100px; } 50% { opacity: 1; left: 60px; } 75% { opacity: 1; left: 100px; } 100% { opacity: 0; left: 500px; } }
    @-moz-keyframes cloud { 0% { opacity: 0; left: -100px; } 50% { opacity: 1; left: 60px; } 75% { opacity: 1; left: 100px; } 100% { opacity: 0; left: 500px; } }
    @-ms-keyframes cloud { 0% { opacity: 0; left: -100px; } 50% { opacity: 1; left: 60px; } 75% { opacity: 1; left: 100px; } 100% { opacity: 0; left: 500px; } }
    @keyframes cloud { 0% { opacity: 0; left: -100px; } 50% { opacity: 1; left: 60px; } 75% { opacity: 1; left: 100px; } 100% { opacity: 0; left: 500px; } }
    .cloud { border-radius: 90px / 30px; width: 160px; height: 50px; background: #fff; position: absolute; top: 10px; }
    .cloud-2 { left: 50px; top: 0; }
    .cloud-3 { left: 110px; top: 20px; }

    #moon { position: relative; opacity: 0; top: 50px; left: -100px; }
    #moon.animate { -webkit-animation:moon 10s ease 0s 1 normal forwards; -webkit-animation-play-state: running; -moz-animation:moon 10s ease 0s 1 normal forwards; -moz-animation-play-state: running; -ms--animation:moon 10s ease 0s 1 normal forwards; -ms-animation-play-state: running; animation:moon 10s ease 0s 1 normal forwards; animation-play-state: running; }
    @-webkit-keyframes moon { 0% { opacity: 0; left: -100px; } 50% { opacity: 0; left: -100px; } 90% { opacity: 0; left: -100px; } 100% { opacity: 1; left: 50px; } }
    @-moz-keyframes moon { 0% { opacity: 0; left: -100px; } 50% { opacity: 0; left: -100px; } 90% { opacity: 0; left: -100px; } 100% { opacity: 1; left: 50px; } }</style>

点击下面DEMO观看动画：
<figure>
<div id="sky" class="target">
    <div id="cloud" class="target">
    <div class="cloud cloud-1"></div>
    <div class="cloud cloud-2"></div>
    <div class="cloud cloud-3"></div>
    </div>
    <div id="sun" class="target"></div>
    <div id="moon" class="target">
    <div class="moon">
    </div>
    <div class="moon moon-2">
    </div>
    </div>
    <div id="ground" class="target"></div>
</div>
<p><input type="button" id="startbutton" value="开始动画"></p>
<figcaption>animation动画 ”一天日月轮回 “</figcaption>
<figure>

<script type="text/javascript">
window.onload = function(){
    jQuery.noConflict();
jQuery(document).ready(function ($) {var s = null, AnimationSpace = { settings:{ startButton: $("#startbutton") }, init:function () { this.startAnimation(); }, startAnimation: function () { s = this.settings; s.startButton.click(function() { $("div.target").toggleClass("animate"); if (s.startButton.attr("value") === "开始动画") { s.startButton.attr("value", "重置动画"); } else { s.startButton.attr("value", "开始动画"); } }); } }; AnimationSpace.init(); });
}
                    
</script>

以上DEMOjs只是控制动画开始和重置，其他都是利用CSS3的 animation 属性。还是很神奇的吧，是不是觉得很有必要了解一下CSS3的 animation 属性呢。o(∩_∩)o

Animation包含了8个独立的属性，分别为animation-name、animation-duration、animation-timing-function、animation-delay、animation-iteration-count、animation-direction、animation-fill-mode，animation-play-state，其中 animation-play-state 为animation的相关属性。下面来一一介绍它们和各自的语法。


## Animation动画
````
    animation：[[ animation-name ] || [ animation-duration ] || [ animation-timing-function ] || [ animation-delay ] || [ animation-iteration-count ] || [ animation-direction ]|| [animation-fill-mode]] [ , [ animation-name ] || [ animation-duration ] || [ animation-timing-function ] || [ animation-delay ] || [ animation-iteration-count ] || [ animation-direction ] || [animation-fill-mode]]*
    相关属性：[ animation-play-state ]
    /* 实际用法*/
    /*关键帧名字前得加浏览器前缀，这里为了减少文章篇幅，就略掉了~*/
    @keyframes sky {
        0% { background-color: #525252; }
        33% { background-color: #6293e5; }
        66% { background-color:#6293e5; }
        100% { background-color: #525252; }
    }
    /*缩写方式：*/
    .classname {
        -webkit-animation:sky 10s ease 1s 1 normal forwards;
        -webkit-animation-play-state: running;/*animation附加属性*/
        -moz-animation:sky  10s ease 1s 1 normal forwards;
        -moz-animation-play-state: running;/*animation附加属性*/
        animation:sky  10s ease 1s 1 normal forwards;
        animation-play-state: running;/*animation附加属性*/
    }
    /*拆分方式跟缩写方式一样前面得加浏览器前缀，一个一个写太占位置，所以就只写W3C标准的*/
    animation-name: sky;
    animation-duration: 10s;
    animation-timing-function: ease;
    animation-iteration-count: 1;
    animation-direction: normal;
    animation-delay: 0;
    animation-fill-mode: forwards;
    animation-play-state: running;
````

## animation取值
1. animation-name：设置对象所应用的动画名称
2. animation-duration：设置对象动画的持续时间
3. animation-timing-function：设置对象动画的过渡类型
4. animation-delay：设置对象动画延迟的时间
5. animation-iteration-count：设置对象动画的循环次数
6. animation-direction：设置对象动画在循环中是否反向运动
7. animation-fill-mode：设置对象动画结束时的状态
8. animation-play-state：animation的相关属性，设置对象动画的状态

### 动画名称以及keyframes
#### 动画名称
animation-nam指定元素的 animation 的名称，必须与规则@keyframes配合使用。animation-name具体语法如下：

````
    animation-name：none | <identifier> [ , none | <identifier> ]*
````
动画具体名字得设置成 Keyframes 一样的名字。
#### Keyframes
keyframes 语法
````
    keyframes-rule: '@keyframes' IDENT '{' keyframes-blocks '}';
    keyframes-blocks: [ keyframe-selectors block ]* ;
    keyframe-selectors: [ 'from' | 'to' | PERCENTAGE ] [ ',' [ 'from' | 'to' | PERCENTAGE ] ]*;
````
````css
    /*具体写法*/
    /* @keyframes IDENT {*/
    @keyframes diagonal-slide {
        from {
            left: 0;
            top: 0;
        }
        to {
            left: 100px;
            top: 100px;
        }
    }
    /*或者全部写成百分比的形式：*/
    @keyframes wobble {
        0% {
        left: 100px;
        }
        40% {
        left: 150px;
        }
        60% {
        left: 75px;
        }
        100% {
        left: 100px;
        }
    }
````
Keyframes的命名是”动画的名称”前带 @符号，后面紧接着一对花括号“{}”，括号中就是一些样式属性，多个属性的话 可以用 ，逗号隔开。

这个 Keyframes 就是Flash里的 时间轴 和 关键帧 的结合体

### 动画的持续时间

animation-duration 指定对象动画的持续时间，跟transition的transition-duration属性一样，取值：time 为数值，单位为s（秒）。其默认值是0，也就是变换时是即时的。语法如下
````css
animation-duration：<time> [ , <time> ]*
````
这里要注意的是：如果是缩写，必须得带 单位 s（秒），定义在 animation-timing-function 单独属性里可以不用加 单位。

### 动画的变化速率

animation-timing-function的变化速率也跟transition的transition-timing-function属性一样，同样可以由cubic-bezier决定速率，也有同样的预留速率值 ease（逐渐变慢）、linear（匀速）等，语法如下：
````
animation-timing-function：linear | ease | ease-in | ease-out | ease-in-out | cubic-bezier(<number>, <number>, <number>, <number>) [ , linear | ease | ease-in | ease-out | ease-in-out | cubic-bezier(<number>, <number>, <number>, <number>) ]*
````


### 动画的延迟执行时间

animation-delay是用来指定一个动画的延迟执行的时间。语法如下：
````
animation-delay：<time> [ , <time> ]*
````

跟动画持续事件一样，如果是缩写，必须得带 单位 s（秒），定义在 animation-timing-function 单独属性里可以不用加 单位。如果不带 单位 s（秒）则会把他当成下面要说的循环次数。

以上介绍的 animation-duration（动画持续时间）、animation-timing-function（动画变化速率）、animation-delay（动画延迟执行）三个属性跟 transition 过渡中的效果是一样。但是animation有transtion过渡没有的属性（transtion是animation的缩减版）。

接下来介绍它们。

### 动画的循环次数
animation-iteration-count是用来制定动画的循环次数，语法如下：
````
animation-iteration-count：infinite | <number> [ , infinite | <number> ]*
````
- ：默认值，代表只循环一次
- <number>：自定义对象动画的具体循环次数
- infinite：无限循环

### 动画的方向
animation-direction是用来指定元素动画播放的方向，语法如下：
````
animation-direction：normal | alternate [ , normal | alternate ]*
````
- normal：动画的每次循环都是正常播放
- alternate：动画的播放将是来回往返，一次是正常的与一次是反向的
**注意：如果 animation-direction 设置成 alternate ，则必须得大于一次，不然 alternate 就白设置了。最好是偶数次循环。**

### 动画结束的时的状态
animation的附属属性：animation-fill-mode表示动画结束时的状态，语法如下：
````
animation-fill-mode：none | forwards | backwards | both [ , none | forwards | backwards | both ]*
````
- none：默认值。不设置对象动画之外的状态
- forwards：设置对象状态为动画结束时的状态
- backwards：设置对象状态为动画开始时的状态
- both：设置对象状态为动画结束或开始的状态，为什么是或？往下看
**注意：forwards 和 backwards 没什么好说的，就是上面那个字面意思，而如果最终状态是both的的话，如果只有一个动画是backwards和forwards是可以的，如果有多个动画而且两个动画最终位置不同，最好设置成both 。不然达不到预期效果。**

### 动画的运动状态
animation-play-state主要是用来控制元素动画的播放状态。其主要有两个值，running（播放）和paused（暂停）其中running为默认值。就像视频里的暂停播放一样.语法如下：
````
animation-play-state：running | paused [ , running | paused ]*
````
### 总结
结合以实例

- 关键帧和动画名字没什么好说的，像 flash 时间轴 一样，只是animation 用半分比来表示。
- 动画名字之后的三个属性就是：持续事件、变化速率和延迟事件，跟 transfrom 中的属性一样，也好理解。
- 之后的循环次数，我们设置成了 1 ，就是告诉这个动画播放一遍就够了，别再播了。
- 再之后的是动画的运动方向，我们设置成了 normal ，动画效果就如所看到，白天→黑夜，就结束了，如果设置成 alternate ，那么动画效果则是 白天→黑夜然后又从黑夜倒回来到了白天直致刚开始的样子，不过我们得把播放次数1 改成大于一的，不然它只会播放到晚上就不往回播放了。
- animation最后一个属性 ，DEMO里都是forwards，任务完成就呆那里吧。不许动了。
- animation的附属属性，DEMO里都是 running的，如果是 paused的话就暂停了，如果js控制的话应该能更像看视频那感觉。