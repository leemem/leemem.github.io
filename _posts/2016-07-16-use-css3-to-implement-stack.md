---
layout: default
title: 利用伪类实现图片堆叠的效果
---
# 利用伪类实现图片堆叠的效果

---

作者： [李茂琦](http://blog.limaoqi.com)

日期： {{ page.date | date_to_string }}

#### 引言
在现在的web前端开发中，你会发现伪类被用得越来越多，特别是:before和:after的使用。今天我们就来看看，怎么用伪类实现图片堆叠的效果。你或许曾看到过这样的效果：

![最终效果图](/images/all-640x214.png)

看起来让人有赏心悦目的感觉，这可以在简单的元素上通过css实现。下面我们就来一步一步看怎么实现吧

#### html内容
首先我们来创建第一张图片的标签，只需要创建一个img和包裹img的div。给div添加class “stackone”

~~~html
<div class="stackone">
     <img src="http://blog.lemontreelife.com/wp-content/uploads/2016/06/IMG_20151005_112551_1444524732662.jpg" />
</div>
~~~

#### 基本样式
设置基本的样式margin/padding为0，并且给body加上背景色。因为我们图片边框是白色的，有一个对比。

我这里使用的图片实际宽高和本文中要显示的大小不一致，为了让图片与显示合适大小，需要设置图片跟父元素一样大。

~~~css
* {margin: 0 padding:0;}
 body {background: #ccd3d7;}
 
img{
	width:100%;
}
~~~

#### 顶部图片效果
现在我们来开始对普普通通的图片加上一些有意思的效果，如下面代码所示：

~~~css
.stackone{
	 border: 6px solid #fff;
	 float: left;
	 height: 200px; width: 200px;
	 margin: 50px;
	 position: relative;
	 -webkit-box-shadow: 2px 2px 5px rgba(0,0,0,0.3);
	 -moz-box-shadow: 2px 2px 5px rgba(0,0,0,0.3);
	 box-shadow: 2px 2px 5px rgba(0,0,0,0.3);
 }
~~~

这里有好几行代码，让们一点一点来解析

~~~css
.stackone{
	 float: left;
	 height: 200px; width: 200px;
	 margin: 50px;
	 position: relative;
 }
~~~

因为我们是要实现多个不同堆叠效果的图片，将图片横向排列。这里我们需要用float将div横向布局。设置margin用来将图片之间隔开，有一定的空间，这样看起来会更舒适。

这里设置了relative，这是为后续堆叠效果的伪类做准备的。当伪类做绝对定位时，相对于这个元素做定位。

剩下的代码是仅仅是为了修饰图片，让图片看起来更好看的。这里我们给图片加了一条窄的白色边框，并且加上阴影效果。

我们来看看效果：

![顶部图片](/images/top_img.png)

是不是有点以前胶片相片的效果了。

#### 第一个伪类元素
现在是时候给图片添加第一层层叠图片了。我们要做的是看起来有另外一张图片放在当前图片的下面。这里我们用:before这个伪类元素来实现，实现的代码与之前的代码差不多，只是增加了一个内容为空的属性

~~~css
.stackone:before{
 	content: "";
 }
~~~

这段样式，将会在浏览器加载页面的时候，给class为stackone的元素内容前面添加一个内容为空的伪类元素。

~~~css
.stackone:before{
	 content: "";
	 height: 200px; width: 200px;
	 background: #eff4de;
	 border: 6px solid #fff;
	 -webkit-box-shadow: 2px 2px 5px rgba(0,0,0,0.3);
	 -moz-box-shadow: 2px 2px 5px rgba(0,0,0,0.3);
	 box-shadow: 2px 2px 5px rgba(0,0,0,0.3);
 }
~~~

关键的地方是，我们并不是要真正的添加一张图片在顶部图片的下面，显示一张图片需要同时加载三张图片，并不是我们想要的。我们只需要用伪类元素模拟出图片的效果放在顶部图片的下面就可以。因为放在顶部图片下面的伪类元素主要区域已经被遮挡，我们只需要用纯色填充背景即可。

这个时候我们再来看看效果：

![初步效果](/images/Snap5.png)

显然这并不是我们想要的效果，我们已经把原来的图片弄乱了。就像你看到的，新插入的伪类元素把当前图片撑开了。为了解决这个问题，我需要给伪类元素增加一些定位的样式，并且用z-index属性将其藏到图片的后面。下面这段代码，我们分段定义了不同的样式，你可以很清楚的看到，没一段样式都做了什么。

~~~css
.stackone:before{
	 content: "";
	 height: 200px; width: 200px;
	 background: #eff4de;
	 border: 6px solid #fff;
	 
	 position: absolute;
	 z-index: -1;
	 top: 0px;
	 left: -10px;
	 
	 -webkit-box-shadow: 2px 2px 5px rgba(0,0,0,0.3);
	 -moz-box-shadow: 2px 2px 5px rgba(0,0,0,0.3);
	 box-shadow: 2px 2px 5px rgba(0,0,0,0.3);
	 
	 -webkit-transform: rotate(-5deg);
	 -moz-transform: rotate(-5deg);
	 -o-transform: rotate(-5deg);
	 -ms-transform: rotate(-5deg);
	 transform: rotate(-5deg);
 }
~~~

通过绝对定位，我们可以设置伪类元素相对于父元素的任何位置，而不至干扰顶层图片。通过设置z-index我们将伪类元素放到顶层元素的后面。最后通过trasform属性，将伪类元素做适当倾斜。于是我们得到了如下的效果：

![初步效果](/images/Snap6.png)

#### 第二个伪类元素
下面开始添加第三层图片效果，你应该也想到了，我们将使用伪类:after来实现。实现与:before一样，我们这里就不做赘述了，只需要在位置、背景和旋转角度做一些变化即可，代码如下所示：

~~~css
.stackone:after{
	 content: "";
	 height: 200px; width: 200px;
	 background: #768590;
	 border: 6px solid #fff;
	 
	 position: absolute;
	 z-index: -1;
	 top: 5px;
	 left: 0;
	 
	 -webkit-box-shadow: 2px 2px 5px rgba(0,0,0,0.3);
	 -moz-box-shadow: 2px 2px 5px rgba(0,0,0,0.3);
	 box-shadow: 2px 2px 5px rgba(0,0,0,0.3);
	 
	 -webkit-transform: rotate(4deg);
	 -moz-transform: rotate(4deg);
	 -o-transform: rotate(4deg);
	 -ms-transform: rotate(4deg);
	 transform: rotate(4deg);
 }
~~~

效果如下：

![初步效果](/images/Snap7.png)

怎么样，看起来还不错吧！到此为止，我们第一张层叠样式的图片就做完了，其他层叠样式的图片，可以依葫芦画瓢，通过改变转角、位置、和背景颜色来实现。
