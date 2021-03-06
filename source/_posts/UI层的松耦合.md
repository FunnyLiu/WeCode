---
title: UI层的松耦合-编写可维护的javascript第五章
date: 2016-03-14 20:24:13
categories: "javascript"
---
# 编写可维护的javascript第五章UI层的松耦合


---

## **前言**

从这里开始就不再是编程规范方面的问题了，而是在编程实践上对代码进行优化，提高可拓展性和可维护性。

---

## **松耦合**

如果两个组件耦合太紧，则说明一个组件和另一个组件直接相关，如果修改一个组件，则另外一个组件的逻辑也需修改。

如果我们可以做到修改一个组件时不需要更改其他的组件，就做到了松耦合。**松耦合对于代码可维护性来说至关重要**。

---

## **将javascript从css中抽离**

首先我们需要避免css表达式，不要在css中处理有关逻辑的任何事情，虽然IE9以上没有了css表达式，但是各种预编译工具也需要注意。

---

## **将css从javascript中抽离**

虽然我们可以通过javascript来直接操作css：
``` javascript
element.style.color = "red";
```

或者利用cssText批量操作：
``` javascript
element.style.cssText = "color:red;left:10px;";
```

但是最好还是通过class来使用css。将css从javascript中抽离意味着所有的样式信息都应当保持在css中，**javascript不应当直接操作样式**，以便保持和css的松耦合。

有一种情况例外，就是当你需要通过技术来定位时，这样不能预估。

---

## **将javascript从html中抽离**

不要在html中写javascript，比如a标签的onclick：
``` html
<button onclick="doSomething()" >click me</button>

```

尽量使用事件监听器addEventListener通过节点来操作。

在html中嵌入javascript的另一种方法是使用`<script>`标签，标签内包含关联的脚本代码。我们最好将所有的javascript代码都放入外置文件去，最后再打包压缩后放入。

---

## **将html从javascript中抽离**

我们发现问题时，第一时间是寻找html代码的结构问题，但是如果我们发现这段html是通过javascript生成了，就变得复杂了不少。我们可以通过innerHTML来在javascript中操作html，这是非常不好的实践。

有几种方法可以解决这个问题。

### 从服务器加载

通过将模板放置于远程服务器，使用XMLHttpRequest对象来获取，通过向服务器发起请求获取字符串，这样可以让html代码以最合适的方法注入到页面中。

### 客户端模板

客户端模板可以理解为坑，这些坑会被javascript程序替换为数据以保证模板的完整可用。有很多框架，这里就不一一介绍了。


---

## **感悟**

说了这么多，其实就是需要html，css，javascript分开各自管理，不要互相操作互相，从而达到松耦合。













