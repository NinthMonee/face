ES5的异步模式共有五种方法，这四种方法可以写出结构更合理，性能更出色，维护更方便的js代码。这四种模式分别为：

1、回调函数

2、事件监听

3、观察者模式

4、发布订阅模式

5、promise对象

一、回调函数

回调函数是实现异步的最基本的方法。

`回调函数的定义`：函数A作为参数传递到另一个函数B，变给这函数B执行函数A，我们就交函数A叫做回调函数，如果没有名称，就叫做匿名回调函数

`优点：`简单，容易理解

 `缺点：`

​       1、不利于代码的阅读和维护

​       2、各部分之间高度耦合，流程会很混乱，每个人物只能指定一个回调函数

​       3、容易造成回调地狱

二、事件监听

采用的是事件驱动的模式。

任务的执行不取决于代码的顺序，而是取决于某一个时间是否发生。

三、观察者模式

观察者模式分为两个角色Subject(Observerable，被观察者)和Observers(观察者)。Subject需要维护一套观察者的集合，这些Observer实现相同的接口，Subject只需要知道，通知Observer时，需要调用哪个统一方法就好了。

观察者模式是松耦合的。

四、发布订阅模式

发布订阅模式中分为三个角色，publisher(发布者)，Subscriber(订阅者)，broker(消息队列)。在该模式中，发布者和订阅者是完全解耦的，发布者并不会直接通知订阅者，他们之间传递消息需要通过broker。

发布者只需要告诉Broker，我要发送的消息是Topic，但我并不关心谁去接收，谁订阅了。

订阅者只需要告诉Broker，我要订阅的消息Topic，至于是谁发布的，我也不关心。

当Broker收到发布消息，然后当有人要订阅的时候，Broker就会把该消息推送给订阅者，当然，订阅者也可以主动去拉取。

*总结*

**从表面上看：**

- 观察者模式里，只有两个角色 —— 观察者 + 被观察者
- 而发布订阅模式里，却不仅仅只有发布者和订阅者两个角色，还有一个经常被我们忽略的 —— 经纪人Broker

**往更深层次讲：**

- 观察者和被观察者，是松耦合的关系
- 发布者和订阅者，则完全不存在耦合

**从使用层面上讲：**

- 观察者模式，多用于单个应用内部
- 发布订阅模式，则更多的是一种跨应用的模式(cross-application pattern)，比如我们常用的消息中间件

五、Promise模式

Promise模式最初是由commonJS工作组提出的一种规范，目的是为了异步编程提供统一的接口。ES6的时候，被纳入了标准。