---
layout: default
title: SVG 中的图片延时加载实现
---
# SVG 中的图片延时加载实现

---

作者： [李茂琦](http://blogs.limaoqi.com)

日期： {{ page.date | date_to_string }}

最近在项目中遇到需要延时加载SVG中图片的实现，这里做一些记录，以供以后查阅。

项目中，SVG标签包含了多张图片，如果不做延时加载处理，在页面打开时需要加载很长时间，用户体验很不好。
以下实现参考stackoverflow上的答案 [How to implement lazy load for an SVG image element?
](https://stackoverflow.com/questions/49040771/how-to-implement-lazy-load-for-an-svg-image-element)

大致实现逻辑是，将原来svg中image标签的xlink:href的值设置在data-href中，初始svg的image属性不设置xlink:href。待svg下载完毕后，通过修改data-href为xlink:href来加载每一张图片。

>
>svg中大部分浏览器已经丢弃xlink:href的写法，直接使用href，但是Safari 例外。
>

初始svg shape.svg内容如下：
~~~xml
<?xml version="1.0"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" id="svgfile" style="position:absolute;height:600px;width:800px" version="1.1" viewBox="0 0 800 600">
  <image id="image1" class="slide" in="0.0" out="1.3" data-href="logo.png" width="1600" height="900" x="0" y="0" style="visibility:hidden"/>
  <image id="image2" class="slide" in="1.3" out="57.9" data-href="presentation/5e09c765d291f0cb6c1bffc79de2ab34e6e52fef-1526516958711/slide-1.png" width="1600" height="900" x="0" y="0" style="visibility:hidden"/>
  <image id="image3" class="slide" in="57.9" out="295.6" data-href="presentation/5e09c765d291f0cb6c1bffc79de2ab34e6e52fef-1526516958711/slide-2.png" width="1600" height="900" x="0" y="0" style="visibility:hidden" />
  <image id="image4" class="slide" in="295.6" out="392.8" data-href="presentation/5e09c765d291f0cb6c1bffc79de2ab34e6e52fef-1526516958711/slide-3.png" width="1600" height="900" x="0" y="0" style="visibility:hidden" />
</svg>
~~~

在html中引用svg
~~~html
<object id="slides" data="shape.svg" width="100%" height="100%"></object>
~~~

加载图片
~~~javascript
var svgDoc = document.getElementById('slides').contentDocument
var images = svgDoc.getElementsByTagName('image');

if(images && images.length>0){
	images[0].onload = function(){
		images[0].removeAttribute('data-href');
		//load next image
	}

	images[0].setAttribute('href',images[0].getAttribute('data-href'));
}
~~~

上面的代码在safari中是无效的，属性设置之后，图片加载不了。我们对代码做一些修改，以适配safari
~~~javascript
var svgDoc = document.getElementById('slides').contentDocument
var images = svgDoc.getElementsByTagName('image');

if(images && images.length>0){
	images[0].onload = function(){
		images[0].removeAttribute('data-href');
		//load next image
	}

	 var userAgent = navigator.userAgent;
   if (userAgent.indexOf("Safari") > -1
    || navigator.userAgent.indexOf("MicroMessenger") != -1) { //MicroMessenger for wechat webview kernel
    	images[0].setAttributeNS('http://www.w3.org/1999/xlink','xlink:href',images[0].getAttribute('data-href'));
   }else{
   	images[0].setAttribute('href',images[0].getAttribute('data-href'));
   }
	
}
~~~