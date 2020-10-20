---
title: 前端运行时依赖（dependencies）和开发依赖（devDependencies）区分
tags: 小总结
categories: Vue
date: 2020-06-29

---

以前一直对package.json中的dependencies和devDependencies抱有疑问，为什么依赖还要分为运行时（前者）和开发依赖（后者）呢？而且在安装一个依赖时，如何判断是应该放在dependencies还是devDependencies呢？

文章中说npm的依赖共分为以下五类（见npm官方文档[https://docs.npmjs.com/files/package.json#dependencies](https://docs.npmjs.com/files/package.json)）：

- dependencies
- devDependencies
- peerDependencies
- bundledDependencies
- optionalDependencies

## 一、dependencies

这是npm最基本的依赖，写在一个简单的对象中，将依赖程序包映射到版本范围。比较常用，命令为

```
npm install/i xxx@version -S/--save
```

如果不指定版本号version，则默认安装最新版本。

## 二、devDependencies

顾名思义devDenpendencies是开发依赖，也就是开发中所使用的的依赖，**线上生产环境上并不需要他们**。命令为

```
npm install/i xxx -D/--save-dev
```

npm官方文档将它定义为开发中所使用的外部的测试或者文档框架。

![img](https://img2018.cnblogs.com/i-beta/1457120/202001/1457120-20200116114639151-441031359.png)

 文章中提到，开发依赖的目的是为了减少在安装依赖时node_modules的体积，提升安装依赖的速度，节省线上及其的硬盘资源以及部署上线的时间。那么那些依赖可以划分为开发依赖呢？

### 1、构建工具

点名webpack、webpack-cli、rollup（其实我没用过）等等。构建工具是为了生成生产环境的代码，在线上使用的代码其实是他们工作的结果，也就是说在线上时，并不需要他们，因此他们可以归为开发依赖。

当然他们衍生出来的插件，如xxx-webpack-plugin，也属于开发依赖。

### 2、预处理器

指的是对源代码进行一定的处理并生成最终代码的工具。常见的有css中的less、scss、sass、stylus，js中的typescript、coffee-script、babel等等。

以 babel 为例，常用的有两种使用方式。其一是内嵌在 webpack 或者 rollup 等构件工具中，一般以 loader 或者 plugin 的形式出现，例如 babel-loader。其二是单独使用（小项目较多），例如 babel-cli。babel 还额外有自己的插件体系，例如 xxx-babel-plugin。类似地，less 也有与之对应的 less-loader 和 lessc。这些都算作开发依赖。

在 babel 中还有一个注意点，那就是 babel-runtime 是 dependencies 而不是 devDependencies。

### 3、测试工具

当然在线上时是用不到测试工具的，因此他们归入开发依赖。常用如chai、e2e等等。

### 4、其他

最后一类很难概括，是开发时需要使用的，实际上显示要么是已经打包成最终代码了，要么是不需要了。比如 webpack-dev-server 支持开发热加载，线上是不用的；babel-register 因为性能原因也不能用在线上。其他还可能和具体业务相关。

## 三、peerDependencies、bundleDependencies、optionalDependencies

作为npm包的使用者，前两项其实已经足够日常使用了，后三项是作为npm包的发布者需要考虑使用的，在此不做过多赘述，如果有兴趣可以查阅原文章以及npm的文档。