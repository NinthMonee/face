##### 1、`process.env`

`process`是`nodejs`中的一个全局对象，提供了当前node进程的信息。

`process.env`返回的也是一个对象，包含了当前shell的所有的环境变量。

`process.env`可以进行一些配置，未配置的时候，只有两个属性。

-   ` BASE_URL`

      字符串类型，表示着是当前页面的路径

-   ` NODE_ENV`

       字符串类型，用该变量确定当前环境是生产阶段还是开发阶段。

       开发阶段为development，生产阶段为production

##### 2、`$router`和`$route`

- `$router`是VueRouter对象的实例，是路由操作对象，只能写对象。它里面包含着`$router.push()`、`$router.replace()`、`$router.go()`等方法

- `$route`是router跳转对象，路由信息对象，里面可以获取name、path、query、params等信息，只能读对象。
- 传递参数使用`this.$router`，接收参数是`this.$route`

##### 3、`params`和`query`传参的区别

- `param`传参，`this.$router.push`的参数只能是`this.$router.push({name:"地址",params:{id:"123"}})`
- `query`传参，`this.$router.push({path:"地址"，query:{id:"123"}})`
- query接收参数，用query接收，params接收，用params来接收
- query刷新页面的时候，参数不会消失，会一直显示到地址栏中
- parms刷新页面，参数会消失，可以考虑用本地存储来解决

#### 4、使用vant组件库或者其他组件库的时候，无法修改样式

在你要添加的选择器前面添加`/deep/`，就可以覆盖样式

