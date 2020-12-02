# 详解this关键字

## :arrow_forward:   this是什么？

- :heavy_check_mark:当把this放在构造函数中，表示实例对象
- :heavy_check_mark:在方法中，this表示该方法所属的对象
- :heavy_check_mark:当它单独使用或者放在函数中，就表示window
- :heavy_check_mark:如果在严格模式中，函数中的this是未定义的undefined
- :heavy_check_mark: 在事件中，this表示接收事件的元素

:heart:总的来说，this总指向属性或方法当前所在的对象。它表示着当前对象的一个引用，

```js
//单独使用
console.log(this)//window
//放在普通函数中
function fn(){
    console.log(this);
    //window
}
//放在严格模式的函数中
function fn1(){
    "use strict"
    console.log(this);
    //undefined
}
//构造函数
var person = {
  firstName: "John",
  lastName : "Doe",
  id       : 5566,
  fullName : function() {
    return this.firstName + " " + this.lastName;
    //this指向的是当前的person
  }
};

```

##  :arrow_forward:  this的设计原理

:heart:this在很多语言中都有它的设计，这与内存里面的数据结构有关系。

```js
let obj={
   a:3
}
```

:heart:要执行这条语句，首先会javaScript引擎在堆内存中开辟一块空间来存储{a:3}，然后会把这个对象的内存地址赋值给变量obj，obj变量存储的地方叫做栈空间，之后，如果要再读取该对象，那么js引擎会根据obj中存储的内存地址，去堆内存中找到对应对象。

:heart:原始对象以字典结构来保存，每一个属性名都对应一个属性描述对象，举个例子来说，上述的obj，虽然只有一个属性a，但是实际上是以下面的形式来保存的。

```js
{
   a:{
     [value]:3,//值
     [writable]:true,//是否可写
     [enumerable]:true,//是否枚举
     [configurable]:true//可否配置
   }
}
```

:heart:这样存储一个对象，结构是很清晰的，但当我们存储的属性值是一个函数或者对象的时候，这时候value存储的就不是实际的值，而是该函数或者对象的内存地址。

:heart:函数是可以在不同的上下文环境中执行的，而函数体内部，也是可以引用当前环境的其他变量的。那么当我想要访问函数所在环境中一个变量，而该变量又与函数内部的变量名相同的时候，就会造成冲突，导致会出现一些无法预料的结果。

```js
let a=10;
function fn(a){
   console.log(a);
}
```

:heart: 所以这时候，我们就需要一种机制，来让我们在函数内内部获取当前函数所在环境的变量和方法。所以this就出现了，设计它的目的就是用于在一个函数体的内部，获取当前函数所在的运行环境。

```js
let f=function(){
  console.log(this.x);
}
let x=1;
let obj={
    f:f,
    x:2
}
f();//1
obj.f();//2
//在该代码中，函数f在全局环境中执行，this.x指向全局环境的x
//在obj环境中执行，this.x指向obj.x
```

## :arrow_forward: this使用场景及其注意事项

### :arrow_down_small:this使用场景

- :key:只要在全局环境中运行，不论在函数内部还是外部，它指向的就是window

  ```js
  this === window;//true
  function  f(){
      console.log(this === window);
  }
  f();//true
  ```

- :key::在构造函数中，使用this，指的就是实例对象。

  ```js
  //ES5
  function Obj(p){
    console.log(this)
  };
  var o=new Obj();//Obj{}
  
  //ES6
  class Person{
      constructor(){
          console.log(this);
      }
  }
  let p=new Person();//Person {}
  ```

- :key:如果对象的方法里面包含this，那么this的指向就是该方法运行时所在的对象，该方法赋值给另一个对象，就会改变this的指向。

  ```js
  var obj ={
    foo: function () {
      console.log(this);
    }
  };
  
  obj.foo() // obj
  //以下几种用法，就会改变this的指向
  // 情况一
  (obj.foo = obj.foo)() // window
  // 情况二
  (false || obj.foo)() // window
  // 情况三
  (1, obj.foo)() // window
  ```

  obj.foo就是一个值，这个值真正被调用的时候，运行环境已经不是obj了，而是全局环境，所以this不再指向obj。

- :key:如果this被嵌套在多层里面，那么this只指向当前层的所在环境的对象

  ```js
  var a = {
    p: 'Hello',
    b: {
      m: function() {
        console.log(this.p);
      }
    }
  };
  
  //此时this指向的是对象b，而不是a
  ```

### :arrow_down_small:注意事项

- :key:避免多层嵌套this
- :key:避免在数组的一些方法中使用this，比如map、foreach、filter、reduce等
- :key:避免在回调函数中使用this
- :key:如果不得不在以上情况中使用this，可以采用设置一个中间变量将this指向固定下来的方式，或者是将this绑定到你所需要的对象上，这种方式，我将在下节做详细介绍。

### 总结

- ES5非严格模式下，全局this和函数中this都指向window

- ES5严格模式、ES6中，全局this仍然指向window，函数this指向undefined

- 对象的this

    ```js
    var obj={
        a:1,
        b:function(){
            // console.log(this);//this不会因为变量引用改变而改变
            // this指向当前对象自身obj
            // console.log(obj.a);//变量会发生引用改变
        },
        c:this.a
        // this指向对象外this的指向
        // 对象没有创建完成时，this还没有生成，this就指向对象外this的指向
    }
    
    var o=obj;
    obj={a:10,d:10}
    
    o.b();
    ```

- 在回调函数中的this

    ```js
    var obj = {
        a: function () {
            console.log(this,"____");//Obj
            //var self=this;//此时的this存储的是obj
            function fn1(fn) {
                 fn();//window
                 arguments[0]();//arguments
                 fn.call(this);//window
            }
            function fn2() {
                console.log(this);//回调函数中
            }
            fn1(fn2);
        },
    };
    
    obj.a();
    
    var obj1={
        b:1,
        a:function(){
            setTimeout(function(){
                console.log(this);//指向window
            }, 2000);
            //   setTimeout(function(obj1){
            //         console.log(obj1.b);
            //   }, 2000,this);
        }
    }
    obj1.a();
    ```

    - 1、如果直接指向的回调函数，this指向最外层window
    -  2、如果通过arguments直接使用参数指向函数，this则指向执行当前函数的arguments
    - 3、如果回调函数通过call,apply,bind重新指向了新的对象时，this就是指向新对象

- 事件中的this

    - 其他浏览器、高版本的IE的this指向事件侦听对象，也就是e.currentTarget
    - IE8 attachEvent侦听事件时，this指向window

    ```js
    //其他浏览器、高版本的IE
    var obj={
        b:1,
        a:function(){
        //特殊的回调函数
            document.addEventListener("click",this.clickHandler);
        },
        clickHandler:function(e){
            console.log(this);//事件侦听的对象 e.currentTarget
        } 
    }
    
    
    //IE低版本
    var obj={
        b:1,
        a:function(){
            //   console.log(this);
            //   特殊的回调函数
            document.addEventListener("click",this.clickHandler);
            //   document.attachEvent("onclick",this.clickHandler);
        },
        clickHandler:function(e){
            // console.log(this===document);//addEventListener事件侦听的对象 e.currentTarget
            // console.log(this===window);//IE8 attachEvent侦听事件时，this指向window
        } 
    } 
    obj.a();
    ```

- ES6类中的this

    ```js
    class Box{
        static _instance;
        constructor(){
            console.log(this);//指向被实例化的对象
        }
        static getInstance(){
            if(!Box._instance){
                Box._instance=new Box();
            }
            return Box._instance;
        }
        play(){
            console.log(this,"|");//指向被实例化的对象
        }
        static run(){
            // console.log(this);
            console.log(this===Box,"____");
            // this就是当前类名也是构造函数
            // 任何的静态方法中this都是当前类
            // 静态方法中无法获取到实例化对象的this的
            return this;
        }
    
        static plays(){
            // this.getInstance().play();
            var o=this.getInstance();
            var o1=this.getInstance();
            console.log(o===o1);
        }
    }
    var b=new Box();//会执行构造函数，这是构造函数中this就是这个b对象
    b.play();//b对象下的方法play，因此play方法中this被指向b，谁执行play，this指向谁
    console.log(Box.run()===b);//false
    console.log(Box.run()===b.constructor);//true
    
    Box.plays();
    ```

- ES5面向对象中的this

    ```js
     function Box(){
         console.log(this);
     }
    Box.prototype.play=function(){
        console.log(this);//this是指向执行该方法的实例化对象
    }
    
    Box.run=function(){
        console.log(this);//Box
    }
    
    // Box();//this是window或者undefined
    var b=new Box();// this是实例化后b对象
    b.play();
    ```

- 箭头函数中的this

    ```js
    var obj = {
        a: function () {
            setTimeout(() => {
                console.log(this);//this是箭头函数外this的指向
                // 上下文环境中this的指向
            }, 2000);
        },
    };
    obj.a();
    
    //对象中的箭头函数
    var obj={
        a:function(){
            console.log(this);//obj
        },
        b:()=>{
            console.log(this);//window
        }
    }
    obj.a();
    obj.b();
    
    //ES5面向对象
    function Box(){
    
    }
    Box.prototype.run=()=>{
        console.log(this);//window
    }
    
    var b=new Box();
    b.run();
    const auto & x: numbers
    ```

- 案例1

    ```js
     class Box{
                constructor(){
    
                }
                play(){
                    var o={
                        run:function(){
                            function fns(){
                                console.log(this);
                            }
                            return fns;
                        },
                        play:function(fn){
                            fn();
                            // arguments[0]();
                        },
                        clickHandler(){
                            // this   实例化的b
                           o.play.call(this,o.run());
                        }
                    }
                    document.addEventListener("click",o.clickHandler.bind(this));
                }
            }
            
            var b=new Box();
            b.play();
    ```

    

