# 一 其他



### 什么是 mvvm？

MVVM 是 Model-View-ViewModel 的缩写。mvvm 是一种设计思想。Model 层代表数据模型，也可以在 Model 中定义数据修改和操作的业务逻辑；View 代表 UI 组件，它负责将数据模型转化成 UI 展现出来，ViewModel 是一个同步 View 和 Model 的对象。

在 MVVM 架构下，View 和 Model 之间并没有直接的联系，而是通过 ViewModel 进行交互，Model 和 ViewModel 之间的交互是双向的， 因此 View 数据的变化会同步到 Model 中，而 Model 数据的变化也会立即反应到 View 上。

ViewModel 通过双向数据绑定把 View 层和 Model 层连接了起来，而 View 和 Model 之间的同步工作完全是自动的，无需人为干涉，因此开发者只需关注业务逻辑，不需要手动操作 DOM, 不需要关注数据状态的同步问题，复杂的数据状态维护完全由 MVVM 来统一管理。

### mvvm 和 mvc 区别？

mvc 和 mvvm 其实区别并不大。都是一种设计思想。主要就是 mvc 中 Controller 演变成 mvvm 中的 viewModel。mvvm 主要解决了 mvc 中大量的 DOM 操作使页面渲染性能降低，加载速度变慢，影响用户体验。和当 Model 频繁发生变化，开发者需要主动更新到 View 。

### Vue 和 React 之间的区别

Vue 的表单可以使用 `v-model` 支持双向绑定，相比于 React 来说开发上更加方便，当然了 `v-model` 其实就是个语法糖，本质上和 React 写表单的方式没什么区别。

改变数据方式不同，Vue 修改状态相比来说要简单许多，React 需要使用 `setState` 来改变状态，并且使用这个 API 也有一些坑点。并且 Vue 的底层使用了依赖追踪，页面更新渲染已经是最优的了，但是 React 还是需要用户手动去优化这方面的问题。

React 16以后，有些钩子函数会执行多次，这是因为引入 Fiber 的原因，这在后续的章节中会讲到。

React 需要使用 JSX，有一定的上手成本，并且需要一整套的工具链支持，但是完全可以通过 JS 来控制页面，更加的灵活。Vue 使用了模板语法，相比于 JSX 来说没有那么灵活，但是完全可以脱离工具链，通过直接编写 `render` 函数就能在浏览器中运行。

在生态上来说，两者其实没多大的差距，当然 React 的用户是远远高于 Vue 的。

在上手成本上来说，Vue 一开始的定位就是尽可能的降低前端开发的门槛，然而 React 更多的是去改变用户去接受它的概念和思想，相较于 Vue 来说上手成本略高。

### react与vue的对比

> 有些是个人意见，仅供参考。

**相同点：**

```
1. 都用虚拟DOM实现快速渲染
2. 我觉得父子，兄弟通信这些都挺像的，也都有自己的状态管理器：react=>redux, vue=>vuex
3. 都是轻量级框架
4. 现在vue也在渐渐吸收react中的一些语法，比如JSX语法，类式声明写法等
```

**不同点：**

```
1. React属于单向数据流——MVC模式，vue则属于双向——MVVM模式。
2. react兼容性比vue好，vue不兼容IE8.
3. react采用JSX语法，vue采用的则是html模板语法。
4. vue的css可以有组件的私有作用域，react则没有。
5. react比vue好的另一点是，它是团队维护，而vue属于个人，一般来说，大型项目更倾向于react，小型则用vue，当然这也不是绝对。
```

### vue和react的虚拟dom有区别吗？

**没有区别。**

react和vue的虚拟dom都是一样的， 都是用JS对象来模拟真实DOM，然后用虚拟DOM的**diff**来最小化更新真实DOM。

除了极个别实现外，两者前半部分（用JS对象来模拟真实DOM）几乎是一样的。

而对于后半部分（用虚拟DOM的diff来最小化更新真实DOM）两者算法也是类似的，包括replace delete insert等

### vue首屏加载过慢如何解决？

- 路由懒加载，会将原来打包一个app.js的文件打包成多个文件 ,异步组件，按需加载
- webpack开启gzip压缩
- 如果图片过多，开启图片懒加载
- 使用cdn资源
- 如果首页是登录页，做多入口



### 你之前有做过spa类型的项目吗？怎么实现的？

spa就是单页面应用程序，而单页面应用程序，主要依靠路由来实现，路由根据不同的hash值来展示不同的组件

# 二 Vue

Vue面试题

https://zhuanlan.zhihu.com/p/97950650

### vue 的优点是什么？

- 低耦合。视图（View）可以独立于 Model 变化和修改，一个 ViewModel 可以绑定到不同的"View"上，当 View 变化的时候 Model 可以不变，当 Model 变化的时候 View 也可以不变。
- 可重用性。你可以把一些视图逻辑放在一个 ViewModel 里面，让很多 view 重用这段视图逻辑。
- 独立开发。开发人员可以专注于业务逻辑和数据的开发（ViewModel），设计人员可以专注于页面设计，使用 Expression Blend 可以很容易设计界面并生成 xml 代码。
- 可测试。界面素来是比较难于测试的，而现在测试可以针对 ViewModel 来写。

### vue生命周期的理解？

vue实例有一个完整的生命周期，生命周期也就是指一个实例从开始创建到销毁的这个过程

总共分为 8 个阶段创建前/后，载入前/后，更新前/后，销毁前/后。

- `beforeCreated()` 在实例创建之间执行，数据未加载状态
- `created()` 在实例创建、数据加载后，能初始化数据，`dom`渲染之前执行
- `beforeMount()` 虚拟`dom`已创建完成，在数据渲染前最后一次更改数据
- `mounted()` 页面、数据渲染完成，真实`dom`挂载完成
- `beforeUpadate()` 重新渲染之前触发
- `updated()` 数据已经更改完成，`dom` 也重新 `render` 完成,更改数据会陷入死循环
- `beforeDestory()` 和 `destoryed()` 前者是销毁前执行（实例仍然完全可用），后者则是销毁后执行

### vue 虚拟dom（vdom）


**什么是虚拟dom呢**
所谓的**Virtual dom**，也就是我们常说的虚拟节点，它是通过**JS**的**Object**对象模拟**DOM**中的节点，然后再通过特定的**render**方法将其渲染成真实的**DOM**的节点。
简单点讲，在Vue的底层实现上，Vue将模板编译成虚拟DOM渲染函数。结合Vue自带的响应系统，在状态改变时，Vue能够智能地计算出重新渲染组件的最小代价并应到DOM操作上。
**dom的解析流程**
浏览器渲染引擎工作流程都差不多，大致分为5步，创建DOM树——创建StyleRules——创建Render树——布局Layout——绘制Painting
**第一步**，用HTML分析器，分析HTML元素，构建一颗DOM树(标记化和树构建)。
**第二步**，用CSS分析器，分析CSS文件和元素上的inline样式，生成页面的样式表。
**第三步**，将DOM树和样式表，关联起来，构建一颗Render树(这一过程又称为Attachment)。每个DOM节点都有attach方法，接受样式信息，返回一个render对象(又名renderer)。这些render对象最终会被构建成一颗Render树。
**第四步**，有了Render树，浏览器开始布局，为每个Render树上的节点确定一个在显示屏上出现的精确坐标。
**第五步**，Render树和节点显示坐标都有了，就调用每个节点paint方法，把它们绘制出来。

### 为什么要用虚拟dom呢？来看下js用dom操作的代价

用我们传统的开发模式，原生JS或JQ操作DOM时，浏览器会从构建DOM树开始从头到尾执行一遍流程。在一次操作中，我需要更新10个DOM节点，浏览器收到第一个DOM请求后并不知道还有9次更新操作，因此会马上执行流程，最终执行10次。例如，第一次计算完，紧接着下一个DOM更新请求，这个节点的坐标值就变了，前一次计算为无用功。计算DOM节点坐标值等都是白白浪费的性能。即使计算机硬件一直在迭代更新，操作DOM的代价仍旧是昂贵的，频繁操作还是会出现页面卡顿，影响用户体验。

### vue等单页面应用及其优缺点

**缺点：**

> 不支持低版本的浏览器，最低只支持到IE9；
> 不利于SEO的优化（如果要支持SEO，建议通过服务端来进行渲染组件）；
> 第一次加载首页耗时相对长一些；
> 不可以使用浏览器的导航按钮需要自行实现前进、后退。

**优点：**

> 无刷新体验,提升了用户体验；
> 前端开发不再以页面为单位，更多地采用组件化的思想，代码结构和组织方式更加规范化，便于修改和调整；
> API 共享，同一套后端程序代码不用修改就可以用于Web界面、手机、平板等多种客户端
> 用户体验好、快，内容的改变不需要重新加载整个页面。



### 那么为什么用虚拟dom呢？

Web界面由DOM树(树的意思是数据结构)来构建，当其中一部分发生变化时，其实就是对应某个DOM节点发生了变化，
虚拟DOM就是为了解决浏览器性能问题而被设计出来的。如前，若一次操作中有10次更新DOM的动作，虚拟DOM不会立即操作DOM，而是将这10次更新的diff内容保存到本地一个JS对象中，最终将这个JS对象一次性attch到DOM树上，再进行后续操作，避免大量无谓的计算量。所以，用JS对象模拟DOM节点的好处是，页面的更新可以先全部反映在JS对象(虚拟DOM)上，操作内存中的JS对象的速度显然要更快，等更新完成后，再将最终的JS对象映射成真实的DOM，交由浏览器去绘制。

大白话：就是为了提高代码效率，渲染效果，就如往常的js jquery这些使用dom操作时都是一段复杂的过程，整个过程要遍历属性，标签啦，就是在你的代码上一遍遍寻找大半天还不知道有没有收获，很慢。而且做多件事情的时候只能一件件去做，浪费很多时间，而我们的虚拟dom呢，就是，把你想要做的事情都用小本子记起来（记在本地js对象上）在虚拟dom上更新完成后，再拿到真是的dom上去渲染，然后交给浏览器绘制页面呀。从头到末尾，人家知道每一步要做什么，而且可以同时做，还不会等到你的页面渲染完成后才出发，人家可是早早出发了，

### 真实DOM和虚拟DOM的区别

那么说回到**真实DOM**，**虚拟DOM**和**真实DOM**又有什么区别呢？
我想，应该会有一下几点：

- 虚拟DOM不会进行排版与重绘操作
- 真实DOM频繁排版与重绘的效率是相当低的
- 虚拟DOM进行频繁修改，然后一次性比较并修改真实DOM中需要改的部分，最后并在真实DOM中进行排版与重绘，减少过多DOM节点排版与重绘损耗
- 虚拟DOM有效降低大面积（真实DOM节点）的重绘与排版，因为最终与真实DOM比较差异，可以只渲染局部

### vue中v-if和v-show有什么区别？

v-if和v-show都会让元素显示和隐藏，但是v-if是通过移除和添加dom元素，v-show是通过display:none来实现隐藏

v-show是css切换，v-if是完整的销毁和重新创建
使用频繁切换时用v-show,运行时较少改变时用v-if
V-if=’false’v-if是条件渲染，当false的时候不会渲染
使用v-if的时候，如果值为false，那么页面将不会有这个html标签生成
v-show则是不管值是为true还是false，html元素都会存在，只是css中的display显示或隐藏
v-show 仅仅控制元素的显示方式，将 display 属性在 block 和 none 来回切换；而v-if会控制这个 DOM 节点的存在与否。当我们需要经常切换某个元素的显示/隐藏时，使用v-show会更加节省性能上的开销；当只需要一次显示或隐藏时，使用v-if更加合理。



### 为什么在使用v-for的时候需要添加key属性

因为vue在更新渲染dom的时候是根据新旧dom树进行对比的，使用key来给每个节点做一个唯一标识，Diff算法就可以正确的识别此节点，找到正确的位置区插入新的节点。Vue的优化点之一

### vue中的父子组件传值和兄弟组件传值都是如何实现的？

父向子传值，主要通过子组件的props，获取父组件绑定的数据

子向父传值，主要通过子组件利用$emit触发父组件上的事件

兄弟组件传值利用eventbus的方式，主要利用创建一个空的vm实例，作为中间者

### 组件之间数据共享

组件是 vue.js最强大的功能之一，而组件实例的作用域是相互独立的，这就意味着不同组件之间的数据无法相互引用。针对不同的使用场景，如何选择行之有效的通信方式？

1：props emit  缺点：如果组件嵌套层次多的话，数据传递比较繁琐
2：provide inject (依赖注入)，缺点：不支持响应式
3：this.$root         this.$parent   this.$refs
4: eventbus 缺点：数据不支持响应式
5: vuex 缺点：数据的读取和修改需要按照流程来操作，不适合小型项目

**父子通信：**
父组件向子组件传递数据可以通过 `props`；
子组件向父组件是通过 `$emit`、`$on`事件；
`provide / inject` ；
还可以通过 `$root`、`$parent`、`$refs`属性相互访问组件实例；
**兄弟通信：** `eventbus` ；`Vuex`；
**跨级通信：** `eventbus` ；`Vuex`；`provide / inject；`

### vue 的双向绑定的原理是什么(常考)

vue.js 是采用数据劫持结合发布者-订阅者模式的方式，通过 Object.defineProperty()来劫持各个属性的 setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调。

具体步骤：
第一步：需要 observe 的数据对象进行递归遍历，包括子属性对象的属性，都加上 setter 和 getter 这样的话，给这个对象的某个值赋值，就会触发 setter，那么就能监听到了数据变化

第二步：compile 解析模板指令，将模板中的变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，更新视图

第三步：Watcher 订阅者是 Observer 和 Compile 之间通信的桥梁，主要做的事情是:

- 在自身实例化时往属性订阅器(dep)里面添加自己
- 自身必须有一个 update()方法
- 待属性变动 dep.notice()通知时，能调用自身的 update() 方法，并触发 Compile 中绑定的回调，则功成身退。

第四步：MVVM 作为数据绑定的入口，整合 Observer、Compile 和 Watcher 三者，通过 Observer 来监听自己的 model 数据变化，通过 Compile 来解析编译模板指令，最终利用 Watcher 搭起 Observer 和 Compile 之间的通信桥梁，达到数据变化 -> 视图更新；视图交互变化(input) -> 数据 model 变更的双向绑定效果。

### 如何让css只在当前组件中起作用

在每一个vue组件中都可以定义各自的css，js，如果希望组件内写的css只对当前组件起作用，只需要在style中写入scoped，即：

```
<style scoped></style>
```

### `$root`、`$parent`、`$refs`


1、`$root` Vue 子组件可以通过`$root` 属性获取vue的根实例，比如在简单的项目中将公共数据放再vue根实例上(可以理解为一个全局 `store` ),因此可以代替vuex实现状态管理；

2、`$parent` 属性可以用来从一个子组件访问父组件的实例，可以替代将数据以 `prop` 的方式传入子组件的方式；当变更父级组件的数据的时候，容易造成调试和理解难度增加；

3、在子组件上使用`ref`特性后，`this.$refs` 属性可以直接访问该子组件。可以代替事件`$emit` 和`$on` 的作用。使用方式是通过 `ref` 特性为这个子组件赋予一个 ID 引用，再通过`this.$refs.testId`获取指定元素。注意：`$refs` 只会在组件渲染完成之后生效，并且它们不是响应式的。这仅作为一个用于直接操作子组件的“逃生舱”——你应该避免在模板或计算属性中访问 `$refs`。

### Vue输入框事件监听blur与change的差异

blur与change事件在绝大部分的情况下表现都非常相似，输入结束后，离开输入框，会先后触发change与blur，唯有两点例外。

1. 没有进行任何输入时，不会触发change
   在这种情况下，输入框并不会触发change事件，但一定会触发blur事件。在判断表单的修改状态时，这种差异会非常有用，通过change事件能轻易地找到哪些字段发生了变更以及其值的变更轨迹。
2. 输入后值并没有发生变更
   这种情况是指，在没有失焦的情况下，在输入框内进行返回的删除与输入操作，但最终的值与原值一样，这种情况下，keydown、input、keyup、blur都会触发，但change依旧不会触发。



### vue store存储commit 和dispatch

主要区别是：

- dispatch：含有异步操作，例如向后台提交数据，写法： `this.$store.dispatch('action方法名',值)`
- commit：同步操作，写法：`this.$store.commit('mutations方法名',值)`





### Vue项目性能优化

#### 1.懒加载

懒加载应该是提高性能的最简单有效的方式了，一个项目加上懒加载速度和逼格都会上一个台阶。懒加载的意义在于按需加载，不会让项目刚开始运行速度就很慢，能大大的优化用户体验。Vue项目懒加载分为图片懒加载和路由懒加载，具体写法如下：

**路由懒加载**

```javascript
{
  path: '/home',
  name: 'home',
  component: resolve => require(['@/components/home'],resolve)
},{
  path: '/index',
  name: 'Index',
  component: resolve => require(['@/components/index'],resolve)
},{
  path: '/about',
  name: 'about',
  component: resolve => require(['@/components/about'],resolve)
} 
```

**图片懒加载**

```
<!--使用前需先安装配置vue-lazyload-->
<a href="javascript:;"><img v-lazy="'/static/img/abc.jpg'"></a>
```

#### 2.代码优化

- 不要将所有的数据都放在data中，data中的数据都会增加getter和setter，会收集对应的 watcher
- vue 在 v-for 时给每项元素绑定事件需要用事件代理
- SPA 页面采用keep-alive缓存组件
- 拆分组件( 提高复用性、增加代码的可维护性,减少不必要的渲染 )
- v-if 当值为false时内部指令不会执行,具有阻断功能，很多情况下使用v-if替代v-show
- key 保证唯一性 ( 默认 vue 会采用就地复用策略 )
- Object.freeze 冻结数据
- 合理使用路由懒加载、异步组件
- 尽量采用runtime运行时版本
- 数据持久化的问题 （防抖、节流）
- 减少本地储存

#### 3.用户体验优化

**添加Loading**

当用户需要等待时间较长时，必须添加等待loading，这个不多说，用处大大地

**添加骨架屏**



**路由逻辑**

路由逻辑是一个项目的核心，如果路由逻辑不通的话，用户很有可能点返回按钮的时候一直在两个页面之间跳转，进入死循环。其次，路由逻辑和用户体验息息相关，比如用户下完单应该跳转到订单详情页，而不是首页等等

**样式统一**

两个页面的相同功能按钮，它的大小颜色如果不用的话就会让人感觉你的App不够专业



### vue-cli如何新增自定义指令？



分为局部指令和全局指令，通过directive，创建，

在单文件中创建局部指令，使用Vue.directive创建全局指令



### vue更新数组时触发视图更新的方法

```js
Vue.set    ==========Vue.set(target,key,value)这个方法主要是用于避开vue不能检测属性被添加的限制
Vue.set(array, indexOfItem, newValue)//indexOfItem指的索引
this.array.$set(indexOfItem, newValue)
Vue.set(obj, keyOfItem, newValue)
this.obj.$set(keyOfItem, newValue)
Vue.delete   这个方法主要用于避开vue不能检测到属性被删除；

Vue.delete(array, indexOfItem)
this.array.$delete(indexOfItem)
Vue.delete(obj, keyOfItem)
this.obj.$delete(keyOfItem)
```







## 三 Vuex

### vuex 原理

vuex 仅仅是作为 vue 的一个插件而存在，不像 Redux,MobX 等库可以应用于所有框架，vuex 只能使用在 vue 上，很大的程度是因为其高度依赖于 vue 的 computed 依赖检测系统以及其插件系统，

vuex 整体思想诞生于 flux,可其的实现方式完完全全的使用了 vue 自身的响应式设计，依赖监听、依赖收集都属于 vue 对对象 Property set get 方法的代理劫持。最后一句话结束 vuex 工作原理，vuex 中的 store 本质就是没有 template 的隐藏着的 vue 组件；

Vuex实现了一个单向数据流，在全局拥有一个State存放数据，当组件要更改State中的数据时，必须通过Mutation进行，Mutation同时提供了订阅者模式供外部插件调用获取State数据的更新。而当所有异步操作(常见于调用后端接口异步获取更新数据)或批量的同步操作需要走action，但action也是无法直接修改State的，还是需要通过Mutation来修改State的数据。最后，根据State的变化，渲染到视图上。

### vuex 是什么？怎么使用？哪种功能场景使用它？

vue 框架中全局状态管理。新建了一个目录 store，….. export 。import 引入 。场景有：单页应用中，组件之间的状态。音乐播放、登录状态、加入购物车



### 简要介绍各模块在流程中的功能：

**state**
Vuex 使用单一状态树,即每个应用将仅仅包含一个store 实例，但单一状态树和模块化并不冲突。存放的数据状态，不可以直接修改里面的数据。
**mutations**
mutations定义的方法动态修改Vuex 的 store 中的状态或数据。
**getters**
类似vue的计算属性，主要用来过滤一些数据。
**action**
actions可以理解为通过将mutations里面处里数据的方法变成可异步的处理数据的方法，简单的说就是异步操作数据。view 层通过 store.dispath 来分发 action。

**modules**
项目特别复杂的时候，可以让每一个模块拥有自己的state、mutation、action、getters,使得结构非常清晰，方便管理。

### vuex 有哪几种属性

有 5 种，分别是 state、getter、mutation、action、module

### vuex 的 store 特性是什么

- vuex 就是一个仓库，仓库里放了很多对象。其中 state 就是数据源存放地，对应于一般 vue 对象里面的 data
- state 里面存放的数据是响应式的，vue 组件从 store 读取数据，若是 store 中的数据发生改变，依赖这相数据的组件也会发生更新
- 它通过 mapState 把全局的 state 和 getters 映射到当前组件的 computed 计算属性



### vuex 的 getter 特性是什么

- getter 可以对 state 进行计算操作，它就是 store 的计算属性
- 虽然在组件内也可以做计算属性，但是 getters 可以在多给件之间复用
- 如果一个状态只在一个组件内使用，是可以不用 getters

### vuex 的 mutation 特性是什么

- action 类似于 muation, 不同在于：action 提交的是 mutation,而不是直接变更状态
- action 可以包含任意异步操作

### vue 中 ajax 请求代码应该写在组件的 methods 中还是 vuex 的 action 中

如果请求来的数据不是要被其他组件公用，仅仅在请求的组件内使用，就不需要放入 vuex 的 state 里

如果被其他地方复用，请将请求放入 action 里，方便复用，并包装成 promise 返回

### 不用 vuex 会带来什么问题

- 可维护性会下降，你要修改数据，你得维护 3 个地方
- 可读性下降，因为一个组件里的数据，你根本就看不出来是从哪里来的
- 增加耦合，大量的上传派发，会让耦合性大大的增加，本来 Vue 用 Component 就是为了减少耦合，现在这么用，和组件化的初衷相背



### 使用 Vuex 只需执行 Vue.use(Vuex)，并在 Vue 的配置中传入一个 store 对象的示例，store 是如何实现注入的？美团

Vue.use(Vuex) 方法执行的是 install 方法，它实现了 Vue 实例对象的 init 方法封装和注入，使传入的 store 对象被设置到 Vue 上下文环境的store中。因此在VueComponent任意地方都能够通过this.store 访问到该 store。

### state 内部支持模块配置和模块嵌套，如何实现的？美团

在 store 构造方法中有 makeLocalContext 方法，所有 module 都会有一个 local context，根据配置时的 path 进行匹配。所以执行如 dispatch('submitOrder', payload)这类 action 时，默认的拿到都是 module 的 local state，如果要访问最外层或者是其他 module 的 state，只能从 rootState 按照 path 路径逐步进行访问。

### 在执行 dispatch 触发 action(commit 同理)的时候，只需传入(type, payload)，action 执行函数中第一个参数 store 从哪里获取的？美团

store 初始化时，所有配置的 action 和 mutation 以及 getters 均被封装过。在执行如 dispatch('submitOrder', payload)的时候，actions 中 type 为 submitOrder 的所有处理方法都是被封装后的，其第一个参数为当前的 store 对象，所以能够获取到 { dispatch, commit, state, rootState } 等数据。

### Vuex 如何区分 state 是外部直接修改，还是通过 mutation 方法修改的？美团

Vuex 中修改 state 的唯一渠道就是执行 commit('xx', payload) 方法，其底层通过执行 this._withCommit(fn) 设置_committing 标志变量为 true，然后才能修改 state，修改完毕还需要还原_committing 变量。外部修改虽然能够直接修改 state，但是并没有修改_committing 标志位，所以只要 watch 一下 state，state change 时判断是否_committing 值为 true，即可判断修改的合法性。







## 四 vue-router

###  vue-router是什么？有哪些组件？

- Vue Router 是 [Vue.js](http://cn.vuejs.org/) 官方的路由管理器。它和 Vue.js 的核心深度集成，让构建单页面应用变得易如反掌。
- `<router-link>`和`<router-view>`和`<keep-alive>`



###  active-class 是哪个组件的属性？

active-class是router-link终端属性，用来做选中样式的切换，当router-link标签被点击时将会应用这个样式

### vue路由的钩子函数

首页可以控制导航跳转，beforeEach，afterEach等，一般用于页面title的修改。一些需要登录才能调整页面的重定向功能。

**beforeEach**主要有3个参数to，from，next：

**to**：route即将进入的目标路由对象，

**from**：route当前导航正要离开的路由

**next**：function一定要调用该方法resolve这个钩子。执行效果依赖next方法的调用参数。可以控制网页的跳转。



### 指令keep-alive

**keep-alive的作用以及好处**

在做电商有关的项目中，当我们第一次进入列表页需要请求一下数据，当我从列表页进入详情页，详情页不缓存也需要请求下数据，然后返回列表页，这时候我们使用keep-alive来缓存组件，防止二次渲染，这样会大大的节省性能。

当引入keep-alive的时候，页面第一次进入，钩子的触发顺序created-> mounted-> activated，退出时触发deactivated。当再次进入（前进或者后退）时，只触发activated。

在vue-router写着keep-alive，keep-alive的含义：

如果把切换出去的组件保留在内存中，可以保留它的状态或避免重新渲染。为此可以添加一个keep-alive指令

<component :is='curremtView' keep-alive></component>

**keep-alive**是 Vue 内置的一个组件，可以使被包含的组件保留状态，或避免重新渲染。
在vue 2.1.0 版本之后，keep-alive新加入了两个属性: include(包含的组件缓存) 与 exclude(排除的组件不缓存，优先级大于include) 。

使用方法

```
<keep-alive include='include_components' exclude='exclude_components'>
  <component>
    <!-- 该组件是否缓存取决于include和exclude属性 -->
  </component>
</keep-alive>
```

参数解释
include - 字符串或正则表达式，只有名称匹配的组件会被缓存
exclude - 字符串或正则表达式，任何名称匹配的组件都不会被缓存
include 和 exclude 的属性允许组件有条件地缓存。二者都可以用“，”分隔字符串、正则表达式、数组。当使用正则或者是数组时，要记得使用v-bind 。

### 怎么定义vue-router的动态路由？怎么获取传过来的值？

- 动态路由的创建，主要是使用path属性过程中，使用动态路径参数，以冒号开头，如下：

```javascript
{
  path: '/details/:id'
  name: 'Details'
  components: Details
}
12345
```

访问details目录下的所有文件，如果details/a，details/b等，都会映射到Details组件上。

- 当匹配到/details下的路由时，参数值会被设置到this.$route.params下，所以通过这个属性可以获取动态参数

```javascript
console.log(this.$route.params.id)
```



### vue-router 传参



name传递
to来传递
采用url传参

Params

- 只能使用name，不能使用path
- 参数不会显示在路径上
- 浏览器强制刷新参数会被清空，

Query:

- 参数会显示在路径上，刷新不会被清空
- name 可以使用path路径



### vue-router的两种模式

hash

- 原理是onhashchage事件，可以在window对象上监听这个事件



history

- 利用了HTML5 History Interface 中新增的pushState()和replaceState()方法。
- 需要后台配置支持。如果刷新时，服务器没有响应响应的资源，会刷出404，



## vue-router 的导航钩子,主要用来作用是拦截导航,让他完成跳转或取消。

全局的:**前置守卫、后置钩子（beforeEach，afterEach）beforeResolve
**单个路由独享的:**beforeEnter
**组件级的:beforeRouteEnter（不能获取组件实例 this）、beforeRouteUpdate、beforeRouteLeave
这是因为在执行路由钩子函数beforRouteEnter时候，组件还没有被创建出来；
先执行beforRouteEnter，再执行组件周期钩子函数beforeCreate，可以通过 next 获取组件的实例对象，如：next( (vm)=>{} )，参数vm就是组件的实例化对象。





### 完整的 vue-router 导航解析流程

1.导航被触发；
2.在失活的组件里调用beforeRouteLeave守卫；
3.调用全局beforeEach守卫；
4.在复用组件里调用beforeRouteUpdate守卫；
5.调用路由配置里的beforeEnter守卫；
6.解析异步路由组件；
7.在被激活的组件里调用beforeRouteEnter守卫；
8.调用全局beforeResolve守卫；
9.导航被确认；
10..调用全局的afterEach钩子；
11.DOM更新；
12.用创建好的实例调用beforeRouteEnter守卫中传给next的回调函数。







# 五 React

### 什么是React？

- React 是 Facebook 在 2011 年开发的前端 JavaScript 库。
- 它遵循基于组件的方法，有助于构建可重用的UI组件。
- 它用于开发复杂和交互式的 Web 和移动 UI。
- 尽管它仅在 2015 年开源，但有一个很大的支持社区。

###  React有什么特点？

React的主要功能如下：

1. 它使用**虚拟DOM **而不是真正的DOM。
2. 它可以用**服务器端渲染**。
3. 它遵循**单向数据流**或数据绑定。

###  列出React的一些主要优点。

React的一些主要优点是：

1. 它提高了应用的性能
2. 可以方便地在客户端和服务器端使用
3. 由于 JSX，代码的可读性很好
4. React 很容易与 Meteor，Angular 等其他框架集成
5. 使用React，编写UI测试用例变得非常容易

###  React有哪些限制？

React的限制如下：

1. React 只是一个库，而不是一个完整的框架
2. 它的库非常庞大，需要时间来理解
3. 新手程序员可能很难理解
4. 编码变得复杂，因为它使用内联模板和 JSX

###  什么是JSX？

JSX 是J avaScript XML 的简写。是 React 使用的一种文件，它利用 JavaScript 的表现力和类似 HTML 的模板语法。这使得 HTML 文件非常容易理解。此文件能使应用非常可靠，并能够提高其性能。下面是JSX的一个例子：

### 什么是虚拟DOM

虚拟DOM是一个js对象。怎么理解这个js对象呢？

我们都知道对象使用之前要定义，不定义直接使用的话，就会报错。也就是说是用不了。

而这个虚拟DOM呢，就像是一个空对象，操作虚拟DOM就像是网这个对象里面添加属性和对应的属性值。对象内的属性全部定义好了之后，

再按照这个对象里面的内容，全部转化，输送给真实的DOM，让其在页面中渲染出来。

 

因为这个对象不可能一直保持不变，在开发中出现代码的增删改是很正常的现象。

操作的是虚拟DOM，那是所以只要对象一改动，就能马上反应到虚拟DOM上。

那又是如何反应的呢？

这里面又牵扯到一个概念就是diff算法。

反应的方式就是通过diff算法来实现的。

对象更新时会生成一个新DOM对象，diff算法就是把新的DOM对象和老的DOM对象进行比较，

1、发现新属性里没有老属性，那么老属性就直接卸载；新属性直接安装

2、发现新属性的和老属性全部相同（属性值也有可能是对象，也要往下面核对），那么就保留

3、发现同一个属性里，新和老的有些值不同，那么则要排列比较，看哪些修改了，哪些是新增的，哪些是删除的。

这样在一定程度上也比更深的遍历发现同更加节约时间。

因为老对象的改变，则会使真实的DOM也会发生改变，这样就在页面上实现了刷新

在上面3的内容中，我们又可以得到一个知识点：那就是虚拟DOM的key值。

key值就相当于给这些属性值加上了一个标签，这样我们就可以通过这个key值去找到对应的值与之比较或是别的操作。

所以，这里我们就要求key值最好是独一无二的。

如果用index作为key值得话，如果值得位置有变动，那么对应的index指向的就是变动后的值，所以变动后的值的位置和原来的位置是不是一样，不能确定

因此，index作为key值会有可能会存在一定偏差。

### 为什么虚拟 dom 会提高性能?(必考)

虚拟 dom 相当于在 js 和真实 dom 中间加了一个缓存，利用 dom diff 算法避免了没有必要的 dom 操作，从而提高性能。

用 JavaScript 对象结构表示 DOM 树的结构；然后用这个树构建一个真正的 DOM 树，插到文档当中当状态变更的时候，重新构造一棵新的对象树。然后用新的树和旧的树进行比较，记录两棵树差异把 2 所记录的差异应用到步骤 1 所构建的真正的 DOM 树上，视图就更新了。



### react diff 原理（常考，大厂必考）

- 把树形结构按照层级分解，只比较同级元素。
- 给列表结构的每个单元添加唯一的 key 属性，方便比较。
- React 只会匹配相同 class 的 component（这里面的 class 指的是组件的名字）
- 合并操作，调用 component 的 setState 方法的时候, React 将其标记为 dirty.到每一个事件循环结束, React 检查所有标记 dirty 的 component 重新绘制.
- 选择性子树渲染。开发人员可以重写 shouldComponentUpdate 提高 diff 的性能。

### react 生命周期函数

- 初始化阶段：
  - getDefaultProps:获取实例的默认属性
  - getInitialState:获取每个实例的初始化状态
  - componentWillMount：组件即将被装载、渲染到页面上
  - render:组件在这里生成虚拟的 DOM 节点
  - componentDidMount:组件真正在被装载之后
- 运行中状态：
  - componentWillReceiveProps:组件将要接收到属性的时候调用
  - shouldComponentUpdate:组件接受到新属性或者新状态的时候（可以返回 false，接收数据后不更新，阻止 render 调用，后面的函数不会被继续执行了）
  - componentWillUpdate:组件即将更新不能修改属性和状态
  - render:组件重新描绘
  - componentDidUpdate:组件已经更新
- 销毁阶段：
  - componentWillUnmount:组件即将销毁













### 展示组件(Presentational component)和容器组件(Container component)之间有何不同

- 展示组件关心组件看起来是什么。展示专门通过 props 接受数据和回调，并且几乎不会有自身的状态，但当展示组件拥有自身的状态时，通常也只关心 UI 状态而不是数据的状态。
- 容器组件则更关心组件是如何运作的。容器组件会为展示组件或者其它容器组件提供数据和行为(behavior)，它们会调用 Flux actions，并将其作为回调提供给展示组件。容器组件经常是有状态的，因为它们是(其它组件的)数据源。

### 类组件(Class component)和函数式组件(Functional component)之间有何不同

- 类组件不仅允许你使用更多额外的功能，如组件自身的状态和生命周期钩子，也能使组件直接访问 store 并维持状态
- 当组件仅是接收 props，并将组件自身渲染到页面时，该组件就是一个 '无状态组件(stateless component)'，可以使用一个纯函数来创建这样的组件。这种组件也被称为哑组件(dumb components)或展示组件

### (组件的)状态(state)和属性(props)之间有何不同

- State 是一种数据结构，用于组件挂载时所需数据的默认值。State 可能会随着时间的推移而发生突变，但多数时候是作为用户事件行为的结果。
- Props(properties 的简写)则是组件的配置。props 由父组件传递给子组件，并且就子组件而言，props 是不可变的(immutable)。组件不能改变自身的 props，但是可以把其子组件的 props 放在一起(统一管理)。Props 也不仅仅是数据--回调函数也可以通过 props 传递。

### React中                                                                                                                                                           tate的用法

state 是一种`数据结构`，用于组件挂载时所需数据的默认值。state 可能会随着时间的推移而发生突变，但多数时候是作为用户事件行为的结果。
Props则是`组件的配置`。props 由父组件传递给子组件，并且就子组件而言，props 是`不可变的(immutable)`。

### setState为什么是异步的

> 参考链接：[React 中 setState() 为什么是异步的？](https://segmentfault.com/a/1190000013040438)、 [react的setstate原理](https://www.jianshu.com/p/89a04c132270)

1.保证内部的一致性

```
因为props是要等到父组件渲染过后才能拿到，也就是不能同步更新，state出于统一性设成异步更新。
```

2.性能优化

```
举例说你正在一个聊天窗口输入，如果来了一条新消息又要render,那就会阻塞你的当前操作，导致延迟什么的。
```

3.支持state在幕后渲染

```
异步可以使state在幕后更新，而不影响你当前旧的页面的交互，提升用户体验。
```



### 为什么建议传递给 setState 的参数是一个 callback 而不是一个对象

因为 this.props 和 this.state 的更新可能是异步的，不能依赖它们的值去计算下一个 state。



### 如何解决setState 异步问题



**this.setState**

 

如果需要修改this.state中的数据 必须调用this.setstate这个方法

这个方法里面有2个参数

参数1：类型 对象 key是this.state中的key值 val是修改后的数据

参数2：类型 函数 1、查看数据是否已经更新 2、可以获取到数据更新后的最新的DOM结构

 

书写方案1：this.setstate({},()=>{})

书写方案2：this.setstate(()=({}),()=>{})





```
//同步的操作
 
this.setState({count:1},()=>{
console.log(this.state.count)//输出count=1
});

//异步的操作  （常用操作）
 
this.setState({count:1})
console.log(this.state.count)
```



### 何为受控组件(controlled component)

在 HTML 中，类似 `<input>`, `<textarea>` 和 `<select>` 这样的表单元素会维护自身的状态，并基于用户的输入来更新。当用户提交表单时，前面提到的元素的值将随表单一起被发送。但在 React 中会有些不同，包含表单元素的组件将会在 state 中追踪输入的值，并且每次调用回调函数时，如 onChange 会更新 state，重新渲染组件。一个输入表单元素，它的值通过 React 的这种方式来控制，这样的元素就被称为"受控元素"。

### 何为高阶组件(higher order component)

高阶组件是一个以组件为参数并返回一个新组件的函数。HOC 运行你重用代码、逻辑和引导抽象。最常见的可能是 Redux 的 connect 函数。除了简单分享工具库和简单的组合，HOC 最好的方式是共享 React 组件之间的行为。如果你发现你在不同的地方写了大量代码来做同一件事时，就应该考虑将代码重构为可重用的 HOC。

hoc是 React 中用于重用组件逻辑的高级技术，它是一个函数，能够接受一个组件并返回一个新的组件。
实现高阶组件的两种方式：

- 属性代理。高阶组件通过包裹的React组件来操作props
- 反向继承。高阶组件继承于被包裹的React组件



### 除了在构造函数中绑定 this，还有其它方式吗

你可以使用属性初始值设定项(property initializers)来正确绑定回调，create-react-app 也是默认支持的。在回调中你可以使用箭头函数，但问题是每次组件渲染时都会创建一个新的回调。

### (在构造函数中)调用 super(props) 的目的是什么

在 super() 被调用之前，子类是不能使用 this 的，在 ES2015 中，子类必须在 constructor 中调用 super()。传递 props 给 super() 的原因则是便于(在子类中)能在 constructor 访问 this.props。

### 描述事件在 React 中的处理方式。

为了解决跨浏览器兼容性问题，您的 React 中的事件处理程序将传递 SyntheticEvent 的实例，它是 React 的浏览器本机事件的跨浏览器包装器。

这些 SyntheticEvent 与您习惯的原生事件具有相同的接口，除了它们在所有浏览器中都兼容。有趣的是，React 实际上并没有将事件附加到子节点本身。React 将使用单个事件监听器监听顶层的所有事件。这对于性能是有好处的，这也意味着在更新 DOM 时，React 不需要担心跟踪事件监听器。

### createElement 和 cloneElement 有什么区别？

React.createElement():JSX 语法就是用 React.createElement()来构建 React 元素的。它接受三个参数，第一个参数可以是一个标签名。如 div、span，或者 React 组件。第二个参数为传入的属性。第三个以及之后的参数，皆作为组件的子组件。

```
React.createElement(
    type,
    [props],
    [...children]
)
```

React.cloneElement()与 React.createElement()相似，不同的是它传入的第一个参数是一个 React 元素，而不是标签名或组件。新添加的属性会并入原有的属性，传入到返回的新元素中，而就的子元素奖杯替换。

```
React.cloneElement(
  element,
  [props],
  [...children]
)
```

### React 中有三种构建组件的方式

React.createClass()、ES6 class 和无状态函数。

### react 组件的划分业务组件技术组件？

- 根据组件的职责通常把组件分为 UI 组件和容器组件。
- UI 组件负责 UI 的呈现，容器组件负责管理数据和逻辑。
- 两者通过 React-Redux 提供 connect 方法联系起来。





### **react性能优化方案**，react在哪个生命周期做优化

（1）重写shouldComponentUpdate来避免不必要的dom操作。
（2）使用 production 版本的react.js。
（3）使用key来帮助React识别列表中所有子组件的最小变化。

`shouldComponentUpdate`，这个方法用来判断是否需要调用 render 方法重绘 dom。
因为 dom 的描绘非常消耗性能，如果我们能在这个方法中能够写出更优化的 dom diff 算法，可以极大的提高性能。

### React如何性能优化



```
1. 充分利用shouldComponentUpdate函数，不过这需要你的组件尽量最小化，如果当前组件数据过于复杂，其实是很难优化的。
2. 给你的DOM遍历上加上唯一的key,注意尽量不要用index,因为如果你新DOM中删了某一个节点，它会重新排列index，
那跟原来同层级一比就都会完全不一样，而重新渲染了，所以最好使用id值什么的作key值。

3. 能用const声明的就用const。
4. DOM里少用箭头函数，当然其实要传参时也还是得用。再者，函数bind尽量写在constructor，避免每次render重新bind。
5. 减少对真实DOM的操作。
6. 如果是用webpack搭建环境的话，当一个包过大加载过慢时，可分打成多个包来优化。
```









### **React 中 refs 的作用是什么？**

Refs 是 React 提供给我们的安全访问 DOM 元素或者某个组件实例的句柄。我们可以为元素添加ref属性然后在回调函数中接受该元素在 DOM 树中的句柄，该值会作为回调函数的第一个参数返回

### **列出一些应该使用 Refs 的情况。**

以下是应该使用 refs 的情况：

- 需要管理焦点、选择文本或媒体播放时
- 触发式动画
- 与第三方 DOM 库集成

### **在生命周期中的哪一步你应该发起 AJAX 请求？**

我们应当将AJAX 请求放到 componentDidMount 函数中执行，主要原因有下：
React 下一代调和算法 Fiber 会通过开始或停止渲染的方式优化应用性能，其会影响到 componentWillMount 的触发次数。对于 componentWillMount 这个生命周期函数的调用次数会变得不确定，React 可能会多次频繁调用 componentWillMount。如果我们将 AJAX 请求放到 componentWillMount 函数中，那么显而易见其会被触发多次，自然也就不是好的选择。
如果我们将 AJAX 请求放置在生命周期的其他函数中，我们并不能保证请求仅在组件挂载完毕后才会要求响应。如果我们的数据请求在组件挂载之前就完成，并且调用了setState函数将数据添加到组件状态中，对于未挂载的组件则会报错。而在 componentDidMount 函数中进行 AJAX 请求则能有效避免这个问题。



### **调用 setState 之后发生了什么？**

在代码中调用setState函数之后，React 会将传入的参数对象与组件当前的状态合并，然后触发所谓的调和过程（Reconciliation）。经过调和过程，React 会以相对高效的方式根据新的状态构建 React 元素树并且着手重新渲染整个UI界面。在 React 得到元素树之后，React 会自动计算出新的树与老树的节点差异，然后根据差异对界面进行最小化重渲染。在差异计算算法中，React 能够相对精确地知道哪些位置发生了改变以及应该如何改变，这就保证了按需更新，而不是全部重新渲染。



## 六 redux 



### 简述 flux 思想

Flux 的最大特点，就是数据的"单向流动"。

1. 用户访问 View
2. View 发出用户的 Action
3. Dispatcher 收到 Action，要求 Store 进行相应的更新
4. Store 更新后，发出一个"change"事件
5. View 收到"change"事件后，更新页面

### React 项目用过什么脚手架（本题是开放性题目）

creat-react-app Yeoman 等

### 了解 redux 么，说一下 redux 把

- redux 是一个应用数据流框架，主要是解决了组件间状态共享的问题，原理是集中式管理，主要有三个核心方法，action，store，reducer，工作流程是 view 调用 store 的 dispatch 接收 action 传入 store，reducer 进行 state 操作，view 通过 store 提供的 getState 获取最新的数据，flux 也是用来进行数据操作的，有四个组成部分 action，dispatch，view，store，工作流程是 view 发出一个 action，派发器接收 action，让 store 进行数据更新，更新完成以后 store 发出 change，view 接受 change 更新视图。Redux 和 Flux 很像。主要区别在于 Flux 有多个可以改变应用状态的 store，在 Flux 中 dispatcher 被用来传递数据到注册的回调事件，但是在 redux 中只能定义一个可更新状态的 store，redux 把 store 和 Dispatcher 合并,结构更加简单清晰
- 新增 state,对状态的管理更加明确，通过 redux，流程更加规范了，减少手动编码量，提高了编码效率，同时缺点时当数据更新时有时候组件不需要，但是也要重新绘制，有些影响效率。一般情况下，我们在构建多交互，多数据流的复杂项目应用时才会使用它们

### Redux是什么?

Redux是当今市场上最热门的前端开发库之一。

它是JavaScript应用程序的可预测状态容器，用于整个应用程序的状态管理。

使用Redux开发的应用程序易于测试，可以在不同的环境中运行，表现出一致的行为。

### Redux遵循的三个原则是什么?

单个事实来源：整个应用程序的状态存储在单个存储中的对象/状态树中。

单状态树使跟踪随时间的变化和调试或检查应用程序变得更容易。

状态是只读的：更改状态的惟一方法是触发一个动作。

一个动作是一个普通的JS对象，用来描述变化。

就像state是数据的最小表示一样，action是数据更改的最小表示。

使用纯函数进行更改：为了指定如何通过操作转换。

### Redux的中间件

中间件提供第三方插件的模式，自定义拦截action -> reducer 的过程。变为 action -> middlewares -> reducer。这种机制可以让我们改变数据流，实现如异步 action ，action 过滤，日志输出，异常报告等功能。

常见的中间件：

Redux-logger：提供日志输出

Redux-thunk：处理异步操作

Redux-promise：处理异步操作，actionCreator的返回值是promise



### Redux有什么好处?

Maintain ability, 由于代码风格要求比较严格，所以可维护性比较强。

Organization, 代码的组织性比较好, 这样实际上做工作的时候相对容易。

Server rendering. 服务器端创建的store会转发到客户端。

Developer tools. 方便而强大的开发者工具为调试和实时的开发提供了便利。

Ease of testing. 小函数，基本上只用来做一件事情，这使得整个测试变得非常简单。



### Redux有什么缺点?

一个组件所需要的数据，必须由父组件传过来，而不能像flux中直接从store取。

当一个组件相关数据更新时，即使父组件不需要用到这个组件，父组件还是会重新render，可能会有效率影响，或者需要写复杂的shouldComponentUpdate进行判断。

### 你如何理解“单一数据源”?

Redux使用“Store”将应用程序的整个状态存储在一个地方。

因此，所有组件的状态都存储在存储中，它们从存储本身接收更新。

单状态树使跟踪随时间的变化和调试或检查应用程序变得更容易。



### react组件的划分业务组件技术组件？

根据组件的职责通常把组件分为UI组件和容器组件。
UI 组件负责 UI 的呈现，容器组件负责管理数据和逻辑。
两者通过React-Redux 提供connect方法联系起来。

### 如何在Redux中定义操作?

React中的操作必须具有type属性，该属性指示正在执行的操作的类型。

它们必须被定义为一个字符串常量，你还可以向它添加更多的属性。

在Redux中，操作是使用名为Action creator的函数创建的。

### Store存储在Redux中的意义是什么?

存储是一个JavaScript对象，它可以保存应用程序的状态，并提供一些帮助方法来访问状态、分派操作和注册侦听器。

应用程序的整个状态/对象树保存在单个存储中。

因此，Redux非常简单和可预测。

我们可以将中间件传递给存储来处理数据，以及保存更改存储状态的各种操作的日志。所有操作都通过reduce返回一个新的状态。

### 解析Reducer的作用

Reducer是纯函数，它指定应用程序的状态如何随操作的变化而变化。

reduce通过获取以前的状态和动作来工作，然后返回一个新的状态。

它根据操作的类型确定需要执行哪种类型的更新，然后返回新值。

如果不需要做任何工作，它将返回以前的状态。







# 七 HOOKS





## 简单介绍下什么是hooks，hooks产生的背景？hooks的优点？

hooks是针对在使用react时存在以下问题而产生的：

- 组件之间复用状态逻辑很难，在hooks之前，实现组件复用，一般采用高阶组件和 Render Props，它们本质是将复用逻辑提升到父组件中，很容易产生很多包装组件，带来嵌套地域。
- 组件逻辑变得越来越复杂，尤其是生命周期函数中常常包含一些不相关的逻辑，完全不相关的代码却在同一个方法中组合在一起。如此很容易产生 bug，并且导致逻辑不一致。
- 复杂的class组件，使用class组件，需要理解 JavaScript 中 this 的工作方式，不能忘记绑定事件处理器等操作，代码复杂且冗余。除此之外，class组件也会让一些react优化措施失效。

针对上面提到的问题，react团队研发了hooks，它主要有两方面作用：

- 用于在函数组件中引入状态管理和生命周期方法
- 取代高阶组件和render props来实现抽象和可重用性

优点也很明显：

- 避免在被广泛使用的函数组件在后期迭代过程中，需要承担一些副作用，而必须重构成类组件，它帮助函数组件引入状态管理和生命周期方法。
- Hooks 出现之后，我们将复用逻辑提取到组件顶层，而不是强行提升到父组件中。这样就能够避免 HOC 和 Render Props 带来的「嵌套地域」
- 避免上面陈述的class组件带来的那些问题



## 知道hoc和render props吗，它们有什么作用？有什么弊端？

Render Props 组件和高阶组件主要用来实现抽象和可重用性。
弊端就是高阶组件和 Render Props 本质上都是将复用逻辑提升到父组件中，很容易产生很多包装组件，带来的「嵌套地域」。由于所有抽象逻辑都被其他 React 组件所隐藏，应用变成了一棵没有可读性的组件树。而那些可见的组件也很难在浏览器的 DOM 中进行跟踪。





## **什么是Render Props**

render props模式是一种非常灵活复用性非常高的模式，它可以把特定行为或功能封装成一个组件，提供给其他组件使用让其他组件拥有这样的能力。他把组件可以动态渲染的地方暴露给外部，你不用再关注组件的内部实现，只要把数据通过函数传出去就好。
**使用场景：**

- 通用业务逻辑的抽取
- 当两个平级组件之间需要单向依赖的时候，比如两个同级组件A、B，A组件需要跟随B组件的内部状态来改变自己的内部状态，我们就说A依赖B；或者B依赖A



##  Hoc


hoc是 React 中用于重用组件逻辑的高级技术，它是一个函数，能够接受一个组件并返回一个新的组件。
实现高阶组件的两种方式：

- 属性代理。高阶组件通过包裹的React组件来操作props
- 反向继承。高阶组件继承于被包裹的React组件



## hooks和hoc和render props有什么不同？

它们之间最大的不同在于，后两者仅仅是一种开发模式，而自定义的hooks是react提供的API模式，它既能更加自然的融入到react的渲染过程也更加符合react的函数编程理念。



## 介绍下常用的hooks？



- useState()，状态钩子。为函数组建提供内部状态

- useContext()，共享钩子。该钩子的作用是，在组件之间共享状态。 可以解决react逐层通过Porps传递数据，它接受React.createContext()的返回结果作为参数，使用useContext将不再需要Provider 和 Consumer。
- useReducer()，Action 钩子。useReducer() 提供了状态管理，其基本原理是通过用户在页面中发起action, 从而通过reducer方法来改变state, 从而实现页面和状态的通信。使用很像redux
- useEffect()，副作用钩子。它接收两个参数， 第一个是进行的异步操作， 第二个是数组，用来给出Effect的依赖项
- useRef()，获取组件的实例；渲染周期之间共享数据的存储(state不能存储跨渲染周期的数据，因为state的保存会触发组件重渲染）
  useRef传入一个参数initValue，并创建一个对象{ current: initValue }给函数组件使用，在整个生命周期中该对象保持不变。
- useMemo和useCallback：可缓存函数的引用或值，useMemo用在计算值的缓存，注意不用滥用。经常用在下面两种场景（要保持引用相等；对于组件内部用到的 object、array、函数等，如果用在了其他 Hook 的依赖数组中，或者作为 props 传递给了下游组件，应该使用 useMemo/useCallback）
- useLayoutEffect：会在所有的 DOM 变更之后同步调用 effect，可以使用它来读取 DOM 布局并同步触发重渲染



##  描述下hooks下怎么模拟生命周期函数，模拟的生命周期和class中的生命周期有什么区别吗



```
// componentDidMount，必须加[],不然会默认每次渲染都执行
useEffect(()=>{
}, [])

// componentDidUpdate
useEffect(()=>{
document.title = `You clicked ${count} times`;
return()=>{
// 以及 componentWillUnmount 执行的内容       
}
}, [count])

//  shouldComponentUpdate, 只有 Parent 组件中的 count state 更新了，Child 才会重新渲染，否则不会。
function Parent() {
  	const [count,setCount] = useState(0);
  	const child = useMemo(()=> <Child count={count} />, [count]);
  	return <>{count}</>
}

function Child(props) {
    return <div>Count:{props.count}</div>
}


```

这里有一个点需要注意，就是默认的useEffect（不带[]）中return的清理函数，它和componentWillUnmount有本质区别的，默认情况下return，在每次useEffect执行前都会执行，并不是只有组件卸载的时候执行。
useEffect在副作用结束之后，会延迟一段时间执行，并非同步执行，和compontDidMount有本质区别。遇到dom操作，最好使用useLayoutEffect。



## hooks中的坑，以及为什么？

- 不要在循环，条件或嵌套函数中调用Hook，必须始终在React函数的顶层使用Hook。这是因为React需要利用调用顺序来正确更新相应的状态，以及调用相应的钩子函数。一旦在循环或条件分支语句中调用Hook，就容易导致调用顺序的不一致性，从而产生难以预料到的后果。
- 使用useState时候，使用push，pop，splice等直接更改数组对象的坑，demo中使用push直接更改数组无法获取到新值，应该采用解构方式，但是在class里面不会有这个问题



```
function Indicatorfilter() {
    let [num,setNums] = useState([0,1,2,3])
    const test = () => {
        // 这里坑是直接采用push去更新num，setNums(num)是无法更新num的，必须使用num = [...num ,1]
        num.push(1)
        // num = [...num ,1]
        setNums(num)
    }
    return (
        <div className='filter'>
            <div onClick={test}>测试</div>
            <div>
                {num.map((item,index) => (
                    <div key={index}>{item}</div>
                ))}
            </div>
        </div>
    )
}


class Indicatorfilter extends React.Component<any,any>{
    constructor(props:any){
        super(props)
        this.state = {
            nums:[1,2,3]
        }
        this.test = this.test.bind(this)
    }

    test(){
         // class采用同样的方式是没有问题的
        this.state.nums.push(1)
        this.setState({
            nums: this.state.nums
        })
    }

    render(){
        let {nums} = this.state
        return(
            <div>
                <div onClick={this.test}>测试</div>
                    <div>
                        {nums.map((item:any,index:number) => (
                            <div key={index}>{item}</div>
                        ))}
                    </div>
            </div>
                       
        )
    }
}

```



- useState设置状态的时候，只有第一次生效，后期需要更新状态，必须通过useEffect



```
// TableDeail是一个公共组件，在调用它的父组件里面，我们通过set改变columns的值，以为传递给TableDeail的columns是最新的值，所以tabColumn每次也是最新的值，但是实际tabColumn是最开始的值，不会随着columns的更新而更新
const TableDeail = ({
    columns,
}:TableData) => {
    const [tabColumn, setTabColumn] = useState(columns) 
}

// 正确的做法是通过useEffect改变这个值
const TableDeail = ({
    columns,
}:TableData) => {
    const [tabColumn, setTabColumn] = useState(columns) 
    useEffect(() =>{setTabColumn(columns)},[columns])
}

```

- 闭包带来的坑，参考demo里面的state变量
  因为每次 render 都有一份新的状态，因此上述代码中的 setTimeout 使用产生了一个闭包，捕获了每次 render 后的 state，也就导致了输出了 0、1、2。如果你希望输出的内容是最新的 state 的话，可以通过 useRef 来保存 state。前文讲过 ref 在组件中只存在一份，无论何时使用它的引用都不会产生变化，因此可以来解决闭包引发的问题。
- 滥用useContent



## useState中的第二个参数更新状态和class中的this.setState区别？

1.获取修改后的值

setState

```
this.setState({
count: this.state.count + 1}, () => {
console.log(this.state.count); // 这里是监控到的最新值
})
```

useState

通过 useEffect 实现

```
const [ count, setCount ] = useState(0);
setCount(1);
useEffect(() => {
console.log(count);// 这里是监控到的最新值
}, [ count ]);
```

通过回调函数实现



##  useEffect和useLayoutEffect区别？

useEffect是render结束后，callback函数执行，但是不会阻断浏览器的渲染，算是某种异步的方式吧。但是class的componentDidMount 和componentDidUpdate是同步的,在render结束后就运行,useEffect在大部分场景下都比class的方式性能更好.

useLayoutEffect里面的callback函数会在DOM更新完成后立即执行,但是会在浏览器进行任何绘制之前运行完成,阻塞了浏览器的绘制.

### hooks useRef和ref用法

获取子组件或者DOM节点的句柄、渲染周期之间共享数据的存储

```

 //创建
 const countRef = useRef();
  //使用
 <Foo count={double} ref={countRef} goClick={goClick}></Foo>
 //获取
 console.log(countRef.current);
```





# 九  react route

### **什么是React 路由？**

React 路由是一个构建在 React 之上的强大的路由库，它有助于向应用程序添加新的屏幕和流。这使 URL 与网页上显示的数据保持同步。它负责维护标准化的结构和行为，并用于开发单页 Web 应用。 React 路由有一个简单的API。

###  **为什么React Router v4中使用 switch 关键字 ？**

虽然 **`<div>`** 用于封装 Router 中的多个路由，当你想要仅显示要在多个定义的路线中呈现的单个路线时，可以使用 “switch” 关键字。使用时，**`<switch>`** 标记会按顺序将已定义的 URL 与已定义的路由进行匹配。找到第一个匹配项后，它将渲染指定的路径。从而绕过其它路线。

### **为什么需要 React 中的路由？**

Router 用于定义多个路由，当用户定义特定的 URL 时，如果此 URL 与 Router 内定义的任何 “路由” 的路径匹配，则用户将重定向到该特定路由。所以基本上我们需要在自己的应用中添加一个 Router 库，允许创建多个路由，每个路由都会向我们提供一个独特的视图

```
<switch>
    <route exact path=’/’ component={Home}/>
    <route path=’/posts/:id’ component={Newpost}/>
    <route path=’/posts’   component={Post}/>
</switch>
```

### **列出 React Router 的优点。**

几个优点是：

1. 就像 React 基于组件一样，在 React Router v4 中，API 是 *'All About Components'*。可以将 Router 可视化为单个根组件（**`<BrowserRouter>`**），其中我们将特定的子路由（**`<route>`**）包起来。
2. 无需手动设置历史值：在 React Router v4 中，我们要做的就是将路由包装在 **`<BrowserRouter>`** 组件中。
3. 包是分开的：共有三个包，分别用于 Web、Native 和 Core。这使我们应用更加紧凑。基于类似的编码风格很容易进行切换。

## react中常用的路由配置项？

BrowserRouter 路由根组件，路径不带#号history路由
HashRouter   路由根组件，路径带#号hash路由
withRouter 对非路由渲染的组件组件进行包裹，让其具备三个属性

Route 定义路由
Link 路由跳转，没有动态属性，使用场景，非tabBar
NavLink 路由跳转，有动态属性，使用场景，tabBar
Switch 路由渲染，被其包裹的组件只会被渲染一个，包裹时最好将子组件 放在这个标签之外，父组件放在内部
Redirect 路由重定向

## reatc路由中Route渲染组件的方法有哪几种？区别是什么？

1）component：使用component渲染的路由组件会有三个自带的属性，localtion、history、match，他的缺点是不可以进行传参，不可以渲染非组件标签；

2）render：使用render渲染的路由组件可以进行路由传参，可以渲染非组件标签，但是不具备三个属性，如果需要使用的话，需要传参传递进来；

## 如何控制路由的路径完全匹配？

在NavLink或者Route标签中添加exact属性，是路径完全匹配

## react中路由传递参数的方法有哪些？

动态路由：

定义路由时：采用/:/:这种形式；

路由跳转时：路径+key形式；

接收：this.props.match.params；

query传值：

定义路由时：与正常时相同；

路由跳转时：使用字符串拼接；

接收：this.props.location.search

对象传值：

定义路由时：与正常时相同；

路由跳转时：使用对象，内部包含定义路径的对象和传递参数的对象；

接收：this.props.history.query

编程式导航：

this.props.history.push({路径部分}，{参数部分})

注意：使用对象传值以及编程式导航传值时如果页面刷新，那么传递的值就会消失；





## 一句话就能回答的面试题

**1.css只在当前组件起作用**
答：在style标签中写入**scoped**即可 例如：<style scoped></style>

**2.v-if 和 v-show 区别**
答：v-if按照条件是否渲染，v-show是display的block或none；

**3.`$route`和`$router`的区别**
答：`$route`是“路由信息对象”，包括path，params，hash，query，fullPath，matched，name等路由信息参数。而`$router`是“路由实例”对象包括了路由的跳转方法，钩子函数等。

**4.vue.js的两个核心是什么？**
答：数据驱动、组件系统

**5.vue几种常用的指令**
答：v-for 、 v-if 、v-bind、v-on、v-show、v-else

**6.vue常用的修饰符？**
答：.prevent: 提交事件不再重载页面；.stop: 阻止单击事件冒泡；.self: 当事件发生在该元素本身而不是子元素的时候会触发；.capture: 事件侦听，事件发生的时候会调用

**7.v-on 可以绑定多个方法吗？**
答：可以

**8.vue中 key 值的作用？**
答：当 Vue.js 用 v-for 正在更新已渲染过的元素列表时，它默认用“就地复用”策略。如果数据项的顺序被改变，Vue 将不会移动 DOM 元素来匹配数据项的顺序， 而是简单复用此处每个元素，并且确保它在特定索引下显示已被渲染过的每个元素。key的作用主要是为了高效的更新虚拟DOM。

**9.什么是vue的计算属性？**
答：在模板中放入太多的逻辑会让模板过重且难以维护，在需要对数据进行复杂处理，且可能多次使用的情况下，尽量采取计算属性的方式。好处：①使得数据处理结构清晰；②依赖于数据，数据更新，处理结果自动更新；③计算属性内部this指向vm实例；④在template调用时，直接写计算属性名即可；⑤常用的是getter方法，获取数据，也可以使用set方法改变数据；⑥相较于methods，不管依赖的数据变不变，methods都会重新计算，但是依赖数据不变的时候computed从缓存中获取，不会重新计算。

**10.vue等单页面应用及其优缺点**
答：优点：Vue 的目标是通过尽可能简单的 API 实现响应的数据绑定和组合的视图组件，核心是一个响应的数据绑定系统。MVVM、数据驱动、组件化、轻量、简洁、高效、快速、模块友好。
缺点：不支持低版本的浏览器，最低只支持到IE9；不利于SEO的优化（如果要支持SEO，建议通过服务端来进行渲染组件）；第一次加载首页耗时相对长一些；不可以使用浏览器的导航按钮需要自行实现前进、后退。

**11.怎么定义 vue-router 的动态路由? 怎么获取传过来的值**
答：在 router 目录下的 index.js 文件中，对 path 属性加上 /:id，使用 router 对象的 params.id 获取。



**13.第一次加载页面会触发哪几个钩子**

第一次页面加载时会触发beforeCreate,created,beforeMount,mounted

**13.dom渲染在哪个周期中就已经完成了**

DOM渲染在mounted中就已经完成了

**14.**



**15.**



**16.**



**17.**









 