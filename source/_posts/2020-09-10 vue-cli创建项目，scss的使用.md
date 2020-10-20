---
title: 2020.09.10开发笔记
tags: CSS
categories: 笔记
date: 2020-09-10
---

# 一、vue-cli 创建项目流程

## 1、为什么使用 vue-cli 

vue-cli 是 vue 官方提供的脚手架工具。脚手架工具简单讲就是自动将项目需要的环境、依赖等信息都配置好

## 2、全局安装 vue-cli

### （1）检查 npm 版本，建议安装到最新版本

```
【命令行查看版本号】
node -v
npm -v
【升级npm（可选操作）】
npm install -g npm
【修改为淘宝镜像（可选操作）】
npm config set registry "https://registry.npm.taobao.org"
```

### （2）安装

```
【全局安装 webpack】
npm install webpack -g

【webpack 4.X 开始，需要安装 webpack-cli 依赖】
npm install webpack webpack-cli -g

【查看版本号】
webpack -v

【全局安装vue-cli】
npm install -g vue-cli

【查看版本号】
vue -V
```

`注：
若出现“Unexpected end of JSON input while parsing near”错误，`
`命令行输入： npm cache clean --force`

## 3、创建项目（命令行方式）

### （1）创建一个文件保存的路径，比如：D:\vue\vue-demo

```
【命令行进入该目录】
cd E:\vue\vue-demo

【下载模板】
vue init webpack vue-demo

【进入交互页面，根据自己情况选择】
?Project name vue-demo # 项目名称，直接回车，按照括号中默认名字（注意这里的名字不能有大写字母，如果有会报错Sorry, name can no longer contain capital letters）。
? Project description A Vue.js project # 项目描述,随便写
? Author # 作者名称
? Vue build standalone # 我选择的运行加编译时
    Runtime + Compiler: recommended for most users
? Install vue-router? Yes # 是否需要 vue-router
? Use ESLint to lint your code? Yes # 是否使用 ESLint 作为代码规范.
? Pick an ESLint preset Standard # 一样的ESlint 相关
? Set up unit tests Yes # 是否安装单元测试
? Pick a test runner 按需选择 # 测试模块
? Setup e2e tests with Nightwatch? 安装选择 # e2e 测试
? Should we run `npm install` for you after the project has been created? (recommended) npm # 包管理器，我选的NPM
```

### （2）安装成功后，进入项目目录

```
【命令行进入该目录】
cd E:\vue\vue-demo

【初始化项目】
 npm install
 
 【启动项目，根据package.json中的数据来启动】
 npm run serve
 
 【关闭项目】
 Ctrl + C
 
 【浏览器访问：】
http://localhost:8080/

【项目打包上线】
npm run build
将打包后生成的dist 目录中的文件拷贝到服务器的相应地址即可（比如tomcat的webapps目录下）。
```

## 4、创建项目（图形界面方式）

在 vue 3.0 以上的版本，可以使用 vue 的 UI 界面来创建项目

```
【下载vue最新版】
npm i -g @vue/cli        或者   cnpm install -g @vue/cli

【打开ui界面】
vue ui
```

在弹出的浏览器界面中，可以在指定的文件夹中创建新的项目

查看 package.json 文件

```
启动项目 npm run serve
打包项目 npm run build
```

# 二、SCSS

## 1、了解SCSS

**CSS预处理器**：

​		定义了一种新的专门的编程语言，编译后形成正常的css文件，为css增加一些编程特性，无需考虑浏览器的兼容性（完全兼容css3），让css更加简洁、适应性更强，可读性更佳，更易于代码的维护等诸多好处。CSS预处理语言有SCSS (SASS) 和LESS、POSTCSS

**SCSS和SASS的区别：**

- .sass是以严格缩进语法规则来编写代码的，不包括大括号和分号，而scss的语法和css书写语法类似；sass的安装和使用不细说了；
- .scss是sass3.0引入的语法，可以理解scss是sass的一个升级版本，是一种SCSS-like语言，弥补了sass和css之间的鸿沟；

## 2、vue 项目开发中 scss 安装和使用

### （1）安装依赖

```
npm install node-sass sass-loader --save-dev
```

### （2）在 build 中  `webpack.base.conf.js`  中，在 rules 添加 scss 规则

```
{
  test: /\.scss$/,
  loaders: ['style', 'css', 'sass']
}
```

### （3）在 vue 文件中使用

```css
<style lang='scss'>
</style>
```

## 3、在 vue 项目中全局引入 scss

### （1）全局引用时需要安装 `sass-resources-loader` 

```
npm install sass-resources-loader --save-dev
```

### （2）在 build 中的 `utils.js`

```
将
scss: generateLoaders('sass')
修改为
scss: generateLoaders('sass').concat(
  {
    loader: 'sass-resources-loader',
    options: {
        //你自己的scss全局文件的路径
      resources: path.resolve(__dirname, '../src/style/common.scss')
    }
  }
)
```

## 4、SCSS 的基本语法

### （1）注释

注释分为三种：

1. /\* \*/css中显示，
2. //css中不显示，
3. /\*重要注释!\*/压缩不会被删掉。

### （2）引入外部文件

@import 命令导入外部sass、scss、css文件

### （3）变量

声明变量的语法是：$+变量名+：+变量值；如下

```
$color:red; //声明变量 $color

$color:red !default; //声明默认变量 $color
$color:purple; //根据需求覆盖默认变量，对默认变量进行重新声明可以实现覆盖默认值
.father01 {
   color:$color;
}
```

**全局变量和局部变量：**

全局变量是元素外声明的变量，局部变量是在元素里声明的变量，重复声明时局部变量会覆盖全局变量；

```
$height:200px; //全局变量声明不在大花括号内
$bgcolor:blue;
body {
   .father01 {  //嵌套
      width:$width;
      height:$height;
      $border:1px solid red; //局部变量是声明在元素内的
      border: $border;
      $bgcolor:purple; //全局变量和局部变量名一致时，调用局部变量进行覆盖
      background-color: $bgcolor;
   }
}
```

**局部变量值后加上 `!global` 关键词可以使得局部变量变成全局变量；**

**变量嵌套引用：即字符串插值，需要使用 #{} 来进行包裹**

```
$left:left;
.father02 {
   width: 300px;
   height:300px;
   border:$border; //使用全局变量
border-#{$left}:2px solid purple; //使用字符串插值之前必须先声明
}
```

### （4）嵌套

可以用 `&` 来引用父元素

```scss
.container {
   &>p {   //可以编译成CSS的 .container>p {} 效果
      color:purple;
   }
}
```

### （5）继承

继承 .class 使用 @extend

```scss
.container {
   color:purple;
   border:2px dashed purple;
}
.myText {
   @extend .container; //这里将继承.container类的所有样式
   font-size: 22px;
}
```

### （6）占位符 %

使用 % 声明的代码块，如果不被 @extend 调用的话就不会被编译。编译出来的代码会将相同的代码合并在一起，代码变得十分简洁。

```scss
%m5 { background-color: lightblue;}
.P1 { @extend %m5; }
```

### （7）重复代码块

使用混合 @mixin 命令定义，以及使用 @include 调用该 mixin 

```scss
@mixin normalStyle {
   //使用@mixin命令定义可重复使用的代码块
   width:300px;
   height:100px;
   color:black;
   background-color:lightblue;
}
.container {
   @include normalStyle;
   //使用@include 命令引用@mixin定义的代码块
}
```

使用 @mixin 和 @include 时，可以传参

```scss
@mixin normalStyle($width,$height,$color,$defaultValue:orange) {
   width:$width;
   height:$height;
   color:$color;
   background-color:$defaultValue;
}
.container {
   @include normalStyle(300px,100px,green,lightgray);
```

（8）SCSS 使用编程式方法

- **条件语句**

```scss
.container {
   p {
      @if 1+1<3 {
         border:1px solid blue;
      } @else {
         border:1ps dashed palevioletred;
      }
   }
}
```

- **三种循环**

**for 循环**

在sass中的@for循环有两种方式：

①@for $i from <start> through <end>

②@for $i from <start> to <end>

其中$i表示变量，start表示开始值，end表示结束值；

through表示包括end这个数值；to表示不包括end这个数值；

**while 循环**

只要@while后面的条件为true就会执行，直到表达式值为false时停止循环；

**each  in循环**

遍历列表，取出有用值。指令形式为：@each $var in <list>($var为变量值，list为sassscript表达式）

```scss
//for 循环
@for $i from 1 to 5 {
   .item-#{$i} {
      border:#{$i}px solid;
   }
}
//while 循环
$m:8;
@while $m > 0 {
   .items-#{$m} {
      width:2em*$m;
   }
   $m:$m - 2 ;
}
//这里可以对$m进行运算 让它每次都减去2
//each 语法
@each $item in class01,class02 { //$item就是遍历了in关键词后面的类名列
   .#{$item} {
      background-color:purple;
   }
}
//会编译成 .class01 , .class02 {background-color:purple;}
```

### （8）@function 自定义函数

```scss
@function double($sn){ //SCSS允许自定义函数
   @return $sn*2;
}
.myText {
   border:1px solid red;
   width:double(200px); //使用在SCSS中自定义的函数
```

### （9）内置颜色函数

```scss
.myText {
   color:black;
   &:hover{
      //以下的lighten()、darken()等是SCSS内置的颜色函数
      color:lighten(#cc3, 10%); // #d6d65c颜色变浅
      color:darken(#cc3, 10%); // #a3a329颜色加深
      color:grayscale(#cc3); // #d6d65c
      color:complement(#cc3); // #a3a329
   }
}
```

# 三、CSS 注意事项

CSS根文件一定要预设格式化样式，一般使用一下样式即可

```css
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  outline: 0;
}
button, input {
    font-size: inherit;
    outline: 0;
    border: 0;
}
html,
body{
  width: 100%;
  height: 100%;
}
body{
  color: #333;
  font: 16px/1.5 -apple-system, "Noto Sans", "Helvetica Neue", Helvetica, "Nimbus Sans L", Arial, "Liberation Sans", "PingFang SC", "Hiragino Sans GB", "Noto Sans CJK SC", "Source Han Sans SC", "Source Han Sans CN", "Microsoft YaHei", "Wenquanyi Micro Hei", "WenQuanYi Zen Hei", "ST Heiti", SimHei, "WenQuanYi Zen Hei Sharp", sans-serif;
}
```

