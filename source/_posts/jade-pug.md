---
title: jade-pug
date: 2017-11-08 20:38:40
tags: 模板
---
jade模板引擎已更名为**[pug](https://pugjs.org/zh-cn/api/getting-started.html)**

### 安装
>		npm install pug-cli -g
>		npm install pug -g

### 标签中的块
有的时候您可能想要写一个非常大的纯文本块。一个典型的例子就是嵌入 Pug 的脚本或者样式。如果要这么做，只需要在标签后面接上一个 .（不要有空格）

### 属性
<!--more-->
  pug使用层级缩减书写，所有元素都为单边标记，也可在jade语法中书写原生语法
  元素属性使用`(key=value)`的形式书写，多个属性间用逗号分隔
	其中div元素标签名可省略`#id`
>		h1#title.h1 pug 
>		a(href='lickcc.top',taget='_black') 匆匆
>		#id.classname

输出html
>		<h1 id="title" class="h1">pug</h1>
>		<a href='lickcc.top' taget="_black">匆匆</a>
>		<div class="classname" id="id"></div>

### 变量
   -var 变量名 = 值  声明一个变量
	 还能将 变量 存在 .json文件中 
	 通过 `#{变量名}` 使用 
	 也可以 `li= 变量名`
   json文件中的变量可以通过 `pug -P -w index.pug -O index.json` 
	 其中 -P 为不格式化文件 -w 为实时编译 -O 为指定json文件
	 声明的变量可以使用JavaScript API 
>		-var course = pug
>		h1 #{course.toUpperCaseto()} good		//toUpperCaseto() 将字符串中的字母转换为大写

输出html
>		<h1>PUG good</h1>

### 表达式
直接上代码吧
#### 条件
>		-var n = 0
		if n == 0
			p #{n}
		else if n > 0
			p #{n}
		ul
			while n<4
				li= n++

输出html
>		<p>0</p>
		<ul>
			<li>0</li>
			<li>1</li>
			<li>2</li>
			<li>3</li>
		</ul>

#### 分支
>		-var name = 'node'
		case name
			when 'name'
				p name
			when 'node'
				p NODE
			default
				p= name	

输出html
>		<p>NODE</p>

#### 循环
循环可以使用 each 或者 for
>		-var arr = ['ejs','jade','pug']
		ul
			each value,key in arr
				li #{key}:#{value}

输出html
>		<ul>
			<li>0:ejs</li>
			<li>1:jade</li>
			<li>2:pug</li>
		</ul>

### mixin 和 block
>		mixin article(title)
		.article
			.article-wrapper
				h1= title
				if block
					block
				else
					p 没有提供内容
		+article('hello world')
		+article('hello world')
			p 这是内容块

输出html
>	  <div class="article">
      <div class="article-wrapper">
        <h1>hello world</h1>
        <p>没有提供内容</p>
      </div>
    </div>
    <div class="article">
      <div class="article-wrapper">
        <h1>hello world</h1>
        <p>这是内容块</p>
      </div>
    </div>

### block
>		block desc
			p desc from layout
		block desc
		block desc

输出html
>		<p>desc from layout</p>
>		<p>desc from layout</p>
>		<p>desc from layout</p>

### 继承
关键字 extends
>		//layout.pug
		doctype html
		<!--[if IE 8]><html class='ie8'><![endif]-->    
		<!--[if IE 9]><html class='ie9'><![endif]-->    
		<!--[if !IE]><html><![endif]-->    
		head
			meta(charset='UTF-8')
			title pug study
			style.
				body{color:#ff00bb}
		body
			block content
		</html>

		//index.pug
		extends layout
		block content
			mixin magic(name,...items)
				ul(class=name)
					each item in items
						li #{item}
			+magic('magic','node','jade','pug')

输出html
>		<!DOCTYPE html>
		<!--[if IE 8]><html class='ie8'><![endif]-->    
		<!--[if IE 9]><html class='ie9'><![endif]-->    
		<!--[if !IE]><html><![endif]-->    
		<head>
			<meta charset="UTF-8">
			<title>pug study</title>
			<style>body{color:#ff00bb}</style>
		</head>
		<body>
			<ul class="magic">
				<li>node</li>
				<li>jade</li>
				<li>pug</li>
			</ul>
		</body>
		</html>

### 包含
关键字 include
>		//layout.pug
		doctype html
		<!--[if IE 8]><html class='ie8'><![endif]-->    
		<!--[if IE 9]><html class='ie9'><![endif]-->    
		<!--[if !IE]><html><![endif]-->    
		head
			include head
		body
			block content
		</html>
		//head.pug
		meta(charset='UTF-8')
		title pug study
		style.
			body{color:#ff00bb}

输出html
>		<!DOCTYPE html>
		<!--[if IE 8]><html class='ie8'><![endif]-->    
		<!--[if IE 9]><html class='ie9'><![endif]-->    
		<!--[if !IE]><html><![endif]-->    
		<head>
			<meta charset="UTF-8">
			<title>pug study</title>
			<style>body{color:#ff00bb}</style>
		</head>
		<body>
			<ul class="magic">
				<li>node</li>
				<li>jade</li>
				<li>pug</li>
			</ul>
		</body>
		</html>

### readerFile
服务端编译 返回html给浏览器
>		//server.js
		var http = require('http');
		var pug = require('pug');
		http.createServer(function(req, res){
			res.writeHead(200,{
				'Content-Type':'text/html',
			})
			// var fn = pug.compile('div #{course}', {});
			// var html = fn({course:'jade'});
			// var html = pug.render('div #{course}',{course:'jade render'});
			var html = pug.renderFile('./index.pug',{course:'pug',pretty:true});
			res.end(html);
		}).listen(1337, '127.0.0.1')
		console.log('server running start')