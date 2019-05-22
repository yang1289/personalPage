---
title: vue 过滤器
tags:
  - vue.js
  - 前端框架
  - 过滤器
categories:
  - vue.js学习
date: 2019-05-18 15:45:24
---

# vue 过滤器

vue 允许自定义过滤器，可作为常见的文本的格式化进行使用。过滤器可以使用在两个地方，mustache插值和v-bind指令中，过滤器应该添加到表达式中的尾部，用*管道符*进行隔开。
```javascript
{{param | filter}}
```
## 全局过滤器
全局过滤器可以放在任何一个vue实例中进行使用。具体的语法如下。
```javascript
//过滤器的function中的第一个参数，永远都是过滤器的管道符的前一个的数据。
Vue.filter("过滤器名",function(){需要执行的函数})
````

### 如何使用过滤器

使用过滤器只能在插值表达式和v-bind中进行使用。

例如：

```javasc
{{ name | 过滤器名 | 过滤器2 .... }}
```

同时还可以传递参数，例如过滤器名叫cosFilter，有两个参数arg1,arg2。表示如下：

```javas
{{name | cosFilter(arg1,arg2)}}
```

<!--more-->

过滤器定义如下：

```javascript
//过滤器的function中的第一个参数，永远都是过滤器的管道符的前一个的数据。
Vue.filter("cosFilter",function(data,arg1,arg2){
    
})
// 过滤器计算出来的结果需要给一个返回值。
```
> 过滤器是可以通过管道符进行多次的调用。

## 私有过滤器

私有过滤器的与全局过滤器基本是一样，唯一不同的是私有过滤器只会对new Vue() 的dom对象有效，其他的dom对象无法使用这个私有的过滤器。

定义私有过滤器如下：

```javascript
var vm = new Vue({
	el:"#app",
	data:{
	
	},
	filters:{
		"过滤器名称":function(data,arg,...){
			//处理函数
		}
	}
});

//该过滤器只能针对于id = app的dom对象。
// 其他的用法与全局过滤器基本相似。
// 如果全局过滤器的名称与私有过滤器的名称一致，会优先使用私有的过滤器。
```

