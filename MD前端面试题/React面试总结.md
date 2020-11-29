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



`React初始化流程`

JSX会经过babel编译成React.createElmenet递归调用的表达式，React，createElmenet render函数被调用的时候执行，换句话说，当render函数被调用的时候，会返回一个element(组成虚拟dom树的节点)

elemenet类型为对象时分为原生DOM(调用ReactDOMCompoent)和自定义(ReactComponetsietComponent)和空节点(ReactDOMEmptyComputer)

上述四种常见的方法mountComputer用户创建组件，而updateComponent用户用户更新组件，而我们自定义组件的生命周期以及render函数都是在这些私有类的方法里被调用的。

其中 ReactDOMComponent 的 mountComponent 方法会自己操作浏览器 DOM 元素。而 ReactCompositeComponentWrapper 的则是实例化自定义组件，最后是通过递归调用到 ReactDOMComponent 的 mountComponent 方法来得到真实 DOM。

自定义元素节点 挂载的过程：

1. 得到实例化APP对象instance

2. renderedElment得到child

3. 初始化renderedElment得到child

4. child.mountComponent(container)

    在第一步得到instance对象之后，就会去看instance.componentWillMount 是否有被定义，有的话调用，而在整个渲染过程结束之后调用componentDidMount

stState过程

- newState存入pendingState队列
- 根据一个变量isBatcingUpdate判断放到队列中还是直接更新state，也就是是直接去放到dirtyComponents中，还是遍历dirtyConponents,调用updateComponent，去更新state或者props
- isBatchingUpdate默认是false，React在调用事件处理函数之前就会调用batchUpdates改变值，以此让state不会立即更新

bacthedUpdate是通过事物的方式去保证一次完整的更新

更新过程中自定义组件的update流程：

​	1、计算出nextState

​	2、render()得到nextRenderElement

​    3、与preRenderElement进行diff比较 更新节点



相关生命周期：

shouldComponentUpdate在第一步调用得到nextState之后调用

当shouldComponentUpdate返回true的时候，会先调用componentWIllUpdate，在整个更新过程结束之后调用componentDidUpdate

ReactDOMComponent 的 updateComponent 流程就是直接更新浏览器 DOM 元素。



`React优化`

优化的方向有两个：一个就是减少render次数、也就是减少diff计算

还有就是减少每一次计算的量，主要是减少重复计算。

对于函数式组件组件来说，每次render都会重新从头开始执行函数调用，

在类组件中主要使用shouldComponentUPdate生命周期和PureComponent去减少render次数。

函数组件中主要使用：

`React.memo:`等同于PurComponent，用它包裹子组件，当父组件需要重新渲染render的时候，如果传给自己的props不变，就不会重新render。memo可以添加第二个参数，是一个函数，参数为前后props，返回true的时候不会重新render

`useCallback:`应用场景是父组件向子组件传递方法，当父组件重新渲染时，代码都会重新执行。所以就算子铸造件包裹了React.memo，也会重新渲染，可以通过useCallback进行记忆传递的方法，并将记忆的方法传递给子组件

`useMemo:`如果在组件类有个变量的值，需要大量的计算才可以德诚。因为函数组件重构渲染就会重新执行代码，所以该变量的值也会重新计算，就可以useMemo做计算结果缓存。

`Fiber`

其作用是会在浏览器空闲时期一次调用函数，这样就可以在主事件循环中执行后台或低优先级的任务，而且不会对像动画和用户交互这样延迟触发而且关键的事情产生影响。函数一般会按先进先调用的顺序执行。除非函数在浏览器调用它之前就到了他的超时时间。

fiber借助单链表结构讲diff算法的递归遍历变为循环遍历

当执行setState()或首次render时，进入工作循环，循环体中处理的单元为Fiber Node ，即是拆分任务的最小单位，从耿介点开始，自顶向下构造workInProgress tree

beginWork主要做的事情时自顶向下生成所有的Fiber Node 并标记Diff

completeUnitOfWork当没有子节点，开始遍历兄弟节点作为下一个处理单元，处理完兄弟节点开始向上回溯，直到再次回去根节点为止，将收集向上回溯过程中的所有diff，拿到diff后开始进入commit阶段。

构建workInProgress tree 的过程就是diff的过程，通过requestIdleCallback来调度执行医嘱任务，每完成一个任务后回来看看有没有更紧急的，把时间控制权交还给主线程，直到下一次requestIdleCallback回调再继续构建workInProgress tree

`requestIdleCallback`的兼容方案

需要原因：兼容性不好，目前只能一秒调用回调函数20次

因为diff的过程需要多次间隔调用，所以可以借助requestAnimationFrame，它回调方法会在每次重绘前执行，另外它还存在一个缺点，页面处于后台时该回调函数不会执行，所以需要setTimeout进行补救，两个定时器内部互相取消对方。

在一帧当中，浏览器可能会响应用户的交互事件，执行js，进行渲染的一系列计算绘制，diff就是在执行js这个过程中。如果在一帧范围内没有执行完毕就会出现掉帧，影响用户体验，所以就需要在当下存在空闲时间我们才去执行任务。否则就等到下一帧 的空闲时间继续运行。

是否有时间继续diff是通过计算剩余时间来判断，金安达来说就是鸡舍当前时间为5000，浏览器支持60帧，那么一帧近似16毫秒，那么就会计算出下一帧时间为5016.得出下一帧时间以后，我们只需对比当前时间是否小于下一帧时间即可，这样就能清清楚楚的知道是否含有空闲时间去执行任务。

在事件循环中，渲染以后只有宏任务是最先会被执行的，所以选择优先级高的MessageChannel

调度的时候先判断任务是否过期，没有过期先计算下一帧时间(通过reequestAnimatonFrame)再调用port。postMessage(undefined)(过期直接调用)，这样渲染之后channel.port1.onmessage就会执行(任务没过期就要对比当前时间和下一帧时间，还有时间就执行任务，没有就看一帧是否能执行任务，过期则直接执行)



`事件机制`

reatc事件并没有绑定在真实dom节点上，而是通过事件代理，在最外层的document上对事件进行统一分发，原生事件在目标阶段执行，React在冒泡阶段执行。

组件挂载更新时，给document注册原生事件回调为dispacthEvnet(统一的事件分发机制)

事件初始化，添加到listenerBank

触发事件的事件时：

- 触发document注册原生事件的回调dispacthEvent
- 获取到触发这个事件最深一级的元素
- 遍历整个元素的所有父元素，一次对每一级元素进行处理
- 构造合成事件
- 将每一级的合成事件存储在eventQueue事件队列中
- 遍历eventQueue
- 通过isPropagationStopped判断当前时间是否执行了阻止冒泡方法
- 如果阻止了冒泡，停止遍历，否则通过excuteDispacth执行合成事件
- 释放处理完成的事件



### diff算法

#### React15

基于两个假设：

- 两个相同的组件产生类似的DOM结构，不同组件产生不同DOM结构
- 对于同一层次的一组子节点，他们可以通过唯一的id区分

tree diff

主要的规则就是跨层级的节点移动不会考虑复用，直接新建再删除

注意：在开发组件时，保持稳定的DOM结构会有助于性能的提升。例如，可以通过CSS隐藏或显示节点，而不是真的移除或添加DOM节点

对于不同的节点类型，react会基于第一条假设，直接删去旧的节点，新建一个新的节点

component diff

如果是同一个类型的组件，根据新节点的props去更新原来根节点的组件实例，触发一个更新的过程，按照原策略继续比较virtual DOM tree

如果不是，则将该组件判断为dirty component 从而替换这个组件下的所有子节点

element diff

当节点处于同一层级时，React diff提供了三种节点操作，分别是插入、移动、删除。

再没有key的情况下对比节点的时候，是一个个按着顺序对比的，比如以下这种情况在没有key的情况下会删除Test3 新建Test2  新建Test3

```js
<!-- before -->
<div>
  <Test1></Test1>
  <Test3></Test3>
</div>
<!-- after -->
<div>
  <Test1></Test1>
  <Test2></Test2>
  <Test3></Test3>
</div>
```

`react-router原理`

hash路由：核心是监听了load和onHashChange事件，在页面刷新或者URL hash改变时渲染不同的页面组件，history API路由：核心是通过replaceState和pushState去改变页面URL ，通过popState事件监听history对象改变的时候改变页面

`react-redux`

Redux 是JavaScript状态容器，能提供可预测话的状态管理，需要它的原因是因为前段有大量的无规律的交互和异步操作，而且随着代码量越来越大，我们要维护的状态也越来越多，它能提供的就是让每个State变化可预测，动作与状态统一管理

`connent原理`

connect方法是一个高阶组件，主要的两个参数都是函数，命名为mapStateToProps和mapDispacthToProps，内部原理获取store添加订阅后，将state和dispacth分别传入上面 的两个方法，返回需要的state和改变state的方法添加到UI组件的props上

前提是在应用顶层已经适用prodiver组件，并应用初始化创建store



`中间件原理`

中间件可以说是dispacth的增强或者替换

applyMiddleware(middleware)返回的是一个函数，createStore内部会使用这个函数的调用结果(参数为createStore)创建store







### react性能优化的方法

- **减少计算的量**。 -> 对应到 React 中就是**减少渲染的节点 或者 降低组件渲染的复杂度**
- **利用缓存**。-> 对应到 React 中就是**如何避免重新渲染，利用函数式编程的 memo 方式来避免组件重新渲染**
- **精确重新计算的范围**。 对应到 React 中就是**绑定组件和状态关系, 精确判断更新的'时机'和'范围'. 只重新渲染'脏'的组件，或者说降低渲染范围**

`减少渲染的节点/降低渲染计算量(复杂度)`

- 不要在渲染函数都进行不必要的计算

- 减少不必要的嵌套

       我们公司是重度的style-componemt使用者，其实吧大部分情况下我们都不需要这个东西，比如纯静态的样式规则，以及需要重度性能优化的场景

    除了性能问题，另外一个困扰我们的是它带来的节点嵌套地狱。所以我们应该理性地选择一些工具

- 虚拟列表

      虚拟列表是常见的‘长列表'和'复杂组件树'优化方式，它优化的本质就是减少渲染的节点。虚拟列表只渲染当前视口可见元素

    - 无限滚动列表，grid, 表格，下拉列表，spreadsheets
    - 无限切换的日历或轮播图
    - 大数据量或无限嵌套的树
    - 聊天窗，数据流(feed), 时间轴
    - 等等

    `相关组件方案:`

    - [react-virtualized](https://link.zhihu.com/?target=https%3A//github.com/bvaughn/react-virtualized)
    - [react-window](https://link.zhihu.com/?target=https%3A//github.com/bvaughn/react-window) 更轻量的 react-virtualized, 

- 惰性渲染

    **惰性渲染的初衷本质上和虚表一样，也就是说我们只在必要时才去渲染对应的节点**。

    举个典型的例子，我们常用 Tab 组件，我们没有必要一开始就将所有 Tab 的 panel 都渲染出来，而是等到该 Tab 被激活时才去惰性渲染。

    还有很多场景会用到惰性渲染，例如树形选择器，模态弹窗，下拉列表，折叠组件等等。

- 选择合适的样式方案

`避免重新渲染`

减少不必要的重新渲染也是 React 组件性能优化的重要方向. 为了避免不必要的组件重新渲染需要在做到两点:

1. 保证组件纯粹性。即控制组件的副作用，如果组件有副作用则无法安全地缓存渲染结果
2. 通过`shouldComponentUpdate`生命周期函数来比对 state 和 props, 确定是否要重新渲染。对于函数组件可以使用`React.memo`包装

- 简化props

    **① 如果一个组件的 props 太复杂一般意味着这个组件已经违背了‘单一职责’，首先应该尝试对组件进行拆解**.

     **② 另外复杂的 props 也会变得难以维护, 比如会影响`shallowCompare`效率, 还会让组件的变动变得难以预测和调试**.

    **简化的 props 更容易理解, 且可以提高组件缓存的命中率**

- 不变的事件处理器、

    ①**避免使用箭头函数形式的事件处理器**, 例如:

- 不可变数据

- 简化state

- 使用recompose精细化比对

`精细化渲染`

响应式数据的精细化渲染

不要滥用Context





### 为什么react要自定义一套事件系统？

- 抹平浏览器之间的兼容性差异

    这是最原始的动机，它还会通过其他事件来模拟一些低版本不兼容的事件，这才是合成的本来意思

- 事件合成就是事件自定义

    事件合成除了处理兼容性问题之外，还可以用来自定义高级事件，比较典型的是React的onChange事件，它为表单元素定义了统一的值变动事件。另外第三方也可以通过React事件插件机制来合成自定义事件。

- 抽象跨平台机制  和VDOM的意义差不多，VDOM抽象了跨平台的渲染方式，那么对应SyntheticEvent目的也是想提供一个抽象的跨平台机制
- React打算做更多的优化  比如利用事件委托机制，大部分事件最终绑定到了document,而不是DOM节点本身，这样简化了DOM事件处理逻辑，减少了内存开销，但这也意味着，React需要更多自己模拟一套事件冒泡的机制
- React打算干预事件分发机制  ：V16引入Fiber架构，React为了优化用户的交互体验，会干预事件的分发，不同类型的事件有不同的优先级，比如高优先级的事件可以中断渲染，让用户代码可以及时的响应用户交互