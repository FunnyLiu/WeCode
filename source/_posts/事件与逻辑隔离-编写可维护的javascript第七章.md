---
title: 事件与逻辑隔离-编写可维护的javascript第七章
date: 2016-03-14 20:16:58
categories: "javascript"
---
# 编写可维护的javascript第七章事件处理


---

## **前言**

前面说到了对于全局变量的处理方法，现在谈谈有关事件处理。

---

## **典型用法**

当事件触发时，事件对象event会作为回调参数1传入事件处理程序中。event对象包含所有和事件相关的信息，包括事件的宿主target以及其他和事件类型相关的数据。

在很多场景中，我们只是用到了event所提供信息的一小部分：

``` javascript
function handleClick(event){
    var popup = document.getElementById("popup");
    popup.style.left = event.clientX + "px";
    popup.style.top = event.clientY + "px";
    popup.className = "reveal";
}

addListener(element,"click",handleClick);
```

这样开起来没有什么问题，其实是不好的写法，因为这样做法有局限性。

---

## **规范1：隔离应用逻辑**

上面那段代码的写法，第一个问题是事件处理程序包含了应用逻辑。应用逻辑是和应用相关的功能性代码，而不是和用户行为相关的。

在上面的实例中，应用逻辑是在特定位置显示一个弹出框。

**将应用逻辑从所有事件处理程序中抽离出来**的做法是一种最佳事件，因为说不定什么时候**其他地方就会触发同一段逻辑**。比如，有时我们需要在用户将鼠标移动到某个元素上时判断是否显示弹出框，或者当按下键盘上的某个键时也作同样的逻辑判断。

将应用逻辑放置于事件处理程序中的另一个缺点是和测试有关的。测试时需要直接触发功能代码，而不必通过模拟对元素的点击来触发。

所以我们对上面的代码进行改造，将处理弹出框逻辑的代码放入一个单独的函数中：

``` javascript
var MyApplication = {
    handleClick: function(event){
        this.showPopup(event);
    },
    
    showPopup: function(event){
        var popup = document.getElementById("popup");
        popup.style.left = event.clientX +"px";
        popup.style.top = event.clientY +"px";
        popup.className = "reveal";
    }

};

addListener(element,"click",function(event){
    MyApplication.handleClick(event);
});
```

现在handleClick方法只做了一件事情，就是调用showPopup。应用逻辑被剥离出去了，复用性也增强了。

---

## **规则2：不要分发事件对象**

上面的代码虽然将应用逻辑分离出去了，还是存在另外一个问题，就是event对象被无节制的分发了，我们修改如下：


``` javascript
var MyApplication = {
    handleClick: function(event){
        this.showPopup(event.clientX,event.clientY);
    },
    
    showPopup: function(x,y){
        var popup = document.getElementById("popup");
        popup.style.left = x +"px";
        popup.style.top = y +"px";
        popup.className = "reveal";
    }

};

addListener(element,"click",function(event){
    MyApplication.handleClick(event);
});
```

这样的写法，可以在测试或代码的任一位置都轻易地直接调用这段逻辑：

``` javascript
MyApplication.showPopup(10,10);
```

当处理事件时，最好让事件处理程序成为接触到event对象的唯一函数。**事件处理程序应当在进入应用逻辑之前针对event对象执行任何必要的操作，包括阻止默认事件或阻止事件冒泡**，都应当直接包含在事件处理程序中。



