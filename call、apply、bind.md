# ECMAScript | 理解并重构call、apply、bind

## :key:前言

:fire:  要理解call、apply、bind，首先得明白什么是this，详情可参考本人的上一篇关于this的文章。

## :key:为什么要有call、apply、bind?

:fire: 在我们开发的时候，不可避免的会大量的使用到this。虽然this指针给我们的开发带来了很大的灵活和便捷，但是同样的，它也带来了不少麻烦，this指针的过于灵活在很多时候会导致编程变得模糊不清，出现一些在意料之外的情况。那么这时候，我们就需要一种机制，来将我们所需要的this指向给固定下来，来避免出现一些预计之外的状况。而在js内部就提供了三种方式来**切换/绑定**this的指向。这三种方式分别是**call**、**apply**、**bind**。

## :key:更改this指向的方法

### :star2: call

`Function.prototype.call(thisArg,arg1,arg2,arg3...)`

*作用：可以指定函数内部this的指向，传入多个参数，然后再调用该函数。*

*参数*:

​		`this.Arg`:

​							*必选。希望this所指向的对象，如果设置为null或者undefined，就相当于指向全局对象*

​       `arg1,arg2,arg3...`:

​							*可选。指定的参数列表项。*

*返回值*:

​          *使用调用者提供的 `this` 值和参数调用该函数的返回值。若该方法没有返回值，则返回 `undefined`。*

```js
let n = 1;
let obj = { n: 2 };

function a() {
  console.log(this.n);
}
a.call() // 1
a.call(null) // 1
a.call(undefined) // 1
a.call(window) // 1
a.call(obj) // 4
`-----------------------------------------------------`
function add(a, b) {
  return a + b;
}

add.call(this, 1, 2) // 3
```

:fire:从上述案例中，可以看出call的第一个参数如果不指定或者指定为null、undefined、window的时候都默认指向window。

:fire:call可以为不同的对象的分配和调用一个属于对象的函数和方法

:fire:call也可以提供新的this值给当前所要调用的函数和方法，比如我们可以用该特性实现对象的继承。

```js
//使用call实现继承
function food(name, price) {
  this.name = name;
  this.price = price;
}

function rice(name,price){
    food.call(rice,name,price);
    this.category="rice";
}
let fo=new rice("a",12);
console.log(fo)
/*
    rice {category: "rice"}
    category: "rice"
    __proto__:
    constructor: ƒ rice(name, price)
    __proto__: Object
*/
```

:fire:我们还可以用call来调用匿名函数。下面的例子中，我们声明了在一个循环体内部定义了一个匿名的自调用函数，通过调用该函数的call方法，让其内部的this指向绑定到数组a，所以这个匿名函数的作用就变成了为数组中的每一个元素添加一个print方法。

```js
let a = [
  { what: 'Lion', name: 'King' },
  { what: 'Whale', name: 'Fail' }
];

for (let i = 0; i < a.length; i++) {
  (function(i) {
    this.print = function() {
      console.log('#' + i + ' ' + this.what
                  + ': ' + this.name);
    }
    this.print();
  }).call(a[i], i);
}
/*
#0 Lion: King1 Whale: Fail
*/
```



### :star2: apply

`Function.prototype.apply(thisArg,[argsArray])`

*作用：可以指定函数内部this的指向，传入一个参数数组，然后再调用该函数。*

*参数：*

​		`this.Arg：`

​							*必选。希望this所指向的对象，如果设置为null或者undefined，就相当于指向全局对象*

​       `argsArray：`

​							*可选。函数调用所需要的参数数组，该数组的所有成员依次作为参数，传入原函数。*

*返回值：*

​          *使用调用者提供的 `this` 值和参数调用该函数的返回值。若该方法没有返回值，则返回 `undefined`。*

```js
//如果传递两个参数
//将Math指向null
//然后调用max方法，取出numbers中最大的值
let numbers = [5, 6, 2, 3, 7];

let max = Math.max.apply(null, numbers);
//不过要注意的是，使用max寻找数组中的最大值，有超出参数长度限制的风险
//如果参数数组的长度十分庞大，就不能直接使用max去寻找数组中的最大

```

:fire:可以看到，apply方法为我们的开发提供了很多的便捷性，比如我们可以将原本不属于数组的方法，强行用apply更改Math的this指向为null，并且将numbers作为参数传递进调用的max方法中，来轻松简单的获取数组中的最大值，而无需去一个个的去做比较。

:fire:根据传入的第二个参数的特点，使用apply，参数列表会按照数数组的形式传入，该数组内的元素会一个一个的传入到原函数中被调用，那么我们就可以利用这个特点，可以做到更为灵活的合并数组方法。

```js
let arr=['a','b','c'];
let arr1=[1,2,3,4];
arr.push.apply(arr,arr1);
console.log(arr);//[ 'a', 'b', 0, 1, 2 ]
arr.unshift.apply(arr,arr1);
console.log(arr);//[  1,  2,  3,  4,  'a', 'b', 'c']
```

:fire:从ES5之后，我们也可以利用该方法，apply也适用一些类数组的方法。只要这个类数组有一个 `length` 属性和`(0..length-1)`范围的整数属性，都可以使用apply方法来将数组方法指定到该类数组上。

```js
Array.prototype.slice.apply({0: 1, length: 1}) // [1]
Array.prototype.slice.apply({0: 1}) // []
Array.prototype.slice.apply({0: 1, length: 2}) // [1, undefined]
Array.prototype.slice.apply({length: 1}) // [undefined]
```

:fire:在面向对象中，我们也可以使用apply来使得我们传入到构造函数中的参数更为灵活。我们在传递到构造函数中的参数，就可以使用一个类数组对象，而不必使用参数列表。

```js
//Object.create方法是利用当前的对象创建一个新的对象，并带有原对象的原型和属性
//可以认作是利用Object.create方法实现创建一个该对象的子类
Function.prototype.construct = function (aArgs) {
  var oNew = Object.create(this.prototype);
  this.apply(oNew, aArgs);
  return oNew;
};
//上述方法可能会产生兼容性问题，可以使用以下方法作为替换
//利用object._proto_方法
Function.prototype.construct = function (aArgs) {
  var oNew = {};
  oNew.__proto__ = this.prototype;
  this.apply(oNew, aArgs);
  return oNew;
};
//利用闭包
Function.prototype.construct = function(aArgs) {
  var fConstructor = this, fNewConstr = function() {
    fConstructor.apply(this, aArgs);
  };
  fNewConstr.prototype = fConstructor.prototype;
  return new fNewConstr();
};
//使用Function构造函数
Function.prototype.construct = function (aArgs) {
  var fNewConstr = new Function("");
  fNewConstr.prototype = this.prototype;
  var oNew = new fNewConstr();
  this.apply(oNew, aArgs);
  return oNew;
};
```

### :star2: bind

`Function.prototype.bind()`

*作用：创建一个新的函数，在 `bind()` 被调用时，这个新函数的 `this` 被指定为 `bind()` 的第一个参数，而其余参数将作为新函数的参数，供调用时使用。*

*参数：*

 	  *thisArg：*

​           *调用绑定函数时作为 `this` 参数传递给目标函数的值。*

- **该参数的作用：**
    - *如果使用`new`运算符构造绑定函数，则忽略该值*
    - *当使用 `bind` 在 `setTimeout` 中创建一个函数（作为回调提供）时，作为 `thisArg` 传递的任何原始值都将转换为 `object`。*
    - *如果 `bind` 函数的参数列表为空，或者`thisArg`是`null`或`undefined`，执行作用域的 `this` 将被视为新函数的 `thisArg`。*

​	  *arg1, arg2, ...：*

​            *当目标函数被调用时，被预置入绑定函数的参数列表中的参数。*



```js
let d = new Date();
d.getTime() 
let p = d.getTime;
p() // Uncaught TypeError: this is not a Date object.

/*
上述代码，将d.Time赋值给p之后，函数内部的this指向就不再指向Date对象了，而是指向的winow,可以使用bind来解决
*/
let p1 = d.getTime.bind(d);
p1()
```

:fire:当bind的第一个参数传入的是null或者undefined的时候，默认指向的是window。下面这个例子中，add内部并没有this，使用bind的主要目的就是绑定参数x，以后每次运行新函数plus5的时候，只需要提供第二个参数y就足够了.

```js
function add(x, y) {
  return x + y;
}

var plus5 = add.bind(null, 5);
plus5(10) // 15
```

:warning:  *注意：*

- `apply`和`call`都不可以作用于回调函数中
- 在ES6中的箭头函数中，apply和call都会失效

- `bind()`方法每运行一次就会生成一个新的函数 ，所以不可以bind()直接绑定到匿名函数中

    ```js
    //错误的代码
    element.addEventListener('click', o.m.bind(o));
    //正确的写法
    let listener = o.m.bind(o);
    element.addEventListener('click', listener);
    ```

- `bind()`可以解决*call和apply不能作用于回调函数*的问题

    ```js
    let obj={a:1};
    SetTimeout(function(){
        console.log(this)
    }.call(obj),5000);
    
    ```

    在上述代码中，如果将一个定时器中要执行的回调函数的执行更改为obj，那么就会造成定时器失效。造成这种情况的原因是因为call和apply都是立即执行函数，而回调函数则是需要等待到某种特点条件才执行的特殊函数。在这种情况下，我们不可以使用call或者apply更改回调函数的this指向。

    ```js
    let obj={a:1};
    SetTimeout(function(){
        console.log(this)
    }.bind(obj),5000);
    ```

    但如果使用bind来更改回调函数中this的指向就没有任何问题。但是仍然有一点需要注意，如果在监听事件中使用bin更改this指向，会导致无法删除监听事件。

    ```js
    var obj={
        a:1,
        b:function(){
           document.addEventListener("click",this.clickHandler.bind(this))
        },
        clickHandler:function(e){
            console.log(this);
            document.removeEventListener("click",this.clickHandler.bind(this))
        }
    }
    obj.b();
    ```

    上述代码，的确可以改变回调函数中的this指向，但是却不能无法删除改变指向之后的事件监听。这是因为bind每次调用都会返回一个新的函数，所以导致无法删除对应的事件。解决这个问题的班费就是固定返回的函数。

    ```js
    var obj={
        a:1,
        b:function(){
            this.handler=this.clickHandler.bind(this);
            document.addEventListener("click",this.handler)
        },
        clickHandler:function(e){
            console.log(this);
            document.removeEventListener("click",this.handler)
        }
    }
    obj.b();
    ```

- `bind()`可以与`call()`方法结合使用

    ```js
    [1, 2, 3].slice(0, 1) // [1]
    // 等同于
    Array.prototype.slice.call([1, 2, 3], 0, 1) // [1]
    
    //上述的方法可以用bind改写
    var slice = Function.prototype.call.bind(Array.prototype.slice);
    slice([1, 2, 3], 0, 1) // [1]
    ```

    ## :key:call、apply、bind方法的区别与总结

    

- call和apply唯一的区别就是参数传递方式的不同，call传递是参数列表，apply传递的是数组

- call与apply都是更改this指向后直接调用，而bind是返回相应的函数，之后才会调用

- 如果我们需要一个函数改变this指向，就用call或者apply

- 如果传递的参数不多，可以使用call

- 如果传递的参数很多，则可以将数组用参数整理好后调用apply

- call与apply不能在回调函数和箭头函数中执行

- bind不能在匿名函数中执行

- 如果期望生成一个新的函数，并且长期绑定某些方法，就可以使用bind