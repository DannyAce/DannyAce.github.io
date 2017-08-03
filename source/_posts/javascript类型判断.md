---
title: javascript类型判断
date: 2017-05-13 14:59:40
tags:
---

js六大数据类型：number、string、object、Boolean、null、undefined

## typeof
最常见的类型判断方法是：typeof，typeof能判断绝大多数的类型，如

```
type of 'aaa'  //string
type of 111 //number
typeof true //boolean
typeof a  //undefined
typeof {}  //object
typeof function(){}   //function
typeof undefined  //undefined

```
但如果是以下情况
```
typeof []  //object
typeof null  //object
var aaa= new Date();
typeof aaa  //object

```
可以看到，这些情况下返回的都是object，那到底如何能够更细分，有没有比typeof更好的方法来判断类型？

## Obejct.prototype.toString
答案当然是肯定的！它就是Obejct.prototype.toString
我们先来看下es5规范中对它的定义：[15.2.4.2](https://es5.github.io/#x15.2.4.2)
翻成中文就是
> 在toString方法被调用时,会执行下面的操作步骤:
如果this的值为undefined,则返回"[object Undefined]".
如果this的值为null,则返回"[object Null]".
让O成为调用ToObject(this)的结果.
让class成为O的内部属性[[Class]]的值.
返回三个字符串"[object ", class, 以及 "]"连接后的新字符串.

通过规范，我们至少知道了调用 Object.prototype.toString 会返回一个由 "[object " 和 class 和 "]" 组成的字符串，而 class 是要判断的对象的内部属性。

让我们写个demo：
```javascript
var   gettype=Object.prototype.toString
Object.prototype.toString.call('aaaa')     // [object String]
Object.prototype.toString.call(2222) // [object Number]
Object.prototype.toString.call(true)  // [object Boolean]
Object.prototype.toString.call(undefined)  // [object Undefined]
Object.prototype.toString.call(null)  // [object Null]
Object.prototype.toString.call({})   // [object Object]
Object.prototype.toString.call([])    // [object Array]
Object.prototype.toString.call(function(){})     // [object Function]
Object.prototype.toString.call(new Date())    // [object Date]

```
除了上面这些常用类型，其实包括dom也是可以判断的
```javascript
Object.prototype.toString.call(document)   // [object HTMLDocument]
Object.prototype.toString.call(document.body) // [object HTMLBodyElement]
```
所以Object.prototype.toString几乎可以满足js中所有对类型的判断
一般实际使用时候可以做封装：
```javascript
var  util = {
    isObj:function(o){
        return  Object.prototype.toString.call(o)=="[object Object]";
     },
     isArray:function(o){
        return  Object.prototype.toString.call(o)=="[object Array]";
     }, 
     ........
}

```

## Array.isArray
针对数组，本身有一个原生方法来判断
```javascript
Array.isArray([1,2,3])  //true

```

## 结语
到这里，常用的js类型判断基本都在这里，可以根据项目实际情况选择使用。



