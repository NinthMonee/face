## 前言

在要理解new关键字之前，我们得先理解什么是对象？什么是构造函数？

## 什么是对象？

可以从以下这两个维度来理解什么是对象？

##### 	(1)对象是单个实例的抽象

​      任何一个事物都可以作为一个对象，一本书、一个人、一张网页甚至一个连接都可以作为对象。

##### 	(2)对象是一个容器，封装了一类事物的属性和行为

 	 属性是对象的状态，而方法就是对象的行为。

**补充：**面向对象具有三大特征：**继承**、**封装**、**多态**，事实上，**抽象**也是面向对象的一大特征之一，事实上，面向对象编程也可以理解为面向**抽象**编程。

对象是将现实中各种复杂的关系，将其抽取为一类具有相同行为特征的容器***(这就是抽象)***。

这些容器内部拥有着一类事物的属性、特征和行为。***(这就是封装)***

而这类事物通常还可以再高度抽象化为一个基类对象，这类事物与这个基类对象的关系，通常是父之类的关系。***(这就是继承)***

每一类对象都可以有着不同的表现形式，比如Person的实例对象可以是Student,Teacher等，Person也可以有不同的状态，比如说睡着的人、醒着的人、运动的人等。***(这就是多态)***

​		对象可以复用、也可以通过继承机制来定制自己期望的功能，因此，面向对象具有灵活、代码可复用、高度模块化等特点，适用于大型的项目的开发。

## 什么是构造函数？

**要使用面向对象进行编程，第一步首先就得生成一个对象。那么对象是怎么生成的呢？**

对象是单个实例的抽象，那么就需要一个模板来表示一类实物的共同特征，然后对象就根据这个模板生成。比较典型的面向对象语言C++/Java，都是基于class(类)这个概念来实现面向对象的，每一个对象都是一个class。但在javascript中，面向对象则是通过**构造函数(constructor)**和**原型链(prototype)**来实现的，尽管在ES6中引入了class这个概念，但是class只是原型链对象的一个语法糖，本质上依旧不变，在写法和使用上，更接近于java这类面向对象的语言。

JavaScript使用**构造函数(constructor)**作为对象的模板，而构造函数，其实也就是专门用来生成实例对象的函数，他就是对象的模板，用来描述对象的基本结构。一个构造函数，可以生成多个实例对象，这些实例对象都有自己相同的结构。构造函数本质上依旧是一个函数，但是这个函数只能通过**new关键词**来调用，且它拥有属于自己的用法。

**构造函数通常有以下特点:**

​		1、函数内部使用了this关键字，代表所要生成的对象实例

​        2、生成对象的时候，必须使用new命令，如果不使用new命令，则只是普通函数的调用。

​        3、ES5中构造函数的名字与类名相同，且首字母大写。ES6中构造函数只需要写**constructor**来表示构造函数就可以了。

## new命令

### 1、基本用法

new命令的作用，就是执行构造函数，返回一个对象实例。

```js
var Person=function(){
    this.name="eric";
}
var p=new Person();
p.name//eric
```

当然，使用new命令，执行构造函数的时候，也是可以传递参数的。

```js
var Person=function(name){
   this.name=name;
}
var p=new Person("ty");
```

但是，有一个问题需要注意，一旦我们使用构造函数且忘记使用new调用的时候，这时候，构造函数也就变成了普通函数，而且this的指向将会更改为指向window，代表全局对象，会造成一些意料之外的结果。

```js
var Person=function(){
    this.name="eric";
}

var p=Person();
p;//undefined
name;//eric
```

为了保证构造函数必须加上new命令一起使用，一个解决办法是，构造函数内部使用严格模式，或者直接使用ES6的模块化的开发。这样的话，一旦忘记使用new命令，那么直接调用函数就会报错。

### 2、new命令的原理

使用new命令的时候，它后面的函数会依次执行下面的步骤。

​    1)  创建一个空对象，作为将要返回的对象实例

​	2)  将这个空对象的原型`_proto_`，指向构造函数的`prototype`属性

​	3)  将这个空对象赋值给函数内部的this属性

​	4)  开始执行构造函数内部的代码

也就是说，构造函数内部，this指的是一个新生成的空对象，所有针对this的操作，都会发生在这个空对象上。构造函数之所以叫“构造函数”，就是说这个函数的目的，就是操作一个空对象(即this对象)，将其构造为需要的样子。

但是要注意的是，如果构造函数内部跟着return语句，且return后面跟着一个对象，那么new命令会返回return语句指定的对象，否则，就会不管return语句，返回this对象。

new命令的简化内部流程，如下所示。

```js
function _new(/*构造函数*/constructor,/*构造函数的参数*/params){
    //将arguments对象转换为数组
    var  args=[].slice.call(arguments);
    //取出构造函数
    var constructor=args.shift();
    //创建一个空对象，继承构造函数的prototype属性
    var context=Object.create(constructor.prototype);
    //执行构造函数
    var result=constructor.apply(contxt,args);
    //如果返回结果是对象，就直接返回，否则返回context对象
    return (typeof result === 'object' && result != null) ? result : context;
}

//实例
var  actor=_new(Person,"啊啊",28);
```

另外，如果对普通函数使用new命令，则会返回一个空对象。

```js
function getMessage() {
  return 'aaa';
}

var msg = new getMessage();

msg // {}
typeof msg // "object"
```

### 3、new.target

函数内部可以使用**new.target**属性。如果当前函数是new命令调用，new.target指向当前函数，否则为undefined。

```js
function f(){
console.log(new.target===f);
}
```

这个属性的主要是可以判断函数调用的时候，是否使用new命令，仅在ES5中使用，ES6中模块化自动会严格模式，不写new，会报错。

### 4、Object.create()

构造函数作为模板，可以生成实例对象。但有时候拿不到构造函数，只能拿到一个现成的对象，如果希望使用现成的对象作为模板，生成新的实例对象，这时候就可以使用Obeject.create()方法。

```js
var person1 = {
  name: '张三',
  age: 38,
  greeting: function() {
    console.log('Hi! I\'m ' + this.name + '.');
  }
};

var person2 = Object.create(person1);

person2.name // 张三
person2.greeting() // Hi! I'm 张三.
```

