### 0、MVVM如何实现原理

### 1、v-if和v-show的区别和使用场景

1、当为false的时候，v-if不会进行渲染，v-show会渲染一次，但是会有一个display:none的属性不显示。

2、当组件或元素更新不频繁的时候，使用v-if，频繁更新的时候就使用v-show，因为使用v-if，dom节点会频繁的销毁和加载，而v-show不会频繁销毁和加载，只是加了一个display:none的属性。

### 2、为何v-for中要用key

- 必须使用key 且不能index 和random
- diff算法中通过tag和key来判断，是否是smeNode
- 减少渲染次数，提升渲染性能

### 3、描述Vue组件生命周期(有父子组件的情况)

### 4、vue组件如何通讯

### 5、 描述组件渲染和更新的过程

### 6、双向数组绑定v-model的实现原理

### 7、基于vue设计一个购物车(组件结构，vuex state) 

### 8、v-html

v-html 尽量不用 会有xss风险  会覆盖子组件

### 9、computed和watch的区别和使用场景

区别：

1、computed是计算属性，它依赖其他属性计算值，而且computed有缓存，data不变则不会重新计算

2、watch是监听到值的变化就会执行回调，在回调中可以进行一些逻辑操作。

3、二者都支持对象的写法，computed对象写法跟普通的写法区别不大。

- 要注意的是watch针对引用类型，还有两个属性，一个属性是`deep`，一个属性是`immediate`，`deep:true`是为了深度监听到对象内部值的变化，immediate是立即触发回调函数。wacth监听的是引用类型的话，是无法拿到oldValue。
- 不应该在watch中使用箭头函数，箭头函数绑定了父级作用域的上下文，所以一旦使用箭头函数，this就不会指向当前Vue实例。而是当前Vue实例运行环境

应用场景：

1、当一个值需要依赖多个值的变化的时候，需要做的业务逻辑不是太复杂的情况下，就使用computed

2、当多个值需要依赖于一个值的变化的时候，需要做一些复杂业务逻辑的时候，就使用watch

### 10、如何遍历对象?

1、可以先在computed中将对象的每一个item取出来放到数组中，这种方法略显麻烦，但是兼容性好，适用于vue的一些低版本。

2、可以用v-for，v-for本来只支持遍历数组，之后也可以用来遍历对象。这种方法适用vue的一些比较新的版本。

v-for支持五种方式的遍历：

 	1、数组 两个参数 item和index

​	 2、对象 三个参数  value key  index

​     3、数字  遍历数字会从1开始遍历

​	 4、字符串，会将字符串中的每一个字符遍历出来

​     5、也支持遍历一些可迭代的值，比如Map、Set，但这个不是响应式的。

使用v-for遍历的时候，需要注意的是key不可以用随机数或者index下标，一定要用跟自己业务相关的唯一属性值来绑定。

v-for跟v-if不能够一起使用，这是因为v-for的优先级比起v-if更高。当一起使用的时候，v-for就先将所有元素遍历出来，先进行一次渲染，然后再去判断元素该不该渲染，这样做的话，性能是比较低的。

### 11、event事件

如果这个函数不带参数，就会默认带一个event，

如果这个函数带有参数，要使用event的话，就必须在使用的时候传一个参数$event

event是原生的、而且事件会被挂载到当前元素

vue给事件添加一些修饰符。

事件修饰符：

- stop  阻止事件继续传播
- prevent 阻止默认事件，提交事件不再重载页面
- 修饰符可以串联
- form元素上可以只有修饰符
- capture  添加事件监听器使用事件捕获模式，也就是内部元素触发的事件先在此处理，然后才交由内部元素进行处理
- self 事件不是从内部元素触发的，只有自身触发的才会处理

按键修饰符:

- 只有ctrl的时候 即使alt或者shift被一同按下之后也会触发
- ctrl+eact 有且只有ctrl被按下的时候才触发

	### 12、组件之间的通讯方式

- 父子组件通信

    - props和this.$emit  

    - `$parent/$children`和`$ref`

        - ref如果在普通的dom元素上使用，引用指向的就是DOM元素，如果在子组件上 ，引用就指向组件实例

        - `$parent/$children`是获取父/子组件的实例

            

- 兄弟组件之间的通信

    - eventBus
        - 这种方式是利用了vue实例中自带的`$on`和`$off`、`$emit`进行通讯
        - `$emit`抛发事件，`$on`监听事件，`$off`销毁事件

- 跨层级之间的组件通讯

    - `provide` 和`inject`(2.2.0新增)
        - **以允许一个祖先组件向其所有子孙后代注入一个依赖，不论组件层次有多深，并在起上下游关系成立的时间里始终生效**。一言而蔽之：祖先组件中通过 provider 来提供变量，然后在子孙组件中通过 inject 来注入变量。
        - **provide / inject API 主要解决了跨级组件间的通信问题，不过它的使用场景，主要是子组件获取上级组件的状态，跨级组件间建立了一种主动提供与依赖注入的关系**。
    - `$attrs`和`$listeners`(2.4.0新增)
        - `$attrs`包含了父作用域中不被prop所识别的特性。当一个组件没有声明任何 prop 时，这里会包含所有父作用域的绑定 (class 和 style 除外)，并且可以通过 v-bind="$attrs" 传入内部组件。通常配合 interitAttrs 选项一起使用。
        - `$listeners`：包含了父作用域中的 (不含 .native 修饰器的) v-on 事件监听器。它可以通过 v-on="$listeners" 传入内部组件

- 任意组件之间的通信

    - vuex
    - vuex涉及到全局变量的更新，如果可以不用，那就尽量不使用

- 利用本地缓存技术实现不同组件间的通信，如localStorage和sessionStorage

    - 这种通信比较简单，缺点是数据和状态比较混乱，不太容易维护

### 13、组件生命周期

- 挂载
- 更新
- 销毁

### 14、自定义v-model

v-model本质上是v-bind和v-on的语法糖。

### 15、$nextTick

- Vue是异步渲染(原理部分会详细讲解)
- data改变之后，Dom不会立刻渲染
- $nextTick会在DOM渲染之后被触发，以获取最新的DOM节点
- 页面渲染会将data中的修改做一个整合，多次连续的data修改只会渲染一次

### 16、slot

### 17、动态、异步组件

- 动态组件是通过`<compoent :is="组件名称" />`来实现的
- 需要根据数据，动态渲染的场景来渲染不同的组件。也就是说组件类型不确定
- 异步组件
    - import函数
    - 按需加载，异步加载大组件

### 18、keep-alive

缓存组件

频繁切换 不需要重新渲染

Vue常见性能优化

### 19、mixin

- 变量来源不明确，不利于阅读
- 多个mixin命名容易冲突
- mixin和组件可能会出现多对多的问题，复杂度较高

### 20、Vuex使用

### 21、Vue router



### 22、MVVM

数据驱动视图：

- Vue MVVM
- React setState

与MVC、mvp的对比，mvvm的优劣

你怎么理解MVVM模型

如何实现mvvm

### 23、Vue响应式原理

- vue2核心API-Object.defineProperty
- vue3核心API-Proxy

这二者的优劣对比

Object.defineProperty实现响应式

- 监听对象 监听数组
- 复杂对象 深度监听
- 几个缺点
    - 深度监听，需要递归到低，一次性计算量过大
    - 无法监听新增属性和删除属性(如果要新增或者删除，一定要使用Vue.set和Vue.delete)
    - 无法原生监听数组，需要特殊处理

### 24、虚拟dom

背景：

- DOM操作非常耗费性能
- 以前用jquery ，可以执行控制DOM操作的时机，手动调整
- Vue react是数据驱动视图，如何有效控制DOM操作

解决方案 v-dom：

- 有了一定复杂度，想减少计算次数比较难
- 能不能把dom操作，更多的转移到js计算，，因为js执行速度相对操作dom来说要快很多
- v-dom  用js模拟dom结构 计算出最小的变更 操作dom
- react v-dom具体事项和vue并不同  



### 25、diff算法

- diff是对比的意思，比如linux diff 和 git diff等
- 两个js对象也可以做diff
- 真实dom树与虚拟dom做对比
- 树的diff的事件复杂度是O(n^3)(不可用的算法)
    - 第一 遍历 tree1  
    - 第二 遍历 tree2
    - 第三 排序
- 优化事件复杂度到O(n)
    - 只比较统一层级，不去跨级比较
    - tag不相同，这直接删掉重建  不去深度比较
    - tag key 两者都相同，认为是相同节点，不去深度比较

### 26、data为什么是一个函数而不能是对象

### 27、ajax请求应该放在哪个生命周期中

mouted

js是单线程的，ajax异步获取数据

放在mounted之前没有用，只会让逻辑更加混乱

### 28、如何将组件所有prop传递到子组件中

$props

<Com v-bind="$props"/> 

### 29、什么时候使用keep-alive

缓存组件 不需要重复渲染

比如说多个静态tab也的切换

优化性能

### 30、什么时候使用beforeDestory

- 解绑自定义事件event.$off
- 清除定时器
- 解绑自定义的DOM事件，如window scroll等

### 31、什么是作用域插槽

### 32、Vuex action和mutation有何区别

- action中处理异步 mutation不可以
- mutation做原子操作
- action可以整合多个mutation

### 33、vue-router常用的路由模式

- hash
- history(需要服务器支持)
- 二者之间的比较

### 34、如何配置vue-router异步加载

### 35、请用vnode描述一个dom结构

```js
{
    tag:'div',
        props:{
            className:'con',
             id:'div1'
        },
            children:[
                {
                    tag:'p',
                    children:'vdom'
                }
            ]
}
```



### 36、请描述响应式原理

监听data变化

组件渲染和更新的流程



### 37、Vue为什么是异步渲染，$nextTick何用

- 异步渲染，可以合并多次data的修改，以提高渲染性能
- $nextTick在dom更新完之后，触发回调

### 38、Vue常见的性能优化方案

- 合理使用v-show和v-if
- 合理使用computed
- v-for 一定要加可以 ，避免与v-if同时使用
- 自定义事件，DOM事件及时销毁
- 合理使用异步组件
- 合理使用keep-alive
- data层级不要太深，因为响应式是会一次性遍历完，去做深度监听的，太深的话，性能太低了。
- 使用vue-loader在开发环境下做模板编译(预编译)
- webpack层面的优化
- 图片懒加载，组件按需加载
- 使用ssr，服务端渲染

### 39、nextTick原理

nextTick可以让我们在下次DOM 更新循环结束之后执行延迟回调，用于获得更新后的DOM。

在vue2.4之前，使用的都是microtasks,但是microtasks的优先级过高，在某些情况下可能会出现比时间冒泡更快的情况。但如果都使用macrotasks又可能会出现渲染的性能问题，所以在新版本中，会默认使用microtasks，但在特殊情况下使用marotasks 比如v-on



对于实现macrotasks  会先能否使用setImmediate  不能的话降级为MessageChannel ,以上都不行的娿就使用setTimout

### 40、vue的虚拟dom 

它使用的是第三方的一个虚拟dom库，snabbdom,根本思想与react的相同，细节上不同

