title: css3-transition
date: 2017-11-06 17:29:25
tags:
---
{% blockquote W3C https://www.w3.org/TR/css3-transitions/ css3-transitions %}
CSS Transitions allows property changes in CSS values to occur smoothly over a specified duration.
{% endblockquote %}

以上是W3C官方解释，翻译过来大概意思就是在某段时间内，按照预定过程的来改变某个CSS属性。
<!--more-->
## Transition
````
    transition：[ transition-property ] || [ transition-duration ] || [ transition-timing-function ] || [ transition-delay ]
    /* 实际用法*/
    /*缩写方式：*/
    -webkit-transition:border-color .5s ease-in .1s, background-color .5s ease-in .1s, color .5s ease-in .1s;/*chrome2.0x+ safari3.1+*/
    -moz-transition:border-color .5s ease-in .1s, background-color .5s ease-in .1s, color .5s ease-in .1s;/*Firefox 4 */
    -o-transition:border-color .5s ease-in .1s, background-color .5s ease-in .1s, color .5s ease-in .1s;/*opera 10.5+ */
    /*IE9不支持，所以-ms-就没了 */
    transition:border-color .5s ease-in .1s, background-color .5s ease-in .1s, color .5s ease-in .1s;/*W3C */
    /*拆分方式跟缩写方式一样前面得加浏览器前缀，一个一个写太占位置，所以就只写W3C标准的*/
    transition-property:border-color, background-color, color;
    transition-duration:.5s, .5s, .5s;
    transition-timing-function:ease-in, ease-in, ease-in;
    transition-delay:.1s, .1s, .1s;
````

Transitions属性中如果提供多个属性值，都是以逗号（“，”）隔开。

### Transitions取值
- [ transition-property ]：设置对象中的参与过渡的属性
- [ transition-duration ]：设置对象过渡的持续时间
- [ transition-timing-function ]：设置对象中过渡的动画类型
- [ transition-delay ]：设置对象延迟过渡的时间

### 参与过渡的属性

transition-property是用来指定元素需要过渡的css属性。语法如下：
````
    transition-property：all | none | <property>[ ,<property> ]*
````
#### transition-property取值
- all：所有可以进行过渡的css属性
- none：不指定过渡的css属性
- <property>：指定要进行过渡的css属性

当指定为all时，这个也是其默认值，则元素产生任何属性值变化时都将执行transition效果；当其值为none时，transition马上停止执行；property是可以指定元素的某一个属性值，如 background-color、opacity、right、width、z-index、text-indent、text-shadow、padding等。具体哪些属性{% link 点击查看 https://www.w3.org/TR/css3-animations/  %} 

### 过渡持续时间
transition-duration针对过渡效果的持续时间，用来指定元素转换过程的持续时间，取值：time 为数值，单位为s（秒）。其默认值是0，也就是变换时是即时的。语法如下：
````
    transition-duration：<time>[ ,<time> ]*
````

### 过渡的变化速率
transition-timing-function 针对了过渡效果的变化速率，有多种特效展示。语法如下：
````
    transition-timing-function：linear | ease | ease-in | ease-out | ease-in-out | cubic-bezier(<number>, <number>, <number>, <number>)[ ,linear | ease | ease-in | ease-out | ease-in-out | cubic-bezier(<number>, <number>, <number>, <number>) ]*
````

所有过渡效果都涉及到一个：贝塞尔曲线 的东西{% link 点击 https://zh.wikipedia.org/wiki/%E8%B2%9D%E8%8C%B2%E6%9B%B2%E7%B7%9A  %}了解贝塞尔曲线。但是W3C预留的几个过渡效果来供我们使用，如下：
1. ease（逐渐变慢）：默认值，ease函数等同于贝塞尔曲线（cubic-bezier曲线）(0.25, 0.1, 0.25, 1.0).
2. linear（匀速）：linear 函数等同于贝塞尔曲线（cubic-bezier曲线）(0.0, 0.0, 1.0, 1.0). 
3. ease-in(加速)：ease-in 函数等同于贝塞尔曲线（cubic-bezier曲线）(0.42, 0, 1.0, 1.0).
4. ease-out（减速）：ease-out 函数等同于贝塞尔曲线（cubic-bezier曲线）(0, 0, 0.58, 1.0).
5. ease-in-out（加速然后减速）：ease-in-out 函数等同于贝塞尔曲线（cubic-bezier曲线）(0.42, 0, 0.58, 1.0).
6. cubic-bezier（该值允许你去自定义一个时间曲线）： 特定的贝塞尔曲线（cubic-bezier曲线）.

 贝塞尔曲线，如下图

 {% img [贝塞尔曲线（cubic-bezier曲线）] http://om64pi295.bkt.clouddn.com/TimingFunction.png %}

 图上有四点，P0-3，其中P0[0,0]、P3[1,1]是默认的点且是固定不变的。而剩下的P1、P2两点则是我们通过cubic-bezier自定义的。

 所以所有值需在[0, 1]区域内，否则无效。`cubic-bezier(x1, y1, x2, y2)` 四个 x1, y1, x2, y2 值就等于曲线上点P1[x1, y1] 和点P2[x2, y2]的坐标值。

 ### 过渡的延迟执行时间
transition-delay是用来指定一个过渡延迟执行的时间。语法如下：
````
    transition-delay：<time>[ ,<time> ]*
````
## Transitions实现的简单hover按钮
asda
<style type="text/css">#demo a.button{ background-color: #700; border-radius: 10px; box-shadow: 0 0 3px #212121; color: #fff; padding: 5px 10px; -webkit-transition: all 1s ease-in-out 0s; -moz-transition: all 1s ease-in-out 0s; -ms-transition: all 1s ease-in-out 0s; -o-transition: all 1s ease-in-out 0s; transition: all 1s ease-in-out 0s; text-decoration: none } #demo a.button:hover{ background-color: #b00; box-shadow: 0 0 10px #000;
}</style>
<figure>
<div id="demo">
    <a class="button" href="">Transitions按钮</a>
</div>
</figure>