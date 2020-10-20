---
title: nodejs 和 yarn 环境变量配置
tags: nodejs
categories: 笔记
date: 2020-10-13  

---

# nodejs 环境变量配置

npm config list 获取npm配置信息

## 环境变量的配置

说明：这里的环境配置主要配置的是npm安装的全局模块所在的路径，以及缓存cache的路径，之所以要配置，是因为以后在执行类似：npm install express [-g] （后面的可选参数-g，g代表global全局安装的意思）的安装语句时，会将安装的模块安装到【C:\Users\用户名\AppData\Roaming\npm】路径中，占C盘空间。
例如：我希望将全模块所在路径和缓存路径放在我node.js安装的文件夹中，则在我安装的文件夹【D:\Program Files\nodejs】下创建两个文件夹【node_global】及【node_cache】

**创建完两个空文件夹之后，打开cmd命令窗口，输入**

```bash
npm config set prefix "D:\ProgramFiles\nodejs\node_global"
npm config set cache "D:\ProgramFiles\nodejs\node_cache"
```

**接下来设置环境变量**

> 在【系统变量】下新建【NODE_PATH】，输入【D:\Program Files\nodejs\node_global\node_modules】
>
> 将【用户变量】下的【Path】修改为【D:\Program Files\nodejs\node_global】

**配置完成**

# yarn 环境变量配置

yarn config list 获取yarn配置信息

## 更改缓存位置

```bash
yarn cache dir #查看缓存位置
yarn cache clean #清除缓存,在目录
yarn config set cache-folder "D:\ProgramFiles\Yarn\yarn_cache"  #设置D盘
yarn cache dir #在输出一下目录 看看缓存位置
```

## 更改全局位置

```
yarn global dir  //查看全局位置
yarn config  set global-folder "D:\ProgramFiles\Yarn\yarn_global"
yarn global dir  //在执行查看位置,已经被修改
```

## 下载模块测试

```
# 必须先下载模块,会自动创建 .bin 目录，再向下走
yarn global add yrm //全局安装
```

## 重新设置bin目录

```
yarn global bin //默认是 c:/,修改到 D:盘
yarn config set prefix "D:\ProgramFiles\Yarn\yarn_global\node_modules\.bin"
```

## 上一步可能没用，需要修改一环境变量

> 将【用户变量】下的【Path】修改为【D:\Program Files\Yarn\yarn_global\node_modules\\.bin】

## 再次打开新的命令窗口输入 yrm ls 测试是否安装成功