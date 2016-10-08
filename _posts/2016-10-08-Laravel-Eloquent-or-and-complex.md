---
layout: default
title: Laravel Eloquent or 和 and 复合语句
---
# Laravel Eloquent or 和 and 复合语句

---

作者： [李茂琦](http://blog.limaoqi.com)

日期： {{ page.date | date_to_string }}

在Laravel中使用Eloquent 时，可能需要使用如下的where语句:

~~~sql
select * from foo where (a1 = 1 or a2 = 2) and (b1 = 1 or b2 = 2);
~~~

我们可以使用如下复杂的where语句来实现：

~~~php
Foo::where(function($query){
	$query->where("a1",1)
		  ->orWhere("a2",2);
})->where(function($query){
	$query->where("b1",1)
		  ->orWhere("b2",2);
});
~~~

数值1和2可能希望通过变了传入，这样的话，你可以使用 `function() use($param1,$param2,...){}` 的方式把变量`$param1` `$param2` ... 传入闭包中，如下：

~~~php
$param1 = 1;
$param2 = 2;
Foo::where(function($query) use($param1, $param2){
	$query->where("a1",$param1)
		  ->orWhere("a2",$param2);
})->where(function($query) use($param1, $param2){
	$query->where("b1",$param1)
		  ->orWhere("b2",$param2);
});
~~~

更多复杂的where的复杂语句请参考Laravel的 [Advanced Where Clauses](https://laravel.com/docs/5.2/queries#advanced-where-clauses) 

