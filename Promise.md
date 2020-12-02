### 1、Promise的含义

`Promise是异步编程的一解决方案，比起传统的解决方案回调函数和事件更为的合理和强大。`它由社区最早提出，ES6将它写进了语言标准，统一了用法。

Promise是一个容器，里面保存着一个异步操作的结果。Promise本身是同步的，但是它又可以获取异步操作的消息，只有当遇到.then或者.catch时候，它才会变为异步。

Promise有三种状态：pending、fulfiiled和rejected

它有以下两个特点：

  (1) 对象的状态不受外界影响

  (2) 一旦状态改变，，就不会在变，任何时候都可以得到这个结果。

优点：

​     1、将回调函数的层层嵌套变成了链式调用。

​     2、可以将异步操作以同步操作的流程表达出来

​     3、提供了统一的接口，使得控制异步操作更加容易

### 2、Promise.prototype.then()

Promise 实例具有`then`方法，也就是说，`then`方法是定义在原型对象`Promise.prototype`上的。它的作用是为 Promise 实例添加状态改变时的回调函数。前面说过，`then`方法的第一个参数是`resolved`状态的回调函数，第二个参数（可选）是`rejected`状态的回调函数。

### 3、Promise.prototype.catch()

`Promise.prototype.catch()`方法是`.then(null, rejection)`或`.then(undefined, rejection)`的别名，用于指定发生错误时的回调函数。

### 4、Promise.prototype.finally()

### 5、Promise.all()

`Promise.all()`方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。

```js
const p = Promise.all([p1, p2, p3]);
```

> `Promise.all()`方法接受一个数组作为参数，`p1`、`p2`、`p3`都是 Promise 实例，如果不是，就会先调用下面讲到的`Promise.resolve`方法，将参数转为 Promise 实例，再进一步处理。另外，`Promise.all()`方法的参数可以不是数组，但必须具有 Iterator 接口，且返回的每个成员都是 Promise 实例。

上述的例子中，`p`的状态由`p1`、`p2`、`p3`决定，分成两种情况。

（1）只有`p1`、`p2`、`p3`的状态都变成`fulfilled`，`p`的状态才会变成`fulfilled`，此时`p1`、`p2`、`p3`的返回值组成一个数组，传递给`p`的回调函数。

（2）只要`p1`、`p2`、`p3`之中有一个被`rejected`，`p`的状态就变成`rejected`，此时第一个被`reject`的实例的返回值，会传递给`p`的回调函数。

```js
// 生成一个Promise对象的数组
const promises = [2, 3, 5, 7, 11, 13].map(function (id) {
  return getJSON('/post/' + id + ".json");
});

Promise.all(promises).then(function (posts) {
  // ...
}).catch(function(reason){
  // ...
});
```



### 6、Promise.race()

`Promise.race()`方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例。

```js
const p=Promise.race([p1,p2,p3])
```

上面代码中，只要`p1`、`p2`、`p3`之中有一个实例率先改变状态，`p`的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给`p`的回调函数。

`Promise.race()`方法的参数与`Promise.all()`方法一样，如果不是 Promise 实例，就会先调用下面讲到的`Promise.resolve()`方法，将参数转为 Promise 实例，再进一步处理。

### 7、Promise.resolve()

有时需要将现有对象转为 Promise 对象，`Promise.resolve()`方法就起到这个作用。

Promise.resolve的方法的参数分为四种情况。

(1) 参数是一个 Promise 实例

(2) 参数是一个thenable对象

(3) 参数不是具有`then`方法的对象，或根本就不是对象

(4) 不带有任何参数

### 8、Promise.reject()

`Promise.reject(reason)`方法也会返回一个新的 Promise 实例，该实例的状态为`rejected`。

### 9、应用