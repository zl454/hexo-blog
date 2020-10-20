---
title: Object API
tags: 小总结
categories: JavaScript
date: 2020-10-15
---

参考：[MDN-Object](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object)

`Object` 构造函数创建一个对象包装器

几乎所有的对象都是 `Object` 类型的实例，它们都会从`Object.prototype`继承属性和方法。`Object` 构造函数为给定值创建一个对象包装器。`Object`构造函数，会根据给定的参数创建对象，具体有以下情况：

- 如果给定值是 [`null`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/null) 或 [`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)，将会创建并返回一个空对象
- 如果传进去的是一个基本类型的值，则会构造其包装类型的对象
- 如果传进去的是引用类型的值，仍然会返回这个值，经他们复制的变量保有和源对象相同的引用地址

## Object 构造函数的属性

| 属性             | 作用                                 |
| ---------------- | ------------------------------------ |
| Object.length    | 值为1                                |
| Object.prototype | 可以为所有 Object 类型的对象添加属性 |

## Object 构造函数的方法

| 方法                              | 作用                                                         |
| --------------------------------- | ------------------------------------------------------------ |
| Object.assign()                   | 通过复制一个或多个对象来创建一个新的对象。                   |
| Object.create()                   | 使用指定的原型对象和属性创建一个新对象。                     |
| Object.defineProperty()           | 给对象添加一个属性并指定该属性的配置。                       |
| Object.defineProperties()         | 给对象添加多个属性并分别指定它们的配置。                     |
| Object.entries()                  | 返回给定对象自身可枚举属性的 `[key, value]` 数组。           |
| Object.freeze()                   | 冻结对象：其他代码不能删除或更改任何属性。                   |
| Object.getOwnPropertyDescriptor() | 返回对象指定的属性配置。                                     |
| Object.getOwnPropertyNames()      | 返回一个数组，它包含了指定对象所有的可枚举或不可枚举的属性名。 |
| Object.getOwnPropertySymbols()    | 返回一个数组，它包含了指定对象自身所有的符号属性。           |
| Object.getPropertyOf()            | 返回指定对象的原型对象。                                     |
| Object.is()                       | 比较两个值是否相同。所有 NaN 值都相等（这与==和===不同）。   |
| Object.isExtensible()             | 判断对象是否可扩展。                                         |
| Object.isFrozen()                 | 判断对象是否已经冻结。                                       |
| Object.isSealed()                 | 判断对象是否已经密封。                                       |
| Object.keys()                     | 返回一个包含所有给定对象**自身**可枚举属性名称的数组。       |
| Object.preventExtensions()        | 防止对象的任何扩展。                                         |
| Object.seal()                     | 防止其他代码删除对象的属性。                                 |
| Object.setPropertyOf()            | 设置对象的原型（即内部 `[[Prototype]]` 属性）。              |
| Object.values()                   | 返回给定对象自身可枚举值的数组。                             |

