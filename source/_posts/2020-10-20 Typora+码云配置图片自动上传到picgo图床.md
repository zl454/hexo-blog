---
title: Typora+码云配置图片自动上传到picgo图床
tags: 教程
categories: 教程
date: 2020-10-20
---

# 所需软件

**typora：**https://www.typora.io/

**picgo：**https://github.com/Molunerfinn/PicGo/releases

**Gitee：**https://gitee.com/

![image-20201020223451565](https://gitee.com/zl454/pic-go/raw/master/img//20201020223549.png)

# 具体操作

要是分为两部分操作，第一部分是Typora的配置，第二部分是Picgo+Gitee的在线图床配置

## Typora配置

打开Typora后，点击菜单栏-文件-偏好设置

找到其中的“图像”设置，将其设置为如下内容

![image-20201020223954050](https://gitee.com/zl454/pic-go/raw/master/img//20201020223954.png)

1）**插入图片时...**下边的选择框内选择**上传图片**

2）勾选上“**对本地位置的图片应用上述规则**”

3）上传服务选择**PicGo(app)**

4）将**PicGo路径**项设置为本地PicGo的安装路径

这样，我们就把Typora配置好了，下面我们进行PicGo+Gitee的网络图床构建

## 网络图床配置

安装好 PicGO 之后，需要给 PicGO 配置插件以支持 Gitee 图床

点击左边的 **插件设置** 一栏，在输入框输入 "gitee" ，如下

![image-20201020224128722](https://gitee.com/zl454/pic-go/raw/master/img//20201020224128.png)

安装插件后，左侧选项栏 **图床设置** 会多一个  **Gitee图床**

![image-20201020224706670](https://gitee.com/zl454/pic-go/raw/master/img//20201020224706.png)

接下来配置 Gitee 仓库以存储图片

1）进入[https://gitee.com/](https://link.zhihu.com/?target=https%3A//gitee.com/)，没有账号的话，先注册账号，注册以后登录，新建一个**公开仓库**，名字为PicGO（可以自己起其他名字）

2）点击右上角，进入**设置**，在左侧的**安全设置-私人令牌**处生成新令牌。（注意：只需勾选 **projects** ，生成的新令牌只会显示一次，一定要保存好！！！）

我们需要做的如下：

1. owner 用户名
2. repo为仓库名（不支持大写，PicGO 需要使用 pig-go 表示 ）
3. path为仓库下用于存储图片的路径，这个可以自行选择
4. Token为刚才在Gitee生成的私人令牌，粘贴到这里就行
5. message 必须填写

## 结束

经过上述操作，我们就把typora+picgo+gitee成功配置好了，之后当我们将本地的图片粘贴到markdown文档内的时候，typora会自动将图片上传到刚才我们配置好的gitee仓库内，并自动把markdown文档内的本地路径转化为gitee的图片外链，便于我们以后进行多端访问文档内的图片。