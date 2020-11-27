### 1、React组件如何通讯

### 2、JSX本质是什么？

### 3、context是什么 有什么用途？

### 4、shouldComponentUpdate(SCU)的用途

### 5、redux流程

`代码流程：`

​		首先，先创建一个存储所有全局状态、数据的仓库store，

在这个store中引入处理store中数据的逻辑、方法reducers。

在store中引入异步中间件，允许reducer操作



### 6、setState是同步还是异步

- setState本身其实无所谓异步还是同步
- 说异步、同步主要是看能否命中batchUpdate机制
- 就是判断isBatchingUpdates

### 7、基于react设计一个todolist(组件结构，redux state数据结构)

### 8、event

event其实react自己封装的一个event类，并不是原生的event

它的`__photo__.contructor`是syntheritcEvent(组合event)

但event.nativeEvent就指向了js的原生event对象

### 9、setState

- state应该在构造函数中定义

- 不要直接修改state，使用不可变数据

    - *注意，不能直接对 this.state.list 进行 push pop splice 等，这样违反不可变值*
    - *注意，不能直接对 this.state.obj 进行属性设置，这样违反不可变值*

    ```js
    // 不可变值（函数式编程，纯函数） - 数组
    const list5Copy = this.state.list5.slice()
    list5Copy.splice(2, 0, 'a') // 中间插入/删除
    this.setState({
        list1: this.state.list1.concat(100), // 追加
        list2: [...this.state.list2, 100], // 追加
        list3: this.state.list3.slice(0, 3), // 截取
        list4: this.state.list4.filter(item => item > 100), // 筛选
        list5: list5Copy // 其他操作
    })
    // 注意，不能直接对 this.state.list 进行 push pop splice 等，这样违反不可变值
    
    // 不可变值 - 对象
    this.setState({
        obj1: Object.assign({}, this.state.obj1, {a: 100}),
        obj2: {...this.state.obj2, a: 100}
    })
    // 注意，不能直接对 this.state.obj 进行属性设置，这样违反不可变值
    ```

    

- setState可能是异步更新(有可能是同步更新)
    - 直接使用是异步的
    - setTimeout 是同步的
    - async  await  同步
    - 自定义事件中使用也是同步的
    
- setState异步更新，更新前可能会被合并
    - 传入对象，会被合并(类似于Object.assign,执行结果只执行一次)
    - 传入函数，不会被合并

### 10、非受控组件

- defaultValue  defaultChecked

- ref

- 手动操作DOM元素

    使用场景：

    - 必须手动操作dom元素，setState实现不了
    - 文件上传<input type="file"/>
    - 某些富文本编辑器 需要传入DOM元素



### 11、Portals

- 组件默认会按照既定层次嵌套渲染
- 如何让组件渲染到父组件以外
- 使用场景：
    - overflow:hidden
    - 父组件 z-index值太小
    - fixed需要放在body第一层级

### 12、context

- 公共信息如何传递给每个组件

- 用props太繁琐

- 用redux小题大做

- 使用场景：

    - 如果你在组件间传递的数据逻辑比较复杂，可以使用redux；

        如果组件层级不多，可以使用props；

        如果层级较深，数据逻辑简单，可以使用context或者发布-订阅模式。
        
        像一些简单的公共信息：主题色、语言等

### 13、SCU

基本用法

```js
shouldCompoentUpdate(nextProps,nextState){
    if(nextProps.count!==this.state.count){
        return true;//可以渲染
    }
    return false;//不重复渲染
}
```

`为什么react不自己实现SCU的功能?`

因为react给了开发者一个定制的权力，有时候并不是值相等就 一定不需要渲染。

React默认父组件有更新，子组件则无条件更新。

因为每一个类组件的SCU默认返回的都是true

正式因为react将很多权利给了开发者，所以性能优化对于React更加重要，这些优化都需要我们手动去操作。

`注意：SCU必须配合state的不可变值，否则的话，就会不生效`

​		因为每个人写代码的方式不一样，react不能要求每个人都去遵守规范，所以一般SCU就是默认true，这样即便不遵守state的不可变值，也可以产生效果。

`SCU一定要每次都用吗?`

不一定，需要的时候才优化。

`总结`：

- SCU默认返回true，即React默认重新渲染所有子组件
- 必须配合state不可变值一起使用
- 可以先不用SCU，有性能问题时再考虑使用
- 深比较的时候，可以考虑采用lodash的_.equals()方法

### 14、PureComponent和memo

- 与Component的区别就是，在SCU中实现了浅比较

- memo，就是函数组件中PureComponent

    - memo是一个高阶函数
    - 它接收两个参数，第一个参数为纯函数的组件，二个参数用于对比props是否刷新，与SCU类似，但不同

    ```js
    import React from "react";
    
    function Child({seconds}){
        console.log('I am rendering');
        return (
            <div>I am update every {seconds} seconds</div>
        )
    };
    
    function areEqual(prevProps, nextProps) {
        if(prevProps.seconds===nextProps.seconds){
            return true
        }else {
            return false
        }
    
    }
    export default React.memo(Child,areEqual)
    ```

    

- 浅比较已经适用大部分情况，尽量不要去做深度比较

### 15、immutable.js

虽然state有不可变值的特性，但实现的并不彻底，如果想彻底实现不可变值，可以使用immutable.js

`对于Immutable对象的任何修改和更新，都会返回一个新的Immuatble对象。`

`它实现的原理是基于数据共享的持久化数据结构，也就是使用旧数据创建新数据时，要保证旧数据同时可用且不变。`

`同时为了避免深拷贝带来性能损耗，它使用了结构与数据共享的，也就是说如果原有对象树中的一个节点发生了改变，只会修改这个节点和受它影响的父节点，其他节点则进行共享。`

优点：

- 降低mutable带来的复杂度
- 节省内存
- 历史追溯性（时间旅行）：时间旅行指的是，每时每刻的值都被保留了，想回退到哪一步只要简单的将数据取出就行，想一下如果现在页面有个撤销的操作，撤销前的数据被保留了，只需要取出就行，这个特性在redux或者flux中特别有用
- 拥抱函数式编程：immutable本来就是函数式编程的概念，纯函数式编程的特点就是，只要输入一致，输出必然一致，相比于面向对象，这样开发组件和调试更方便

缺点：

- 需要重新学习api
- 资源包大小增加（源码5000行左右）
- 容易与原生对象混淆：由于api与原生不同，混用的话容易出错。

### 16、关于组件公共逻辑的抽离

- mixin，被React弃用

- 高阶组件HOC

- Render Props

    核心思想就是通过class组件的state作为props传递给纯函数组件

```js
class Factory extends React.Component{
    constructor(){
        this.state={
            /* state即多个组件的公共逻辑的数据*/
        }
    }
    /*修改state*/
    render(){
        return <div>{this.props.render(this.state)}</div>
    }
}

const App=()=>{
    <Factory
     render={
         /*redner是一个函数组件*/
         (props)=><p>{props.a}{props.b}<p/>
     }
    />
}
```

### 17、路由懒加载

用到了lazy和Suspense

```js
import React,{Suspense,lazy} from 'react'

const Home=lazy(()=>import('./routes/Home'))
const About=lazy(()=>import('./routes/About'))

const App=()=>(
<Router>
    <Suspense fallback={<div>loading....</div>}>
    	<Switch>
    		<Route exact path="/" component={Home}/>
    		<Route path="/about" component={About}/> 
    	</Switch>
    <Suspense/>
</Router>
)
```

### 18、合成事件机制

- 所有事件都会挂载到document上
- event不是原生的，是SyntheticEvnent合成事件对象
- 它和Vue事件不同，和DOM事件也不同
- 合成事件模拟了原生dom事件的所有能力
- 但也没有完全抛弃原生事件，event.native就是原生事件

流程：

1. 元素触发事件，事件会冒泡到顶层
2. 之后会被合成事件接收处理，将这些事件统一都是实例化为统一的react event
3. 最后就将这些不同的react派发给不同的事件处理函数处理

为什么需要合成事件？

- 为了更好的兼容性和跨平台
    - 当自己实现了一套事物逻辑之后，底层的事件机制可以不限于浏览器
- 挂载到doucment，减少内存消耗，避免频繁解绑
- 方便事件的统一管理(如事物机制)，减少对dom的依赖

### 19、setState和bacthUpdate

`setState主流程：`

- 当调用this.setState(newState)的时候，newState会存入pending队列
- 然后会去判断newState是否处于batch update中
- 如果是，就说明是异步更新，就保存组件于dirtyComponents中
- 如果不是，就说明是同步更新，就会遍历所有的dirtyComponents，调用updateComponent,更新pending state or props

`bacthUpdate机制：`

​	开始：先设置isbatchUpdate=true

​	结束：再设置isBatchingUpdates=false

如果是正常使用this.setState(),那么this.setState就是异步更新

如果this.setState处于async或者异步回调函数中，那么因为同步的isBatchingUpdates=false会先执行，所以this.setState就是通过更新。

`什么操作可以命中bacthUpdate机制？`

- 生命周期 和它调用的函数
- react中注册的事件(和它调用的函数)
- React可以管理的入口

`什么操作不能命中bacthUpdate机制？`

- setTimout  setInterval等（和它调用的函数）
- 自定义的dom事件（和它调用的函数）
- react管不到的入口

`trnaction（事务）机制：`

### 20、组件渲染和更新过程

- JSX如何渲染页面
- setState如何更新页面
- 考察全流程

  `回顾vue组件渲染和更新过程`

1、先是模板渲染出来一个render函数

2、执行render函数渲染出来一个v-dom结构

3、去渲染这个虚拟dom的同时，也会去触发data

4、被触发的data会作为依赖收集到wacther中，这一过程触发的是data的getter

5、如果是组件更新，会触发setter,setter回去更新wacther里面已经被收集过的属性，然后再去重新走一遍渲染流程 

`回顾jsx本质和vdom`

JSX即createElement函数

执行生成vnode

patch(elem,vnode)和patch(vnode,newVnode)

`react 组件渲染过程`

- props  state

- render 解析jsx 生成vnode

- patch (elem,vnode) 将组件渲染成真实的DOM

    patch不是暴力替换节点，而是在现有的DOM上进行修改来达到渲染视图的目的。对现有DOM进行修改许要做三件事：

    - 创建新增的节点
    - 删除已经废弃的节点
    - 修改需要更新的节点

`更新的两个阶段`

setState的时候就会生成dirtyComponents，这个可能会有子组件

遍历所有的dirtyComponents，render去生成newVnode

然后patch会被拆分为两个阶段

reconciliation阶段  ---执行diff算法，纯js运算

commit阶段 ---diff算法完毕之后，才会去执行dom操作

`为什么要分为这两个阶段?`

js它是单线程，而且和DOM渲染共用一个线程

当组件足够复杂，组件更新时计算和渲染和压力都很大

同时再有DOM操作需求 (动画、拖拽)，将卡顿



解决方案：`React Fiber`

- 将reconciliation阶段进行任务拆分(commit无法拆分)

    ​	这个阶段主要是各个组件，data数据之间的一个比较，所以可以将一整个任务拆分为多个子任务片段

- DOM需要渲染时暂停，空闲时回复

- `那什么时候需要暂停和恢复呢？`

    有一个API  window.requestIdleCallback，通过这个API去判断是你吗时候暂停和恢复。

### 21、React性能优化

- 渲染列表时加key
- 自定义事件 DOM事件及时销毁
- 合理使用异步组件
- 减少函数bind this的次数
- 合理使用SCU PureComponent 和memo
- 合理使用INmmutable.js
- 图片懒加载
- 减少http请求次数，尽量合并请求
- 使用ssr

### 22、React和Vue的区别

相同点：

- 都支持组件化
- 都是数据驱动视图
- 都使用 v-dom操作dom

不同点：

- react使用jsx拥抱js，而vue使用模板拥抱html
- react是函数式编程，Vue声明式编程 
- React需要更多的自力更生，vue大部分工作都已经自己做了

### 23、react15生命周期与react16生命周期

`react15`中的以下三个生命周期函数会在17中完全删除：

componentWillReceiveProps(nextProps){}

componentWillMount(){}

componentWillReceiveUpdate(nextProps,nextUpdate)

在react16中列为不安全的生命周期函数。

react16中增加了三个生命周期函数：

1、static getDerivedStateFromProps(nextProps,prevState)

它是用于替换`componentWillReceiveProps`，该函数会在初始化和`update`时被调用

因为该函数是静态函数，所以取不到`this`

如果需要对比`preProps`需要单独在`state`中维护.

this获取属性只能依靠原型链，而静态方法不在原型链上，所以，在组件实例内无发通过this调用static方法

2、getSnapshotBeforeUpdate(preProps,preState)

首先，从字面来理解“snapshot”是快照的意思。在dom更前之前调用。返回的值将被传给componentDidUpdate(prevProps, prevState, snaphot)。
这个会比较少用到，但对于处理比如数据更新后的滚动条的差异滚动，对用户体验，很有帮助。

3、componentDidCatch(error,info)

以往，当组件发生错误（可以用throw new Error模拟）时，会导致整个react程序死掉，这对于程序的稳定性来说非常不好。
componentDidCatch可以捕获子组件中任何一个错误，捕获到错误后可以对错误进行处理。
如果发生错误的组件的父组件没有设置componentDidCatch错误捕获，将继续递归父组件的父组件中的componentDidCatch，找到则停止。

### 24、优化了哪些语法？

`ref优化`

ref有很多作用，通过ref父组件可以调用子组件内的方法，配合ReactDOM.findDOMNode(ref)可以获取到组件内对应的dom，ref与key一样无法通过this.props.ref获取

以前版本的react，给子组件添加ref="inputName",就可以通过this.refs['inputName']获取子组件实例，然后可以进行一些操作



### 26、react中的虚拟dom diff算法

web界面由DOM树构成，当其中某一部分发生变化的时候，其实就是对应的某个DOM节点发生了变化，在React中，构建UI界面的思路是由当前状态决定当前界面，前后两个状态就对应两个界面。

给定任意两棵树，找到最少的转换步骤，但是标准的DIFF算法复杂度需要O(n^3),这显然无法满足性能要求。所以react优化这个diff算法，使得它的时间复杂度优化到O(n),他们结合web界面的特点做了两个假设：

1. 两个相同组件产生类似的DOM结构，不同的组件产生不同的DOM结构；
2. 对于同一层次的一组子节点，它们可以通过唯一的id进行区分

算法优化：

1. DOM节点的跨层级移动忽略不计，即先创建移动节点到目标位置然后删除移动节点

2. 同级节点通过唯一key进行区分

    diff算法到Fiber架构

    diff算法产生的问题：面对数量庞大层级复杂的节点业务，大量dom的更新会占用过多的内存和计算时间，这样就会导致渲染UI不够流畅，用户操作得不到及时反馈

    

    fiber架构实现的要素

    原本的虚拟dom编译后是递归的代码执行，栈调度式的执行，无法在执行中途进行break、continue、suspend操作。

    这时候就需要一种链表结构，使其更具有灵活性，性能也要递归更好

    

    `如何决定在什么时间段执行？`

    下面是一些自救措施： （帧数控制调用，闲时调用，多线程调用， 进入可视区调用）

    - requestAnimationFrame
    - requestIdleCallback
    - web worker
    - IntersectionObserver

    updateFiberAndView有出两个时间段，一个给自己的虚拟dom渲染，一个给浏览器的
     requestAnimationFrame帮我们执行第二个时间段保证流畅性，
     requestIdleCallback会在帧结束时并且有空闲时间。或者用户不与网页交互时，在下次时间循环之前执行回调

    

    react为了兼容，自己实现requestIdleCallback
     requestAnimationFrame + 计算帧时间及下一帧时间 + MessageChannel 就是我们实现 requestIdleCallback

### 27React重点

React构成：

react 分为基础模块、渲染模块、核心模块。

基础模块包括一些react基础API以及逐渐类，组件内定义render，setState方法和生命周期相关的回调方法

渲染模块，针对不同数组环境采用不同的渲染方法实现，如react-dom、react-native、react-webgl、react-art,这些渲染方法都依赖react-reconciler，针对不同的平台，它就会注入相应的渲染方法到reconciler中，这也是react跨平台的机制。

核心模块：负责调度算法以及fiber 算法、diff算法 ，可以连接react和render模块，注入setState方法到component实例中，在diff阶段执行react组件中render方法，在patch阶段执行reactreact组件中生命周期回调并且调用render模块注入相应的方法去渲染真实视图结构。



React初始化流程

JSX会经过babel编译成React.createElmenet递归调用的表达式，React，createElmenet render函数被调用的时候执行，换句话说，当render函数被调用的时候，会返回一个element(组成虚拟dom树的节点)

elemenet类型为对象时飞机位原生DOM(调用ReactDOMCompoent)和自定义(ReactComponetsietComponent)和空节点(ReactDOMEmptyComputer)

上述四种常见的方法mountComputer用户创建组件，而updateComponent用户用户更新组件，而我们自定义组件的生命周期以及render函数都是在这些私有类的方法里被调用的。