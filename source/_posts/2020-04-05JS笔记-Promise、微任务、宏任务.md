---
title: Promise、微任务、宏任务
tags: JavaScript基础
categories: JavaScript
date: 2020-04-05 11:09:12
---

# Promise

Promise的构造函数接收一个参数，是函数，并且传入两个参数：resolve，reject，分别表示异步操作执行成功后的回调函数和异步操作执行失败后的回调函数。其实这里用“成功”和“失败”俩标书并不准确，按照标准来讲，resolve是讲Promise的状态置为fullfiled，reject是将Promise的状态置为rejected。

```javascript
var promise=new Promise(function (resolve,reject) {
    var img=new Image();
    img.src="./img/1-.jpg";
    img.onload=function () {
        resolve(this);
    };
    img.onerror=function () {
        reject("加载失败");
    }
});
promise.then(function (data) {
    console.log(data,"__________");
},function (error) {
    console.log(error,"|||||||||");
})
```

**链式加载**

```javascript
function loadImg(src) {
    return new Promise(function (res,rej) {
        var img=new Image();
        img.onload=function () {
            res(this);
        };
        img.onerror=function () {
            rej("加载错误");
        };
        img.src=src;
    });
}

var arr=[];
loadImg("./img/3-.jpg").then(function (data) {
    arr.push(data);
    return loadImg("./img/4-.jpg");
},function (error) {
    console.log(error);
}).then(function (data) {
    arr.push(data);
    console.log(arr);
},function (error) {
    console.log(error);
})
```

# 同步和异步

**同步**

​	是指代码从上向下执行,执行完一条,才去执行下一条,是按照顺序按照步骤的执行

**异步**

​	代码执行需要有一个过程,或者需要一定的时间,或者开始的时间不确定,这时候我们先让别的不相关的代码执行,而当前代码当执行完成后去执行一个回调函数

**注意**,如果代码写在script中,并且写在函数外部,那么这个代码他只能执行一次,并且是在开始时就同步执行了,显然这种方式不利于代码中出现交互,因此,代码就需要写在函数中,减少代码之间同步执行方式,函数外通常仅用来定义变量(全局)和执行初始化函数

# 宏任务和微任务

**宏任务：**  setTimeout setInterval

**微任务：**  Promise

同一个队列中,先执行的是宏任务，再执行其他任务，最后执行微任务

在当前队列中出现的异步，如果是微任务就会放在当前任务队列最底端,如果当前队列出现的异步是宏任务，就会出现在下一个队列最顶端

也就是说在同一个队列中触发异步,微任务先执行,宏任务后执行

```javascript
setTimeout(function () {
    console.log("1");
},0);//宏任务
new Promise(function (res,rej) {
    console.log("2");//同步执行
    res();
}).then(function () {
    console.log("3");//异步执行
});
console.log("4")
//打印顺序2，4，3，1
```

# 鼠标事件

|   事件名    | 作用                                          |
| :---------: | :-------------------------------------------- |
|    click    | 单击(鼠标左键)                                |
|  dblclick   | 双击                                          |
|  mousemove  | 在目标上移动鼠标                              |
|  mouseover  | 鼠标经过  针对e.target 目标(目标改变就会触发) |
|  mouseout   | 鼠标滑出 针对e.target 目标(目标改变就会触发)  |
| mouseenter  | 鼠标进入 针对e.currentTarget侦听的对象        |
| mouseleave  | 鼠标离开 针对e.currentTarget侦听的对象        |
|  mousedown  | 按下鼠标(鼠标三键都会触发)                    |
|   mouseup   | 释放鼠标(鼠标三键都会触发)                    |
| contextmenu | 右键点击呼出菜单                              |

