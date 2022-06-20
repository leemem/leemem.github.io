---
layout: default
title: Laravel Eloquent 添加自定义字段
---
# Laravel Eloquent 添加自定义字段

---

作者： [李茂琦](http://blogs.limaoqi.com)

日期： {{ page.date | date_to_string }}

在Laravel中使用Eloquent 时，我希望可以获取自定义的字段，这个字段在数据库中是不存在的，我可以这么做

在对应的模型(Modle)文件中，添加如下信息:

~~~php
protected $appends = array('conf_elapse');
public function getConfElapseAttribute()
    {	
    	$starttime = $this->attributes['conf_runningtime'];
    	$curtime = date("Y-m-d H:i:s");    	
    	$elapse = strtotime($curtime)-strtotime($starttime);    	

    	//这里返回你想要给这个字段返回的值
    	return $elapse;    	
    }
~~~

这里有几点需要注意的：

> 1. 添加字段，需要把字段名称添加到`$appends`中
> 2. 使用`get`函数获取属性值，`get`函数命名规则使用驼峰命名规则，首字母和下划线之后的字母需要大写，最后加上`Attribute`
> 3. 在`get`函数中，可以使用`$this->attributes['filed_name']`获取指定字段`filed_name`的值

