title: css3-transfrom
date: 2017-11-07 10:42:59
tags:
---


{% blockquote W3C https://www.w3.org/TR/css-transforms-1/ css-transforms %}
CSS transforms allows elements styled with CSS to be transformed in two-dimensional or three-dimensional space. 
{% endblockquote %}

以上是W3C官方解释，翻译过来大概意思就是：transforms 可以让元素的css在二维或者三维空间变化。
<!--more-->
## Transition
````
transform：none | matrix(<number>,<number>,<number>,<number>,<number>,<number>)? translate(<length>[,<length>])? translateX(<length>)? translateY(<length>)? rotate(<angle>)? scale(<number>[,<number>])? scaleX(<number>)? scaleY(<number>)? skew(<angle>[,<angle>])? skewX(<<angle>) || skewY(<angle>)?
````
````css
    /* 实际用法*/
    -webkit-transform: rotate(4deg) scale(1) skew(1deg) translate(10px);//chrome1.0x+ safari3.1+
    -moz-transform: rotate(4deg) scale(1) skew(1deg) translate(10px);//firefox3.5+
    -o-transform: rotate(4deg) scale(1) skew(1deg) translate(10px);//opera 10.5+
    -ms-transform: rotate(4deg) scale(1) skew(1deg) translate(10px);//IE9+
    transform: rotate(4deg) scale(1) skew(1deg) translate(10px);//W3C标准
````

Transform 属性中如果提供多个属性值，都是以逗号（“，”）隔开。

### Transfrom（变形）取值

- translate(<length>[, <length>])：指定对象的2D translation（2D平移）。第一个参数对应X轴，第二个参数对应Y轴。如果第二个参数未提供，则默认值为0
- translateX(<length>)：指定对象X轴（水平方向）的平移 
- translateY(<length>)：指定对象Y轴（垂直方向）的平移
- rotate(<angle>)：指定对象的2D rotation（2D旋转），按照 ransform-origin 属性的定义为基点，默认为 center,center
- scale(<number>[, <number>])：指定对象的2D scale（2D缩放）。第一个参数对应X轴，第二个参数对应Y轴。如果第二个参数未提供，则默认取第一个参数的值
- scaleX(<number>)：指定对象X轴的（水平方向）缩放
- scaleY(<number>)：指定对象Y轴的（垂直方向）缩放
- skew(<angle> [, <angle>])：指定对象skew transformation（斜切扭曲）。第一个参数对应X轴，第二个参数对应Y轴。如果第二个参数未提供，则默认值为0
- skewX(<angle>)：指定对象X轴的（水平方向）扭曲
- skewY(<angle>)：指定对象Y轴的（垂直方向）扭曲 
- matrix(<number>,<number>,<number>,<number>,<number>,<number>)：以一个含六值的(a,b,c,d,e,f)变换矩阵的形式指定一个2D变换，相当于直接应用一个[a,b,c,d,e,f]变换矩阵


直接看DEMO，加深印象


<div class="transfrom">
<ul class="clearfix"> <li class="item translate">Translate</li> <li class="item translate-x">TranslateX</li> <li class="item translate-y">TranslateY</li> <li class="item rotate">Rotate</li> <li class="item scale">Scale</li> <li class="item scale-x">ScaleX</li> <li class="item scale-y">ScaleY</li> <li class="item skew">Skew</li> <li class="item skew-x">SkewX</li> <li class="item skew-y">SkewY</li> <li class="item matrix">Matrix</li> </ul> </div>

<style type="text/css">.transfrom{padding: 20px;overflow:hidden;}.transfrom ul li{ color: #222; float: left; margin: .9em; padding:0 .4em; font-size: 14px; height: 50px; line-height: 50px; text-align: center; width: 70px; border:1px #ddd solid; background: #fff; box-shadow: 0 0 1px #ccc,inset 0 0 2px #fff; text-shadow: 0 1px 1px #686868; list-style:none; } .transfrom ul li.translate a:hover { -moz-transform: translate(-10px,-10px); -webkit-transform: translate(-10px,-10px); -o-transform: translate(-10px,-10px); -ms-transform: translate(-10px, -10px); transform: translate(-10px,-10px); } .transfrom ul li.translate-x{ -moz-transform: translateX(-10px); -webkit-transform: translateX(-10px); -o-transform: translateX(-10px); -ms-transform: translateX(-10px); transform: translateX(-10px); } .transfrom ul li.translate-y{ -moz-transform: translateY(-10px); -webkit-transform: translateY(-10px); -o-transform: translateY(-10px); -ms-transform: translateY(-10px); transform: translateY(-10px); } .transfrom ul li.rotate{ -moz-transform: rotate(45deg); -webkit-transform: rotate(45deg); -o-transform: rotate(45deg); -ms-transform: rotate(45deg); transform: rotate(45deg); } .transfrom ul li.scale{ -moz-transform: scale(0.8,0.8); -webkit-transform: scale(0.8,0.8); -o-transform: scale(0.8,0.8); -ms-transform: scale(0.8,0.8); transform: scale(0.8,0.8); } .transfrom ul li.scale-x{ -moz-transform: scaleX(0.8); -webkit-transform: scaleX(0.8); -o-transform: scaleX(0.8); -ms-transform: scaleX(0.8); transform: scaleX(0.8); } .transfrom ul li.scale-y{ -moz-transform: scaleY(1.2); -webkit-transform: scaleY(1.2); -o-transform: scaleY(1.2); -ms-transform: scaleY(1.2); transform: scaleY(1.2); } .transfrom ul li.skew{ -moz-transform: skew(45deg,15deg); -webkit-transform: skew(45deg,15deg); -o-transform: skew(45deg,15deg); -ms-transform: skew(45deg,15deg); transform: skew(45deg,15deg); } .transfrom ul li.skew-x{ -moz-transform: skewX(-30deg); -webkit-transform: skewX(-30deg); -o-transform: skewX(-30deg); -ms-transform: skewX(-30deg); transform: skewX(-30deg); } .transfrom ul li.skew-y{ -moz-transform: skewY(30deg); -webkit-transform: skewY(30deg); -o-transform: skewY(30deg); -ms-transform: skewY(30deg); transform: skewY(30deg); } .transfrom ul li.matrix{ -moz-transform: matrix(1,1,-1,0,0,0); -webkit-transform: matrix(1,1,-1,0,0,0); -o-transform: matrix(1,1,-1,0,0,0); -ms-transform: matrix(1,1,-1,0,0,0); transform: matrix(1,1,-1,0,0,0); -moz-transform-origin:top left; }</style>


### transform-origin（改变元素基点）

````
transform-origin：[ <percentage> | <length> | left | center | right ] [ <percentage> | <length> | top | center | bottom ]?
/* 实际用法*/
-webkit-transform-origin:top left;//chrome1.0x+ safari3.1+
-moz-transform-origin:top left;//firefox3.5+
-o-transform-origin:top left;//opera 10.5+
-ms-transform-origin:top left;//IE9+
-transform-origin:top left;//W3C标准
````

- <percentage>：用百分比指定坐标值。可以为负值。
- <length>：用长度值指定坐标值。可以为负值。
- left：指定原点的横坐标为left
- center：指定原点的横坐标为center
- right：指定原点的横坐标为right
- top：指定原点的纵坐标为top
- center：指定原点的纵坐标为center 
- bottom：指定原点的纵坐标为bottom 

left,center right是水平方向取值，对应的百分值为left=0%;center=50%;right=100%；top center bottom是垂直方向的取值，对应的百分值为top=0%;center=50%;bottom=100%;如果只取一个值，表示垂直方向值不变
















