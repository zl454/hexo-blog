---
title: JavaScript运行原理浅谈
tags: JavaScript基础
categories: JavaScript
date: 2020-03-25 10:20:59
---

# 1.JavaScript 运行三部曲

- 语法分析
- 预编译
- 解释执行

在执行代码前，还有两个步骤
**语法分析**很简单，就是引擎检查你的代码有没有什么低级的语法错误
**解释执行**顾名思义便是执行代码了
**预编译**简单理解就是在内存中开辟一些空间，存放一些变量与函数

## 预编译前奏

**规律1：任何变量，如果未经声明就赋值，此变量是属于 window 的属性**，而且不会做变量提升。（注意，无论在哪个作用域内赋值）

比如说，如果我们直接在代码里写 `console.log(a)`，这肯定会报错的，提示找不到 `a`。但如果我直接写 `a = 100`，这就不会报错，此时，这个 `a` 就是 `window.a`。

**规律2：一切声明的全局变量，全是window的属性**。（注意，我说的是在全局作用域内声明的全局变量，不是说局部变量）

比如说，当我定义 `var a = 200` 时，这此时这个 `a` 就是 `window.a`。

由此，我们可以看出：**window 代表了全局作用域**（是说「代表」，没说「等于」）。

## 预编译

### 预编译（函数执行前）

（1）创建AO对象。AO即 Activation Object 活跃对象，其实就是「执行期上下文」。

（2）找形参和变量声明，将形参名和变量作为 AO 的属性名，值为undefined。

（3）将实参值和形参统一，实参的值赋给形参。

（4）查找函数声明，函数名作为 AO 对象的属性名，值为整个函数体。

这个地方比较难理解。但只有了解了函数的预编译，才能理解明白函数的执行顺序。

### 预编译（脚本代码块script执行前）

（1）查找全局变量声明（包括隐式全局变量声明，省略var声明），变量名作全局对象的属性，值为undefined

（2）查找函数声明，函数名作为全局对象的属性，值为函数引用

# 2.执行期上下文

当**函数执行**时（准确来说，是在函数发生预编译的前一刻），会创建一个执行期上下文的内部对象。一个执行期上下文定义了一个函数执行时的环境。

每调用一次函数，就会创建一个新的上下文对象，他们之间是相互独立且独一无二的。当函数执行完毕，它所产生的执行期上下文会被销毁。

# 3.this

解析器在调用函数每次都会向函数内部传递进一个隐含的参数，这个隐含的参数就是this，this指向的是一个对象，这个对象我们称为函数执行的 上下文对象。

根据函数的调用方式的不同，this会指向不同的对象：【重要】

- 1.以函数的形式调用时，this永远都是window。比如`fun();`相当于`window.fun();`

- 2.以方法的形式调用时，this是调用方法的那个对象

- 3.以构造函数的形式调用时，this是新创建的那个对象

- 4.使用call和apply调用时，this是指定的那个对象

# 4.递归

## 递归的三大要素

**第一要素：明确你这个函数想要干什么**

**第二要素：寻找递归结束条件**

**第三要素：找出函数的等价关系式**

# 5.创建自定义对象的几种方法

## 方式一：对象字面量

**对象的字面量**就是一个{}。里面的属性和方法均是**键值对**：

- 键：相当于属性名。

- 值：相当于属性值，可以是任意类型的值（数字类型、字符串类型、布尔类型，函数类型等）。

```javascript
var o = {
            name: "flynn",
            age: 26,
            isBoy: true,
            sayHi: function() {
                console.log(this.name);
            }
        };

console.log(o); //Object{name:"flynn",age:26,isBoy:true,sayHi:sayHi()}
```

## 方式二：工厂模式

通过该方法可以大批量的创建对象。

```javascript
    /*
        * 使用工厂方法创建对象
        *  通过该方法可以大批量的创建对象
        */
    function createPerson(name, age, gender) {
        //创建一个新的对象
        var obj = new Object();
        //向对象中添加属性
        obj.name = name;
        obj.age = age;
        obj.gender = gender;
        obj.sayName = function() {
            alert(this.name);
        };
        //将新的对象返回
        return obj;
    }

    var obj2 = createPerson("猪八戒", 28, "男");
    var obj3 = createPerson("白骨精", 16, "女");
    var obj4 = createPerson("蜘蛛精", 18, "女");
```

如果简化一下，可以写成下面这种形式，更容易理解：（也就是，利用new Object创建对象）

```javascript
var obj = new Obect();
obj.name = '猪八戒';
obj.age = 28;
obj.gender = '男';
obj.sayHi = function() {
    alert('hello world');
};
```


**弊端：**

使用工厂方法创建的对象，使用的构造函数都是Object。**所以创建的对象都是Object这个类型，就导致无法区分出多种不同类型的对象**。

### 方式三：利用构造函数

```javascript
        //利用构造函数自定义对象
        var stu1 = new Student("smyh");
        console.log(stu1);
        stu1.sayHi();

        var stu2 = new Student("vae");
        console.log(stu2);
        stu2.sayHi();


        // 创建一个构造函数
        function Student(name) {
            this.name = name;    //this指的是当前对象实例【重要】
            this.sayHi = function () {
                console.log(this.name + "厉害了");
            }
        }
```

打印结果

![](http://img.smyhvae.com/20180125_1350.png)

# 6.构造函数

### 代码引入


```javascript
      // 创建一个构造函数，专门用来创建Person对象
      function Person(name, age, gender) {
        this.name = name;
        this.age = age;
        this.gender = gender;
        this.sayName = function() {
          alert(this.name);
        };
      }

      var per = new Person("孙悟空", 18, "男");
      var per2 = new Person("玉兔精", 16, "女");
      var per3 = new Person("奔波霸", 38, "男");

      // 创建一个构造函数，专门用来创建 Dog 对象
      function Dog() {}

      var dog = new Dog();
```

### 构造函数的概念

**构造函数**：是一种特殊的函数，主要用来创建和初始化对象，也就是为对象的成员变量赋初始值。它与 `new` 一起使用才有意义。

可以把对象中一些公共的属性和方法抽取出来，然后封装到这个构造函数里面。

**this的指向也有所不同**：

- 1.以函数的形式调用时，this永远都是window。比如`fun();`相当于`window.fun();`

- 2.以方法的形式调用时，this是调用方法的那个对象

- 3.以构造函数的形式调用时，this是新创建的那个对象

### new 一个构造函数的执行流程

new 在执行时，会做下面这四件事：

（1）开辟内存空间，在内存中创建一个新的空对象。

（2）让 this 指向这个新的对象。

（3）执行构造函数里面的代码，给这个新对象添加属性和方法。

（4）返回这个新对象（所以构造函数里面不需要return）。

因为this指的是new一个Object之后的对象实例。于是，下面这段代码：

```javascript
        // 创建一个函数
        function createStudent(name) {
            var student = new Object();
            student.name = name;      //第一个name指的是student对象定义的变量。第二个name指的是createStudent函数的参数。二者不一样
        }

//改进后
        // 创建一个函数
        function Student(name) {
            this.name = name;       //this指的是构造函数中的对象实例
        }

```

### 类、实例

使用同一个构造函数创建的对象，我们称为一类对象，也将一个构造函数称为一个**类**。

通过一个构造函数创建的对象，称为该类的**实例**。

### instanceof

使用 instanceof 可以检查**一个对象是否为一个类的实例**。

**语法如下**：

```javascript
对象 instanceof 构造函数

	  function Person() {}

      function Dog() {}

      var person1 = new Person();

      var dog1 = new Dog();

      console.log(person1 instanceof Person); // 打印结果： true
      console.log(dog1 instanceof Person); // 打印结果：false

      console.log(dog1 instanceof Object); // 所有的对象都是Object的后代。因此，打印结果为：true
```

如果是，则返回true；否则返回false。

# 7.json

> 对象字面量和json比较像

JSON：JavaScript Object Notation（JavaScript对象表示形式）。

**JSON和对象字面量的区别**：JSON的属性必须用双引号引起来，对象字面量可以省略。

json举例：

```
      {
            "name" : "zs",
            "age" : 18,
            "sex" : true,
            "sayHi" : function() {
                console.log(this.name);
            }
        };
```

注：json里一般放常量、数组、对象等，但很少放function。

另外，对象和json没有长度，json.length的打印结果是undefined。于是乎，自然也就不能用for循环遍历（因为遍历时需要获取长度length）。

**json遍历的方法：**

json 采用 `for...in...`进行遍历，和数组的遍历方式不同。

```html
<script>
    var myJson = {
        "name": "smyhvae",
        "aaa": 111,
        "bbb": 222
    };

    //json遍历的方法：for...in...
    for (var key in myJson) {
        console.log(key);   //获取 键
        console.log(myJson[key]); //获取 值（第二种属性绑定和获取值的方法）
        console.log("------");
    }
</script>
```

打印结果：

![](http://img.smyhvae.com/20180203_1518.png)

例：

```javascript
var obj={
    a:1,
    b:2,
    c:3,
    d:function () {
        console.log("aa");
    }
};

for(var prop in obj){
    console.log(prop,obj[prop]);
}

var obj1={};
for(var key in obj){
    obj1[key]=obj[key];
}	//复制obj属性给obj1
obj.a=40;	//修改obj属性a
console.log(obj1,obj);	//obj1的a属性不会被修改


function setStyle(elem,style) {
    for(var prop in style){
        elem.style[prop]=style[prop];
    }
}

var div0=document.getElementById("div0");
setStyle(div0,{
    width:"100px",
    height:"100px",
    backgroundColor:"red"
});		//遍历div0属性并写入
```
