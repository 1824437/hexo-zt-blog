---
title: SASS
categories: web
tags: [sass,css]
---

语法大全 [https://www.sass.hk/docs/](https://www.sass.hk/docs/)

### 规则

* 一个变量不存在时，引用该变量的样式声明会被删除。


### 语法

1. 嵌套
	* 声明嵌套
	* 属性嵌套
	* 父选择器 &

2. 占位符 %

3. 注释/* */, //, 其中 //注释在编译时删除

4. 变量 $

	* 局部变量变全局变量添加!global声明`$width: 12px !global`  
	* 特殊变量, 一个复合值如font:"12px/1.5";需要#{vername}
	
		```SASS
		$var1:12px;$var2:1.5;$var3=.header; 
		font:#{$var1}/#{$var2} 
		.main #{$var3}:after{}
		```
		
	* 定义变量值为多个值时，如字体，应该用#{}
	
	`$ff: #{"Helvetica Neue", Helvetica, STHeiTi, sans-serif, st}`
	
	* 数组，map
	
		```scss
		
		$px2: 10px 20px 30px 40px;
		$px3: 10px 20px, 30px 40px; //分组
		$px4: (10px 20px) (30px 40px); //分组
		$util: (padding:20px,margin:30px); //对象
		
		```

5. 运算
	* 数字运算符 +, -, *, /,%
	* 关系运算符 <, >, <=, >=
	* 相等 == !=
	* 插值#{}
		`content:"a#{1+3}c"`
	* 布尔运算 and, or, not  
	

6. 函数

	所有函数列表[http://sass-lang.com/documentation/Sass/Script/Functions.html](http://sass-lang.com/documentation/Sass/Script/Functions.html)
	* 常用
		* ie-hex-str()
		* nth()
		* map-get()

	* 自定义函数 @function

7. 默认值 !default 未定义的变量取带默认值的值，否则不取默认值
8. @import 导入sass文件
	
	* 与CSS原生的@import的区分：当发生以下情况不会使用sass解析
		1. 文件扩展名是.css
		2. 文件名以协义开头
		3. 使用url()包裹文件
		4. 带有媒体查询的    
		`@import url("//qunarzz.com/css/reset.css") screen`
		
		5. 以上情况除外即使不在扩展名也会查找sass文件，找到即导入。 
		6. 当scss文件不想被处理时，在文件改名，前加下划线即可。
			`_a.scss; @import a.scss`
			
		7. 不可以在混合指令mixin或控制指令(if,for...)中使用导入。
	
9. @media 支持媒体查询，支持动态变量插入
10. @extend 样式继承`.a{} .b{@extend .a;}`

	* 样子继承会无限继承底层样式及超类自身继承的样式。所以在.a下定义的其它样式都会继承，所以.a @extend的样式也会被继承。
	* 可以继承多个超类，如发生冲突，后定义的会覆盖先定义的。
	* 使用%占位继承,%定义部分单独使用不会生效
		
		```sass
		#a h1%level{color:red};
		//a h1不会有color:red样式
		.l3 {@extend %level}
		// #a h1.l3 {color:red;}
		
		```
	* 在指令中扩展时，只能扩展指令内部的样式，外部样式不能。
	
11. 控制指令 @if @else if @else @for @each @while

	`@if <condition> {} @else {}`
	

12. 混合指令 mixin
	
	* @include来应用混合
	
		```scss
		@mixin fonts {}
		a {@include fonts}
		```
	* 加入参数

		```scss
		//参数名格式与定义变量是一样的
		@mixin fonts ($color, $fontsize) {
			color: $color;
			font-size:$fontsize;
		}
		a {@include fonts}
		```
	
	* 不定参数	(和JS解构赋值及剩余参数差不多）
	
	```scss
	@mixin fonts ($args...){
		box-shadow: $args
	}
	.a{@include fonts(9px 1px 2px 4px #555)}
	//或者
	$xxx = 9px 1px 2px 4px;
	.a{@include fonts($xxx...)
	```
	
	

-
	
	
	
	

	
	
## 常用函数

* global-variable-exists

	检查一个全局变量(`! global`)是否存在,变量名引用不需要$  
	`@if global-variable-exists(){}`
	

 

## 变量; map变量; list变量; 特殊变量;
**定义**  

```scss
$px: 20px;
$px2: 10px 20px 30px 40px;
$px3: 10px 20px, 30px 40px;
$px4: (10px 20px), (30px 40px);
$util: (padding:20px,margin:30px);

```

**使用**

``` scss
.a{
  font-size: $px1; // 20px;
}
.b{
  font-size: nth($px2,3); // 30px;
}
.c{
  font-size: nth($px3,1); // 10px 20px
}
.d{
  font-size: nth($px4,2); //30px 40px
}
.e{
  padding: map-get($util,padding); // 20px
}

```

## 占位符号
**定义**

``` scss
%base {
	margin:10px;padding:20px;
}
```
**使用**

``` scss
.a{
	@extend %base; //margin:10px;padding:20px;
	color:20px;
}
```

## 嵌套

**使用**

``` scss
.a{
	font-size:10px;
	&:hover{ // .a:hover
	};
	.title{ // .a .title
		font-size:20px;
		&_bd{ // .a .title_bd
			font-size:30px;
		}
	}
	border:{
		style: solid;
		width: 1px;
		left: { // border-left
			color: red;
		}
	}
}
```