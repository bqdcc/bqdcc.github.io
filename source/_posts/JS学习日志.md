---
title: JS学习日志
date: 2018-01-19 20:40:41
tags: javascript
---

#### [好看的if...else](https://mp.weixin.qq.com/s/cInFsWjCRGtKnZ17IfFXUw)
首先看个难看点的。。。

```javascript
    var code = 0;
    if(code=0){
        show('显示')
    }else if(code=1){
        show('不显示')
    }
```
但它可以好看 一点
```javascript
    var codeInfo=['显示','不显示'];
    show(codeInfo[code]);
```
<!--more-->
可以是对象的形式
```javascript
    var codeInfo={
        0:'显示',
        1:'不显示'
    }
    show(codeInfo[code]);
```
可以是包含func
```javascript
    var codeFunc={
        0:function(){...},
        1:function(){...}
    }
    codeFunc[code]();
```
[原作者的话：](https://mp.weixin.qq.com/s/cInFsWjCRGtKnZ17IfFXUw)
[我以前的后端领导跟我讲过

写代码就跟建管道一样

一条管道永远是最易维护的

所以我们要把预先不要的东西放到最上面

比如]

```javascript
    if(isA){
        func(){...}
    }else{
        return
    }

    if(!isA) return;
    func(){...}
```

#### 链式三元运算符
我们经常这样做
```javascript
    //根据A的值为B赋值
    //如果A大于5，则赋值200，否则赋值1
    var B = A > 5 ? 200 : 1;
```
但如果你想这么做，当A多少时如何，当A大于多少时如何，链式三元运算非常好用
```javascript
    var B = 
    A < 5 ? 200 : 
    A < 10 ? 300 :
    A < 15 ? 400 :
    A < 20 ? 500 :
    1;
    //比 if(A<5) 更有效率
```

#### 双向绑定
```javascript
    var obj = {}
    Object.defineProperty(obj,'val',{
        get:function(){
            console.log('得到值');
        },
        set:function(val){
            console.log('设置值');
            //此处写改变值的操作 如根据id修改innerHTML
        }
    });
    document.getElementById('input').addEventListener('keyup',function(){
        obj.val = event.target.value;
    })
```

#### 隐式转换
```javascript
    var a = 1,b="2";
    console.log(a+b);//12
    console.log(a + + b);//3
    console.log(a - - b);//3
```

#### js去除字符串两端空格
[MDN知识点](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/Trim)
用处有 处理输入框的输入(连续的空格)
```javascript
    var orig = '   foo  ';
    console.log(orig.trim()); // 'foo'
    // 另一个.trim()例子，只从一边删除
    var orig = 'foo    ';
    console.log(orig.trim()); // 'foo'
```
##### 兼容旧环境
如果 trim() 不存在，可以在所有代码前执行下面代码
```javascript
    if (!String.prototype.trim) {
        String.prototype.trim = function () {
            return this.replace(/^[\s\uFEFF\xA0]+|[\s\uFEFF\xA0]+$/g, '');
        };
    }
```

#### null 与 undefined
undefined ----- 不存在
null      ----- 存在但没有
```javascript
    undefined == null //结果 true
    undefined === null //结果 false
    var obj = {a:1,b:2}
    obj.c == null //结果 true
    obj.c === null //结果 false
    obj.c === undefined //结果 true
    //判断对象是否存在对应属性时建议用双等号 其他等值判断建议用三等号
```





[渴望下一个知识点的到来。。。](/)