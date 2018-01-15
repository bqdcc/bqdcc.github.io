---
title: sass
date: 2017-11-15 14:19:25
tags: css
---

## [sass,scss](https://www.sass.hk/docs/)

#### 安装 
	https://www.sass.hk/install/

<!--more-->

#### 运算
	支持数字的加减乘除、取整等运算 (+, -, *, /, %)，如果必要会在不同单位间转换值。
	`()` 影响运算顺序

#### 函数

	Sass 支持自定义函数，并能在任何属性值或 Sass script 中使用：

		$grid-width: 40px;
		$gutter-width: 10px;

		@function grid-width($n) {
			@return $n * $grid-width + ($n - 1) * $gutter-width;
		}

		#sidebar { width: grid-width(5); }
		编译为

		#sidebar {
			width: 240px; }

#### 变量

		$main-fs:16px;

sass的变量名可以与css中的属性名和选择器名称相同，包括中划线和下划线。这完全取决于个人的喜好，有些人喜欢使用中划线来分隔变量中的多个词（如$highlight-color），而有些人喜欢使用下划线（如$highlight_color）。使用中划线的方式更为普遍，这也是compass和本文都用的方式。
$link-color和$link_color指向的是同一个变量。

		$highlight-color: #F90;
		.selected {
			border: 1px solid $highlight-color;
		}

		//编译后

		.selected {
			border: 1px solid #F90;
		}

<!--more-->

#### CSS嵌套规则

		#content {
			article {
				h1 { color: #333 }
				p { margin-bottom: 1.4em }
			}
			aside { background-color: #EEE }
		}

		编译后

		#content article h1 { color: #333 }
		#content article p { margin-bottom: 1.4em }
		#content aside { background-color: #EEE }

父选择器的标识符&;

		div{
			background-color: #fff;
			&:hover{
				background-color: #f0f0f0;
			}
		}

		编译后

		div { background-color: #fff; }
		div:hover { background-color: #f0f0f0; }

在为父级选择器添加：hover等伪类时，这种方式非常有用。同时父选择器标识符还有另外一种用法，你可以在父选择器之前添加选择器。举例来说，当用户在使用IE浏览器时，你会通过JavaScript在<body>标签上添加一个ie的类名，为这种情况编写特殊的样式如下：

		#content aside {
			color: red;
			body.ie & { color: green }
		}

		编译后

		#content aside {color: red};
		body.ie #content aside { color: green }

群组选择器

		.container {
			h1, h2, h3 {margin-bottom: .8em}
		}

		编译后

		.container h1, .container h2, .container h3 { margin-bottom: .8em }

		nav, aside {
			a {color: blue}
		}

		编译后

		nav a, aside a {color: blue}

属性嵌套

		nav {
			border: {
				style: solid;
				width: 1px;
				color: #ccc;
			}
		}

		编译后

		nav {
			border-style: solid;
			border-width: 1px;
			border-color: #ccc;
		}

对于属性的缩写形式，你甚至可以像下边这样来嵌套，指明例外规则：

		nav {
			border: 1px solid #ccc {
				left: 0px;
				right: 0px;
			}
		}

这比下边这种同等样式的写法要好：

		nav {
			border: 1px solid #ccc;
			border-left: 0px;
			border-right: 0px;
		}

#### @import 'xxx', 'xxx'; 导入 sass文件

sass局部文件的文件名以下划线开头。这样，sass就不会在编译时单独编译这个文件输出css，而只把这个文件用作导入。如`_variables.scss`

		_blue-theme.scss

		aside {
			background: blue;
			color: white;
		}

		.blue-theme {@import "blue-theme"}

		//生成的结果跟你直接在.blue-theme选择器内写_blue-theme.scss文件的内容完全一样。

		.blue-theme {
			aside {
				background: blue;
				color: #fff;
			}
		}

#### 注释

		body {
			color: #333; // 这种注释内容不会出现在生成的css文件中
			padding: 0; /* 这种注释内容会出现在生成的css文件中 */
		}

#### 混合器

	定义了一个非常简单的混合器，目的是添加跨浏览器的圆角边框。

		@mixin rounded-corners {
			-moz-border-radius: 5px;
			-webkit-border-radius: 5px;
			border-radius: 5px;
		}

	使用

		.notice {
			background-color: green;
			border: 2px solid #00aa00;
			@include rounded-corners;
		}

	编译后

		.notice {
			background-color: green;
			border: 2px solid #00aa00;
			-moz-border-radius: 5px;
			-webkit-border-radius: 5px;
			border-radius: 5px;
		}

混合器中不仅可以包含属性，也可以包含css规则，包含选择器和选择器中的属性，如下代码:

		@mixin no-bullets {
			list-style: none;
			li {
				list-style-image: none;
				list-style-type: none;
				margin-left: 0px;
			}
		}
当一个包含css规则的混合器通过@include包含在一个父规则中时，在混合器中的规则最终会生成父规则中的嵌套规则。举个例子，看看下边的sass代码，这个例子中使用了no-bullets这个混合器：

		ul.plain {
			color: #444;
			@include no-bullets;
		}

	编译后:

		ul.plain {
			color: #444;
			list-style: none;
		}
		ul.plain li {
			list-style-image: none;
			list-style-type: none;
			margin-left: 0px;
		}

混合器传参

		@mixin link-colors($normal, $hover, $visited) {
			color: $normal;
			&:hover { color: $hover; }
			&:visited { color: $visited; }
		}

		a {
			@include link-colors(blue, red, green);
		}

		//Sass最终生成的是：

		a { color: blue; }
		a:hover { color: red; }
		a:visited { color: green; }

	sass允许通过语法$name: value的形式指定每个参数的值。这种形式的传参，参数顺序就不必再在乎了，只需要保证没有漏掉参数即可：

		a {
				@include link-colors(
					$normal: blue,
					$visited: green,
					$hover: red
			);
		}

默认 参数
	参数默认值使用$name: default-value的声明形式，默认值可以是任何有效的css属性值，甚至是其他参数的引用，如下代码：

		@mixin link-colors($normal,$hover: $normal,$visited: $normal) {
			color: $normal;
			&:hover { color: $hover; }
			&:visited { color: $visited; }
		}
	
	如果像下边这样调用：`@include link-colors(red)` `$hover`和`$visited`也会被自动赋值为`red`。

#### 继承

	选择器继承是说一个选择器可以继承为另一个选择器定义的所有样式。这个通过@extend语法实现，如下代码:

		//通过选择器继承继承样式
		.error {
			border: 1px solid red;
			background-color: #fdd;
		}
		.seriousError {
			@extend .error;
			border-width: 3px;
		}

	编译后

		.error, .seriousError {
			border: 1px solid red;
			background-color: #fdd;
		}
		.seriousError {
			border-width: 3px;
		}		

	如果不希望 `.error` 被编译到 css 文件中 可以 是 `%error` 的形式声明 

		%error {
			border: 1px solid red;
			background-color: #fdd;
		}
		.seriousError {
			@extend %error;
			border-width: 3px;
		}

	编译后

		.seriousError {
			border: 1px solid red;
			background-color: #fdd;
		}
		.seriousError {
			border-width: 3px;
		}		

	.seriousError不仅会继承.error自身的所有样式，任何跟.error有关的组合选择器样式也会被.seriousError以组合选择器的形式继承，如下代码:

		//.seriousError从.error继承样式
		.error a{  //应用到.seriousError a
			color: red;
			font-weight: 100;
		}
		h1.error { //应用到hl.seriousError
			font-size: 1.2rem;
		}

