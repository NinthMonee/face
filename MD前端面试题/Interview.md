

## 面试题

### 公司经验

#### 与后端沟通配合

前后端都要充分了解项目的需求

后端只提供能力（如数据库能力、消息能力、应用协同能力等等）

前端组织业务（如组织Auth、组织数据POST/GET/DELETE）

接口设计（出接口文档），前后端沟通设计接口，前端需要后台返回什么样的数据（格式），后台需要前端传递什么参数（哪些参数是必须的，哪些参数是可选的，采用get还是post，哪些数据需要前端先进行校验，哪些需要双方都校验）。共同制定出整个程序所有的接口说明，形成文档。前后台按照约定好的接口进行开发

#### 项目遇到了的难点及解决方案

短时间的大访问量：	网站服务器	同网站，不同项目部署，/独立域名 避免对网站造成影响

高并发问题不停刷新：	数据库	页面静态化

带宽 200k的页面 并发1w次 ：	秒杀页缓存cdn 租借临时带宽，反向代理服务器，nginx ，甚至用户浏览器。（cookie）

不能提前下单：	服务器url动态化，＋随机数

下单之后的被抢的问题：	库加 乐观锁





#### 项目开发流程

![img](https://mmbiz.qpic.cn/mmbiz_png/iappkPENyMQic0yZRtgb303icrOSKkhut3FF4033ga1tZdDeu2R2wTsubibzwGSXfamtFW0YEKJGQcWhUBq6ZkTLNQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

PM：产品经理，也是一个项目的推动者，即兼职项目经理的角色。

UE：交互设计师，负责页面布局、交互的设计，不负责视图的细节。

UI：视觉设计师，交互确定之后，设计页面样式。注意，很多情况下，UE 和 UI 是一个人。

RD：后端开发人员。

CRD：客户端开发人员，安卓和 ios 都是。

FE：前端开发人员。

QA：测试人员。

OP：服务器运维人员，一般负责审批上线单



#### 对前端职位是怎么样理解的？它的前景怎样

- 前端是最贴近用户的程序员，比后端、数据库、产品经理、运营、安全都近
  - 实现界面交互
  - 提升用户体验
  - 基于NodeJS，可跨平台开发
- 前端是最贴近用户的程序员，前端的能力就是能让产品从 90分进化到 100 分，甚至更好，
- 与团队成员，`UI`设计，产品经理的沟通；
- 做好的页面结构，页面重构和用户体验；

#### 你觉得前端工程的价值体现在哪

- 为简化用户使用提供技术支持（交互部分）
- 为多个浏览器兼容性提供支持
- 为提高用户浏览速度（浏览器性能）提供支持
- 为跨平台或者其他基于webkit或其他渲染引擎的应用提供支持
- 为展示数据提供支持（数据接口）

#### 平时如何管理你的项目

- 先期团队必须确定好全局样式（`globe.css`），编码模式(`utf-8`) 等；
- 编写习惯必须一致（例如都是采用继承式的写法，单样式都写成一行）；
- 标注样式编写人，各模块都及时标注（标注关键样式调用的地方）；
- 页面进行标注（例如 页面 模块 开始和结束）；
- `CSS`跟`HTML` 分文件夹并行存放，命名都得统一（例如`style.css`）；
- `JS` 分文件夹存放 命名以该`JS`功能为准的英文翻译。
- 图片采用整合的 `images.png png8` 格式文件使用 - 尽量整合在一起使用方便将来的管理

#### 面试完你还有什么问题要问的吗

#### 你有什么爱好?

#### 你最大的优点和缺点是什么?

#### 你为什么会选择这个行业，职位?

#### 你觉得你适合从事这个岗位吗?

#### 你有什么职业规划?

#### 你对工资有什么要求?

#### 如何看待前端开发？

#### 未来三到五年的规划是怎样的？

#### 你们部门的开发流程是怎样的

#### 你认为哪个项目做得最好？

#### 最近在看哪些前端方面的书？

#### 平时是如何学习前端开发的？

#### 你最有成就感的一件事

#### 你为什么要离开前一家公司？

#### 你对加班的看法

#### 你希望通过这份工作获得什么？

------



### 原理思想

#### diff算法

React用 三大策略 将O(n^3)复杂度 转化为 **O(n)复杂度**

策略一（**tree diff**）：
 Web UI中DOM节点跨层级的移动操作特别少，可以忽略不计。

策略二（**component diff**）：
 拥有相同类的两个组件 生成相似的树形结构，
 拥有不同类的两个组件 生成不同的树形结构。

策略三（**element diff**）：
 对于同一层级的一组子节点，通过唯一id区分

#### 虚拟DOM

虚拟 dom 相当于在 js 和真实 dom 中间加了一个缓存，利用 dom diff 算法避免了没有必要的 dom 操作，从而提高性能。

用 JavaScript 对象结构表示 DOM 树的结构；然后用这个树构建一个真正的 DOM 树，插到文档当中当状态变更的时候，重新构造一棵新的对象树。然后用新的树和旧的树进行比较，记录两棵树差异把 2 所记录的差异应用到步骤 1 所构建的真正的 DOM 树上，视图就更新了。

#### 状态管理

##### redux

是什么：是一个应用数据流框架，集中式管理

怎么用：view 调用 store 的 dispatch 接收 action 传入 store，reducer 进行 state 操作，view 通过 store 提供的 getState 获取最新的数据

缺点： state必须由父组件传过来，子组件相关数据更新，父组件会强渲

##### flux 

单向数据流

1. 用户访问 View
2. View 发出用户的 Action
3. Dispatcher 收到 Action，要求 Store 进行相应的更新
4. Store 更新后，发出一个"change"事件
5. View 收到"change"事件后，更新页面

##### vuex

：vuex 仅仅是作为 vue 的一个插件而存在，不像 Redux,MobX 等库可以应用于所有框架，vuex 只能使用在 vue 上，很大的程度是因为其高度依赖于 vue 的 computed 依赖检测系统以及其插件系统，

vuex 整体思想诞生于 flux,可其的实现方式完完全全的使用了 vue 自身的响应式设计，依赖监听、依赖收集都属于 vue 对对象 Property set get 方法的代理劫持。最后一句话结束 vuex 工作，vuex 中的 store 本质就是没有 template 的隐藏着的 vue 组件；

使用：在 main.js 引入 store，注入。新建了一个目录 store，….. export 

场景：单页应用中，组件之间的状态。音乐播放、登录状态、加入购物车



#### MVVM

MVVM 是 Model-View-ViewModel 的缩写。mvvm 是一种设计思想。Model 层代表数据模型，也可以在 Model 中定义数据修改和操作的业务逻辑；View 代表 UI 组件，它负责将数据模型转化成 UI 展现出来，ViewModel 是一个同步 View 和 Model 的对象。

在 MVVM 架构下，View 和 Model 之间并没有直接的联系，而是通过 ViewModel 进行交互，Model 和 ViewModel 之间的交互是双向的， 因此 View 数据的变化会同步到 Model 中，而 Model 数据的变化也会立即反应到 View 上。

ViewModel 通过双向数据绑定把 View 层和 Model 层连接了起来，而 View 和 Model 之间的同步工作完全是自动的，无需人为干涉，因此开发者只需关注业务逻辑，不需要手动操作 DOM, 不需要关注数据状态的同步问题，复杂的数据状态维护完全由 MVVM 来统一管理

#### mvc

mvc 和 mvvm 其实区别并不大。都是一种设计思想。主要就是 mvc 中 Controller 演变成 mvvm 中的 viewModel。mvvm 主要解决了 mvc 中大量的 DOM 操作使页面渲染性能降低，加载速度变慢，影响用户体验。和当 Model 频繁发生变化，开发者需要主动更新到 View



#### 双向绑定

vue.js 是采用数据劫持结合发布者-订阅者模式的方式，通过 Object.defineProperty()来劫持各个属性的 setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调。

第一步：需要 observe 的数据对象进行递归遍历，包括子属性对象的属性，都加上 setter 和 getter 这样的话，给这个对象的某个值赋值，就会触发 setter，那么就能监听到了数据变化

第二步：compile 解析模板指令，将模板中的变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，更新视图

第三步：Watcher 订阅者是 Observer 和 Compile 之间通信的桥梁，主要做的事情是:

- 在自身实例化时往属性订阅器(dep)里面添加自己
- 自身必须有一个 update()方法
- 待属性变动 dep.notice()通知时，能调用自身的 update() 方法，并触发 Compile 中绑定的回调，则功成身退。

第四步：MVVM 作为数据绑定的入口，整合 Observer、Compile 和 Watcher 三者，通过 Observer 来监听自己的 model 数据变化，通过 Compile 来解析编译模板指令，最终利用 Watcher 搭起 Observer 和 Compile 之间的通信桥梁，达到数据变化 -> 视图更新；视图交互变化(input) -> 数据 model 变更的双向绑定效果

#### 路由

hash模式：在浏览器中符号“#”，用window.location.hash读取

特点：hash虽然在URL中，但不被包括在HTTP请求中；用来指导浏览器动作，对服务端安全无用，hash不会重加载页面。仅 hash 符号之前的内容会被包含在请求中，如 http://www.xxx.com，因此对于后端来说，即使没有做到对路由的全覆盖，也不会返回 404 错误。

history模式：history采用HTML5的新特性；且提供了两个新方法：pushState（），replaceState（）可以对浏览器历史记录栈进行修改，以及popState事件的监听到状态变更。

特点：前端的 URL 必须和实际向后端发起请求的 URL 一致，如 http://www.xxx.com/items/id。后端如果缺少对 /items/id 的路由处理，将返回 404 错误。你要在服务端增加一个覆盖所有情况的候选资源：如果 URL 匹配不到任何静态资源，则应该返回同一个 index.html 页面，这个页面就是你 app 依赖的页面。

#### 框架处理事件

React 的事件是合成事件，React 在组件加载(mount)和更新(update)时，将事件通过 addEventListener  统一注册到 document 上，然后会有一个事件池存储了所有的事件，当事件触发的时候，通过 dispatchEvent 进行事件分发

#### 高阶组件(higher order component)

高阶组件是一个以组件为参数并返回一个新组件的函数。HOC 运行你重用代码、逻辑和引导抽象。最常见的可能是 Redux 的 connect 函数。除了简单分享工具库和简单的组合，HOC 最好的方式是共享 React 组件之间的行为。如果你发现你在不同的地方写了大量代码来做同一件事时，就应该考虑将代码重构为可重用的 HOC。

#### axios

axios其实就是在ajax的基础上加了promise，具体如下：

```
const myAxios = {
        get: function(url) {
            return new Promise((resolve, reject) => {
                var xhr = new XMLHttpRequest();
                xhr.open('GET', url, true);
                xhr.onreadystatechange = function() {
                    if (xhr.readyState == 4 && xhr.status == 200) {
                        resolve(xhr.responseText)
                    }
                };
                xhr.send();
            })
        }
```

#### 304的缓存

```
1.服务器首先产生Etag,服务器可在稍后使用它来判断页面是否被修改。本质上，客户端通过该记号传回服务器要求服务器验证（客户端）缓存）
2.304是	HTTP的状态码，服务器用来标识这个文件没有被修改，不返回内容，浏览器接受到这个状态码会去去找浏览器缓存的文件
3.流程：客户端请求一个页面A。服务器返回页面A，并在A上加一个Tage客服端渲染该页面，并把Tage也存储在缓存中。客户端再次请求页面A
	并将上次请求的资源和ETage一起传递给服务器。服务器检查Tage.并且判断出该页面自上次客户端请求之后未被修改。直接返回304

last-modified: 客服端请求资源，同时有一个last-modified的属性标记此文件在服务器最后修改的时间
		客服端第二次请求此url时，根据http协议。浏览器会向服务器发送一个If-Modified-Since报头，
		询问该事件之后文件是否被修改，没修改返回304

 有了Last-Modified，为什么还要用ETag？
  1、因为如果在一秒钟之内对一个文件进行两次更改，Last-Modified就会不正确（Last—Modified不能识别秒单位的修改）
  2、某些服务器不能精确的得到文件的最后修改时间
  3、一些文件也行会周期新的更改，但是他的内容并不改变（仅仅改变修改的事件），这个时候我们并不希望客户端认为文件被修改，而重新Get

ETag，为什么还要用Last-Modified？
  1、两者互补，ETag的判断的缺陷，比如一些图片等静态文件的修改
  2、如果每次扫描内容都生成ETag比较，显然要比直接比较修改时间慢的多。


ETag是被请求变量的实体值（文件的索引节，大小和最后修改的时间的Hash值）
  1、ETag的值服务器端对文件的索引节，大小和最后的修改的事件进行Hash后得到的。

```

#### 浏览器渲染

```
流程：解析html以及构建dom树 -> 构建render树 ->  布局render树 -> 绘制render树
概念：1.构建DOM树： 渲染引擎解析HTML文档，首先将标签转换成DOM树中的DOM node(包括js生成的标签)生成内容树
      2.构建渲染树： 解析对应的css样式文件信息（包括js生成的样式和外部的css）
      3.布局渲染树：从根节点递归调用，计算每一个元素的大小，位置等。给出每个节点所在的屏幕的精准位置
      4.绘制渲染树：遍历渲染树，使用UI后端层来绘制每一个节点

重绘：当盒子的位置、大小以及其他属性，例如颜色、字体大小等到确定下来之后，浏览器便把这些颜色都按照各自的特性绘制一遍，将内容呈现在页面上
	触发重绘的条件：改变元素外观属性。如：color，background-color等
	重绘是指一个元素外观的改变所触发的浏览器行为，浏览器会根据元素的新属性重新绘制，使元素呈现新的外观
注意：table及其内部元素需要多次计算才能确定好其在渲染树中节点的属性值，比同等元素要多发时间，要尽量避免使用table布局

重排（重构/回流/reflow）： 当渲染书中的一部分（或全部）因为元素的规模尺寸，布局，隐藏等改变而需要重新构建，这就是回流。
	每个页面都需要一次回流，就是页面第一次渲染的时候

重排一定会影响重绘，但是重绘不一定会影响重排

```

#### babel

> `ES6、7`代码输入 -> `babylon`进行解析 -> 得到`AST`（抽象语法树）-> `plugin`用b`abel-traverse`对`AST`树进行遍历转译 ->得到新的`AST`树->用`babel-generator`通过`AST`树生成`ES5`代码

#### SSR

- `app.js` 作为客户端与服务端的公用入口，导出 `Vue` 根实例，供客户端 `entry` 与服务端 `entry` 使用。客户端 `entry` 主要作用挂载到 `DOM` 上，服务端 `entry` 除了创建和返回实例，还进行路由匹配与数据预获取。
- `webpack` 为客服端打包一个 `Client Bundle` ，为服务端打包一个 `Server Bundle` 。
- 服务器接收请求时，会根据 `url`，加载相应组件，获取和解析异步数据，创建一个读取 `Server Bundle` 的 `BundleRenderer`，然后生成 `html` 发送给客户端。
- 客户端混合，客户端收到从服务端传来的 `DOM` 与自己的生成的 DOM 进行对比，把不相同的 `DOM` 激活，使其可以能够响应后续变化，这个过程称为客户端激活 。为确保混合成功，客户端与服务器端需要共享同一套数据。在服务端，可以在渲染之前获取数据，填充到 `stroe` 里，这样，在客户端挂载到 `DOM` 之前，可以直接从 `store`里取数据。首屏的动态数据通过 `window.__INITIAL_STATE__`发送到客户端

> `Vue SSR` 的实现，主要就是把 `Vue` 的组件输出成一个完整 `HTML`, `vue-server-renderer` 就是干这事的

- `Vue SSR`需要做的事多点（输出完整 HTML），除了`complier -> vnode`，还需如数据获取填充至 `HTML`、客户端混合（`hydration`）、缓存等等。 相比于其他模板引擎（`ejs`, `jade` 等），最终要实现的目的是一样的，性能上可能要差点

#### Vue computed

- 建立与其他属性（如：`data`、 `Store`）的联系；
- 属性改变后，通知计算属性重新计算

> 实现时，主要如下

- 初始化 `data`， 使用 `Object.defineProperty` 把这些属性全部转为 `getter/setter`。
- 初始化 `computed`, 遍历 `computed` 里的每个属性，每个 `computed` 属性都是一个 `watch` 实例。每个属性提供的函数作为属性的 `getter`，使用 `Object.defineProperty` 转化。
- `Object.defineProperty getter` 依赖收集。用于依赖发生变化时，触发属性重新计算。
- 若出现当前 `computed` 计算属性嵌套其他 `computed` 计算属性时，先进行其他的依赖收集

#### Vue complier

- 模板解析这种事，本质是将数据转化为一段 `html` ，最开始出现在后端，经过各种处理吐给前端。随着各种 `mv*` 的兴起，模板解析交由前端处理。
- 总的来说，`Vue complier` 是将 `template` 转化成一个 `render` 字符串。

> 可以简单理解成以下步骤：

- `parse` 过程，将 `template` 利用正则转化成`AST` 抽象语法树。
- `optimize` 过程，标记静态节点，后 `diff` 过程跳过静态节点，提升性能。
- `generate` 过程，生成 `render` 字符串

#### 离线储存

- 在线的情况下，浏览器发现`html`头部有`manifest`属性，它会请求`manifest`文件，如果是第一次访问`app`，那么浏览器就会根据manifest文件的内容下载相应的资源并且进行离线存储。如果已经访问过`app`并且资源已经离线存储了，那么浏览器就会使用离线的资源加载页面，然后浏览器会对比新的`manifest`文件与旧的`manifest`文件，如果文件没有发生改变，就不做任何操作，如果文件改变了，那么就会重新下载文件中的资源并进行离线存储。
- 离线的情况下，浏览器就直接使用离线存储的资源。

------



### vue

#### 聊聊对vue的理解

> vue是一个渐进式的JS框架。他易用，灵活，高效； 可以把一个页面分隔成多个组件；当其他页面有类似功能时，直接让封装的组件进行复用； 他是构建用户界面的声明式框架，只关心图层；不关心具体是如何实现的

#### V-model的是什么？

> Vue的双向数据绑定是由数据劫持结合发布者订阅者实现的。 数据劫持是通过Object.defineProperty()来劫持对象数据的setter和getter操作。 在数据变动时作你想做的事
>
> -  通过Observer来监听自己的model数据变化，通过Compile来解析编译模板指令，最终利用Watcher搭起Observer和Compile之间的通信桥梁，达到数据变化->视图更新 在初始化vue实例时，遍历data这个对象，给每一个键值对利用Object.definedProperty对data的键值对新增get和set方法，利用了事件监听DOM的机制，让视图去改变数据

#### 谈谈对生命周期的理解

- beforeCreate阶段：vue实例的挂载元素el和数据对象data都是undefined，还没有初始化。
- created阶段：vue实例的数据对象data有了，可以访问里面的数据和方法，未挂载到DOM，el还没有
- beforeMount阶段：vue实例的el和data都初始化了，但是挂载之前为虚拟的dom节点
- mounted阶段：vue实例挂载到真实DOM上，就可以通过DOM获取DOM节点
- beforeUpdate阶段：响应式数据更新时调用，发生在虚拟DOM打补丁之前，适合在更新之前访问现有的DOM，比如手动移除已添加的事件监听器
- updated阶段：虚拟DOM重新渲染和打补丁之后调用，组成新的DOM已经更新，避免在这个钩子函数中操作数据，防止死循环
- beforeDestroy阶段：实例销毁前调用，实例还可以用，this能获取到实例，常用于销毁定时器，解绑事件
- destroyed阶段：实例销毁后调用，调用后所有事件监听器会被移除，所有的子实例都会被销毁

#### VUE和REACT有什么区别？

> react整体是函数式的思想，把组件设计成纯组件，状态和逻辑通过参数传入，所以在react中，是单向数据流；

> vue的思想是响应式的，也就是基于是数据可变的，通过对每一个属性建立Watcher来监听，当属性变化的时候，响应式的更新对应的虚拟dom。

#### vuex的流程

```
页面通过mapAction异步提交事件到action。action通过commit把对应参数同步提交到mutation。
mutation会修改state中对于的值。 最后通过getter把对应值跑出去，在页面的计算属性中
通过mapGetter来动态获取state中的值

```

#### vuex有哪几种状态和属性

> - state中保存着共有数据，数据是响应式的
> - getter可以对state进行计算操作，主要用来过滤一些数据，可以在多组件之间复用
> - mutations定义的方法动态修改state中的数据，通过commit提交方法，方法必须是同步的
> - actions将mutations里面处理数据的方法变成异步的，就是异步操作数据，通store.dispatch来分发actions，把异步的方法写在actions中，通过commit提交mutations，进行修改数据。
> - modules：模块化vuex

#### vue路由的两种模式

> - hash ——即地址栏URL中的#符号（此hsah 不是密码学里的散列运算） hash 虽然出现URL中，但不会被包含在HTTP请求中，对后端完全没有影响，因此改变hash不会重新加载页面。
> - history ——利用了HTML5 History Interface 中新增的pushState() 和replaceState() 方法
>
> 这两个方法应用于浏览器的历史记录站，在当前已有的back、forward、go 的基础之上，它们提供了对历史记录进行修改的功能。只是当它们执行修改是，虽然改变了当前的URL，但你浏览器不会立即向后端发送请求。

#### vue中 key 值的作用

> 当 Vue.js 用v-for正在更新已渲染过的元素列表时，它默认用“就地复用”策略。 如果数据项的顺序被改变，Vue 将不会移动 DOM 元素来匹配数据项的顺序，而是简单复用此处每个元素，并且确保它在特定索引下显示已被渲染过的每个元素。

> key的作用主要是为了高效的更新虚拟DOM。

#### `$route`和`$router`的区别

> - $route是“路由信息对象”，包括path，params，hash，query，fullPath，matched，name等路由信息参数。
> - $router是“路由实例”对象包括了路由的跳转方法，钩子函数等。

#### vue-router守卫

> - 导航守卫 router.beforeEach 全局前置守卫

- to: Route: 即将要进入的目标（路由对象）
- from: Route: 当前导航正要离开的路由
- next: Function: 一定要调用该方法来 resolve 这个钩子。（一定要用这个函数才能去到下一个路由，如果不用就拦截） 执行效果依赖 next 方法的调用参数。
- next(): 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 confirmed (确认的)。
- next(false): 取消进入路由，url地址重置为from路由地址(也就是将要离开的路由地址)。

```
// main.js 入口文件
    import router from './router'; // 引入路由
    router.beforeEach((to, from, next) => { 
      next();
    });
    router.beforeResolve((to, from, next) => {
      next();
    });
    router.afterEach((to, from) => {
      console.log('afterEach 全局后置钩子');
    });


```

路由独享的守卫 你可以在路由配置上直接定义 beforeEnter 守卫

```
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})

```

组件内的守卫 你可以在路由组件内直接定义以下路由导航守卫

```
const Foo = {
  template: `...`,
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用，我们用它来禁止用户离开
    // 可以访问组件实例 `this`
    // 比如还未保存草稿，或者在用户离开前，
    将setInterval销毁，防止离开之后，定时器还在调用。
  }
}

```

#### axios是什么？怎么使用？描述使用它实现登录功能的流程？

> 请求后台资源的模块。

```
$ npm install axios -S装好

```

> 然后发送的是跨域，需在配置文件中config/index.js进行设置。后台如果是Tp5则定义一个资源路由。 js中使用import进来，然后.get或.post。返回在.then函数中如果成功，失败则是在.catch函数中

#### vue修饰符

- stop：阻止事件的冒泡
- prevent：阻止事件的默认行为
- once：只触发一次
- self：只触发自己的事件行为时，才会执行

#### vue项目中的性能优化

> 1.不要在模板里面写过多表达式

> 2.循环调用子组件时添加key

> 3.频繁切换的使用v-show，不频繁切换的使用v-if

> 4.尽量少用float，可以用flex

> 5.按需加载，可以用require或者import()按需加载需要的组件

> 6.路由懒加载

#### vue.extend和vue.component

> - extend 是构造一个组件的语法器。 然后这个组件你可以作用到Vue.component这个全局注册方法里 还可以在任意vue模板里使用组件。 也可以作用到vue实例或者某个组件中的components属性中并在内部使用apple组件。
> - Vue.component 你可以创建 ，也可以取组件。

#### Vue组件间的参数传递

**父组件与子组件传值**

> 父组件传给子组件：子组件通过`props`方法接受数据；

- 子组件传给父组件： `$emit` 方法传递参数

**非父子组件间的数据传递，兄弟组件传值**

> `eventBus`，就是创建一个事件中心，相当于中转站，可以用它来传递事件和接收事件。项目比较小时，用这个比较合适（虽然也有不少人推荐直接用`VUEX`，具体来说看需求）

#### Vue的路由实现：hash模式 和 history模式

- `hash`模式：在浏览器中符号`“#”`，#以及#后面的字符称之为`hash`，用 `window.location.hash` 读取。特点：`hash`虽然在`URL`中，但不被包括在`HTTP`请求中；用来指导浏览器动作，对服务端安全无用，`hash`不会重加载页面。
- `history`模式：h`istory`采用`HTML5`的新特性；且提供了两个新方法： `pushState()`， `replaceState()`可以对浏览器历史记录栈进行修改，以及`popState`事件的监听到状态变更

#### v-if 和 v-show 区别

- `v-if`按照条件是否渲染，`v-show`是`display`的`block`或`none`

#### `keep-alive`的作用是什么

- 包裹动态组件时，会缓存不活动的组件实例,主要用于保留组件状态或避免重新渲染

> 比如有一个列表和一个详情，那么用户就会经常执行打开详情=>返回列表=>打开详情…这样的话列表和详情都是一个频率很高的页面，那么就可以对列表组件使用``进行缓存，这样用户每次返回列表的时候，都能从缓存中快速渲染，而不是重新渲染

#### 指令v-el的作用是什么

> 提供一个在页面上已存在的 `DOM`元素作为 `Vue`实例的挂载目标.可以是 CSS 选择器，也可以是一个 `HTMLElement` 实例,

#### 在Vue中使用插件的步骤

- 采用`ES6`的`import ... from ...`语法或`CommonJS`的`require()`方法引入插件
- 使用全局方法`Vue.use( plugin )`使用插件,可以传入一个选项对象`Vue.use(MyPlugin, { someOption: true })`

#### vue的优点是什么？

- 低耦合。视图（`View`）可以独立于`Model`变化和修改，一个`ViewModel`可以绑定到不同的`"View"`上，当View变化的时候Model可以不变，当`Model`变化的时候`View`也可以不变
- 可重用性。你可以把一些视图逻辑放在一个`ViewModel`里面，让很多`view`重用这段视图逻辑
- 可测试。界面素来是比较难于测试的，而现在测试可以针对`ViewModel`来写

####  路由之间跳转？

**声明式（标签跳转）**

```text
<router-link :to="index">
```

**编程式（ js跳转）**

```text
router.push('index')
```

- 

#### Vue 组件 data 为什么必须是函数

- 每个组件都是 `Vue` 的实例。
- 组件共享 `data` 属性，当 `data` 的值是同一个引用类型的值时，改变其中一个会影响其他

- 

#### 怎么快速定位哪个组件出现性能问题

> 用 `timeline` 工具。 大意是通过 `timeline` 来查看每个函数的调用时常，定位出哪个函数的问题，从而能判断哪个组件出了问题

#### extend 能做什么

> 这个 `API` 很少用到，作用是扩展组件生成一个构造器，通常会与 `$mount` 一起使用。

```js
// 创建组件构造器
let Component = Vue.extend({
  template: '<div>test</div>'
})
// 挂载到 #app 上
new Component().$mount('#app')
// 除了上面的方式，还可以用来扩展已有的组件
let SuperComponent = Vue.extend(Component)
new SuperComponent({
    created() {
        console.log(1)
    }
})
new SuperComponent().$mount('#app')
```

#### mixin 和 mixins 区别

> `mixin` 用于全局混入，会影响到每个组件实例，通常插件都是这样做初始化的

```js
Vue.mixin({
    beforeCreate() {
        // ...逻辑
        // 这种方式会影响到每个组件的 beforeCreate 钩子函数
    }
})
```

- 虽然文档不建议我们在应用中直接使用 `mixin`，但是如果不滥用的话也是很有帮助的，比如可以全局混入封装好的 `ajax` 或者一些工具函数等等。
- `mixins` 应该是我们最常使用的扩展组件的方式了。如果多个组件中有相同的业务逻辑，就可以将这些逻辑剥离出来，通过 `mixins` 混入代码，比如上拉下拉加载数据这种逻辑等等。
- 另外需要注意的是 `mixins` 混入的钩子函数会先于组件内的钩子函数执行，并且在遇到同名选项的时候也会有选择性的进行合并，具体可以阅读 文档。

#### computed 和 watch 区别

- `computed` 是计算属性，依赖其他属性计算值，并且 `computed` 的值有缓存，只有当计算值变化才会返回内容。
- `watch` 监听到值的变化就会执行回调，在回调中可以进行一些逻辑操作。
- 所以一般来说需要依赖别的属性来动态获得值的时候可以使用 `computed`，对于监听到值的变化需要做一些复杂业务逻辑的情况可以使用 `watch`。
- 另外 `computer` 和 `watch` 还都支持对象的写法，这种方式知道的人并不多。

```js
vm.$watch('obj', {
    // 深度遍历
    deep: true,
    // 立即触发
    immediate: true,
    // 执行的函数
    handler: function(val, oldVal) {}
})
var vm = new Vue({
  data: { a: 1 },
  computed: {
    aPlus: {
      // this.aPlus 时触发
      get: function () {
        return this.a + 1
      },
      // this.aPlus = 1 时触发
      set: function (v) {
        this.a = v - 1
      }
    }
  }
})


```

#### Proxy 相比于 defineProperty 的优势

- 数组变化也能监听到
- 不需要深度遍历监听

```js
let data = { a: 1 }
let reactiveData = new Proxy(data, {
	get: function(target, name){
		// ...
	},
	// ...
})
```

------



### react

#### 给我介绍一下react

1. 以前我们没有jquery的时候，我们大概的流程是从后端通过ajax获取到数据然后使用jquery生成dom结果然后更新到页面当中，但是随着业务发展，我们的项目可能会越来越复杂，我们每次请求到数据，或则数据有更改的时候，我们又需要重新组装一次dom结构，然后更新页面，这样我们手动同步dom和数据的成本就越来越高，而且频繁的操作dom，也使我我们页面的性能慢慢的降低。
2. 这个时候mvvm出现了，mvvm的双向数据绑定可以让我们在数据修改的同时同步dom的更新，dom的更新也可以直接同步我们数据的更改，这个特定可以大大降低我们手动去维护dom更新的成本，mvvm为react的特性之一，虽然react属于单项数据流，需要我们手动实现双向数据绑定。
3. 有了mvvm还不够，因为如果每次有数据做了更改，然后我们都全量更新dom结构的话，也没办法解决我们频繁操作dom结构(降低了页面性能)的问题，为了解决这个问题，react内部实现了一套虚拟dom结构，也就是用js实现的一套dom结构，他的作用是讲真实dom在js中做一套缓存，每次有数据更改的时候，react内部先使用算法，也就是鼎鼎有名的diff算法对dom结构进行对比，找到那些我们需要新增、更新、删除的dom节点，然后一次性对真实DOM进行更新，这样就大大降低了操作dom的次数。 那么diff算法是怎么运作的呢，首先，diff针对类型不同的节点，会直接判定原来节点需要卸载并且用新的节点来装载卸载的节点的位置；针对于节点类型相同的节点，会对比这个节点的所有属性，如果节点的所有属性相同，那么判定这个节点不需要更新，如果节点属性不相同，那么会判定这个节点需要更新，react会更新并重渲染这个节点。
4. react设计之初是主要负责UI层的渲染，虽然每个组件有自己的state，state表示组件的状态，当状态需要变化的时候，需要使用setState更新我们的组件，但是，我们想通过一个组件重渲染它的兄弟组件，我们就需要将组件的状态提升到父组件当中，让父组件的状态来控制这两个组件的重渲染，当我们组件的层次越来越深的时候，状态需要一直往下传，无疑加大了我们代码的复杂度，我们需要一个状态管理中心，来帮我们管理我们状态state。
5. 这个时候，redux出现了，我们可以将所有的state交给redux去管理，当我们的某一个state有变化的时候，依赖到这个state的组件就会进行一次重渲染，这样就解决了我们的我们需要一直把state往下传的问题。redux有action、reducer的概念，action为唯一修改state的来源，reducer为唯一确定state如何变化的入口，这使得redux的数据流非常规范，同时也暴露出了redux代码的复杂，本来那么简单的功能，却需要完成那么多的代码。
6. 后来，社区就出现了另外一套解决方案，也就是mobx，它推崇代码简约易懂，只需要定义一个可观测的对象，然后哪个组价使用到这个可观测的对象，并且这个对象的数据有更改，那么这个组件就会重渲染，而且mobx内部也做好了是否重渲染组件的生命周期shouldUpdateComponent，不建议开发者进行更改，这使得我们使用mobx开发项目的时候可以简单快速的完成很多功能，连redux的作者也推荐使用mobx进行项目开发。但是，随着项目的不断变大，mobx也不断暴露出了它的缺点，就是数据流太随意，出了bug之后不好追溯数据的流向，这个缺点正好体现出了redux的优点所在，所以针对于小项目来说，社区推荐使用mobx，对大项目推荐使用redux



#### react和vue的区别

```
   =>  相同点：
	1.数据驱动页面，提供响应式的试图组件
	2.都有virtual DOM,组件化的开发，通过props参数进行父子之间组件传递数据，都实现了webComponents规范
	3.数据流动单向，都支持服务器的渲染SSR
	4.都有支持native的方法，react有React native， vue有wexx
=>  不同点：
	1.数据绑定：Vue实现了双向的数据绑定，react数据流动是单向的
	2.数据渲染：大规模的数据渲染，react更快
	3.使用场景：React配合Redux架构适合大规模多人协作复杂项目，Vue适合小快的项目
	4.开发风格：react推荐做法jsx + inline style把html和css都写在js了
		    vue是采用webpack + vue-loader单文件组件格式，html, js, css同一个文件

```

#### 纯函数

```
Redux数据流里，reduces其实是根据之前的状态（previous state）和现有的action（current action）
更新state(这个state可以理解为上下累加器的结果）
每次redux reducer被执行时，state和action被传入，这个state根据action进行累加或者是'自身消减'(reduce),
进而返回最新的state,这也就是典型reduce函数的用法：state ->  action ->  state

```

#### react的refs

```
refs就想一个逃生窗，允许我们之间访问dom元素或者组件实例，可以向组件添加一个ref属性的值是一个回调函数，
它将接受地城dom元素或组件的已挂在实例，作为第一个参数

```

#### react中的keys

```
帮组我们跟踪哪些项目已更改、添加、从列表中删除，key是独一无二的，可以让我们高效的去定位元素，并且操作它

```

#### React的生命周期

```
三个状态：Mounting(已插入真实的DOM）
	  Updating(正在被重新渲染)
	  Unmounting(已移除真实的DOM)
componentDIdMount 在第一次渲染后调用，只在客服端。之后组件已经生成对应的DOM结构，
componentDidUpdate 在组件完成更新后立即调用，在出初始化是不会调用

```

#### React子组件向父组件传值

```
父组件通过props 给子组件传递数据，子组件则是通过调用父组件传给它的函数给父组件传递数据。

```

#### 为什么虚拟DOM会提高性能 

```
虚拟DOM相当于在js和真实dom中间加了一个缓存，利用dom diff算法避免了没有必要的doom操作，从而提高性能
具体实现步骤：
	·用JavaScript对象结构表示DOM树的结构；然后用这个树构建一个真正的DOM树，插到文档中
        ·当状态变更的时候，重新构造一棵树的对象树，然后用新的树和旧的树进行对比，记录两棵树差异
	·把2所记录的差异应用到步骤1所构建的真正的DOM树上，试图就更新了。

```

#### reac性能优化是哪个周期函

```
shouldComponentUpdate 这个方法用来判断是否需要调用render方法重新描绘dom.因为dom的描绘非常消耗性能，
如果我们在shouldComponentUpdate方法中能够写出更优化的dom diff算法，可以极大的提高性能

```

#### 怎么划分业务组件和技术组件

```
根据组件的职责通常把组件分为UI组件和容器组件
UI组件负责UI的呈现，容器组件负责管理数据和逻辑
两者通过React-redux提供connect方法联系起来

```

#### setState

```
setState通过一个队列机制实现state更新，当执行setState时，会将需要更新的state很后放入状态队列
而不会立即更新this.state，队列机制可以高效地批量更新state。如果不通过setState而直接修改this.state的值	
那么该state将不会被放入状态队列中。当下次调用setState并对状态队列进行合并时，就会忽略之前修改的state，造成不可预知的错误

同时，也利用了队列机制实现了setState的异步更新，避免了频繁的重复更新state

同步更新state:
	setState 函数并不会阻塞等待状态更新完毕，因此 setNetworkActivityIndicatorVisible 有可能先于数据渲染完毕就执行。
	第二个参数是一个回调函数，在setState的异步操作结束并且组件已经重新渲染的时候执行
	也就是说，我们可以通过这个回调来拿到更新的state的值，实现代码的同步

例子：componentDidMount() {

	fetch('https://test.com')
    
	.then((res) => res.json())
    
	.then(
    (data) => {
this.setState({ data:data });
			StatusBar.setNetworkActivityIndicatorVisible(false);
        }
```

#### shouldComponentUpdate 

shouldComponentUpdate 这个方法用来判断是否需要调用 render 方法重新描绘 dom。因为 dom 的描绘非常消耗性能，如果我们能在 shouldComponentUpdate 方法中能够写出更优化的 dom diff 算法，可以极大的提高性能。

#### 展示组件(Presentational component)和容器组件(Container component)之间有何不同

- 展示组件关心组件看起来是什么。展示专门通过 props 接受数据和回调，并且几乎不会有自身的状态，但当展示组件拥有自身的状态时，通常也只关心 UI 状态而不是数据的状态。
- 容器组件则更关心组件是如何运作的。容器组件会为展示组件或者其它容器组件提供数据和行为(behavior)，它们会调用 Flux actions，并将其作为回调提供给展示组件。容器组件经常是有状态的，因为它们是(其它组件的)数据源。

#### 类组件(Class component)和函数式组件(Functional component)之间有何不同

- 类组件不仅允许你使用更多额外的功能，如组件自身的状态和生命周期钩子，也能使组件直接访问 store 并维持状态
- 当组件仅是接收 props，并将组件自身渲染到页面时，该组件就是一个 '无状态组件(stateless component)'，可以使用一个纯函数来创建这样的组件。这种组件也被称为哑组件(dumb components)或展示组件

#### 状态(state)和属性(props)之间有何不同

- State 是一种数据结构，用于组件挂载时所需数据的默认值。State 可能会随着时间的推移而发生突变，但多数时候是作为用户事件行为的结果。
- Props(properties 的简写)则是组件的配置。props 由父组件传递给子组件，并且就子组件而言，props 是不可变的(immutable)。组件不能改变自身的 props，但是可以把其子组件的 props 放在一起(统一管理)。Props 也不仅仅是数据--回调函数也可以通过 props 传递。

#### 何为受控组件(controlled component)

在 HTML 中，类似 ``, `` 和 `` 这样的表单元素会维护自身的状态，并基于用户的输入来更新。当用户提交表单时，前面提到的元素的值将随表单一起被发送。但在 React 中会有些不同，包含表单元素的组件将会在 state 中追踪输入的值，并且每次调用回调函数时，如 onChange 会更新 state，重新渲染组件。一个输入表单元素，它的值通过 React 的这种方式来控制，这样的元素就被称为"受控元素"。

#### 何为高阶组件(higher order component)

高阶组件是一个以组件为参数并返回一个新组件的函数。HOC 运行你重用代码、逻辑和引导抽象。最常见的可能是 Redux 的 connect 函数。除了简单分享工具库和简单的组合，HOC 最好的方式是共享 React 组件之间的行为。如果你发现你在不同的地方写了大量代码来做同一件事时，就应该考虑将代码重构为可重用的 HOC。

#### 为什么建议传递给 setState 的参数是一个 callback 而不是一个对象

因为 this.props 和 this.state 的更新可能是异步的，不能依赖它们的值去计算下一个 state。

#### 除了在构造函数中绑定 this，还有其它方式吗

你可以使用属性初始值设定项(property initializers)来正确绑定回调，create-react-app 也是默认支持的。在回调中你可以使用箭头函数，但问题是每次组件渲染时都会创建一个新的回调。

#### (在构造函数中)调用 super(props) 的目的是什么

在 super() 被调用之前，子类是不能使用 this 的，在 ES2015 中，子类必须在 constructor 中调用 super()。传递 props 给 super() 的原因则是便于(在子类中)能在 constructor 访问 this.props。

#### 应该在 React 组件的何处发起 Ajax 请求

在 React 组件中，应该在 componentDidMount 中发起网络请求。这个方法会在组件第一次“挂载”(被添加到 DOM)时执行，在组件的生命周期中仅会执行一次。更重要的是，你不能保证在组件挂载之前 Ajax 请求已经完成，如果是这样，也就意味着你将尝试在一个未挂载的组件上调用 setState，这将不起作用。在 componentDidMount 中发起网络请求将保证这有一个组件可以更新了。

#### 事件在 React 中的处理方式

为了解决跨浏览器兼容性问题，您的 React 中的事件处理程序将传递 SyntheticEvent 的实例，它是 React 的浏览器本机事件的跨浏览器包装器。

这些 SyntheticEvent 与您习惯的原生事件具有相同的接口，除了它们在所有浏览器中都兼容。有趣的是，React 实际上并没有将事件附加到子节点本身。React 将使用单个事件监听器监听顶层的所有事件。这对于性能是有好处的，这也意味着在更新 DOM 时，React 不需要担心跟踪事件监听器。

#### createElement 和 cloneElement 有什么区别

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

#### React 中有三种构建组件的方式

React.createClass()、ES6 class 和无状态函数。

#### react 组件的划分业务组件技术组件

- 根据组件的职责通常把组件分为 UI 组件和容器组件。
- UI 组件负责 UI 的呈现，容器组件负责管理数据和逻辑。
- 两者通过 React-Redux 提供 connect 方法联系起来。

------



### 小程序

#### 微信小程序有几个文件

- `WXSS (WeiXin Style Sheets)`是一套样式语言，用于描述 `WXML` 的组件样式， `js` 逻辑处理，网络请求`json`小程序设置，如页面注册，页面标题及 `tabBar`。
- `app.json` 必须要有这个文件，如果没有这个文件，项目无法运行，因为微信框架把这个作为配置文件入口，整个小程序的全局配置。包括页面注册，网络设置，以及小程序的`window` 背景色，配置导航条样式，配置默认标题。
- `app.js` 必须要有这个文件，没有也是会报错！但是这个文件创建一下就行 什么都不需要写以后我们可以在这个文件中监听并处理小程序的生命周期函数、声明全局变量。
- `app.wxss` 配置全局 `css`

#### 微信小程序怎样跟事件传值

> 给 `HTML` 元素添加 `data-*`属性来传递我们需要的值，然后通过 `e.currentTarget.dataset` 或`onload`的`param`参数获取。但 `data -` 名称不能有大写字母和不可以存放对象

#### 小程序的 wxss 和 css 有哪些不一样的

- `wxss`的图片引入需使用外链地址
- 没有 `Body`；样式可直接使用 `import` 导入

#### 关联微信公众号如何确定用户的唯一性

> 使用 `wx.getUserInfo` 方法 `withCredentials` 为 `true` 时 可获取 `encryptedData`，里面有 `union_id`。后端需要进行对称解密

#### 微信小程序与vue区别

- 生命周期不一样，微信小程序生命周期比较简单
- 数据绑定也不同，微信小程序数据绑定需要使用`{{}}`，`vue` 直接`:`就可以
- 显示与隐藏元素，`vue`中，使用 `v-if` 和 `v-show` 控制元素的显示和隐藏，小程序中，使用`wx-if` 和`hidden` 控制元素的显示和隐藏
- 事件处理不同，小程序中，全用 `bindtap(bind+event)`，或者 `catchtap(catch+event)` 绑定事件,`vue：`使用 `v-on:event` 绑定事件，或者使用`@event` 绑定事件
- 数据双向绑定也不也不一样在 `vue`中,只需要再表单元素上加上 `v-model`,然后再绑定 `data`中对应的一个值，当表单元素内容发生变化时，`data`中对应的值也会相应改变，这是 `vue`非常 nice 的一点。微信小程序必须获取到表单元素，改变的值，然后再把值赋给一个 `data`中声明的变量。

#### 登录

**unionid和openid**

> 了解小程序登陆之前，我们写了解下小程序/公众号登录涉及到两个最关键的用户标识：

- `OpenId` 是一个用户对于一个小程序／公众号的标识，开发者可以通过这个标识识别出用户。
- `UnionId` 是一个用户对于同主体微信小程序／公众号／`APP`的标识，开发者需要在微信开放平台下绑定相同账号的主体。开发者可通过`UnionId`，实现多个小程序、公众号、甚至APP 之间的数据互通了。

**关键Api**

- `wx.login` 官方提供的登录能力
- `wx.checkSession`校验用户当前的`session_key`是否有效
- `wx.authorize` 提前向用户发起授权请求
- `wx.getUserInfo` 获取用户基本信息

**登录流程设计**

- **利用现有登录体系**

> 直接复用现有系统的登录体系，只需要在小程序端设计用户名，密码/验证码输入页面，便可以简便的实现登录，只需要保持良好的用户体验即可

- **利用`OpenId` 创建用户体系**

> `OpenId` 是一个小程序对于一个用户的标识，利用这一点我们可以轻松的实现一套基于小程序的用户体系，值得一提的是这种用户体系对用户的打扰最低，可以实现静默登录。具体步骤如下

- 小程序客户端通过 `wx.login` 获取 `code`
- 传递 `code` 向服务端，服务端拿到 code 调用微信登录凭证校验接口，微信服务器返回 `openid` 和会话密钥 `session_key` ，此时开发者服务端便可以利用 `openid` 生成用户入库，再向小程序客户端返回自定义登录态
- 小程序客户端缓存 （通过`storage`）自定义登录态（`token`），后续调用接口时携带该登录态作为用户身份标识即可

**利用 Unionid 创建用户体系**

> 如果想实现多个小程序，公众号，已有登录系统的数据互通，可以通过获取到用户 `unionid` 的方式建立用户体系。因为 `unionid` 在同一开放平台下的所所有应用都是相同的，通过 `unionid` 建立的用户体系即可实现全平台数据的互通，更方便的接入原有的功能，那如何获取 `unionid` 呢，有以下两种方式

- 如果户关注了某个相同主体公众号，或曾经在某个相同主体`App`、公众号上进行过微信登录授权，通过 `wx.login` 可以直接获取 到 `unionid`

- 结合

   

  ```
  wx.getUserInfo
  ```

   

  和

   

  ```
  <button open-type="getUserInfo"><button/>
  ```

   

  这两种方式引导用户主动授权，主动授权后通过返回的信息和服务端交互 (这里有一步需要服务端解密数据的过程，很简单，微信提供了示例代码) 即可拿到

   

  ```
  unionid
  ```

   

  建立用户体系， 然后由服务端返回登录态，本地记录即可实现登录，附上微信提供的最佳实践

  - 调用 `wx.login` 获取 `code`，然后从微信后端换取到 `session_key`，用于解密 `getUserInfo`返回的敏感数据

  - 使用

     

    ```
    wx.getSetting
    ```

     

    获取用户的授权情况

    - 如果用户已经授权，直接调用 `API` `wx.getUserInfo` 获取用户最新的信息；
    - 用户未授权，在界面中显示一个按钮提示用户登入，当用户点击并授权后就获取到用户的最新信息

  - 获取到用户数据后可以进行展示或者发送给自己的后端。

注意事项

- 需要获取 `unionid` 形式的登录体系，在以前（18年4月之前）是通过以下这种方式来实现，但后续微信做了调整（因为一进入小程序，主动弹起各种授权弹窗的这种形式，比较容易导致用户流失），调整为必须使用按钮引导用户主动授权的方式，这次调整对开发者影响较大，开发者需要注意遵守微信的规则，并及时和业务方沟通业务形式，不要存在侥幸心理，以防造成小程序不过审等情况

```text
 wx.login(获取code) ===> wx.getUserInfo(用户授权) ===> 获取 unionid
```

- 因为小程序不存在 `cookie` 的概念， 登录态必须缓存在本地，因此强烈建议为登录态设置过期时间
- 值得一提的是如果需要支持风控安全校验，多平台登录等功能，可能需要加入一些公共参数，例如`platform`，`channel`，`deviceParam`等参数。在和服务端确定方案时，作为前端同学应该及时提出这些合理的建议，设计合理的系统。
- `openid` ， `unionid` 不要在接口中明文传输，这是一种危险的行为，同时也很不专业

#### 图片导出

> 这是一种常见的引流方式，一般同时会在图片中附加一个小程序二维码。

**基本**

- 借助 `canvas` 元素，将需要导出的样式首先在 `canvas` 画布上绘制出来 （`api`基本和`h5`保持一致，但有轻微差异，使用时注意即可
- 借助微信提供的 `canvasToTempFilePath` 导出图片，最后再使用 `saveImageToPhotosAlbum` （需要授权）保存图片到本地

**如何优雅实现**

- 绘制出需要的样式这一步是省略不掉的。但是我们可以封装一个绘制库，包含常见图形的绘制，例如矩形，圆角矩形，圆， 扇形， 三角形， 文字，图片减少绘制代码，只需要提炼出样式信息，便可以轻松的绘制，最后导出图片存入相册。笔者觉得以下这种方式绘制更为优雅清晰一些，其实也可以使用加入一个type参数来指定绘制类型，传入的一个是样式数组，实现绘制。
- 结合上一步的实现，如果对于同一类型的卡片有多次导出需求的场景，也可以使用自定义组件的方式，封装同一类型的卡片为一个通用组件，在需要导出图片功能的地方，引入该组件即可。

```js
class CanvasKit {
   constructor() {
   }
   drawImg(option = {}) {
     ...
     return this
   }
   drawRect(option = {}) {
     return this
   }
   drawText(option = {}) {
     ...
     return this
   }
   static exportImg(option = {}) {
     ...
   }
 }

 let drawer = new CanvasKit('canvasId').drawImg(styleObj1).drawText(styleObj2)
 drawer.exportImg()
```

注意事项

- 小程序中无法绘制网络图片到`canvas`上，需要通过`downLoadFile` 先下载图片到本地临时文件才可以绘制
- 通常需要绘制二维码到导出的图片上，有一种方式导出二维码时，需要携带的参数必须做编码，而且有具体的长度（`32`可见字符）限制，可以借助服务端生成 短链接 的方式来解决

#### 数据统计

> 数据统计作为目前一种常用的分析用户行为的方式，小程序端也是必不可少的。小程序采取的曝光，点击数据埋点其实和h5是一样的。但是埋点作为一个和业务逻辑不相关的需求，我们如果在每一个点击事件，每一个生命周期加入各种埋点代码，则会干扰正常的业务逻辑，和使代码变的臃肿，笔者提供以下几种思路来解决数据埋点

**设计一个埋点sdk**

> 小程序的代码结构是，每一个 `Page` 中都有一个 `Page` 方法，接受一个包含生命周期函数，数据的 业务逻辑对象 包装这层数据，借助小程序的底层逻辑实现页面的业务逻辑。通过这个我们可以想到思路，对`Page`进行一次包装，篡改它的生命周期和点击事件，混入埋点代码，不干扰业务逻辑，只要做一些简单的配置即可埋点，简单的代码实现如下

```js
// 代码仅供理解思路
 page = function(params) {
   let keys = params.keys()
   keys.forEach(v => {
       if (v === 'onLoad') {
         params[v] = function(options) {
           stat()   //曝光埋点代码
           params[v].call(this, options)
         }
       }
       else if (v.includes('click')) {
         params[v] = funciton(event) {
           let data = event.dataset.config
           stat(data)  // 点击埋点
           param[v].call(this)
         }
       }
   })
 }
```

> 这种思路不光适用于埋点，也可以用来作全局异常处理，请求的统一处理等场景。

**分析接口**

> 对于特殊的一些业务，我们可以采取 接口埋点，什么叫接口埋点呢？很多情况下，我们有的`api`并不是多处调用的，只会在某一个特定的页面调用，通过这个思路我们可以分析出，该接口被请求，则这个行为被触发了，则完全可以通过服务端日志得出埋点数据，但是这种方式局限性较大，而且属于分析结果得出过程，可能存在误差，但可以作为一种思路了解一下。

**微信自定义数据分析**

> 微信本身提供的数据分析能力，微信本身提供了常规分析和自定义分析两种数据分析方式，在小程序后台配置即可。借助小程序数据助手这款小程序可以很方便的查看

#### 工程化

**工程化做什么**

> 目前的前端开发过程，工程化是必不可少的一环，那小程序工程化都需要做些什么呢，先看下目前小程序开发当中存在哪些问题需要解决：

- 不支持 `css`预编译器,作为一种主流的 `css`解决方案，不论是 `less`,`sass`,`stylus` 都可以提升`css`效率
- 不支持引入npm包 （这一条，从微信公开课中听闻，微信准备支持）
- 不支持`ES7`等后续的`js`特性，好用的`async await`等特性都无法使用
- 不支持引入外部字体文件，只支持`base64`
- 没有 `eslint` 等代码检查工具

**方案选型**

> 对于目前常用的工程化方案，`webpack`，`rollup`，`parcel`等来看，都常用与单页应用的打包和处理，而小程序天生是 “多页应用” 并且存在一些特定的配置。根据要解决的问题来看，无非是文件的编译，修改，拷贝这些处理，对于这些需求，我们想到基于流的 `gulp`非常的适合处理，并且相对于`webpack`配置多页应用更加简单。所以小程序工程化方案推荐使用 `gulp`

**具体开发思路**

> 通过 `gulp` 的 `task` 实现：

- 实时编译 `less` 文件至相应目录
- 引入支持`async`，`await`的运行时文件
- 编译字体文件为`base64` 并生成相应`css`文件，方便使用
- 依赖分析哪些地方引用了`npm`包，将`npm`包打成一个文件，拷贝至相应目录
- 检查代码规范

#### 小程序架构

![img](https://user-images.githubusercontent.com/2350193/44563914-ff97c380-a792-11e8-8e77-6d0970891e24.png)

> 微信小程序的框架包含两部分 `View` 视图层、`App Service`逻辑层。`View` 层用来渲染页面结构，`AppService` 层用来逻辑处理、数据请求、接口调用。

它们在两个线程里运行。

> 视图层和逻辑层通过系统层的 `JSBridage` 进行通信，逻辑层把数据变化通知到视图层，触发视图层页面更新，视图层把触发的事件通知到逻辑层进行业务处理

![img](https://user-images.githubusercontent.com/2350193/44186238-db146980-a14a-11e8-8096-bcb8fa6d28b2.png)

- 视图层使用 `WebView` 渲染，`iOS`中使用自带 `WKWebView`，在 `Android` 使用腾讯的 `x5`内核（基于 `Blink`）运行。
- 逻辑层使用在 `iOS` 中使用自带的 `JSCore` 运行，在 `Android`中使用腾讯的 `x5` 内核（基于 `Blink`）运行。
- 开发工具使用 `nw.js` 同时提供了视图层和逻辑层的运行环境。

#### WXML && WXSS

**WXML**

- 支持数据绑定
- 支持逻辑算术、运算
- 支持模板、引用
- 支持添加事件（`bindtap`）
- `Wxml`编译器：`Wcc` 把 `Wxml`文件 转为 `JS`
- 执行方式：`Wcc index.wxml`
- 使用 `Virtual DOM`，进行局部更新

**WXSS**

- wxss编译器：`wcsc` 把`wxss`文件转化为 `js`
- 执行方式： `wcsc index.wxss`

**尺寸单位 rpx**

> `rpx（responsive pixel`）: 可以根据屏幕宽度进行自适应。规定屏幕宽为 `750rpx`。公式：

```js
const dsWidth = 750

export const screenHeightOfRpx = function () {
  return 750 / env.screenWidth * env.screenHeight
}

export const rpxToPx = function (rpx) {
  return env.screenWidth / 750 * rpx
}

export const pxToRpx = function (px) {
  return 750 / env.screenWidth * px
}
```

**样式导入**

> 使用 `@import`语句可以导入外联样式表，`@import`后跟需要导入的外联样式表的相对路径，用 `;` 表示语句结束

**内联样式**

> 静态的样式统一写到 `class` 中。`style` 接收动态的样式，在运行时会进行解析，请尽量避免将静态的样式写进 `style` 中，以免影响渲染速度

**全局样式与局部样式**

> 定义在 `app.wxss` 中的样式为全局样式，作用于每一个页面。在`page` 的 `wxss` 文件中定义的样式为局部样式，只作用在对应的页面，并会覆盖 `app.wxss` 中相同的选择器

#### 小程序有什么问题

- 小程序仍然使用 `WebView` 渲染，并非原生渲染。（部分原生）
- 服务端接口返回的头无法执行，比如：`Set-Cookie`。
- 依赖浏览器环境的 `JS`库不能使用。
- 不能使用 `npm`，但是可以自搭构建工具或者使用 `mpvue`。（未来官方有计划支持）
- 不能使用 `ES7`，可以自己用`babel+webpack`自搭或者使用 `mpvue`。
- 不支持使用自己的字体（未来官方计划支持）。
- 可以用 `base64` 的方式来使用 `iconfont`。
- 小程序不能发朋友圈（可以通过保存图片到本地，发图片到朋友前。二维码可以使用B接口）。
- 获取二维码/小程序接口的限制
- 程序推送只能使用“服务通知” 而且需要用户主动触发提交 `formId`，`formId` 只有7天有效期。（现在的做法是在每个页面都放入`form`并且隐藏以此获取更多的 `formId`。后端使用原则为：优先使用有效期最短的）
- 小程序大小限制 2M，分包总计不超过 8M
- 转发（分享）小程序不能拿到成功结果，原来可以。链接（小游戏造的孽）

#### 授权获取用户信息流程

- `session_key` 有有效期，有效期并没有被告知开发者，只知道用户越频繁使用小程序，`session_key` 有效期越长
- 在调用 `wx.login` 时会直接更新 `session_key`，导致旧 `session_key` 失效
- 小程序内先调用 `wx.checkSession` 检查登录态，并保证没有过期的 `session_key` 不会被更新，再调用 `wx.login` 获取 `code`。接着用户授权小程序获取用户信息，小程序拿到加密后的用户数据，把加密数据和 `code` 传给后端服务。后端通过 `code` 拿到 `session_key` 并解密数据，将解密后的用户信息返回给小程序

#### 先授权获取用户信息再 login 会发生什么

- 用户授权时，开放平台使用旧的 `session_key` 对用户信息进行加密。调用 `wx.login` 重新登录，会刷新 `session_key`，这时后端服务从开放平台获取到新 `session_key`，但是无法对老 `session_key` 加密过的数据解密，用户信息获取失败
- 在用户信息授权之前先调用 `wx.checkSession` 呢？`wx.checkSession` 检查登录态，并且保证 wx.login 不会刷新 `session_key`，从而让后端服务正确解密数据。但是这里存在一个问题，如果小程序较长时间不用导致 `session_key` 过期，则 `wx.login` 必定会重新生成 `session_key`，从而再一次导致用户信息解密失败

------



### webpack

#### webpack打包文件体积过大

```
1.异步加载模块
2.提取第三库
3.代码压缩
4.去除不必要的插件

```

#### 如何优化webpack构建的性能

```
一、减少代码体积 1.使用CommonsChunksPlugin 提取多个chunk之间的通用模块，减少总体代码体积
		 2.把部分依赖转移到CDN上，避免每次编译过程都由Webpack处理
		 3.对一些组件库采用按需加载，避免无用的代码
二、减少目录检索范围
		 ·在使用loader的时候，通过制定exclude和include选项，减少loader遍历的目录范围，从而加快webpack编译速度
	
三、减少检索路经：resolve.alias可以配置webpack模块解析的别名，对于比较深的解析路经，可以对其配置alias

```

#### 移动端的性能优化

```
  1、首屏加载和按需加载，懒加载
  2、资源预加载
  3、图片压缩处理，使用base64内嵌图片
  4、合理缓存dom对象
  5、使用touchstart代替click（click 300毫秒的延迟）
  6、利用transform:translateZ(0)，开启硬件GUP加速
  7、不滥用web字体，不滥用float（布局计算消耗性能），减少font-size声明
  8、使用viewport固定屏幕渲染，加速页面渲染内容
  9、尽量使用事件代理，避免直接事件绑定

```

#### 优化打包速度

- 减少文件搜索范围
  - 比如通过别名
  - `loader` 的 `test`，`include & exclude`
- `Webpack4` 默认压缩并行
- `Happypack` 并发调用
- `babel` 也可以缓存编译

#### Babel 

- 本质就是编译器，当代码转为字符串生成 `AST`，对 `AST` 进行转变最后再生成新的代码
- 分为三步：词法分析生成 `Token`，语法分析生成 `AST`，遍历 `AST`，根据插件变换相应的节点，最后把 `AST`转换为代码

#### 如何实现一个插件

- 调用插件 `apply` 函数传入 `compiler` 对象
- 通过 `compiler` 对象监听事件

比如你想实现一个编译结束退出命令的插件

```js
apply (compiler) {
  const afterEmit = (compilation, cb) => {
    cb()
    setTimeout(function () {
      process.exit(0)
    }, 1000)
  }

  compiler.plugin('after-emit', afterEmit)
}
}

module.exports = BuildEndPlugin
```

#### 打包效率

- 开发环境采用增量构建，启用热更新
- 开发环境不做无意义的工作如提取`css`计算文件hash等
- 配置`devtool`
- 选择合适的`loader`
- 个别`loader`开启`cache` 如`babel-loader`
- 第三方库采用引入方式
- 提取公共代码
- 优化构建时的搜索路径 指明需要构建目录及不需要构建目录
- 模块化引入需要的部分

#### Loader

编写一个loader

> ```
> loader`就是一个`node`模块，它输出了一个函数。当某种资源需要用这个`loader`转换时，这个函数会被调用。并且，这个函数可以通过提供给它的`this`上下文访问`Loader API`。 `reverse-txt-loader
> ```

#### plugin优化

**构建优化**

- 减少编译体积 `ContextReplacementPugin`、`IgnorePlugin`、`babel-plugin-import`、`babel-plugin-transform-runtime`
- 并行编译 `happypack`、`thread-loader`、`uglifyjsWebpackPlugin`开启并行
- 缓存 `cache-loader`、`hard-source-webpack-plugin`、`uglifyjsWebpackPlugin`开启缓存、`babel-loader`开启缓存
- 预编译 `dllWebpackPlugin && DllReferencePlugin`、`auto-dll-webapck-plugin`

**性能优化**

- 减少编译体积 `Tree-shaking`、`Scope Hositing`
- `hash`缓存 `webpack-md5-plugin`
- 拆包 `splitChunksPlugin`、`import()`、`require.ensure`

------



### 服务端&浏览器

#### 状态码

```
  2XX（成功处理了请求状态）
      200 服务器已经成功处理请求，并提供了请求的网页
      201 用户新建或修改数据成功
      202 一个请求已经进入后台
      204 用户删除成功
  3XX（每次请求使用的重定向不要超过5次）
      304 网页上次请求没有更新，节省带宽和开销
  4XX（表示请求可能出错，妨碍了服务器的处理）
      400 服务器不理解请求的语法
      401 用户没有权限（用户名，密码输入错误）
      403 用户得到授权（401相反），但是访问被禁止
      404 服务器找不到请求的网页，
  5XX（表示服务器在处理请求的时候发生内部错误）
      500 服务器遇到错误，无法完成请求
      503 服务器目前无法使用（超载或停机维护）     

```

#### get/post的区别

```
1.get数据是存放在url之后，以？分割url和传输数据，参数之间以&相连； post方法是把提交的数据放在http包的Body中
2.get提交的数据大小有限制，（因为浏览器对url的长度有限制），post的方法提交的数据没有限制
3.get需要request.queryString来获取变量的值，而post方式通过request.from来获取变量的值
4.get的方法提交数据，会带来安全问题，比如登录一个页面，通过get的方式提交数据，用户名和密码就会出现在url上

```

#### http协议的理解

```
1.超文本的传输协议，是用于从万维网服务器超文本传输到本地资源的传输协议
2.基于TCP/IP通信协议来传递数据（HTML，图片资源）
3.基于运用层的面向对象的协议，由于其简洁、快速的方法、适用于分布式超媒体信息系统
4.http请求信息request：
	请求行（request line）、请求头部（header）,空行和请求数据四部分构成

	请求行，用来说明请求类型,要访问的资源以及所使用的HTTP版本.
	请求头部，用来说明服务器要使用的附加信息
	空行，请求头部后面的空行是必须的
	请求数据也叫主体，可以添加任意的其他数据。
5.http相应信息Response
	状态行、消息报头、空行和响应正文

	状态行，由HTTP协议版本号， 状态码， 状态消息 三部分组成
	消息报头，用来说明客户端要使用的一些附加信息
	空行，消息报头后面的空行是必须的
	响应正文，服务器返回给客户端的文本信息。

```

#### http和https

```
https：是以安全为目标的HTTP通道，简单讲是HTTP的安全版本，通过SSL加密
http：超文本传输协议。是一个客服端和服务器端请求和应答的标准（tcp）,使浏览器更加高效，使网络传输减少

```

#### http1.0 1.1 2.0的区别

```
长连接：HTTP1.0需要使用keep-alive参数来告知服务器建立一个长连接，而HTP1.1默认支持长连接
节约宽带：HTTP1.1支持只发送一个header信息（不带任何body信息）
host域（设置虚拟站点，也就是说，web server上的多个虚拟站点可以共享同一个ip端口）：HTTP1.0没有host域

1.http2采用的二进制文本传输数据，而非http1文本格式，二进制在协议的解析和扩展更好
2.数据压缩：对信息头采用了HPACK进行压缩传输，节省了信息头带来的网络流量
3.多路复用：一个连接可以并发处理多个请求
4.服务器推送：我们对支持HTTP2.0的web server请求数据的时候，服务器会顺便把一些客户端需要的资源一起推送到客户端，免得客户端再次创建连接发送请求到服务器端获取。这种方式非常合适加载静态资源

```

#### web缓存

```
1.web缓存就是存在于客户端与服务器之间的一个副本、当你第一个发出请求后，缓存根据请求保存输出内容的副本
2.缓存的好处
        （1）减少不必要的请求
    （2）降低服务器的压力，减少服务器的消耗
    （3）降低网络延迟，加快页面打开速度（直接读取浏览器的数据）

```

#### web安全及防护

```
1.sql注入：是将sql代码伪装到输入参数中，传递到服务器解析并执行的一种攻击手法。也就是说，
            在一些对server端发起的请求参数中植入一些sql代码，server端在执行sql操作时，会拼接对应参数，
            同时也将一些sql注入攻击的“sql”拼接起来，导致会执行一些预期之外的操作。
		防范：1.对用户输入进行校验
		       2.不适用动态拼接sql
2.XSS（跨站脚本攻击）：往web页面插入恶意的html标签或者js代码。
		        举例子：在论坛放置一个看是安全的链接，窃取cookie中的用户信息
			防范：1.尽量采用post而不使用get提交表单
			      2.避免cookie中泄漏用户的隐式
3.CSRF(跨站请求伪装）：通过伪装来自受信任用户的请求
			举例子：黄轶老师的webapp音乐请求数据就是利用CSRF跨站请求伪装来获取QQ音乐的数据
			防范：在客服端页面增加伪随机数，通过验证码
XSS和CSRF的区别：
   1.XSS是获取信息，不需要提前知道其他用户页面的代码和数据包
   2.CSRF代替用户完成指定的动作，需要知道其他页面的代码和数据包

```

#### CDN

```
1.尽可能的避开互联网有可能影响数据传输速度和稳定性的瓶颈和环节。使内容传输的更快更稳定。
2.关键技术：内容存储和分发技术中
3.基本：广泛采用各种缓存服务器，将这些缓存服务器分布到用户访问相对的地区或者网络中。当用户访问网络时利用全局负载技术
	    将用户的访问指向距离最近的缓存服务器，由缓存服务器直接相应用户的请求（全局负载技术）

```

#### TCP三次握手

```
客服端发c起请求连接服务器端s确认，服务器端也发起连接确认客服端确认。
第一次握手：客服端发送一个请求连接，服务器端只能确认自己可以接受客服端发送的报文段
第二次握手： 服务端向客服端发送一个链接，确认客服端收到自己发送的报文段
第三次握手： 服务器端确认客服端收到了自己发送的报文段

```

#### TCP四次挥手

- 客户端 -- FIN --> 服务端， FIN—WAIT
- 服务端 -- ACK --> 客户端， CLOSE-WAIT
- 服务端 -- ACK,FIN --> 客户端， LAST-ACK
- 客户端 -- ACK --> 服务端，CLOSED

#### 从输入url到获取页面过程

```
1.查询NDS(域名解析),获取域名对应的IP地址  查询浏览器缓存
2.浏览器与服务器建立tcp链接（三次握手）
3.浏览器向服务器发送http请求(请求和传输数据）
4.服务器接受到这个请求后，根据路经参数，经过后端的一些处理生成html代码返回给浏览器
5.浏览器拿到完整的html页面代码开始解析和渲染，如果遇到外部的css或者js，图片一样的步骤
6.浏览器根据拿到的资源对页面进行渲染，把一个完整的页面呈现出来

```

#### 为什么css放在顶部而js写在后面

```
1.浏览器预先加载css后，可以不必等待HTML加载完毕就可以渲染页面了
2.其实HTML渲染并不会等到完全加载完在渲染页面，而是一边解析DOM一边渲染。
3.js写在尾部，主要是因为js主要扮演事件处理的功能，一方面很多操作是在页面渲染后才执行的。另一方面可以节省加载时间，使页面能够更加的加载，提高用户的良好体验

但是随着JS技术的发展，JS也开始承担页面渲染的工作。比如我们的UI其实可以分被对待，把渲染页面的js放在前面，时间处理的js放在后面

```

#### 存储方式与传输方式

```
1.indexBD: 是h5的本地存储库，把一些数据存储到浏览器中，没网络，浏览器可以从这里读取数据，离线运用。5m
2.Cookie: 通过浏览器记录信息确认用户身份，最大4kb,这也就限制了传输的数据，请求的性能会受到影响
3.Session: 服务器端使用的一种记录客户状态的机制（session_id存在set_cookie发送到客服端，保存为cookie）
4.localStroage: h5的本地存储，数据永久保存在客服端

```

#### cookie，sessionStorage，localStorage

1、cookie，sessionStorage，localStorage是存放在客户端，session对象数据是存放在服务器上 实际上浏览器和服务器之间仅需传递session id即可，服务器根据session-id找到对应的用户session对象 session存储数据更安全一些，一般存放用户信息，浏览器只适合存储一般的数据 2、cookie数据始终在同源的http请求中携带，在浏览器和服务器来回传递，里面存放着session-id sessionStorage，localStorage仅在本地保存 3、大小限制区别，cookie数据不超过4kb，localStorage在谷歌浏览中2.6MB 4、数据有效期不同，cookie在设置的（服务器设置）有效期内有效，不管窗口和浏览器关闭 sessionStorage仅在当前浏览器窗口关闭前有效，关闭即销毁（临时存储） localStorage始终有效

SessionStorage和localStorage区别： 1.sessionStorage用于本地存储一个会话（session）中的数据，这些数据只有在用一个会话的页面中才能被访问（也就是说在第一次通信过程中） 并且在会话结束后数据也随之销毁，不是一个持久的本地存储，会话级别的储存 2.localStorage用于持久化的本地存储，除非主动删除数据，否则不会过期

#### token、cookie、session

```
1、token就是令牌，比如你授权(登录)一个程序时,他就是个依据,判断你是否已经授权该软件（最好的身份认证，安全性好，且是唯一的）
    用户身份的验证方式    

2、cookie是写在客户端一个txt文件，里面包括登录信息之类的，这样你下次在登录某个网站，就会自动调用cookie自动登录用户名
    服务器生成，发送到浏览器、浏览器保存，下次请求再次发送给服务器（存放着登录信息）

3、session是一类用来客户端和服务器之间保存状态的解决方案，会话完成被销毁（代表的就是服务器和客户端的一次会话过程）
    cookie中存放着sessionID，请求会发送这个id。sesion因为request对象而产生。

```

#### 基于Token的身份验证

```
  1、用户通过用户名和密码发送请求
  2、服务器端验证
  3、服务器端返回一个带签名的token，给客户端
  4、客户端储存token，并且每次用于发送请求
  5、服务器验证token并且返回数据
  每一次请求都需要token

```

#### cookie与session区别

```
  1、cookie数据存放在客户的浏览器上，session数据放在服务器上。
  2、cookie不是很安全，别人可以分析存放在本地的COOKIE并进行COOKIE欺骗考虑到安全应当使用session。
  3、session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能考虑到减轻服务器性能方面，应当使用COOKIE。
  4、单个cookie保存的数据不能超过4K，很多浏览器都限制一个站点最多保存20个cookie。

```

#### session与token区别

```
  1、session认证只是把简单的User的信息存储Session里面，sessionID不可预测，一种认证手段。只存在服务端，不能共享到其他的网站和第三方App
  2、token是oAuth Token，提供的是认证和授权，认证针对用户，授权是针对App，目的就是让某APP有权访问某用户的的信息。Token是唯一的，
     token不能转移到其他的App，也不能转到其他用户上。（适用于App）
  3、session的状态是存在服务器端的，客户端只存在session id， Token状态是存储在客户端的

```

#### Cookie的弊端有哪些

```
  1、数量和长度的限制。每个特定的域名下最多生成20个cookie（chorme和safari没有限制）
  2、安全性问题。

```

#### 怎么判断页面是否加载完成

- `Load` 事件触发代表页面中的 `DOM`，`CSS`，`JS`，图片已经全部加载完毕。
- `DOMContentLoaded` 事件触发代表初始的 `HTML` 被完全加载和解析，不需要等待 `CSS`，`JS`，图片加载

#### Service worker

service worker

`Service workers` 本质上充当Web应用程序与浏览器之间的代理服务器，也可以在网络可用时作为浏览器和网络间的代理。它们旨在（除其他之外）使得能够创建有效的离线体验，拦截网络请求并基于网络是否可用以及更新的资源是否驻留在服务器上来采取适当的动作。他们还允许访问推送通知和后台同步API

目前该技术通常用来做缓存文件，提高首屏速度，可以试着来实现这个功能

```js
// index.js
if (navigator.serviceWorker) {
  navigator.serviceWorker
    .register("sw.js")
    .then(function(registration) {
      console.log("service worker 注册成功");
    })
    .catch(function(err) {
      console.log("servcie worker 注册失败");
    });
}
// sw.js
// 监听 `install` 事件，回调中缓存所需文件
self.addEventListener("install", e => {
  e.waitUntil(
    caches.open("my-cache").then(function(cache) {
      return cache.addAll(["./index.html", "./index.js"]);
    })
  );
});

// 拦截所有请求事件
// 如果缓存中已经有请求的数据就直接用缓存，否则去请求数据
self.addEventListener("fetch", e => {
  e.respondWith(
    caches.match(e.request).then(function(response) {
      if (response) {
        return response;
      }
      console.log("fetch source");
    })
  );
});
```



#### 浏览器缓存

> 缓存对于前端性能优化来说是个很重要的点，良好的缓存策略可以降低资源的重复加载提高网页的整体加载速度。

- 通常浏览器缓存策略分为两种：强缓存和协商缓存。

**强缓存**

> 实现强缓存可以通过两种响应头实现：`Expires` 和 `Cache-Control` 。强缓存表示在缓存期间不需要请求，`state code` 为 `200`

```text
Expires: Wed, 22 Oct 2018 08:41:00 GMT
```

> `Expires` 是 `HTTP / 1.0` 的产物，表示资源会在`Wed`,`22 Oct 2018 08:41:00 GMT` 后过期，需要再次请求。并且 `Expires` 受限于本地时间，如果修改了本地时间，可能会造成缓存失效。

```text
Cache-control: max-age=30
```

- `Cache-Control` 出现于 `HTTP / 1.1`，优先级高于 `Expires` 。该属性表示资源会在 `30` 秒后过期，需要再次请求。

**协商缓存**

- 如果缓存过期了，我们就可以使用协商缓存来解决问题。协商缓存需要请求，如果缓存有效会返回 `304`。
- 协商缓存需要客户端和服务端共同实现，和强缓存一样，也有两种实现方式

**Last-Modified 和 If-Modified-Since**

- `Last-Modified`表示本地文件最后修改日期，`If-Modified-Since` 会将 `Last-Modified` 的值发送给服务器，询问服务器在该日期后资源是否有更新，有更新的话就会将新的资源发送回来。
- 但是如果在本地打开缓存文件，就会造成 `Last-Modified`被修改，所以在 `HTTP / 1.1` 出现了 `ETag`

**ETag 和 If-None-Match**

> `ETag` 类似于文件指纹，`If-None-Match` 会将当前 `ETag`发送给服务器，询问该资源 `ETag` 是否变动，有变动的话就将新的资源发送回来。并且 `ETag` 优先级比 `Last-Modified` 高

**选择合适的缓存策略**

> 对于大部分的场景都可以使用强缓存配合协商缓存解决，但是在一些特殊的地方可能需要选择特殊的缓存策略

- 对于某些不需要缓存的资源，可以使用 `Cache-control: no-store` ，表示该资源不需要缓存
- 对于频繁变动的资源，可以使用 `Cache-Control: no-cache`并配合 `ETag` 使用，表示该资源已被缓存，但是每次都会发送请求询问资源是否更新。
- 对于代码文件来说，通常使用 `Cache-Control: max-age=31536000` 并配合策略缓存使用，然后对文件进行指纹处理，一旦文件名变动就会立刻下载新的文件

#### 浏览器性能问题

**重绘（Repaint）和回流（Reflow）**

- 重绘和回流是渲染步骤中的一小节，但是这两个步骤对于性能影响很大。
- 重绘是当节点需要更改外观而不会影响布局的，比如改变 `color`就叫称为重绘
- 回流是布局或者几何属性需要改变就称为回流。
- 回流必定会发生重绘，重绘不一定会引发回流。回流所需的成本比重绘高的多，改变深层次的节点很可能导致父节点的一系列回流。

**所以以下几个动作可能会导致性能问题**：

- 改变 `window` 大小
- 改变字体
- 添加或删除样式
- 文字改变
- 定位或者浮动
- 盒模型

**很多人不知道的是，重绘和回流其实和 Event loop 有关**。

- 当 `Event loop` 执行完 `Microtasks`后，会判断 `document` 是否需要更新。- 因为浏览器是 `60Hz` 的刷新率，每 `16ms`才会更新一次。
- 然后判断是否有`resize` 或者 `scroll` ，有的话会去触发事件，所以 `resize` 和 `scroll` 事件也是至少 16ms 才会触发一次，并且自带节流功能。
- 判断是否触发了 `media query`
- 更新动画并且发送事件
- 判断是否有全屏操作事件
- 执行 `requestAnimationFrame`回调
- 执行 `IntersectionObserver` 回调，该方法用于判断元素是否可见，可以用于懒加载上，但是兼容性不好
- 更新界面
- 以上就是一帧中可能会做的事情。如果在一帧中有空闲时间，就会去执行 `requestIdleCallback` 回调。

**减少重绘和回流**

> 使用 `translate` 替代 `top`

```html
<div class="test"></div>
<style>
	.test {
		position: absolute;
		top: 10px;
		width: 100px;
		height: 100px;
		background: red;
	}
</style>
<script>
	setTimeout(() => {
        // 引起回流
		document.querySelector('.test').style.top = '100px'
	}, 1000)
</script>
```

- 使用 `visibility` 替换 `display: none` ，因为前者只会引起重绘，后者会引发回流（改变了布局）
- 把 `DOM` 离线后修改，比如：先把 `DOM` 给 `display:none`(有一次 `Reflow`)，然后你修改`100`次，然后再把它显示出来
- 不要把 `DOM`结点的属性值放在一个循环里当成循环里的变量

```js
for(let i = 0; i < 1000; i++) {
    // 获取 offsetTop 会导致回流，因为需要去获取正确的值
    console.log(document.querySelector('.test').style.offsetTop)
}
```

- 不要使用 `table` 布局，可能很小的一个小改动会造成整个 `table` 的重新布局 动画实现的速度的选择，动画速度越快，回流次数越多，也可以选择使用 `requestAnimationFrame`
- `CSS`选择符从右往左匹配查找，避免 `DOM` 深度过深
- 将频繁运行的动画变为图层，图层能够阻止该节点回流影响别的元素。比如对于 `video` 标签，浏览器会自动将该节点变为图层。

#### 负载均衡

> 多台服务器共同协作，不让其中某一台或几台超额工作，发挥服务器的最大作用

- `http`重定向负载均衡：调度者根据策略选择服务器以302响应请求，缺点只有第一次有效果，后续操作维持在该服务器 dns负载均衡：解析域名时，访问多个`ip`服务器中的一个（可监控性较弱）
- 反向代理负载均衡：访问统一的服务器，由服务器进行调度访问实际的某个服务器，对统一的服务器要求大，性能受到 服务器群的数量

#### 浏览器 Eventloop 和 Node 中的有什么区别

> 众所周知 JS 是门非阻塞单线程语言，因为在最初 JS 就是为了和浏览器交互而诞生的。如果 JS 是门多线程的语言话，我们在多个线程中处理 DOM 就可能会发生问题（一个线程中新加节点，另一个线程中删除节点），当然可以引入读写锁解决这个问题。

- `JS` 在执行的过程中会产生执行环境，这些执行环境会被顺序的加入到执行栈中。如果遇到异步的代码，会被挂起并加入到 `Task`（有多种 `task`） 队列中。一旦执行栈为空，`Event Loop` 就会从 `Task` 队列中拿出需要执行的代码并放入执行栈中执行，所以本质上来说 `JS` 中的异步还是同步行为

```js
console.log('script start');

setTimeout(function() {
  console.log('setTimeout');
}, 0);

console.log('script end');
```

- 以上代码虽然 `setTimeout` 延时为 `0`，其实还是异步。这是因为 `HTML5` 标准规定这个函数第二个参数不得小于 `4` 毫秒，不足会自动增加。所以 `setTimeout`还是会在 `script end` 之后打印。
- 不同的任务源会被分配到不同的 `Task`队列中，任务源可以分为 微任务（`microtask`） 和 宏任务（`macrotask`）。在 ES6 规范中，`microtask` 称为 jobs，`macrotask` 称为 `task`。

```js
console.log('script start');

setTimeout(function() {
  console.log('setTimeout');
}, 0);

new Promise((resolve) => {
    console.log('Promise')
    resolve()
}).then(function() {
  console.log('promise1');
}).then(function() {
  console.log('promise2');
});

console.log('script end');
// script start => Promise => script end => promise1 => promise2 => setTimeout
```

- 以上代码虽然 `setTimeout` 写在 `Promise` 之前，但是因为 `Promise` 属于微任务而 `setTimeout`属于宏任务，所以会有以上的打印。
- **微任务包括** `process.nextTick` ，`promise` ，`Object.observe`，`MutationObserver`
- **宏任务包括** `script` ， `setTimeout` ，`setInterval`，`setImmediate` ，`I/O` ，`UI renderin`

> 很多人有个误区，认为微任务快于宏任务，其实是错误的。因为宏任务中包括了 `script` ，浏览器会先执行一个宏任务，接下来有异步代码的话就先执行微任务

**所以正确的一次 Event loop 顺序是这样的**

- 执行同步代码，这属于宏任务
- 执行栈为空，查询是否有微任务需要执行
- 执行所有微任务
- 必要的话渲染 `UI`
- 然后开始下一轮 `Event loop`，执行宏任务中的异步代码

> 通过上述的 `Event loop` 顺序可知，如果宏任务中的异步代码有大量的计算并且需要操作 `DOM` 的话，为了更快的 界面响应，我们可以把操作 `DOM` 放入微任务中

#### 重绘与回流

> 当元素的样式发生变化时，浏览器需要触发更新，重新绘制元素。这个过程中，有两种类型的操作，即重绘与回流。

> - 重绘(repaint): 当元素样式的改变不影响布局时，浏览器将使用重绘对元素进行更新，此时由于只需要UI层面的重新像素绘制，因此 损耗较少
> - 回流(reflow): 当元素的尺寸、结构或触发某些属性时，浏览器会重新渲染页面，称为回流。此时，浏览器需要重新经过计算，计算后还需要重新页面布局，因此是较重的操作。会触发回流的操作:

- 页面初次渲染
- 浏览器窗口大小改变
- 元素尺寸、位置、内容发生改变
- 元素字体大小变化
- 添加或者删除可见的 `dom` 元素
- 激活 `CSS` 伪类（例如：`:hover`）
- 查询某些属性或调用某些方法
  - `clientWidth、clientHeight、clientTop、clientLeft`
  - `offsetWidth、offsetHeight、offsetTop、offsetLeft`
  - `scrollWidth、scrollHeight、scrollTop、scrollLeft`
  - `getComputedStyle()`
  - `getBoundingClientRect()`
  - `scrollTo()`

> 回流必定触发重绘，重绘不一定触发回流。重绘的开销较小，回流的代价较高。

**最佳实践:**

**css**

- 避免使用`table`布局
- 将动画效果应用到`position`属性为`absolute`或`fixed`的元素上

**javascript**

- 避免频繁操作样式，可汇总后统一 一次修改
- 尽量使用`class`进行样式修改
- 减少`dom`的增删次数，可使用 字符串 或者 `documentFragment` 一次性插入
- 极限优化时，修改样式可将其`display: none`后修改
- 避免多次触发上面提到的那些会触发回流的方法，可以的话尽量用 变量存住



------



### 安全

#### XSS

> 跨网站指令码（英语：`Cross-site scripting`，通常简称为：`XSS`）是一种网站应用程式的安全漏洞攻击，是代码注入的一种。它允许恶意使用者将程式码注入到网页上，其他使用者在观看网页时就会受到影响。这类攻击通常包含了 `HTML` 以及使用者端脚本语言

> ```
> XSS` 分为三种：反射型，存储型和 `DOM-based
> ```

**如何攻击**

- `XSS` 通过修改 `HTML`节点或者执行 `JS`代码来攻击网站。
- 例如通过 `URL` 获取某些参数

```html
<!-- http://www.domain.com?name=<script>alert(1)</script> -->
<div>{{name}}</div>    
```

> 上述 `URL` 输入可能会将 `HTML` 改为 `alert(1)` ，这样页面中就凭空多了一段可执行脚本。这种攻击类型是反射型攻击，也可以说是 `DOM-based` 攻击

**如何防御**

> 最普遍的做法是转义输入输出的内容，对于引号，尖括号，斜杠进行转义

```js
function escape(str) {
	str = str.replace(/&/g, "&amp;");
	str = str.replace(/</g, "&lt;");
	str = str.replace(/>/g, "&gt;");
	str = str.replace(/"/g, "&quto;");
	str = str.replace(/'/g, "&##39;");
	str = str.replace(/`/g, "&##96;");
    str = str.replace(/\//g, "&##x2F;");
    return str
}
```

> 通过转义可以将攻击代码 `alert(1)` 变成

```js
// -> &lt;script&gt;alert(1)&lt;&##x2F;script&gt;
escape('<script>alert(1)</script>')
```

> 对于显示富文本来说，不能通过上面的办法来转义所有字符，因为这样会把需要的格式也过滤掉。这种情况通常采用白名单过滤的办法，当然也可以通过黑名单过滤，但是考虑到需要过滤的标签和标签属性实在太多，更加推荐使用白名单的方式

```js
var xss = require("xss");
var html = xss('<h1 id="title">XSS Demo</h1><script>alert("xss");</script>');
// -> <h1>XSS Demo</h1>&lt;script&gt;alert("xss");&lt;/script&gt;
console.log(html);
```

> 以上示例使用了 `js-xss`来实现。可以看到在输出中保留了 `h1` 标签且过滤了 `script` 标签

#### CSRF

> 跨站请求伪造（英语：`Cross-site request forgery`），也被称为 `one-click attack`或者 `session riding`，通常缩写为 `CSRF` 或者 `XSRF`， 是一种挟制用户在当前已登录的`Web`应用程序上执行非本意的操作的攻击方法

> `CSRF` 就是利用用户的登录态发起恶意请求

**如何攻击**

> 假设网站中有一个通过 Get 请求提交用户评论的接口，那么攻击者就可以在钓鱼网站中加入一个图片，图片的地址就是评论接口

```text
<img src="http://www.domain.com/xxx?comment='attack'"/>
```

**如何防御**

- `Get` 请求不对数据进行修改
- 不让第三方网站访问到用户 `Cookie`
- 阻止第三方网站请求接口
- 请求时附带验证信息，比如验证码或者 `token`

#### 密码安全

**加盐**

> 对于密码存储来说，必然是不能明文存储在数据库中的，否则一旦数据库泄露，会对用户造成很大的损失。并且不建议只对密码单纯通过加密算法加密，因为存在彩虹表的关系

- 通常需要对密码加盐，然后进行几次不同加密算法的加密

```js
// 加盐也就是给原密码添加字符串，增加原密码长度
sha256(sha1(md5(salt + password + salt)))
```

> 但是加盐并不能阻止别人盗取账号，只能确保即使数据库泄露，也不会暴露用户的真实密码。一旦攻击者得到了用户的账号，可以通过暴力破解的方式破解密码。对于这种情况，通常使用验证码增加延时或者限制尝试次数的方式。并且一旦用户输入了错误的密码，也不能直接提示用户输错密码，而应该提示账号或密码错误

#### **前端加密**

> 虽然前端加密对于安全防护来说意义不大，但是在遇到中间人攻击的情况下，可以避免明文密码被第三方获取

#### 点击劫持

> 涉及面试题：什么是点击劫持？如何防范点击劫持？

> 点击劫持是一种视觉欺骗的攻击手段。攻击者将需要攻击的网站通过 `iframe` 嵌套的方式嵌入自己的网页中，并将 `iframe` 设置为透明，在页面中透出一个按钮诱导用户点击

![image.png](https://upload-images.jianshu.io/upload_images/1480597-e2fd2624a105fce7.png)

> 对于这种攻击方式，推荐防御的方法有两种

**1. X-FRAME-OPTIONS**

> `X-FRAME-OPTIONS` 是一个 `HTTP` 响应头，在现代浏览器有一个很好的支持。这个 HTTP 响应头 就是为了防御用`iframe` 嵌套的点击劫持攻击。

> 该响应头有三个值可选，分别是

- `DENY`，表示页面不允许通过 `iframe` 的方式展示
- `SAMEORIGIN`，表示页面可以在相同域名下通过 `iframe` 的方式展示
- `ALLOW-FROM`，表示页面可以在指定来源的 `iframe` 中展示

**2. JS 防御**

> 对于某些远古浏览器来说，并不能支持上面的这种方式，那我们只有通过 JS 的方式来防御点击劫持了。

```html
<head>
  <style id="click-jack">
    html {
      display: none !important;
    }
  </style>
</head>
<body>
  <script>
    if (self == top) {
      var style = document.getElementById('click-jack')
      document.body.removeChild(style)
    } else {
      top.location = self.location
    }
  </script>
</body>
```

> 以上代码的作用就是当通过 `iframe` 的方式加载页面时，攻击者的网页直接不显示所有内容了

------



### 数据结构

#### 栈

**概念**

- 栈是一个线性结构，在计算机中是一个相当常见的数据结构。
- 栈的特点是只能在某一端添加或删除数据，遵循先进后出的原则

![img](https://user-gold-cdn.xitu.io/2018/5/20/1637b785d2d68735?w=640&h=460&f=png&s=6932)

**实现**

> 每种数据结构都可以用很多种方式来实现，其实可以把栈看成是数组的一个子集，所以这里使用数组来实现

```js
class Stack {
  constructor() {
    this.stack = []
  }
  push(item) {
    this.stack.push(item)
  }
  pop() {
    this.stack.pop()
  }
  peek() {
    return this.stack[this.getCount() - 1]
  }
  getCount() {
    return this.stack.length
  }
  isEmpty() {
    return this.getCount() === 0
  }
}
```

**应用**

> 匹配括号，可以通过栈的特性来完成

```js
var isValid = function (s) {
  let map = {
    '(': -1,
    ')': 1,
    '[': -2,
    ']': 2,
    '{': -3,
    '}': 3
  }
  let stack = []
  for (let i = 0; i < s.length; i++) {
    if (map[s[i]] < 0) {
      stack.push(s[i])
    } else {
      let last = stack.pop()
      if (map[last] + map[s[i]] != 0) return false
    }
  }
  if (stack.length > 0) return false
  return true
};
```

#### 队列

**概念**

> 队列一个线性结构，特点是在某一端添加数据，在另一端删除数据，遵循先进先出的原则

![img](https://user-gold-cdn.xitu.io/2018/5/20/1637cba2a6155793?w=640&h=419&f=png&s=15737)

**实现**

> 这里会讲解两种实现队列的方式，分别是单链队列和循环队列

- **单链队列**

```js
class Queue {
  constructor() {
    this.queue = []
  }
  enQueue(item) {
    this.queue.push(item)
  }
  deQueue() {
    return this.queue.shift()
  }
  getHeader() {
    return this.queue[0]
  }
  getLength() {
    return this.queue.length
  }
  isEmpty() {
    return this.getLength() === 0
  }
}
```

> 因为单链队列在出队操作的时候需要 `O(n)` 的时间复杂度，所以引入了循环队列。循环队列的出队操作平均是 `O(1)` 的时间复杂度

- **循环队列**

```js
class SqQueue {
  constructor(length) {
    this.queue = new Array(length + 1)
    // 队头
    this.first = 0
    // 队尾
    this.last = 0
    // 当前队列大小
    this.size = 0
  }
  enQueue(item) {
    // 判断队尾 + 1 是否为队头
    // 如果是就代表需要扩容数组
    // % this.queue.length 是为了防止数组越界
    if (this.first === (this.last + 1) % this.queue.length) {
      this.resize(this.getLength() * 2 + 1)
    }
    this.queue[this.last] = item
    this.size++
    this.last = (this.last + 1) % this.queue.length
  }
  deQueue() {
    if (this.isEmpty()) {
      throw Error('Queue is empty')
    }
    let r = this.queue[this.first]
    this.queue[this.first] = null
    this.first = (this.first + 1) % this.queue.length
    this.size--
    // 判断当前队列大小是否过小
    // 为了保证不浪费空间，在队列空间等于总长度四分之一时
    // 且不为 2 时缩小总长度为当前的一半
    if (this.size === this.getLength() / 4 && this.getLength() / 2 !== 0) {
      this.resize(this.getLength() / 2)
    }
    return r
  }
  getHeader() {
    if (this.isEmpty()) {
      throw Error('Queue is empty')
    }
    return this.queue[this.first]
  }
  getLength() {
    return this.queue.length - 1
  }
  isEmpty() {
    return this.first === this.last
  }
  resize(length) {
    let q = new Array(length)
    for (let i = 0; i < length; i++) {
      q[i] = this.queue[(i + this.first) % this.queue.length]
    }
    this.queue = q
    this.first = 0
    this.last = this.size
  }
}
```

#### 链表

**概念**

> 链表是一个线性结构，同时也是一个天然的递归结构。链表结构可以充分利用计算机内存空间，实现灵活的内存动态管理。但是链表失去了数组随机读取的优点，同时链表由于增加了结点的指针域，空间开销比较大

![img](https://user-gold-cdn.xitu.io/2018/5/22/16388487759b1152?w=1060&h=178&f=png&s=20820)

**实现**

- **单向链表**

```js
class Node {
  constructor(v, next) {
    this.value = v
    this.next = next
  }
}
class LinkList {
  constructor() {
    // 链表长度
    this.size = 0
    // 虚拟头部
    this.dummyNode = new Node(null, null)
  }
  find(header, index, currentIndex) {
    if (index === currentIndex) return header
    return this.find(header.next, index, currentIndex + 1)
  }
  addNode(v, index) {
    this.checkIndex(index)
    // 当往链表末尾插入时，prev.next 为空
    // 其他情况时，因为要插入节点，所以插入的节点
    // 的 next 应该是 prev.next
    // 然后设置 prev.next 为插入的节点
    let prev = this.find(this.dummyNode, index, 0)
    prev.next = new Node(v, prev.next)
    this.size++
    return prev.next
  }
  insertNode(v, index) {
    return this.addNode(v, index)
  }
  addToFirst(v) {
    return this.addNode(v, 0)
  }
  addToLast(v) {
    return this.addNode(v, this.size)
  }
  removeNode(index, isLast) {
    this.checkIndex(index)
    index = isLast ? index - 1 : index
    let prev = this.find(this.dummyNode, index, 0)
    let node = prev.next
    prev.next = node.next
    node.next = null
    this.size--
    return node
  }
  removeFirstNode() {
    return this.removeNode(0)
  }
  removeLastNode() {
    return this.removeNode(this.size, true)
  }
  checkIndex(index) {
    if (index < 0 || index > this.size) throw Error('Index error')
  }
  getNode(index) {
    this.checkIndex(index)
    if (this.isEmpty()) return
    return this.find(this.dummyNode, index, 0).next
  }
  isEmpty() {
    return this.size === 0
  }
  getSize() {
    return this.size
  }
}
```

#### 树

**二叉树**

- 树拥有很多种结构，二叉树是树中最常用的结构，同时也是一个天然的递归结构。
- 二叉树拥有一个根节点，每个节点至多拥有两个子节点，分别为：左节点和右节点。树的最底部节点称之为叶节点，当一颗树的叶数量数量为满时，该树可以称之为满二叉树

![img](https://user-gold-cdn.xitu.io/2018/5/22/163884f74c9f4e4d?w=320&h=267&f=png&s=11622)

**二分搜索树**

- 二分搜索树也是二叉树，拥有二叉树的特性。但是区别在于二分搜索树每个节点的值都比他的左子树的值大，比右子树的值小
- 这种存储方式很适合于数据搜索。如下图所示，当需要查找 6 的时候，因为需要查找的值比根节点的值大，所以只需要在根节点的右子树上寻找，大大提高了搜索效率

![img](https://user-gold-cdn.xitu.io/2018/5/22/1638850ba7458208?w=596&h=485&f=png&s=36796)

- **实现**

```text
class Node {
  constructor(value) {
    this.value = value
    this.left = null
    this.right = null
  }
}
class BST {
  constructor() {
    this.root = null
    this.size = 0
  }
  getSize() {
    return this.size
  }
  isEmpty() {
    return this.size === 0
  }
  addNode(v) {
    this.root = this._addChild(this.root, v)
  }
  // 添加节点时，需要比较添加的节点值和当前
  // 节点值的大小
  _addChild(node, v) {
    if (!node) {
      this.size++
      return new Node(v)
    }
    if (node.value > v) {
      node.left = this._addChild(node.left, v)
    } else if (node.value < v) {
      node.right = this._addChild(node.right, v)
    }
    return node
  }
}
```

- 以上是最基本的二分搜索树实现，接下来实现树的遍历。

> 对于树的遍历来说，有三种遍历方法，分别是先序遍历、中序遍历、后序遍历。三种遍历的区别在于何时访问节点。在遍历树的过程中，每个节点都会遍历三次，分别是遍历到自己，遍历左子树和遍历右子树。如果需要实现先序遍历，那么只需要第一次遍历到节点时进行操作即可

```js
// 先序遍历可用于打印树的结构
// 先序遍历先访问根节点，然后访问左节点，最后访问右节点。
preTraversal() {
  this._pre(this.root)
}
_pre(node) {
  if (node) {
    console.log(node.value)
    this._pre(node.left)
    this._pre(node.right)
  }
}
// 中序遍历可用于排序
// 对于 BST 来说，中序遍历可以实现一次遍历就
// 得到有序的值
// 中序遍历表示先访问左节点，然后访问根节点，最后访问右节点。
midTraversal() {
  this._mid(this.root)
}
_mid(node) {
  if (node) {
    this._mid(node.left)
    console.log(node.value)
    this._mid(node.right)
  }
}
// 后序遍历可用于先操作子节点
// 再操作父节点的场景
// 后序遍历表示先访问左节点，然后访问右节点，最后访问根节点。
backTraversal() {
  this._back(this.root)
}
_back(node) {
  if (node) {
    this._back(node.left)
    this._back(node.right)
    console.log(node.value)
  }
}
```

> 以上的这几种遍历都可以称之为深度遍历，对应的还有种遍历叫做广度遍历，也就是一层层地遍历树。对于广度遍历来说，我们需要利用之前讲过的队列结构来完成

```js
breadthTraversal() {
  if (!this.root) return null
  let q = new Queue()
  // 将根节点入队
  q.enQueue(this.root)
  // 循环判断队列是否为空，为空
  // 代表树遍历完毕
  while (!q.isEmpty()) {
    // 将队首出队，判断是否有左右子树
    // 有的话，就先左后右入队
    let n = q.deQueue()
    console.log(n.value)
    if (n.left) q.enQueue(n.left)
    if (n.right) q.enQueue(n.right)
  }
}
```

> 接下来先介绍如何在树中寻找最小值或最大数。因为二分搜索树的特性，所以最小值一定在根节点的最左边，最大值相反

```js
getMin() {
  return this._getMin(this.root).value
}
_getMin(node) {
  if (!node.left) return node
  return this._getMin(node.left)
}
getMax() {
  return this._getMax(this.root).value
}
_getMax(node) {
  if (!node.right) return node
  return this._getMin(node.right)
}
```

> 向上取整和向下取整，这两个操作是相反的，所以代码也是类似的，这里只介绍如何向下取整。既然是向下取整，那么根据二分搜索树的特性，值一定在根节点的左侧。只需要一直遍历左子树直到当前节点的值不再大于等于需要的值，然后判断节点是否还拥有右子树。如果有的话，继续上面的递归判断

```js
floor(v) {
  let node = this._floor(this.root, v)
  return node ? node.value : null
}
_floor(node, v) {
  if (!node) return null
  if (node.value === v) return v
  // 如果当前节点值还比需要的值大，就继续递归
  if (node.value > v) {
    return this._floor(node.left, v)
  }
  // 判断当前节点是否拥有右子树
  let right = this._floor(node.right, v)
  if (right) return right
  return node
}
```

> 排名，这是用于获取给定值的排名或者排名第几的节点的值，这两个操作也是相反的，所以这个只介绍如何获取排名第几的节点的值。对于这个操作而言，我们需要略微的改造点代码，让每个节点拥有一个 size 属性。该属性表示该节点下有多少子节点（包含自身）

```js
class Node {
  constructor(value) {
    this.value = value
    this.left = null
    this.right = null
    // 修改代码
    this.size = 1
  }
}
// 新增代码
_getSize(node) {
  return node ? node.size : 0
}
_addChild(node, v) {
  if (!node) {
    return new Node(v)
  }
  if (node.value > v) {
    // 修改代码
    node.size++
    node.left = this._addChild(node.left, v)
  } else if (node.value < v) {
    // 修改代码
    node.size++
    node.right = this._addChild(node.right, v)
  }
  return node
}
select(k) {
  let node = this._select(this.root, k)
  return node ? node.value : null
}
_select(node, k) {
  if (!node) return null
  // 先获取左子树下有几个节点
  let size = node.left ? node.left.size : 0
  // 判断 size 是否大于 k
  // 如果大于 k，代表所需要的节点在左节点
  if (size > k) return this._select(node.left, k)
  // 如果小于 k，代表所需要的节点在右节点
  // 注意这里需要重新计算 k，减去根节点除了右子树的节点数量
  if (size < k) return this._select(node.right, k - size - 1)
  return node
}
```

> 接下来讲解的是二分搜索树中最难实现的部分：删除节点。因为对于删除节点来说，会存在以下几种情况

- 需要删除的节点没有子树
- 需要删除的节点只有一条子树
- 需要删除的节点有左右两条树
- 对于前两种情况很好解决，但是第三种情况就有难度了，所以先来实现相对简单的操作：删除最小节点，对于删除最小节点来说，是不存在第三种情况的，删除最大节点操作是和删除最小节点相反的，所以这里也就不再赘述

```js
delectMin() {
  this.root = this._delectMin(this.root)
  console.log(this.root)
}
_delectMin(node) {
  // 一直递归左子树
  // 如果左子树为空，就判断节点是否拥有右子树
  // 有右子树的话就把需要删除的节点替换为右子树
  if ((node != null) & !node.left) return node.right
  node.left = this._delectMin(node.left)
  // 最后需要重新维护下节点的 `size`
  node.size = this._getSize(node.left) + this._getSize(node.right) + 1
  return node
}
```

- 最后讲解的就是如何删除任意节点了。对于这个操作，`T.Hibbard` 在 `1962`年提出了解决这个难题的办法，也就是如何解决第三种情况。
- 当遇到这种情况时，需要取出当前节点的后继节点（也就是当前节点右子树的最小节点）来替换需要删除的节点。然后将需要删除节点的左子树赋值给后继结点，右子树删除后继结点后赋值给他。
- 你如果对于这个解决办法有疑问的话，可以这样考虑。因为二分搜索树的特性，父节点一定比所有左子节点大，比所有右子节点小。那么当需要删除父节点时，势必需要拿出一个比父节点大的节点来替换父节点。这个节点肯定不存在于左子树，必然存在于右子树。然后又需要保持父节点都是比右子节点小的，那么就可以取出右子树中最小的那个节点来替换父节点

```js
delect(v) {
  this.root = this._delect(this.root, v)
}
_delect(node, v) {
  if (!node) return null
  // 寻找的节点比当前节点小，去左子树找
  if (node.value < v) {
    node.right = this._delect(node.right, v)
  } else if (node.value > v) {
    // 寻找的节点比当前节点大，去右子树找
    node.left = this._delect(node.left, v)
  } else {
    // 进入这个条件说明已经找到节点
    // 先判断节点是否拥有拥有左右子树中的一个
    // 是的话，将子树返回出去，这里和 `_delectMin` 的操作一样
    if (!node.left) return node.right
    if (!node.right) return node.left
    // 进入这里，代表节点拥有左右子树
    // 先取出当前节点的后继结点，也就是取当前节点右子树的最小值
    let min = this._getMin(node.right)
    // 取出最小值后，删除最小值
    // 然后把删除节点后的子树赋值给最小值节点
    min.right = this._delectMin(node.right)
    // 左子树不动
    min.left = node.left
    node = min
  }
  // 维护 size
  node.size = this._getSize(node.left) + this._getSize(node.right) + 1
  return node
}
```

#### 堆

**概念**

- 堆通常是一个可以被看做一棵树的数组对象。
- 堆的实现通过构造二叉堆，实为二叉树的一种。这种数据结构具有以下性质。
- 任意节点小于（或大于）它的所有子节点 堆总是一棵完全树。即除了最底层，其他层的节点都被元素填满，且最底层从左到右填入。
- 将根节点最大的堆叫做最大堆或大根堆，根节点最小的堆叫做最小堆或小根堆。
- 优先队列也完全可以用堆来实现，操作是一模一样的。

**实现大根堆**

> 堆的每个节点的左边子节点索引是 `i * 2 + 1`，右边是 `i * 2 + 2`，父节点是 `(i - 1) /2`。

- 堆有两个核心的操作，分别是 `shiftUp` 和 `shiftDown` 。前者用于添加元素，后者用于删除根节点。
- `shiftUp` 的核心思路是一路将节点与父节点对比大小，如果比父节点大，就和父节点交换位置。
- `shiftDown` 的核心思路是先将根节点和末尾交换位置，然后移除末尾元素。接下来循环判断父节点和两个子节点的大小，如果子节点大，就把最大的子节点和父节点交换

![img](https://user-gold-cdn.xitu.io/2018/6/15/164009e58a5a21f8?w=537&h=394&f=png&s=77222)

```js
class MaxHeap {
  constructor() {
    this.heap = []
  }
  size() {
    return this.heap.length
  }
  empty() {
    return this.size() == 0
  }
  add(item) {
    this.heap.push(item)
    this._shiftUp(this.size() - 1)
  }
  removeMax() {
    this._shiftDown(0)
  }
  getParentIndex(k) {
    return parseInt((k - 1) / 2)
  }
  getLeftIndex(k) {
    return k * 2 + 1
  }
  _shiftUp(k) {
    // 如果当前节点比父节点大，就交换
    while (this.heap[k] > this.heap[this.getParentIndex(k)]) {
      this._swap(k, this.getParentIndex(k))
      // 将索引变成父节点
      k = this.getParentIndex(k)
    }
  }
  _shiftDown(k) {
    // 交换首位并删除末尾
    this._swap(k, this.size() - 1)
    this.heap.splice(this.size() - 1, 1)
    // 判断节点是否有左孩子，因为二叉堆的特性，有右必有左
    while (this.getLeftIndex(k) < this.size()) {
      let j = this.getLeftIndex(k)
      // 判断是否有右孩子，并且右孩子是否大于左孩子
      if (j + 1 < this.size() && this.heap[j + 1] > this.heap[j]) j++
      // 判断父节点是否已经比子节点都大
      if (this.heap[k] >= this.heap[j]) break
      this._swap(k, j)
      k = j
    }
  }
  _swap(left, right) {
    let rightValue = this.heap[right]
    this.heap[right] = this.heap[left]
    this.heap[left] = rightValue
  }
}
```

------



### 算法

#### 时间复杂度

- 通常使用最差的时间复杂度来衡量一个算法的好坏。
- 常数时间 `O(1)` 代表这个操作和数据量没关系，是一个固定时间的操作，比如说四则运算。
- 对于一个算法来说，可能会计算出如下操作次数 `aN +`1，`N` 代表数据量。那么该算法的时间复杂度就是 `O(N)`。因为我们在计算时间复杂度的时候，数据量通常是非常大的，这时候低阶项和常数项可以忽略不计。
- 当然可能会出现两个算法都是 `O(N)` 的时间复杂度，那么对比两个算法的好坏就要通过对比低阶项和常数项了

#### 位运算

- 位运算在算法中很有用，速度可以比四则运算快很多。
- 在学习位运算之前应该知道十进制如何转二进制，二进制如何转十进制。这里说明下简单的计算方式
- 十进制 `33` 可以看成是 `32 + 1` ，并且 `33` 应该是六位二进制的（因为 `33`近似 `32`，而 `32` 是 `2`的五次方，所以是六位），那么 十进制 `33` 就是 `100001` ，只要是 2 的次方，那么就是 `1`否则都为 `0` 那么二进制 `100001` 同理，首位是 `2^5`，末位是 `2^0` ，相加得出 `33`

**左移 <<**

```text
10 << 1 // -> 20
```

> 左移就是将二进制全部往左移动，`10`在二进制中表示为 `1010` ，左移一位后变成 `10100` ，转换为十进制也就是 `20`，所以基本可以把左移看成以下公式 `a * (2 ^ b)`

**算数右移 >>**

```text
10 >> 1 // -> 5
```

- 算数右移就是将二进制全部往右移动并去除多余的右边，10 在二进制中表示为 `1010` ，右移一位后变成 `101` ，转换为十进制也就是 `5`，所以基本可以把右移看成以下公式 `int v = a / (2 ^ b)`
- 右移很好用，比如可以用在二分算法中取中间值

```text
13 >> 1 // -> 6
```

**按位操作**

- **按位与**

> 每一位都为 1，结果才为 1

```text
8 & 7 // -> 0
// 1000 & 0111 -> 0000 -> 0
```

- **按位或**

> 其中一位为 1，结果就是 1

```text
8 | 7 // -> 15
// 1000 | 0111 -> 1111 -> 15
```

- **按位异或**

> 每一位都不同，结果才为 1

```text
8 ^ 7 // -> 15
8 ^ 8 // -> 0
// 1000 ^ 0111 -> 1111 -> 15
// 1000 ^ 1000 -> 0000 -> 0
```

**两个数不使用四则运算得出和**

> 这道题中可以按位异或，因为按位异或就是不进位加法，`8 ^ 8 = 0` 如果进位了，就是 `16` 了，所以我们只需要将两个数进行异或操作，然后进位。那么也就是说两个二进制都是 1 的位置，左边应该有一个进位 `1`，所以可以得出以下公式 `a + b = (a ^ b) + ((a & b) << 1)` ，然后通过迭代的方式模拟加法

```js
function sum(a, b) {
    if (a == 0) return b
    if (b == 0) return a
    let newA = a ^ b
    let newB = (a & b) << 1
    return sum(newA, newB)
}
```

#### 排序

**冒泡排序**

> 冒泡排序的如下，从第一个元素开始，把当前元素和下一个索引元素进行比较。如果当前元素大，那么就交换位置，重复操作直到比较到最后一个元素，那么此时最后一个元素就是该数组中最大的数。下一轮重复以上操作，但是此时最后一个元素已经是最大数了，所以不需要再比较最后一个元素，只需要比较到 `length - 1` 的位置

![img](https://user-gold-cdn.xitu.io/2018/4/12/162b895b452b306c?w=670&h=508&f=gif&s=282307)

> 以下是实现该算法的代码

```js
function bubble(array) {
  checkArray(array);
  for (let i = array.length - 1; i > 0; i--) {
    // 从 0 到 `length - 1` 遍历
    for (let j = 0; j < i; j++) {
      if (array[j] > array[j + 1]) swap(array, j, j + 1)
    }
  }
  return array;
}
```

> 该算法的操作次数是一个等差数列 `n + (n - 1) + (n - 2) + 1` ，去掉常数项以后得出时间复杂度是`O(n * n)`

**插入排序**

> 入排序的如下。第一个元素默认是已排序元素，取出下一个元素和当前元素比较，如果当前元素大就交换位置。那么此时第一个元素就是当前的最小数，所以下次取出操作从第三个元素开始，向前对比，重复之前的操作

![img](https://user-gold-cdn.xitu.io/2018/4/12/162b895c7e59dcd1?w=670&h=508&f=gif&s=609549)

以下是实现该算法的代码

```js
function insertion(array) {
  checkArray(array);
  for (let i = 1; i < array.length; i++) {
    for (let j = i - 1; j >= 0 && array[j] > array[j + 1]; j--)
      swap(array, j, j + 1);
  }
  return array;
}
```

> 该算法的操作次数是一个等差数列 `n + (n - 1) + (n - 2) + 1` ，去掉常数项以后得出时间复杂度是 `O(n * n)`

**选择排序**

> 选择排序的如下。遍历数组，设置最小值的索引为 0，如果取出的值比当前最小值小，就替换最小值索引，遍历完成后，将第一个元素和最小值索引上的值交换。如上操作后，第一个元素就是数组中的最小值，下次遍历就可以从索引 1 开始重复上述操作

![img](https://user-gold-cdn.xitu.io/2018/4/13/162bc8ea14567e2e?w=670&h=508&f=gif&s=965636)

以下是实现该算法的代码

```js
function selection(array) {
  checkArray(array);
  for (let i = 0; i < array.length - 1; i++) {
    let minIndex = i;
    for (let j = i + 1; j < array.length; j++) {
      minIndex = array[j] < array[minIndex] ? j : minIndex;
    }
    swap(array, i, minIndex);
  }
  return array;
}
```

> 该算法的操作次数是一个等差数列 `n + (n - 1) + (n - 2) + 1` ，去掉常数项以后得出时间复杂度是 `O(n * n)`

**归并排序**

> 归并排序的如下。递归的将数组两两分开直到最多包含两个元素，然后将数组排序合并，最终合并为排序好的数组。假设我有一组数组 `[3, 1, 2, 8, 9, 7, 6]`，中间数索引是 3，先排序数组 `[3, 1, 2, 8]` 。在这个左边数组上，继续拆分直到变成数组包含两个元素（如果数组长度是奇数的话，会有一个拆分数组只包含一个元素）。然后排序数组 `[3, 1]` 和 `[2, 8]` ，然后再排序数组 `[1, 3, 2, 8]` ，这样左边数组就排序完成，然后按照以上思路排序右边数组，最后将数组 `[1, 2, 3, 8]` 和 `[6, 7, 9]` 排序

![img](https://user-gold-cdn.xitu.io/2018/4/13/162be13c7e30bd86?w=896&h=1008&f=gif&s=937952)

以下是实现该算法的代码

```js
function sort(array) {
  checkArray(array);
  mergeSort(array, 0, array.length - 1);
  return array;
}

function mergeSort(array, left, right) {
  // 左右索引相同说明已经只有一个数
  if (left === right) return;
  // 等同于 `left + (right - left) / 2`
  // 相比 `(left + right) / 2` 来说更加安全，不会溢出
  // 使用位运算是因为位运算比四则运算快
  let mid = parseInt(left + ((right - left) >> 1));
  mergeSort(array, left, mid);
  mergeSort(array, mid + 1, right);

  let help = [];
  let i = 0;
  let p1 = left;
  let p2 = mid + 1;
  while (p1 <= mid && p2 <= right) {
    help[i++] = array[p1] < array[p2] ? array[p1++] : array[p2++];
  }
  while (p1 <= mid) {
    help[i++] = array[p1++];
  }
  while (p2 <= right) {
    help[i++] = array[p2++];
  }
  for (let i = 0; i < help.length; i++) {
    array[left + i] = help[i];
  }
  return array;
}
```

> 以上算法使用了递归的思想。递归的本质就是压栈，每递归执行一次函数，就将该函数的信息（比如参数，内部的变量，执行到的行数）压栈，直到遇到终止条件，然后出栈并继续执行函数。对于以上递归函数的调用轨迹如下

```js
mergeSort(data, 0, 6) // mid = 3
  mergeSort(data, 0, 3) // mid = 1
    mergeSort(data, 0, 1) // mid = 0
      mergeSort(data, 0, 0) // 遇到终止，回退到上一步
    mergeSort(data, 1, 1) // 遇到终止，回退到上一步
    // 排序 p1 = 0, p2 = mid + 1 = 1
    // 回退到 `mergeSort(data, 0, 3)` 执行下一个递归
  mergeSort(2, 3) // mid = 2
    mergeSort(3, 3) // 遇到终止，回退到上一步
  // 排序 p1 = 2, p2 = mid + 1 = 3
  // 回退到 `mergeSort(data, 0, 3)` 执行合并逻辑
  // 排序 p1 = 0, p2 = mid + 1 = 2
  // 执行完毕回退
  // 左边数组排序完毕，右边也是如上轨迹
```

> 该算法的操作次数是可以这样计算：递归了两次，每次数据量是数组的一半，并且最后把整个数组迭代了一次，所以得出表达式 `2T(N / 2) + T(N)` （`T` 代表时间，`N` 代表数据量）。根据该表达式可以套用 该公式 得出时间复杂度为 `O(N * logN)`

**快排**

> 快排的如下。随机选取一个数组中的值作为基准值，从左至右取值与基准值对比大小。比基准值小的放数组左边，大的放右边，对比完成后将基准值和第一个比基准值大的值交换位置。然后将数组以基准值的位置分为两部分，继续递归以上操作。

![img](https://user-gold-cdn.xitu.io/2018/4/16/162cd23e69ca9ea3?w=824&h=506&f=gif&s=867744)

以下是实现该算法的代码

```js
function sort(array) {
  checkArray(array);
  quickSort(array, 0, array.length - 1);
  return array;
}

function quickSort(array, left, right) {
  if (left < right) {
    swap(array, , right)
    // 随机取值，然后和末尾交换，这样做比固定取一个位置的复杂度略低
    let indexs = part(array, parseInt(Math.random() * (right - left + 1)) + left, right);
    quickSort(array, left, indexs[0]);
    quickSort(array, indexs[1] + 1, right);
  }
}
function part(array, left, right) {
  let less = left - 1;
  let more = right;
  while (left < more) {
    if (array[left] < array[right]) {
      // 当前值比基准值小，`less` 和 `left` 都加一
	   ++less;
       ++left;
    } else if (array[left] > array[right]) {
      // 当前值比基准值大，将当前值和右边的值交换
      // 并且不改变 `left`，因为当前换过来的值还没有判断过大小
      swap(array, --more, left);
    } else {
      // 和基准值相同，只移动下标
      left++;
    }
  }
  // 将基准值和比基准值大的第一个值交换位置
  // 这样数组就变成 `[比基准值小, 基准值, 比基准值大]`
  swap(array, right, more);
  return [less, more];
}
```

> 该算法的复杂度和归并排序是相同的，但是额外空间复杂度比归并排序少，只需 `O(logN)`，并且相比归并排序来说，所需的常数时间也更少

**面试题**

> Sort Colors：该题目来自 LeetCode，题目需要我们将 `[2,0,2,1,1,0]` 排序成 `[0,0,1,1,2,2]`，这个问题就可以使用三路快排的思想

```js
var sortColors = function(nums) {
  let left = -1;
  let right = nums.length;
  let i = 0;
  // 下标如果遇到 right，说明已经排序完成
  while (i < right) {
    if (nums[i] == 0) {
      swap(nums, i++, ++left);
    } else if (nums[i] == 1) {
      i++;
    } else {
      swap(nums, i, --right);
    }
  }
};
```

#### 链表

**反转单向链表**

> 该题目来自 LeetCode，题目需要将一个单向链表反转。思路很简单，使用三个变量分别表示当前节点和当前节点的前后节点，虽然这题很简单，但是却是一道面试常考题

```js
var reverseList = function(head) {
    // 判断下变量边界问题
    if (!head || !head.next) return head
    // 初始设置为空，因为第一个节点反转后就是尾部，尾部节点指向 null
    let pre = null
    let current = head
    let next
    // 判断当前节点是否为空
    // 不为空就先获取当前节点的下一节点
    // 然后把当前节点的 next 设为上一个节点
    // 然后把 current 设为下一个节点，pre 设为当前节点
    while(current) {
        next = current.next
        current.next = pre
        pre = current
        current = next
    }
    return pre
};
```

#### 树

**二叉树的先序，中序，后序遍历**

- 先序遍历表示先访问根节点，然后访问左节点，最后访问右节点。
- 中序遍历表示先访问左节点，然后访问根节点，最后访问右节点。
- 后序遍历表示先访问左节点，然后访问右节点，最后访问根节点

**递归实现**

递归实现相当简单，代码如下

```js
function TreeNode(val) {
  this.val = val;
  this.left = this.right = null;
}
var traversal = function(root) {
  if (root) {
    // 先序
    console.log(root);
    traversal(root.left);
    // 中序
    // console.log(root);
    traversal(root.right);
    // 后序
    // console.log(root);
  }
};
```

> 对于递归的实现来说，只需要理解每个节点都会被访问三次就明白为什么这样实现了

**非递归实现**

> 非递归实现使用了栈的结构，通过栈的先进后出模拟递归实现。

以下是先序遍历代码实现

```js
function pre(root) {
  if (root) {
    let stack = [];
    // 先将根节点 push
    stack.push(root);
    // 判断栈中是否为空
    while (stack.length > 0) {
      // 弹出栈顶元素
      root = stack.pop();
      console.log(root);
      // 因为先序遍历是先左后右，栈是先进后出结构
      // 所以先 push 右边再 push 左边
      if (root.right) {
        stack.push(root.right);
      }
      if (root.left) {
        stack.push(root.left);
      }
    }
  }
}
```

以下是中序遍历代码实现

```js
function mid(root) {
  if (root) {
    let stack = [];
    // 中序遍历是先左再根最后右
    // 所以首先应该先把最左边节点遍历到底依次 push 进栈
    // 当左边没有节点时，就打印栈顶元素，然后寻找右节点
    // 对于最左边的叶节点来说，可以把它看成是两个 null 节点的父节点
    // 左边打印不出东西就把父节点拿出来打印，然后再看右节点
    while (stack.length > 0 || root) {
      if (root) {
        stack.push(root);
        root = root.left;
      } else {
        root = stack.pop();
        console.log(root);
        root = root.right;
      }
    }
  }
}
```

> 以下是后序遍历代码实现，该代码使用了两个栈来实现遍历，相比一个栈的遍历来说要容易理解很多

```js
function pos(root) {
  if (root) {
    let stack1 = [];
    let stack2 = [];
    // 后序遍历是先左再右最后根
	// 所以对于一个栈来说，应该先 push 根节点
    // 然后 push 右节点，最后 push 左节点
    stack1.push(root);
    while (stack1.length > 0) {
      root = stack1.pop();
      stack2.push(root);
      if (root.left) {
        stack1.push(root.left);
      }
      if (root.right) {
        stack1.push(root.right);
      }
    }
    while (stack2.length > 0) {
      console.log(s2.pop());
    }
  }
}
```

**中序遍历的前驱后继节点**

> 实现这个算法的前提是节点有一个 `parent` 的指针指向父节点，根节点指向 `null`

![img](https://user-gold-cdn.xitu.io/2018/4/24/162f61ad8e8588b7?w=682&h=486&f=png&s=41027)

> 如图所示，该树的中序遍历结果是 `4, 2, 5, 1, 6, 3, 7`

**前驱节点**

> 对于节点 2 来说，他的前驱节点就是 4 ，按照中序遍历原则，可以得出以下结论

- 如果选取的节点的左节点不为空，就找该左节点最右的节点。对于节点 1 来说，他有左节点 2 ，那么节点 2 的最右节点就是 5
- 如果左节点为空，且目标节点是父节点的右节点，那么前驱节点为父节点。对于节点 5 来说，没有左节点，且是节点 2 的右节点，所以节点 2 是前驱节点
- 如果左节点为空，且目标节点是父节点的左节点，向上寻找到第一个是父节点的右节点的节点。对于节点 6 来说，没有左节点，且是节点 3 的左节点，所以向上寻找到节点 1 ，发现节点 3 是节点 1 的右节点，所以节点 1 是节点 6 的前驱节点

以下是算法实现

```js
function predecessor(node) {
  if (!node) return
  // 结论 1
  if (node.left) {
    return getRight(node.left)
  } else {
    let parent = node.parent
    // 结论 2 3 的判断
    while(parent && parent.right === node) {
      node = parent
      parent = node.parent
    }
    return parent
  }
}
function getRight(node) {
  if (!node) return
  node = node.right
  while(node) node = node.right
  return node
}
```

**后继节点**

> 对于节点 2 来说，他的后继节点就是 5 ，按照中序遍历原则，可以得出以下结论

- 如果有右节点，就找到该右节点的最左节点。对于节点 1 来说，他有右节点 3 ，那么节点 3 的最左节点就是 6
- 如果没有右节点，就向上遍历直到找到一个节点是父节点的左节点。对于节点 5 来说，没有右节点，就向上寻找到节点 2 ，该节点是父节点 1 的左节点，所以节点 1 是后继节点 以下是算法实现

```js
function successor(node) {
  if (!node) return
  // 结论 1
  if (node.right) {
    return getLeft(node.right)
  } else {
    // 结论 2
    let parent = node.parent
    // 判断 parent 为空
    while(parent && parent.left === node) {
      node = parent
      parent = node.parent
    }
    return parent
  }
}
function getLeft(node) {
  if (!node) return
  node = node.left
  while(node) node = node.left
  return node
}
```

**树的深度**

> 树的最大深度：该题目来自 Leetcode，题目需要求出一颗二叉树的最大深度

以下是算法实现

```js
var maxDepth = function(root) {
    if (!root) return 0
    return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1
};
```

> 对于该递归函数可以这样理解：一旦没有找到节点就会返回 0，每弹出一次递归函数就会加一，树有三层就会得到3

------



### JavaScript

#### 变量提升

> 当执行 `JS` 代码时，会生成执行环境，只要代码不是写在函数中的，就是在全局执行环境中，函数中的代码会产生函数执行环境，只此两种执行环境。

```js
b() // call b
console.log(a) // undefined

var a = 'Hello world'

function b() {
    console.log('call b')
}
```

> 想必以上的输出大家肯定都已经明白了，这是因为函数和变量提升的原因。通常提升的解释是说将声明的代码移动到了顶部，这其实没有什么错误，便于大家理解。但是更准确的解释应该是：在生成执行环境时，会有两个阶段。第一个阶段是创建的阶段，`JS` 解释器会找出需要提升的变量和函数，并且给他们提前在内存中开辟好空间，函数的话会将整个函数存入内存中，变量只声明并且赋值为 `undefined`，所以在第二个阶段，也就是代码执行阶段，我们可以直接提前使用

- 在提升的过程中，相同的函数会覆盖上一个函数，并且函数优先于变量提升

```js
b() // call b second

function b() {
    console.log('call b fist')
}
function b() {
    console.log('call b second')
}
var b = 'Hello world'
```

> `var` 会产生很多错误，所以在 ES6中引入了 `let`。`let`不能在声明前使用，但是这并不是常说的 `let` 不会提升，`let`提升了，在第一阶段内存也已经为他开辟好了空间，但是因为这个声明的特性导致了并不能在声明前使用

#### bind、call、apply 区别

- `call` 和 `apply` 都是为了解决改变 `this` 的指向。作用都是相同的，只是传参的方式不同。
- 除了第一个参数外，`call` 可以接收一个参数列表，`apply` 只接受一个参数数组

```js
let a = {
    value: 1
}
function getValue(name, age) {
    console.log(name)
    console.log(age)
    console.log(this.value)
}
getValue.call(a, 'yck', '24')
getValue.apply(a, ['yck', '24'])
```

> `bind` 和其他两个方法作用也是一致的，只是该方法会返回一个函数。并且我们可以通过 `bind` 实现柯里化

#### 实现一个 bind 函数

对于实现以下几个函数，可以从几个方面思考

- 不传入第一个参数，那么默认为 `window`
- 改变了 `this` 指向，让新的对象可以执行该函数。那么思路是否可以变成给新的对象添加一个函数，然后在执行完以后删除？

```js
Function.prototype.myBind = function (context) {
  if (typeof this !== 'function') {
    throw new TypeError('Error')
  }
  var _this = this
  var args = [...arguments].slice(1)
  // 返回一个函数
  return function F() {
    // 因为返回了一个函数，我们可以 new F()，所以需要判断
    if (this instanceof F) {
      return new _this(...args, ...arguments)
    }
    return _this.apply(context, args.concat(...arguments))
  }
}
```

#### 实现一个 call 函数

```js
Function.prototype.myCall = function (context) {
  var context = context || window
  // 给 context 添加一个属性
  // getValue.call(a, 'yck', '24') => a.fn = getValue
  context.fn = this
  // 将 context 后面的参数取出来
  var args = [...arguments].slice(1)
  // getValue.call(a, 'yck', '24') => a.fn('yck', '24')
  var result = context.fn(...args)
  // 删除 fn
  delete context.fn
  return result
}
```

#### 实现一个 apply 函数

```js
Function.prototype.myApply = function (context) {
  var context = context || window
  context.fn = this

  var result
  // 需要判断是否存储第二个参数
  // 如果存在，就将第二个参数展开
  if (arguments[1]) {
    result = context.fn(...arguments[1])
  } else {
    result = context.fn()
  }

  delete context.fn
  return result
}
```

#### 简单说下原型链？

![img](https://user-gold-cdn.xitu.io/2018/9/19/165f189f736f19fd?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

- 每个函数都有 `prototype` 属性，除了 `Function.prototype.bind()`，该属性指向原型。
- 每个对象都有 `__proto__` 属性，指向了创建该对象的构造函数的原型。其实这个属性指向了 `[[prototype]]`，但是 `[[prototype]]`是内部属性，我们并不能访问到，所以使用 `_proto_`来访问。
- 对象可以通过 `__proto__` 来寻找不属于该对象的属性，`__proto__` 将对象连接起来组成了原型链。

#### 怎么判断对象类型

- 可以通过 `Object.prototype.toString.call(xx)`。这样我们就可以获得类似 `[object Type]` 的字符串。
- `instanceof` 可以正确的判断对象的类型，因为内部机制是通过判断对象的原型链中是不是能找到类型的 `prototype`

#### 箭头函数的特点

```js
function a() {
    return () => {
        return () => {
        	console.log(this)
        }
    }
}
console.log(a()()())
```

> 箭头函数其实是没有 `this` 的，这个函数中的 `this` 只取决于他外面的第一个不是箭头函数的函数的 `this`。在这个例子中，因为调用 `a` 符合前面代码中的第一个情况，所以 `this` 是`window`。并且 `this`一旦绑定了上下文，就不会被任何代码改变

#### This

```js
function foo() {
	console.log(this.a)
}
var a = 1
foo()

var obj = {
	a: 2,
	foo: foo
}
obj.foo()

// 以上两者情况 `this` 只依赖于调用函数前的对象，优先级是第二个情况大于第一个情况

// 以下情况是优先级最高的，`this` 只会绑定在 `c` 上，不会被任何方式修改 `this` 指向
var c = new foo()
c.a = 3
console.log(c.a)

// 还有种就是利用 call，apply，bind 改变 this，这个优先级仅次于 new
```

#### async、await 优缺点

> `async` 和 `await` 相比直接使用 `Promise` 来说，优势在于处理 then 的调用链，能够更清晰准确的写出代码。缺点在于滥用 `await` 可能会导致性能问题，因为 `await` 会阻塞代码，也许之后的异步代码并不依赖于前者，但仍然需要等待前者完成，导致代码失去了并发性

下面来看一个使用 `await` 的代码。

```js
var a = 0
var b = async () => {
  a = a + await 10
  console.log('2', a) // -> '2' 10
  a = (await 10) + a
  console.log('3', a) // -> '3' 20
}
b()
a++
console.log('1', a) // -> '1' 1
```

- 首先函数`b` 先执行，在执行到 `await 10` 之前变量 `a` 还是 `0`，因为在 `await` 内部实现了 `generators` ，`generators` 会保留堆栈中东西，所以这时候 `a = 0` 被保存了下来
- 因为 `await` 是异步操作，遇到`await`就会立即返回一个`pending`状态的`Promise`对象，暂时返回执行代码的控制权，使得函数外的代码得以继续执行，所以会先执行 `console.log('1', a)`
- 这时候同步代码执行完毕，开始执行异步代码，将保存下来的值拿出来使用，这时候 `a = 10`
- 然后后面就是常规执行代码了

#### generator 原理

> `Generator` 是 `ES6`中新增的语法，和 `Promise` 一样，都可以用来异步编程

```js
// 使用 * 表示这是一个 Generator 函数
// 内部可以通过 yield 暂停代码
// 通过调用 next 恢复执行
function* test() {
  let a = 1 + 2;
  yield 2;
  yield 3;
}
let b = test();
console.log(b.next()); // >  { value: 2, done: false }
console.log(b.next()); // >  { value: 3, done: false }
console.log(b.next()); // >  { value: undefined, done: true }
```

> 从以上代码可以发现，加上 `*`的函数执行后拥有了 `next` 函数，也就是说函数执行后返回了一个对象。每次调用 `next` 函数可以继续执行被暂停的代码。以下是 `Generator` 函数的简单实现

```js
// cb 也就是编译过的 test 函数
function generator(cb) {
  return (function() {
    var object = {
      next: 0,
      stop: function() {}
    };

    return {
      next: function() {
        var ret = cb(object);
        if (ret === undefined) return { value: undefined, done: true };
        return {
          value: ret,
          done: false
        };
      }
    };
  })();
}
// 如果你使用 babel 编译后可以发现 test 函数变成了这样
function test() {
  var a;
  return generator(function(_context) {
    while (1) {
      switch ((_context.prev = _context.next)) {
        // 可以发现通过 yield 将代码分割成几块
        // 每次执行 next 函数就执行一块代码
        // 并且表明下次需要执行哪块代码
        case 0:
          a = 1 + 2;
          _context.next = 4;
          return 2;
        case 4:
          _context.next = 6;
          return 3;
		// 执行完毕
        case 6:
        case "end":
          return _context.stop();
      }
    }
  });
}
```

#### Promise

- `Promise` 是 `ES6` 新增的语法，解决了回调地狱的问题。
- 可以把 `Promise`看成一个状态机。初始是 `pending` 状态，可以通过函数 `resolve` 和 `reject`，将状态转变为 `resolved` 或者 `rejected` 状态，状态一旦改变就不能再次变化。
- `then` 函数会返回一个 `Promise` 实例，并且该返回值是一个新的实例而不是之前的实例。因为 `Promise` 规范规定除了 `pending` 状态，其他状态是不可以改变的，如果返回的是一个相同实例的话，多个 `then` 调用就失去意义了。 对于 `then` 来说，本质上可以把它看成是 `flatMap`

#### 实现一个 Promise

```js
// 三种状态
const PENDING = "pending";
const RESOLVED = "resolved";
const REJECTED = "rejected";
// promise 接收一个函数参数，该函数会立即执行
function MyPromise(fn) {
  let _this = this;
  _this.currentState = PENDING;
  _this.value = undefined;
  // 用于保存 then 中的回调，只有当 promise
  // 状态为 pending 时才会缓存，并且每个实例至多缓存一个
  _this.resolvedCallbacks = [];
  _this.rejectedCallbacks = [];

  _this.resolve = function (value) {
    if (value instanceof MyPromise) {
      // 如果 value 是个 Promise，递归执行
      return value.then(_this.resolve, _this.reject)
    }
    setTimeout(() => { // 异步执行，保证执行顺序
      if (_this.currentState === PENDING) {
        _this.currentState = RESOLVED;
        _this.value = value;
        _this.resolvedCallbacks.forEach(cb => cb());
      }
    })
  };

  _this.reject = function (reason) {
    setTimeout(() => { // 异步执行，保证执行顺序
      if (_this.currentState === PENDING) {
        _this.currentState = REJECTED;
        _this.value = reason;
        _this.rejectedCallbacks.forEach(cb => cb());
      }
    })
  }
  // 用于解决以下问题
  // new Promise(() => throw Error('error))
  try {
    fn(_this.resolve, _this.reject);
  } catch (e) {
    _this.reject(e);
  }
}

MyPromise.prototype.then = function (onResolved, onRejected) {
  var self = this;
  // 规范 2.2.7，then 必须返回一个新的 promise
  var promise2;
  // 规范 2.2.onResolved 和 onRejected 都为可选参数
  // 如果类型不是函数需要忽略，同时也实现了透传
  // Promise.resolve(4).then().then((value) => console.log(value))
  onResolved = typeof onResolved === 'function' ? onResolved : v => v;
  onRejected = typeof onRejected === 'function' ? onRejected : r => throw r;

  if (self.currentState === RESOLVED) {
    return (promise2 = new MyPromise(function (resolve, reject) {
      // 规范 2.2.4，保证 onFulfilled，onRjected 异步执行
      // 所以用了 setTimeout 包裹下
      setTimeout(function () {
        try {
          var x = onResolved(self.value);
          resolutionProcedure(promise2, x, resolve, reject);
        } catch (reason) {
          reject(reason);
        }
      });
    }));
  }

  if (self.currentState === REJECTED) {
    return (promise2 = new MyPromise(function (resolve, reject) {
      setTimeout(function () {
        // 异步执行onRejected
        try {
          var x = onRejected(self.value);
          resolutionProcedure(promise2, x, resolve, reject);
        } catch (reason) {
          reject(reason);
        }
      });
    }));
  }

  if (self.currentState === PENDING) {
    return (promise2 = new MyPromise(function (resolve, reject) {
      self.resolvedCallbacks.push(function () {
        // 考虑到可能会有报错，所以使用 try/catch 包裹
        try {
          var x = onResolved(self.value);
          resolutionProcedure(promise2, x, resolve, reject);
        } catch (r) {
          reject(r);
        }
      });

      self.rejectedCallbacks.push(function () {
        try {
          var x = onRejected(self.value);
          resolutionProcedure(promise2, x, resolve, reject);
        } catch (r) {
          reject(r);
        }
      });
    }));
  }
};
// 规范 2.3
function resolutionProcedure(promise2, x, resolve, reject) {
  // 规范 2.3.1，x 不能和 promise2 相同，避免循环引用
  if (promise2 === x) {
    return reject(new TypeError("Error"));
  }
  // 规范 2.3.2
  // 如果 x 为 Promise，状态为 pending 需要继续等待否则执行
  if (x instanceof MyPromise) {
    if (x.currentState === PENDING) {
      x.then(function (value) {
        // 再次调用该函数是为了确认 x resolve 的
        // 参数是什么类型，如果是基本类型就再次 resolve
        // 把值传给下个 then
        resolutionProcedure(promise2, value, resolve, reject);
      }, reject);
    } else {
      x.then(resolve, reject);
    }
    return;
  }
  // 规范 2.3.3.3.3
  // reject 或者 resolve 其中一个执行过得话，忽略其他的
  let called = false;
  // 规范 2.3.3，判断 x 是否为对象或者函数
  if (x !== null && (typeof x === "object" || typeof x === "function")) {
    // 规范 2.3.3.2，如果不能取出 then，就 reject
    try {
      // 规范 2.3.3.1
      let then = x.then;
      // 如果 then 是函数，调用 x.then
      if (typeof then === "function") {
        // 规范 2.3.3.3
        then.call(
          x,
          y => {
            if (called) return;
            called = true;
            // 规范 2.3.3.3.1
            resolutionProcedure(promise2, y, resolve, reject);
          },
          e => {
            if (called) return;
            called = true;
            reject(e);
          }
        );
      } else {
        // 规范 2.3.3.4
        resolve(x);
      }
    } catch (e) {
      if (called) return;
      called = true;
      reject(e);
    }
  } else {
    // 规范 2.3.4，x 为基本类型
    resolve(x);
  }
}
```

#### == 和 ===区别，什么情况用 ==

![img](https://user-gold-cdn.xitu.io/2018/3/30/16275f89ebf931e9?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

> 这里来解析一道题目 `[] == ![] // -> true` ，下面是这个表达式为何为 `true` 的步骤

```js
// [] 转成 true，然后取反变成 false
[] == false
// 根据第 8 条得出
[] == ToNumber(false)
[] == 0
// 根据第 10 条得出
ToPrimitive([]) == 0
// [].toString() -> ''
'' == 0
// 根据第 6 条得出
0 == 0 // -> true
```

> `===`用于判断两者类型和值是否相同。 在开发中，对于后端返回的 `code`，可以通过 `==`去判断

#### 基本数据类型和引⽤类型在存储上的差别

> 前者存储在栈上，后者存储在堆上

> 

#### setTimeout 倒计时误差

> `JS` 是单线程的，所以 `setTimeout` 的误差其实是无法被完全解决的，原因有很多，可能是回调中的，有可能是浏览器中的各种事件导致。这也是为什么页面开久了，定时器会不准的原因，当然我们可以通过一定的办法去减少这个误差。

```js
// 以下是一个相对准备的倒计时实现
var period = 60 * 1000 * 60 * 2
var startTime = new Date().getTime();
var count = 0
var end = new Date().getTime() + period
var interval = 1000
var currentInterval = interval

function loop() {
  count++
  var offset = new Date().getTime() - (startTime + count * interval); // 代码执行所消耗的时间
  var diff = end - new Date().getTime()
  var h = Math.floor(diff / (60 * 1000 * 60))
  var hdiff = diff % (60 * 1000 * 60)
  var m = Math.floor(hdiff / (60 * 1000))
  var mdiff = hdiff % (60 * 1000)
  var s = mdiff / (1000)
  var sCeil = Math.ceil(s)
  var sFloor = Math.floor(s)
  currentInterval = interval - offset // 得到下一次循环所消耗的时间
  console.log('时：'+h, '分：'+m, '毫秒：'+s, '秒向上取整：'+sCeil, '代码执行时间：'+offset, '下次循环间隔'+currentInterval) // 打印 时 分 秒 代码执行时间 下次循环间隔

  setTimeout(loop, currentInterval)
}

setTimeout(loop, currentInterval)
```

#### 数组降维

```js
[1, [2], 3].flatMap(v => v)
// -> [1, 2, 3]
```

> 如果想将一个多维数组彻底的降维，可以这样实现

```js
const flattenDeep = (arr) => Array.isArray(arr)
  ? arr.reduce( (a, b) => [...a, ...flattenDeep(b)] , [])
  : [arr]

flattenDeep([1, [[2], [3, [4]], 5]])
```

#### 深拷贝

> 这个问题通常可以通过 `JSON.parse(JSON.stringify(object))` 来解决

```js
let a = {
    age: 1,
    jobs: {
        first: 'FE'
    }
}
let b = JSON.parse(JSON.stringify(a))
a.jobs.first = 'native'
console.log(b.jobs.first) // FE
```

**但是该方法也是有局限性的**：

- 会忽略 `undefined`
- 会忽略 `symbol`
- 不能序列化函数
- 不能解决循环引用的对象

```js
let obj = {
  a: 1,
  b: {
    c: 2,
    d: 3,
  },
}
obj.c = obj.b
obj.e = obj.a
obj.b.c = obj.c
obj.b.d = obj.b
obj.b.e = obj.b.c
let newObj = JSON.parse(JSON.stringify(obj))
console.log(newObj)
复
```

> 在遇到函数、 `undefined` 或者 `symbol` 的时候，该对象也不能正常的序列化

```js
let a = {
    age: undefined,
    sex: Symbol('male'),
    jobs: function() {},
    name: 'yck'
}
let b = JSON.parse(JSON.stringify(a))
console.log(b) // {name: "yck"}
```

> 但是在通常情况下，复杂数据都是可以序列化的，所以这个函数可以解决大部分问题，并且该函数是内置函数中处理深拷贝性能最快的。当然如果你的数据中含有以上三种情况下，可以使用 `lodash` 的深拷贝函数

#### typeof 于 instanceof 区别

> `typeof` 对于基本类型，除了 `null`都可以显示正确的类型

```js
typeof 1 // 'number'
typeof '1' // 'string'
typeof undefined // 'undefined'
typeof true // 'boolean'
typeof Symbol() // 'symbol'
typeof b // b 没有声明，但是还会显示 undefined
```

> ```
> typeof` 对于对象，除了函数都会显示 `object
> ```

```js
typeof [] // 'object'
typeof {} // 'object'
typeof console.log // 'function'
```

> 对于 `null` 来说，虽然它是基本类型，但是会显示 `object`，这是一个存在很久了的 `Bug`

```text
typeof null // 'object'
```

> ```
> instanceof` 可以正确的判断对象的类型，因为内部机制是通过判断对象的原型链中是不是能找到类型的 `prototype
> ```

```js
我们也可以试着实现一下 instanceof
function instanceof(left, right) {
    // 获得类型的原型
    let prototype = right.prototype
    // 获得对象的原型
    left = left.__proto__
    // 判断对象的类型是否等于类型的原型
    while (true) {
    	if (left === null)
    		return false
    	if (prototype === left)
    		return true
    	left = left.__proto__
    }
}
```

#### AMD，CMD，CommonJs，ES6 Module

- **AMD**：`requirejs` 在推广过程中对模块定义的规范化产出，提前执行，推崇依赖前置
- **CMD**：`seajs` 在推广过程中对模块定义的规范化产出，延迟执行，推崇依赖就近
- **CommonJs**：模块输出的是一个值的 `copy`，运行时加载，加载的是一个对象（`module.exports` 属性），该对象只有在脚本运行完才会生成
- **ES6 Module**：模块输出的是一个值的引用，编译时输出接口，`ES6`模块不是对象，它对外接口只是一种静态定义，在代码静态解析阶段就会生成。

#### 防抖

> 你是否在日常开发中遇到一个问题，在滚动事件中需要做个复杂计算或者实现一个按钮的防二次点击操作。

- 这些需求都可以通过函数防抖动来实现。尤其是第一个需求，如果在频繁的事件回调中做复杂计算，很有可能导致页面卡顿，不如将多次计算合并为一次计算，只在一个精确点做操作
- PS：防抖和节流的作用都是防止函数多次调用。区别在于，假设一个用户一直触发这个函数，且每次触发函数的间隔小于`wait`，防抖的情况下只会调用一次，而节流的 情况会每隔一定时间（参数`wait`）调用函数

```js
// 这个是用来获取当前时间戳的
function now() {
  return +new Date()
}
/**
 * 防抖函数，返回函数连续调用时，空闲时间必须大于或等于 wait，func 才会执行
 *
 * @param  {function} func        回调函数
 * @param  {number}   wait        表示时间窗口的间隔
 * @param  {boolean}  immediate   设置为ture时，是否立即调用函数
 * @return {function}             返回客户调用函数
 */
function debounce (func, wait = 50, immediate = true) {
  let timer, context, args

  // 延迟执行函数
  const later = () => setTimeout(() => {
    // 延迟函数执行完毕，清空缓存的定时器序号
    timer = null
    // 延迟执行的情况下，函数会在延迟函数中执行
    // 使用到之前缓存的参数和上下文
    if (!immediate) {
      func.apply(context, args)
      context = args = null
    }
  }, wait)

  // 这里返回的函数是每次实际调用的函数
  return function(...params) {
    // 如果没有创建延迟执行函数（later），就创建一个
    if (!timer) {
      timer = later()
      // 如果是立即执行，调用函数
      // 否则缓存参数和调用上下文
      if (immediate) {
        func.apply(this, params)
      } else {
        context = this
        args = params
      }
    // 如果已有延迟执行函数（later），调用的时候清除原来的并重新设定一个
    // 这样做延迟函数会重新计时
    } else {
      clearTimeout(timer)
      timer = later()
    }
  }
}
```

- 对于按钮防点击来说的实现：如果函数是立即执行的，就立即调用，如果函数是延迟执行的，就缓存上下文和参数，放到延迟函数中去执行。一旦我开始一个定时器，只要我定时器还在，你每次点击我都重新计时。一旦你点累了，定时器时间到，定时器重置为 `null`，就可以再次点击了。
- 对于延时执行函数来说的实现：清除定时器`ID`，如果是延迟调用就调用函数

#### 节流

> 防抖动和节流本质是不一样的。防抖动是将多次执行变为最后一次执行，节流是将多次执行变成每隔一段时间执行

```js
/**
 * underscore 节流函数，返回函数连续调用时，func 执行频率限定为 次 / wait
 *
 * @param  {function}   func      回调函数
 * @param  {number}     wait      表示时间窗口的间隔
 * @param  {object}     options   如果想忽略开始函数的的调用，传入{leading: false}。
 *                                如果想忽略结尾函数的调用，传入{trailing: false}
 *                                两者不能共存，否则函数不能执行
 * @return {function}             返回客户调用函数   
 */
_.throttle = function(func, wait, options) {
    var context, args, result;
    var timeout = null;
    // 之前的时间戳
    var previous = 0;
    // 如果 options 没传则设为空对象
    if (!options) options = {};
    // 定时器回调函数
    var later = function() {
      // 如果设置了 leading，就将 previous 设为 0
      // 用于下面函数的第一个 if 判断
      previous = options.leading === false ? 0 : _.now();
      // 置空一是为了防止内存泄漏，二是为了下面的定时器判断
      timeout = null;
      result = func.apply(context, args);
      if (!timeout) context = args = null;
    };
    return function() {
      // 获得当前时间戳
      var now = _.now();
      // 首次进入前者肯定为 true
	  // 如果需要第一次不执行函数
	  // 就将上次时间戳设为当前的
      // 这样在接下来计算 remaining 的值时会大于0
      if (!previous && options.leading === false) previous = now;
      // 计算剩余时间
      var remaining = wait - (now - previous);
      context = this;
      args = arguments;
      // 如果当前调用已经大于上次调用时间 + wait
      // 或者用户手动调了时间
 	  // 如果设置了 trailing，只会进入这个条件
	  // 如果没有设置 leading，那么第一次会进入这个条件
	  // 还有一点，你可能会觉得开启了定时器那么应该不会进入这个 if 条件了
	  // 其实还是会进入的，因为定时器的延时
	  // 并不是准确的时间，很可能你设置了2秒
	  // 但是他需要2.2秒才触发，这时候就会进入这个条件
      if (remaining <= 0 || remaining > wait) {
        // 如果存在定时器就清理掉否则会调用二次回调
        if (timeout) {
          clearTimeout(timeout);
          timeout = null;
        }
        previous = now;
        result = func.apply(context, args);
        if (!timeout) context = args = null;
      } else if (!timeout && options.trailing !== false) {
        // 判断是否设置了定时器和 trailing
	    // 没有的话就开启一个定时器
        // 并且不能不能同时设置 leading 和 trailing
        timeout = setTimeout(later, remaining);
      }
      return result;
    };
  };
```

#### 继承

> 在 ES5 中，我们可以使用如下方式解决继承的问题

```js
function Super() {}
Super.prototype.getNumber = function() {
  return 1
}

function Sub() {}
let s = new Sub()
Sub.prototype = Object.create(Super.prototype, {
  constructor: {
    value: Sub,
    enumerable: false,
    writable: true,
    configurable: true
  }
})
```

- 以上继承实现思路就是将子类的原型设置为父类的原型
- 在 `ES6` 中，我们可以通过 `class` 语法轻松解决这个问题

```js
class MyDate extends Date {
  test() {
    return this.getTime()
  }
}
let myDate = new MyDate()
myDate.test()
```

- 但是 `ES6` 不是所有浏览器都兼容，所以我们需要使用 `Babel` 来编译这段代码。
- 如果你使用编译过得代码调用 `myDate.test()`你会惊奇地发现出现了报错

> 因为在 `JS` 底层有限制，如果不是由 `Date`构造出来的实例的话，是不能调用 `Date` 里的函数的。所以这也侧面的说明了：`ES6` 中的 `class` 继承与 `ES5` 中的一般继承写法是不同的。

- 既然底层限制了实例必须由 `Date` 构造出来，那么我们可以改变下思路实现继承

```js
function MyData() {

}
MyData.prototype.test = function () {
  return this.getTime()
}
let d = new Date()
Object.setPrototypeOf(d, MyData.prototype)
Object.setPrototypeOf(MyData.prototype, Date.prototype)
```

- 以上继承实现思路：先创建父类实例 => 改变实例原先的 `_proto__`转而连接到子类的 `prototype`=> 子类的 `prototype` 的 `__proto__` 改为父类的 `prototype`。
- 通过以上方法实现的继承就可以完美解决 `JS` 底层的这个限制

#### 实现一个Generator

> `Generator` 是 `ES6`中新增的语法，和 `Promise` 一样，都可以用来异步编程

```js
// 使用 * 表示这是一个 Generator 函数
// 内部可以通过 yield 暂停代码
// 通过调用 next 恢复执行
function* test() {
  let a = 1 + 2;
  yield 2;
  yield 3;
}
let b = test();
console.log(b.next()); // >  { value: 2, done: false }
console.log(b.next()); // >  { value: 3, done: false }
console.log(b.next()); // >  { value: undefined, done: true }
```

> 从以上代码可以发现，加上 `*` 的函数执行后拥有了 `next`函数，也就是说函数执行后返回了一个对象。每次调用 `next`函数可以继续执行被暂停的代码。以下是 `Generator` 函数的简单实现

```js
// cb 也就是编译过的 test 函数
function generator(cb) {
  return (function() {
    var object = {
      next: 0,
      stop: function() {}
    };

    return {
      next: function() {
        var ret = cb(object);
        if (ret === undefined) return { value: undefined, done: true };
        return {
          value: ret,
          done: false
        };
      }
    };
  })();
}
// 如果你使用 babel 编译后可以发现 test 函数变成了这样
function test() {
  var a;
  return generator(function(_context) {
    while (1) {
      switch ((_context.prev = _context.next)) {
        // 可以发现通过 yield 将代码分割成几块
        // 每次执行 next 函数就执行一块代码
        // 并且表明下次需要执行哪块代码
        case 0:
          a = 1 + 2;
          _context.next = 4;
          return 2;
        case 4:
          _context.next = 6;
          return 3;
		// 执行完毕
        case 6:
        case "end":
          return _context.stop();
      }
    }
  });
}
```

#### Proxy

> `Proxy` 是 `ES6` 中新增的功能，可以用来自定义对象中的操作

```js
let p = new Proxy(target, handler);
// `target` 代表需要添加代理的对象
// `handler` 用来自定义对象中的操作
可以很方便的使用 Proxy 来实现一个数据绑定和监听

let onWatch = (obj, setBind, getLogger) => {
  let handler = {
    get(target, property, receiver) {
      getLogger(target, property)
      return Reflect.get(target, property, receiver);
    },
    set(target, property, value, receiver) {
      setBind(value);
      return Reflect.set(target, property, value);
    }
  };
  return new Proxy(obj, handler);
};

let obj = { a: 1 }
let value
let p = onWatch(obj, (v) => {
  value = v
}, (target, property) => {
  console.log(`Get '${property}' = ${target[property]}`);
})
p.a = 2 // bind `value` to `2`
p.a // -> Get 'a' = 2
```

#### 跨域

> 涉及面试题：什么是跨域？为什么浏览器要使用同源策略？你有几种方式可以解决跨域问题？了解预检请求嘛？

- 因为浏览器出于安全考虑，有同源策略。也就是说，如果协议、域名或者端口有一个不同就是跨域，`Ajax` 请求会失败。
- 那么是出于什么安全考虑才会引入这种机制呢？ 其实主要是用来防止 `CSRF` 攻击的。简单点说，`CSRF` 攻击是利用用户的登录态发起恶意请求。
- 也就是说，没有同源策略的情况下，`A` 网站可以被任意其他来源的 `Ajax` 访问到内容。如果你当前 `A` 网站还存在登录态，那么对方就可以通过 `Ajax` 获得你的任何信息。当然跨域并不能完全阻止 `CSRF`。

> 然后我们来考虑一个问题，请求跨域了，那么请求到底发出去没有？ 请求必然是发出去了，但是浏览器拦截了响应。你可能会疑问明明通过表单的方式可以发起跨域请求，为什么 `Ajax`就不会。因为归根结底，跨域是为了阻止用户读取到另一个域名下的内容，`Ajax` 可以获取响应，浏览器认为这不安全，所以拦截了响应。但是表单并不会获取新的内容，所以可以发起跨域请求。同时也说明了跨域并不能完全阻止 `CSRF`，因为请求毕竟是发出去了。

接下来我们将来学习几种常见的方式来解决跨域的问题

##### JSONP

> `JSONP` 的原理很简单，就是利用 `` 标签没有跨域限制的漏洞。通过 ``标签指向一个需要访问的地址并提供一个回调函数来接收数据当需要通讯时

```html
<script src="http://domain/api?param1=a&param2=b&callback=jsonp"></script>
<script>
    function jsonp(data) {
    	console.log(data)
	}
</script>    
```

> `JSONP` 使用简单且兼容性不错，但是只限于 `get` 请求。

> 在开发中可能会遇到多个 `JSONP` 请求的回调函数名是相同的，这时候就需要自己封装一个 `JSONP`，以下是简单实现

```js
function jsonp(url, jsonpCallback, success) {
  let script = document.createElement('script')
  script.src = url
  script.async = true
  script.type = 'text/javascript'
  window[jsonpCallback] = function(data) {
    success && success(data)
  }
  document.body.appendChild(script)
}
jsonp('http://xxx', 'callback', function(value) {
  console.log(value)
})
```

##### CORS

- `CORS` 需要浏览器和后端同时支持。`IE 8` 和 `9` 需要通过 `XDomainRequest` 来实现。
- 浏览器会自动进行 `CORS` 通信，实现 `CORS` 通信的关键是后端。只要后端实现了 `CORS`，就实现了跨域。
- 服务端设置 `Access-Control-Allow-Origin` 就可以开启 `CORS`。 该属性表示哪些域名可以访问资源，如果设置通配符则表示所有网站都可以访问资源。 虽然设置 `CORS`和前端没什么关系，但是通过这种方式解决跨域问题的话，会在发送请求时出现两种情况，分别为简单请求和复杂请求。

**简单请求**

> 以 `Ajax` 为例，当满足以下条件时，会触发简单请求

1. 使用下列方法之一：

- `GET`
- `HEAD`
- `POST`

1. `Content-Type` 的值仅限于下列三者之一：

- text/plain
- `multipart/form-data`
- `application/x-www-form-urlencoded`

> 请求中的任意 `XMLHttpRequestUpload` 对象均没有注册任何事件监听器； `XMLHttpRequestUpload` 对象可以使用 `XMLHttpRequest.upload` 属性访问

**复杂请求**

> 对于复杂请求来说，首先会发起一个预检请求，该请求是 `option` 方法的，通过该请求来知道服务端是否允许跨域请求。

对于预检请求来说，如果你使用过 `Node` 来设置 `CORS` 的话，可能会遇到过这么一个坑。

以下以 `express`框架举例

```js
app.use((req, res, next) => {
  res.header('Access-Control-Allow-Origin', '*')
  res.header('Access-Control-Allow-Methods', 'PUT, GET, POST, DELETE, OPTIONS')
  res.header(
    'Access-Control-Allow-Headers',
    'Origin, X-Requested-With, Content-Type, Accept, Authorization, Access-Control-Allow-Credentials'
  )
  next()
})
```

- 该请求会验证你的 `Authorization` 字段，没有的话就会报错。
- 当前端发起了复杂请求后，你会发现就算你代码是正确的，返回结果也永远是报错的。因为预检请求也会进入回调中，也会触发 `next` 方法，因为预检请求并不包含 `Authorization` 字段，所以服务端会报错。

> 想解决这个问题很简单，只需要在回调中过滤 `option` 方法即可

```js
res.statusCode = 204
res.setHeader('Content-Length', '0')
res.end()
```

##### document.domain

- 该方式只能用于主域名相同的情况下，比如 `a.test.com` 和 `b.test.com` 适用于该方式。
- 只需要给页面添加 `document.domain = 'test.com'` 表示主域名都相同就可以实现跨域

##### postMessage

> 这种方式通常用于获取嵌入页面中的第三方页面数据。一个页面发送消息，另一个页面判断来源并接收消息

```js
// 发送消息端
window.parent.postMessage('message', 'http://test.com')
// 接收消息端
var mc = new MessageChannel()
mc.addEventListener('message', event => {
  var origin = event.origin || event.originalEvent.origin
  if (origin === 'http://test.com') {
    console.log('验证通过')
  }
})
```

#### 函数柯里化

> 在一个函数中，首先填充几个参数，然后再返回一个新的函数的技术，称为函数的柯里化。通常可用于在不侵入函数的前提下，为函数 预置通用参数，供多次重复调用。

```js
const add = function add(x) {
	return function (y) {
		return x + y
	}
}

const add1 = add(1)

add1(2) === 3
add1(20) === 21
```