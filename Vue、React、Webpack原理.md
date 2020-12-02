# Vue原理

## 组件化基础

数据驱动视图（mvvm，setState）

+ 传统组件，只是静态渲染，更新还需要依赖于操作DOM
+ 数据驱动视图—Vue MVVM
+ 数据驱动视图—React setState

### Vue MVVM

![image-20201123215212055](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201123215212055.png)

View=>HTML

Model=>data

VM=>相当于DOM点击事件v-on去调用method方法更改数据

## Vue 响应式

+  组件data的数据一旦变化，立即触发视图的更新
+ 实现数据驱动视图的第一步
+ Vue 考察原理的第一题

### 核心API - Object.defineProperty

+ Object.defineProperty的一些缺点（Vue3.0 启动 Proxy）

+ Proxy有兼容性问题，Proxy兼容性不好，且无法使用polyfill

#### Object.defineProperty的使用

![image-20201123220344819](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201123220344819.png)

#### Object.defineProperty 实现响应式

+ 监听对象，监听数组
+ 复杂对象，深度监听
+ 几个缺点

```js
// 准备数据
const data = {
    name: 'zhangsan',
    age: 20,
    info: {
        address: '北京' // 需要深度监听
    },
    nums: [10, 20, 30]
}

// 监听数据
observer(data)

// 监听对象属性
function observer(target) {
    if (typeof target !== 'object' || target === null) {
        // 不是对象或数组
        return target
    }
    
    // 污染全局的 Array 原型
    // Array.prototype.push = function () {
    //     updateView()
    //     ...
    // }

    if (Array.isArray(target)) {
        target.__proto__ = arrProto
    }

    // 重新定义各个属性（for in 也可以遍历数组）
    for (let key in target) {
        defineReactive(target, key, target[key])
    }
}

// 重新定义属性，监听起来
function defineReactive(target, key, value) {
    // 深度监听
    observer(value)

    // 核心 API
    Object.defineProperty(target, key, {
        get() {
            return value
        },
        set(newValue) {
            if (newValue !== value) {
                // 深度监听
                observer(newValue)

                // 设置新值
                // 注意，value 一直在闭包中，此处设置完之后，再 get 时也是会获取最新的值
                value = newValue

                // 触发更新视图
                updateView()
            }
        }
    })
}

// 触发更新视图
function updateView() {
    console.log('视图更新')
}


// 重新定义数组原型
const oldArrayProperty = Array.prototype
//oldArrayProperty（__proto__）
// 创建新对象，原型指向 oldArrayProperty ，再扩展新的方法不会影响原型
const arrProto = Object.create(oldArrayProperty);
['push', 'pop', 'shift', 'unshift', 'splice'].forEach(methodName => {
    arrProto[methodName] = function () {
        updateView() // 触发视图更新
        oldArrayProperty[methodName].call(this, ...arguments)
        // Array.prototype.push.call(this, ...arguments)
    }
})
```

缺点：

+ 深度监听，需要递归到底，计算量很大
+ 无法监听新增属性/删除属性（Vue.set  Vue.delete）
+ 无法监听原生数组，需要特殊处理

## vdom和diff

+ vdom是实现vue和React的重要基石
+ diff算法是vdom中最核心、最关键的部门
+ vdom是一个热门话题，也是面试中的热门问题



+ DOM操作非常消耗性能
+ 以前使用jQuery，可以执行控制DOM操作的时机，手动调正
+ Vue和React是数据驱动视图，如何有效控制DOM操作？



### 解决方案 - vdom

+ 有了一定复杂度，想减少计算次数比较难
+ 能不能把计算，更多的转移JS计算？因为JS执行速度很快
+ vdom - 用JS模拟DOM结构，计算出最小的变更，操作DOM

用JS模拟DOM结构

![image-20201123223730760](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201123223730760.png)

+ Vue3.0 重写了vdom代码，优化了性能
+ 但vdom的基本理念不变，面试考点也不变

### diff算法

+ diff算法是vdom中最核心、最关键的部门
+ diff算法能在日常使用vue React中提现出来（如key）

diff算法概述

+ diff即对比，是一个广泛的概念，如linux diff命令，git diff等
+ 两个js对象也可以做diff
+ 两棵树做diff，如这里的vdom diff

![image-20201123224616947](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201123224616947.png)

树diff的事件复杂度O（n^3）

+ 第一，遍历tree1
+ 第二，遍历tree2
+ 第三，排序

优化事件复杂度到O（n）

+ 只比较同一层级，不跨级比较
+ tag不相同，则直接删除重建，不再深度比较
+ tag和key，两者都相同，则认为是相同节点，不再深度比较

![image-20201123224950720](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201123224950720.png)

![image-20201123225003324](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201123225003324.png)

### snabbdom - 源码解读

h函数包裹node信息，返回vnode函数

```js
export function h (sel: any, b?: any, c?: any): VNode {
  var data: VNodeData = {}, children: any, text: any, i: number;
  if (c !== undefined) {
    if (b !== null) { data = b; }
    if (is.array(c)) {
      children = c;
    } else if (is.primitive(c)) {
      text = c;
    } else if (c && c.sel) {
      children = [c];
    }
  } else if (b !== undefined && b !== null) {
    if (is.array(b)) {
      children = b;
    } else if (is.primitive(b)) {
      text = b;
    } else if (b && b.sel) {
      children = [b];
    } else { data = b; }
  }
  if (children !== undefined) {
    for (i = 0; i < children.length; ++i) {
      if (is.primitive(children[i])) children[i] = vnode(undefined, undefined, undefined, children[i], undefined);
    }
  }
  if (
    sel[0] === 's' && sel[1] === 'v' && sel[2] === 'g' &&
    (sel.length === 3 || sel[3] === '.' || sel[3] === '#')
  ) {
    addNS(data, children, sel);
  }

  // 返回 vnode
  return vnode(sel, data, children, text, undefined);
};
```

vnode函数，返回一个对象{sel,data,children,text（children与text不能共存）,elm（vnode是对应的那个dom元素）,key（可以理解成v-for的key）}

```js
function vnode (sel: string | undefined,
  data: any | undefined,
  children: Array<VNode | string> | undefined,
  text: string | undefined,
  elm: Element | Text | undefined): VNode {
  let key = data === undefined ? undefined : data.key;
  return { sel, data, children, text, elm, key };
}
```

patch函数，参数（oldVnode，vnode），执行pre hook（生命周期），如果第一个参数不是Vnode，则执行emptyNodeAt函数（把dom和vnode做一个绑定）创建一个空的vnode，关联到这个dom元素

```js
function patch (oldVnode: VNode | Element, vnode: VNode): VNode {
    let i: number, elm: Node, parent: Node;
    const insertedVnodeQueue: VNodeQueue = [];
    // 执行 pre hook
    for (i = 0; i < cbs.pre.length; ++i) cbs.pre[i]();

    // 第一个参数不是 vnode
    if (!isVnode(oldVnode)) {
      // 创建一个空的 vnode ，关联到这个 DOM 元素
      oldVnode = emptyNodeAt(oldVnode);
    }

    // 相同的 vnode（key 和 sel 都相等）
    if (sameVnode(oldVnode, vnode)) {
      // vnode 对比
      patchVnode(oldVnode, vnode, insertedVnodeQueue);
    
    // 不同的 vnode ，直接删掉重建
    } else {
      elm = oldVnode.elm!;
      parent = api.parentNode(elm);

      // 重建
      createElm(vnode, insertedVnodeQueue);

      if (parent !== null) {
        api.insertBefore(parent, vnode.elm!, api.nextSibling(elm));
        removeVnodes(parent, [oldVnode], 0, 0);
      }
    }

    for (i = 0; i < insertedVnodeQueue.length; ++i) {
      insertedVnodeQueue[i].data!.hook!.insert!(insertedVnodeQueue[i]);
    }
    for (i = 0; i < cbs.post.length; ++i) cbs.post[i]();
    return vnode;
}


function emptyNodeAt (elm: Element) {
    const id = elm.id ? '#' + elm.id : '';
    const c = elm.className ? '.' + elm.className.split(' ').join('.') : '';
    return vnode(api.tagName(elm).toLowerCase() + id + c, {}, [], undefined, elm);
}

function sameVnode (vnode1: VNode, vnode2: VNode): boolean {
  // key 和 sel 都相等
  // undefined === undefined // true
  return vnode1.key === vnode2.key && vnode1.sel === vnode2.sel;
}


function patchVnode (oldVnode: VNode, vnode: VNode, insertedVnodeQueue: VNodeQueue) {
    // 执行 prepatch hook
    const hook = vnode.data?.hook;
    hook?.prepatch?.(oldVnode, vnode);

    // 设置 vnode.elem
    const elm = vnode.elm = oldVnode.elm!;

    // 旧 children
    let oldCh = oldVnode.children as VNode[];
    // 新 children
    let ch = vnode.children as VNode[];

    if (oldVnode === vnode) return;

    // hook 相关
    if (vnode.data !== undefined) {
      for (let i = 0; i < cbs.update.length; ++i) cbs.update[i](oldVnode, vnode);
      vnode.data.hook?.update?.(oldVnode, vnode);
    }

    // vnode.text === undefined （vnode.children 一般有值）
    if (isUndef(vnode.text)) {
      // 新旧都有 children
      if (isDef(oldCh) && isDef(ch)) {
        if (oldCh !== ch) updateChildren(elm, oldCh, ch, insertedVnodeQueue);
      // 新 children 有，旧 children 无 （旧 text 有）
      } else if (isDef(ch)) {
        // 清空 text
        if (isDef(oldVnode.text)) api.setTextContent(elm, '');
        // 添加 children
        addVnodes(elm, null, ch, 0, ch.length - 1, insertedVnodeQueue);
      // 旧 child 有，新 child 无
      } else if (isDef(oldCh)) {
        // 移除 children
        removeVnodes(elm, oldCh, 0, oldCh.length - 1);
      // 旧 text 有
      } else if (isDef(oldVnode.text)) {
        api.setTextContent(elm, '');
      }

    // else : vnode.text !== undefined （vnode.children 无值）
    } else if (oldVnode.text !== vnode.text) {
      // 移除旧 children
      if (isDef(oldCh)) {
        removeVnodes(elm, oldCh, 0, oldCh.length - 1);
      }
      // 设置新 text
      api.setTextContent(elm, vnode.text!);
    }
    hook?.postpatch?.(oldVnode, vnode);
}


function removeVnodes (parentElm: Node,
    vnodes: VNode[],
    startIdx: number,
    endIdx: number): void {
    for (; startIdx <= endIdx; ++startIdx) {
      let listeners: number, rm: () => void, ch = vnodes[startIdx];
      if (ch != null) {
        if (isDef(ch.sel)) {
          invokeDestroyHook(ch); // hook 操作

          // 移除 DOM 元素
          listeners = cbs.remove.length + 1;
          rm = createRmCb(ch.elm!, listeners);
          for (let i = 0; i < cbs.remove.length; ++i) cbs.remove[i](ch, rm);
          const removeHook = ch?.data?.hook?.remove;
          if (isDef(removeHook)) {
            removeHook(ch, rm);
          } else {
            rm();
          }
        } else { // Text node
          api.removeChild(parentElm, ch.elm!);
        }
      }
    }
}


function addVnodes (
    parentElm: Node,
    before: Node | null,
    vnodes: VNode[],
    startIdx: number,
    endIdx: number,
    insertedVnodeQueue: VNodeQueue
    ) {
    for (; startIdx <= endIdx; ++startIdx) {
      const ch = vnodes[startIdx];
      if (ch != null) {
        api.insertBefore(parentElm, createElm(ch, insertedVnodeQueue), before);
      }
    }
}


function updateChildren (parentElm: Node,
    oldCh: VNode[],
    newCh: VNode[],
    insertedVnodeQueue: VNodeQueue) {
    let oldStartIdx = 0, newStartIdx = 0;
    let oldEndIdx = oldCh.length - 1;
    let oldStartVnode = oldCh[0];
    let oldEndVnode = oldCh[oldEndIdx];
    let newEndIdx = newCh.length - 1;
    let newStartVnode = newCh[0];
    let newEndVnode = newCh[newEndIdx];
    let oldKeyToIdx: KeyToIndexMap | undefined;
    let idxInOld: number;
    let elmToMove: VNode;
    let before: any;

    while (oldStartIdx <= oldEndIdx && newStartIdx <= newEndIdx) {
      if (oldStartVnode == null) {
        oldStartVnode = oldCh[++oldStartIdx]; // Vnode might have been moved left
      } else if (oldEndVnode == null) {
        oldEndVnode = oldCh[--oldEndIdx];
      } else if (newStartVnode == null) {
        newStartVnode = newCh[++newStartIdx];
      } else if (newEndVnode == null) {
        newEndVnode = newCh[--newEndIdx];

      // 开始和开始对比
      } else if (sameVnode(oldStartVnode, newStartVnode)) {
        patchVnode(oldStartVnode, newStartVnode, insertedVnodeQueue);
        oldStartVnode = oldCh[++oldStartIdx];
        newStartVnode = newCh[++newStartIdx];

      // 结束和结束对比
      } else if (sameVnode(oldEndVnode, newEndVnode)) {
        patchVnode(oldEndVnode, newEndVnode, insertedVnodeQueue);
        oldEndVnode = oldCh[--oldEndIdx];
        newEndVnode = newCh[--newEndIdx];

      // 开始和结束对比
      } else if (sameVnode(oldStartVnode, newEndVnode)) { // Vnode moved right
        patchVnode(oldStartVnode, newEndVnode, insertedVnodeQueue);
        api.insertBefore(parentElm, oldStartVnode.elm!, api.nextSibling(oldEndVnode.elm!));
        oldStartVnode = oldCh[++oldStartIdx];
        newEndVnode = newCh[--newEndIdx];

      // 结束和开始对比
      } else if (sameVnode(oldEndVnode, newStartVnode)) { // Vnode moved left
        patchVnode(oldEndVnode, newStartVnode, insertedVnodeQueue);
        api.insertBefore(parentElm, oldEndVnode.elm!, oldStartVnode.elm!);
        oldEndVnode = oldCh[--oldEndIdx];
        newStartVnode = newCh[++newStartIdx];

      // 以上四个都未命中
      } else {
        if (oldKeyToIdx === undefined) {
          oldKeyToIdx = createKeyToOldIdx(oldCh, oldStartIdx, oldEndIdx);
        }
        // 拿新节点 key ，能否对应上 oldCh 中的某个节点的 key
        idxInOld = oldKeyToIdx[newStartVnode.key as string];

        // 没对应上
        if (isUndef(idxInOld)) { // New element
          api.insertBefore(parentElm, createElm(newStartVnode, insertedVnodeQueue), oldStartVnode.elm!);
          newStartVnode = newCh[++newStartIdx];

        // 对应上了
        } else {
          // 对应上 key 的节点
          elmToMove = oldCh[idxInOld];

          // sel 是否相等（sameVnode 的条件）
          if (elmToMove.sel !== newStartVnode.sel) {
            // New element
            api.insertBefore(parentElm, createElm(newStartVnode, insertedVnodeQueue), oldStartVnode.elm!);

          // sel 相等，key 相等
          } else {
            patchVnode(elmToMove, newStartVnode, insertedVnodeQueue);
            oldCh[idxInOld] = undefined as any;
            api.insertBefore(parentElm, elmToMove.elm!, oldStartVnode.elm!);
          }
          newStartVnode = newCh[++newStartIdx];
        }
      }
    }
    if (oldStartIdx <= oldEndIdx || newStartIdx <= newEndIdx) {
      if (oldStartIdx > oldEndIdx) {
        before = newCh[newEndIdx + 1] == null ? null : newCh[newEndIdx + 1].elm;
        addVnodes(parentElm, before, newCh, newStartIdx, newEndIdx, insertedVnodeQueue);
      } else {
        removeVnodes(parentElm, oldCh, oldStartIdx, oldEndIdx);
      }
    }
}
```

![image-20201124195320021](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201124195320021.png)

### diff算法总结

1. h函数包裹node信息，返回vnode函数

2. vnode函数，返回一个vnode对象{sel，data，children，text（children与text不能共存），elm（vnode是对应的那个dom元素），key（可以理解成v-for的key）}

3. + patch函数，参数（oldVnode，vnode），如果第一个参数不是Vnode，则执行emptyNodeAt函数（把dom和vnode做一个绑定）创建一个空的vnode，关联到这个dom元素；
   + 调用sameVnode函数，去比对新旧node的sel和key值是否相等；
   + 如果相等，调用pathchVnode函数再进行比对
   + 如果不相等，直接将旧node删掉（removeVnodes）重建（createElm）

4. patchVnode函数

   + 设置 vnode.elem
   + 定义oldCh（旧children），ch（新children）
   + 如果新node没有text文本内容
     + 如果新旧node都有children
       + 如果新旧children不相等，调用updateChildren函数去比对新旧children
     + 如果新children有，旧children无（旧text有）
       + 清空旧text
       + 调用addVnodes函数添加新children
     + 如果旧children有，新children无
       + 删除旧节点
     + 如果旧text有，清空旧text
   + 如果新node有text文本（没有children）
     + 删除旧node
     + 设置新text

5. updateChildren函数

   + 遍历新旧的节点

   ![image-20201124195320021](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201124195320021.png)

   + 排除一些节点空值
   + 开始和开始对比
   + 结束和结束对比
   + 开始和结束对比
   + 结束和开始对比
   + 如果以上其中一个对比上了，则调用patchVnode函数，再去比对，递归循环比对完成后，直到合成一个，则完成
   + 如果以上四个都未命中，拿新node的key能否能对比上旧node的key
     + 如果没有对应上，则重建插入
     + 如果对应上了，则比对sel
       + 如果sel不同，则重建插入
       + 如果sel相同，则再去调用patchVnode函数比对，最后合成一个

### key值为什么不能用随机数或者index

随机数：因为要拿key去比对新旧的节点是否相同，如果以随机数作为key，则无法比对，都不相同

index：如果节点发生了重新排序，则会新旧节点出现混乱

减少渲染次数，提升渲染性能

## 模板编译

+ 模板时vue开发种最常用的部分，即与使用相关联的原理
+ 它不是html，有指令、插值、JS表达式，到底是什么？
+ 面试不会直接问，但会通过“组件渲染和更新过程”考察

### with语法

![image-20201124205250530](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201124205250530.png)

+ 改变{}内自由变量的查找规则，当作obj属性来查找
+ 如果找不到匹配的obj属性，就会报错
+ with要慎用，它打破了作用域规则，易读性变差

### 模板编译

```js
const compiler = require('vue-template-compiler')

// 插值
// const template = `<p>{{message}}</p>`
// with(this){return createElement('p',[createTextVNode(toString(message))])}
// h -> vnode
// createElement -> vnode

// // 表达式
// const template = `<p>{{flag ? message : 'no message found'}}</p>`
// // with(this){return _c('p',[_v(_s(flag ? message : 'no message found'))])}

// // 属性和动态属性
// const template = `
//     <div id="div1" class="container">
//         <img :src="imgUrl"/>
//     </div>
// `
// with(this){return _c('div',
//      {staticClass:"container",attrs:{"id":"div1"}},
//      [
//          _c('img',{attrs:{"src":imgUrl}})])}

// // 条件
// const template = `
//     <div>
//         <p v-if="flag === 'a'">A</p>
//         <p v-else>B</p>
//     </div>
// `
// with(this){return _c('div',[(flag === 'a')?_c('p',[_v("A")]):_c('p',[_v("B")])])}

// 循环
// const template = `
//     <ul>
//         <li v-for="item in list" :key="item.id">{{item.title}}</li>
//     </ul>
// `
// with(this){return _c('ul',_l((list),function(item){return _c('li',{key:item.id},[_v(_s(item.title))])}),0)}

// 事件
// const template = `
//     <button @click="clickHandler">submit</button>
// `
// with(this){return _c('button',{on:{"click":clickHandler}},[_v("submit")])}

// v-model
const template = `<input type="text" v-model="name">`
// 主要看 input 事件
// with(this){return _c('input',{directives:[{name:"model",rawName:"v-model",value:(name),expression:"name"}],attrs:{"type":"text"},domProps:{"value":(name)},on:{"input":function($event){if($event.target.composing)return;name=$event.target.value}}})}

// render 函数
// 返回 vnode
// patch

// 编译
const res = compiler.compile(template)
console.log(res.render)

// ---------------分割线--------------

// // 从 vue 源码中找到缩写函数的含义
// function installRenderHelpers (target) {
//     target._o = markOnce;
//     target._n = toNumber;
//     target._s = toString;
//     target._l = renderList;
//     target._t = renderSlot;
//     target._q = looseEqual;
//     target._i = looseIndexOf;
//     target._m = renderStatic;
//     target._f = resolveFilter;
//     target._k = checkKeyCodes;
//     target._b = bindObjectProps;
//     target._v = createTextVNode;
//     target._e = createEmptyVNode;
//     target._u = resolveScopedSlots;
//     target._g = bindObjectListeners;
//     target._d = bindDynamicKeys;
//     target._p = prependModifier;
// }
```

+ vue-template-compiler模板template编译成render函数，执行render函数返回vnode
+ 基于vnode再执行patch和diff
+ 使用webpack vue-loader，会在开发环境下编译模板（重要）

### v-model

原理是在dom添加了input事件，name属性设为$event.target.value，将name赋值为value

### vue组件种使用render代替template

![image-20201124212323652](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201124212323652.png)

+ 有些复杂情况下，不能用template，可以考虑用render
+ React一直都用render（没有模板）

### 总结

+ with语法
+ 模板到render函数，再到vnode，再到渲染和更新
+ vue组件可以用render代替template

## 渲染过程

+ 一个组件渲染到页面，修改data触发更新（数据驱动视图）
+ 其背后的原理是什么，需要掌握哪些要点？
+ 考察对流程了解的全面程度vnode

### 初次渲染过程

+ 解析模板为render函数（或在开发环境已完成，vue-loader）
+ 触发响应式，监听data属性getter  setter
+ 执行render函数，调用getter方法，生成vnode，patch（elem，vnode）

![image-20201124213946417](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201124213946417.png)

### 更新过程

+ 修改data，触发setter（此前在getter中已被监听）
+ 重新执行render函数，生成newVnode
+ pathch（vnode，newVnode）

### 完成流程图

![image-20201124214305230](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201124214305230.png)

### 异步渲染

+ 减少DOM操作次数，提高性能，汇总data的修改，一次性更新视图

## 前端路由

+ 稍微复杂一点的SPA，都需要路由

### hash的特点

+ hash变化会触发网页跳转，即浏览器的前进、后退
+ hash变化不会刷新页面，SPA必须的特点
+ hash永远不会提交到server端

监听hashchange事件，去监听新旧hash值，去触发渲染

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>hash test</title>
</head>
<body>
    <p>hash test</p>
    <button id="btn1">修改 hash</button>

    <script>
        // hash 变化，包括：
        // a. JS 修改 url
        // b. 手动修改 url 的 hash
        // c. 浏览器前进、后退
        window.onhashchange = (event) => {
            console.log('old url', event.oldURL)
            console.log('new url', event.newURL)

            console.log('hash:', location.hash)
        }

        // 页面初次加载，获取 hash
        document.addEventListener('DOMContentLoaded', () => {
            console.log('hash:', location.hash)
        })

        // JS 修改 url
        document.getElementById('btn1').addEventListener('click', () => {
            location.href = '#/user'
        })
    </script>
</body>
</html>
```

### H5 history

+ 用url规范的路由，但跳转时不刷新页面
+ history.pushState
+ window.onpopstate

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>history API test</title>
</head>
<body>
    <p>history API test</p>
    <button id="btn1">修改 url</button>

    <script>
        // 页面初次加载，获取 path
        document.addEventListener('DOMContentLoaded', () => {
            console.log('load', location.pathname)
        })

        // 打开一个新的路由
        // 【注意】用 pushState 方式，浏览器不会刷新页面
        document.getElementById('btn1').addEventListener('click', () => {
            const state = { name: 'page1' }
            console.log('切换路由到', 'page1')
            history.pushState(state, '', 'page1') // 重要！！
        })

        // 监听浏览器前进、后退
        window.onpopstate = (event) => { // 重要！！
            console.log('onpopstate', event.state, location.pathname)
        }

        // 需要 server 端配合，可参考
        // https://router.vuejs.org/zh/guide/essentials/history-mode.html#%E5%90%8E%E7%AB%AF%E9%85%8D%E7%BD%AE%E4%BE%8B%E5%AD%90
    </script>
</body>
</html>
```

# Vue3

## Vue3 升级内容

+ 全部用ts重写（响应式、vdom、模板编译等）
+ 性能提升，打包代码量减少
+ 会调整部分API

## Proxy 实现响应式

### Proxy基本使用

![image-20201125090321966](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201125090321966.png)

### Reflect使用

+ 和Proxy能力一一对应
+ 规范化、标准化、函数式
+ 代替Object上的工具函数

### 实现

```js
// 创建响应式
function reactive(target = {}) {
    if (typeof target !== 'object' || target == null) {
        // 不是对象或数组，则返回
        return target
    }

    // 代理配置
    const proxyConf = {
        get(target, key, receiver) {
            // 只处理本身（非原型的）属性
            const ownKeys = Reflect.ownKeys(target)
            if (ownKeys.includes(key)) {
                console.log('get', key) // 监听
            }
    
            const result = Reflect.get(target, key, receiver)
        
            // 深度监听
            // 性能如何提升的？
            return reactive(result)
        },
        set(target, key, val, receiver) {
            // 重复的数据，不处理
            if (val === target[key]) {
                return true
            }
    
            const ownKeys = Reflect.ownKeys(target)
            if (ownKeys.includes(key)) {
                console.log('已有的 key', key)
            } else {
                console.log('新增的 key', key)
            }

            const result = Reflect.set(target, key, val, receiver)
            console.log('set', key, val)
            // console.log('result', result) // true
            return result // 是否设置成功
        },
        deleteProperty(target, key) {
            const result = Reflect.deleteProperty(target, key)
            console.log('delete property', key)
            // console.log('result', result) // true
            return result // 是否删除成功
        }
    }

    // 生成代理对象
    const observed = new Proxy(target, proxyConf)
    return observed
}

// 测试数据
const data = {
    name: 'zhangsan',
    age: 20,
    info: {
        city: 'beijing',
        a: {
            b: {
                c: {
                    d: {
                        e: 100
                    }
                }
            }
        }
    }
}

const proxyData = reactive(data)
```

# React原理

## React使用

### setState

1、不可变值（函数式编程，纯函数）

+ 不能直接对this.state.list进行push pop splice等，这样违反不可变值
+ 不能直接对this.state.obj进行属性设置，这样违反不可变值

2、可能是异步更新

+ setState是异步的，拿不到最新值
+ 当setState中加入回调函数，就可以拿到最新值
+ setTimeout中的setState，是同步的
+ 自己定义的DOM事件中使用，是同步的

3、可能会被合并

+ setState传入的对象的话，因为是异步更新的，会将结果合并起来，相当于object.assign({count:1},{count:1})
+ setState传入函数（参数：preState，props）的话，不会被合并

### 组件生命周期

![image-20201127075714595](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201127075714595.png)

### React高级特性

#### 函数组件

![image-20201127081242071](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201127081242071.png)

+ 纯函数，输入props，输出JSX
+ 没有实例，没有生命周期，没有state
+ 不能扩展其他方法

#### 非受控组件

+ defaultValue、defaultChecked 、ref

使用场景：

+ 必须手动操作DOM元素，setState实现不了
+ 文件上传
+ 某些富文本编辑器，需要传入DOM元素

受控组件VS非受控组件

+ 优先使用受控组件，符合React设计原则
+ 必须操作DOM时，再使用非受控组件

#### Portals(传送门)

使用场景

+ overflow：hidden
+ 父组件z-index值太小
+ fixed需要放在body第一层级

#### context

+ 公共信息（语言、主题）如何传递给每个组件？
+ 用props太繁琐
+ 用redux小题大做

#### 异步组件

+ import()
+ React.lazy
+ React.Suspense

### 性能优化

+ shouldComponentUpdata（简称scu）(nextProps,nextState)
+ PureComponent和React.meno
+ 不可变值immutable.js

#### SCU使用总结

+ 默认返回true，即React默认重新渲染所有子组件
+ 必须配合“不可变值”一起使用
+ 可先不用SCU，有性能问题的时候再考虑使用

#### PureComponent和meno

+ PureComponent，SCU中实现了浅比较
+ meno，函数组件中的PureComponent
+ 浅比较已使用大部分情况（尽量不要做深比较）

#### immutable.js

+ 彻底拥抱“不可变值”
+ 基于共享数据（不是深拷贝），速度好
+ 有一定的学习成本和迁移成本，按需使用

#### 组件公共逻辑的抽离

+ mixin，已被React弃用
+ 高阶组件HOC：模式简单，但会增加组件层级
+ Render Props：代码整洁，学习成本较高

### redux使用

+ 不可变值，纯函数

#### 单项数据流概述

+ dispatch（action）
+ reducer -> newState（不可变值）
+ subscribe触发通知

#### 中间件

![image-20201127230933158](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201127230933158.png)

![image-20201127231330600](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201127231330600.png)

![image-20201127231350783](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201127231350783.png)

### React-router

+ 路由模式（hash、H5 history）
+ 路由配置

## React原理

### 函数式编程

+ 一种编程范式，概念比较多
+ 纯函数
+ 不可变值

### 回顾vdom和diff

+ h函数
+ vnode数据结构
+ patch函数

+ 值比较同一层级，不跨级比较
+ tag不相同，则直接删除重建，不再深度比较
+ tag和key，两者都相同，则认为是相同节点，不再深度比较

### JSX本质

+ JSX等同于Vue模板
+ Vue模板不是html
+ JSX也不是JS

#### JSX模板编译

React.createElement("tag",null,React.createElement(child1),React.createElement(child2))->普通

React.createElement(List,null,React.createElement(child1),React.createElement(child2))->组件（首字母大写来判别）

React.createElement("tag",null,[React.createElement(child1),React.createElement(child2)])->子节点可以用数组包裹，比如map时，就是用数组遍历

相当于h函数，返回vnode(很早之前是不支持jsx的，是直接用createElement原生来写)

![image-20201128110136172](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201128110136172.png)

![image-20201128105731002](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201128105731002.png)

### 合成事件

1. event是SyntheticEvent，模拟出来DOM事件所有能力
2. event.nativeEvent是原生事件对象
3. 所有的事件，都被挂载到document上
4. 和DOM事件不一样，和Vue事件也不一样

![image-20201128111411298](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201128111411298.png)

为什么要合成事件机制？

+ 更好的兼容性和跨平台
+ 载到document，减少内存消耗，避免频繁解绑 
+ 方便事件的统一管理（如事务机制）

### setState和batchUpdate

+ 有时异步（普通使用），有时同步（setTimeout、DOM事件）
+ 有时合并（对象形式），有时不合并（函数形式）
+ 后者比较好理解（像Object.assign）

#### 核心要点

##### setState主流程

![image-20201128112755557](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201128112755557.png)

![image-20201128112901216](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201128112901216.png)

![image-20201128112922583](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201128112922583.png)

![image-20201128113201624](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201128113201624.png)

##### setState异步还是同步？

+ setState无所谓异步还是同步
+ 看是否能命中batchUpdate机制
+ 判断isBatchingUpdates

##### 那些能命中batchUpdate机制（看入口，异步）

+ 生命周期（和它调用的函数）
+ React中注册的事件（和他调用的函数）
+ React可以“管理”的入口

##### 哪些不能命中batchUpdate机制（同步）

+ setTimeout、setInterval等（和它调用的函数）
+ 自定义的DOM事件（和它调用的函数）
+ React“管不到”的入口

##### transaction事务机制

![image-20201128113836418](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201128113836418.png)

![image-20201128113902437](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201128113902437.png)

![image-20201128114043188](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201128114043188.png)

### 组件渲染和更新过程

+ JSX如何渲染为页面
+ setState之后如何更新页面
+ 面试考察全流程

#### 组件渲染过程

+ props state
+ render()生成vnode
+ patch(elem,vnode)

#### 组件更新过程

+ setState（newState）--> dirtyComponents(可能有子组件)
+ render()生成newVnode
+ patch(vnode,newVnode)

更新的两个阶段

+ 上述的patch被拆分为两个阶段：
+ reconciliation阶段--执行diff算法，纯JS计算
+ commit阶段--将diff结构渲染DOM

可能会有性能问题

+ JS是单线程，且和DOM渲染共用一个线程
+ 当组件足够复杂，组件更新时计算和渲染都压力大
+ 同时再有DOM操作需求（动画，鼠标拖拽等），将卡顿

### Fiber解决性能问题

+ 将reconciliation阶段进行任务拆分（commit无法拆分）
+ DOM需要渲染时暂停、空闲时恢复
+ window.requestIdleCallback(去检测DOM需要渲染)

## React和Vue的区别

+ 都是数据驱动视图
+ 都可以使用组件化
+ 都使用vdom操作DOM
+ React使用JSX拥抱JS，Vue使用模板拥抱html
+ React函数式编程，Vue声明式编程
+ React更多需要自力更生，Vue把想要的都给你

# Webpack原理

## webpack基本配置

### 拆分配置和merge

#### package.json

```js
"scripts": {
    "devBuild": "webpack --config build-optimization（文件夹名）/webpack.dev.js",
    "dev": "webpack-dev-server --config build-optimization（文件夹名）/webpack.dev.js",
    "build": "webpack --config build-optimization（文件夹名）/webpack.prod.js"
}
```

![image-20201128161627259](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201128161627259.png)

#### common

```js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const { srcPath, distPath } = require('./paths')

module.exports = {
    entry: path.join(srcPath, 'index'),
    module: {
        rules: [
            {
                test: /\.js$/,
                loader: ['babel-loader'],
                include: srcPath,
                exclude: /node_modules/
            },
            // {
            //     test: /\.vue$/,
            //     loader: ['vue-loader'],
            //     include: srcPath
            // },
            // {
            //     test: /\.css$/,
            //     // loader 的执行顺序是：从后往前（知识点）
            //     loader: ['style-loader', 'css-loader']
            // },
            {
                test: /\.css$/,
                // loader 的执行顺序是：从后往前
                loader: ['style-loader', 'css-loader', 'postcss-loader'] // 加了 postcss
            },
            {
                test: /\.less$/,
                // 增加 'less-loader' ，注意顺序
                loader: ['style-loader', 'css-loader', 'less-loader']
            }
        ]
    },
    plugins: [
        new HtmlWebpackPlugin({
            template: path.join(srcPath, 'index.html'),
            filename: 'index.html'
        })
    ]
}
```

#### dev

```js
const path = require('path')
const webpack = require('webpack')
const webpackCommonConf = require('./webpack.common.js')
const { smart } = require('webpack-merge')
const { srcPath, distPath } = require('./paths')

module.exports = smart(webpackCommonConf, {
    mode: 'development',
    module: {
        rules: [
            // 直接引入图片 url
            {
                test: /\.(png|jpg|jpeg|gif)$/,
                use: 'file-loader'
            }
        ]
    },
    plugins: [
        new webpack.DefinePlugin({
            // window.ENV = 'development'
            ENV: JSON.stringify('development')
        })
    ],
    devServer: {
        port: 8080,
        progress: true,  // 显示打包的进度条
        contentBase: distPath,  // 根目录
        open: true,  // 自动打开浏览器
        compress: true,  // 启动 gzip 压缩

        // 设置代理
        proxy: {
            // 将本地 /api/xxx 代理到 localhost:3000/api/xxx
            '/api': 'http://localhost:3000',

            // 将本地 /api2/xxx 代理到 localhost:3000/xxx
            '/api2': {
                target: 'http://localhost:3000',
                pathRewrite: {
                    '/api2': ''
                }
            }
        }
    }
})

```

#### prod

```js
const path = require('path')
const webpack = require('webpack')
const { CleanWebpackPlugin } = require('clean-webpack-plugin')
const webpackCommonConf = require('./webpack.common.js')
const { smart } = require('webpack-merge')
const { srcPath, distPath } = require('./paths')

module.exports = smart(webpackCommonConf, {
    mode: 'production',
    output: {
        filename: 'bundle.[contentHash:8].js',  // 打包代码时，加上 hash 戳
        path: distPath,
        // publicPath: 'http://cdn.abc.com'  // 修改所有静态文件 url 的前缀（如 cdn 域名），这里暂时用不到
    },
    module: {
        rules: [
            // 图片 - 考虑 base64 编码的情况
            {
                test: /\.(png|jpg|jpeg|gif)$/,
                use: {
                    loader: 'url-loader',
                    options: {
                        // 小于 5kb 的图片用 base64 格式产出
                        // 否则，依然延用 file-loader 的形式，产出 url 格式
                        limit: 5 * 1024,

                        // 打包到 img 目录下
                        outputPath: '/img1/',

                        // 设置图片的 cdn 地址（也可以统一在外面的 output 中设置，那将作用于所有静态资源）
                        // publicPath: 'http://cdn.abc.com'
                    }
                }
            },
        ]
    },
    plugins: [
        new CleanWebpackPlugin(), // 会默认清空 output.path 文件夹
        new webpack.DefinePlugin({
            // window.ENV = 'production'
            ENV: JSON.stringify('production')
        })
    ]
})

```

### 启动本地服务

```js
devServer: {
    port: 8080,
    progress: true,  // 显示打包的进度条
    contentBase: distPath,  // 根目录
    open: true,  // 自动打开浏览器
    compress: true,  // 启动 gzip 压缩

    // 设置代理
    proxy: {
        // 将本地 /api/xxx 代理到 localhost:3000/api/xxx
        '/api': 'http://localhost:3000',

        // 将本地 /api2/xxx 代理到 localhost:3000/xxx
        '/api2': {
            target: 'http://localhost:3000',
            pathRewrite: {
                '/api2': ''
            }
        }
    }
}
```

### 处理ES6(babel)

```js
{
    test: /\.js$/,
    loader: ['babel-loader'],
    include: srcPath,
    exclude: /node_modules/
}
```

### 处理样式

```js
{
    test: /\.css$/,
    // loader 的执行顺序是：从后往前
    loader: ['style-loader', 'css-loader', 'postcss-loader'] // 加了 postcss
},
{
    test: /\.less$/,
    // 增加 'less-loader' ，注意顺序
    loader: ['style-loader', 'css-loader', 'less-loader']
}
```

postcss为了更好的浏览器兼容（CSS3），还需要添加配置postcss.config.js

```js
module.exports = {
    plugins: [require('autoprefixer')]
}
```

### 处理图片

```js
module: {
    rules: [
        // 图片 - 考虑 base64 编码的情况
        {
            test: /\.(png|jpg|jpeg|gif)$/,
            use: {
                loader: 'url-loader',
                options: {
                    // 小于 5kb 的图片用 base64 格式产出
                    // 否则，依然延用 file-loader 的形式，产出 url 格式
                    limit: 5 * 1024,

                    // 打包到 img 目录下
                    outputPath: '/img1/',

                    // 设置图片的 cdn 地址（也可以统一在外面的 output 中设置，那将作用于所有静态资源）
                    // publicPath: 'http://cdn.abc.com'
                }
            }
        },
    ]
}
```

### 模块化

## webpack高级配置

### 多入口

```js
entry: {
    index: path.join(srcPath, 'index.js'),
    other: path.join(srcPath, 'other.js')
}
```

```js
output: {
    // filename: 'bundle.[contentHash:8].js',  // 打包代码时，加上 hash 戳
    filename: '[name].[contentHash:8].js', // name 即多入口时 entry 的 key
    path: distPath,
    // publicPath: 'http://cdn.abc.com'  // 修改所有静态文件 url 的前缀（如 cdn 域名），这里暂时用不到
}
```

```js
plugins: [
    // new HtmlWebpackPlugin({
    //     template: path.join(srcPath, 'index.html'),
    //     filename: 'index.html'
    // })

    // 多入口 - 生成 index.html
    new HtmlWebpackPlugin({
        template: path.join(srcPath, 'index.html'),
        filename: 'index.html',
        // chunks 表示该页面要引用哪些 chunk （即上面的 index 和 other），默认全部引用
        chunks: ['index']  // 只引用 index.js，对应entry的名字
    }),
    // 多入口 - 生成 other.html
    new HtmlWebpackPlugin({
        template: path.join(srcPath, 'other.html'),
        filename: 'other.html',
        chunks: ['other']  // 只引用 other.js，对应entry的名字
    })
]
```

```js
plugins: [
    new CleanWebpackPlugin(), // 会默认清空 output.path 文件夹，重新打包生成
]
```

### 抽离CSS文件

在prod中使用抽离，优化项目

```js
module: {
    rules: [
        // 抽离 css
        {
            test: /\.css$/,
            loader: [
                MiniCssExtractPlugin.loader,  // 注意，这里不再用 style-loader
                'css-loader',
                'postcss-loader'
            ]
        },
        // 抽离 less --> css
        {
            test: /\.less$/,
            loader: [
                MiniCssExtractPlugin.loader,  // 注意，这里不再用 style-loader
                'css-loader',
                'less-loader',
                'postcss-loader'
            ]
        }
    ]
}
```

```js
plugins: [
    // 抽离 css 文件
    new MiniCssExtractPlugin({
        filename: 'css/main.[contentHash:8].css'
    })
]
```

```js
optimization: {
    // 压缩 css
    minimizer: [new TerserJSPlugin({}), new OptimizeCSSAssetsPlugin({})],
}
```

### 抽离公共代码

```js
optimization: {
    // 分割代码块
    splitChunks: {
        chunks: 'all',
        /**
         * initial 入口 chunk，对于异步导入的文件不处理
            async 异步 chunk，只对异步导入的文件处理
            all 全部 chunk
         */

        // 缓存分组
        cacheGroups: {
            // 第三方模块
            vendor: {
                name: 'vendor', // chunk 名称
                priority: 1, // 权限更高，优先抽离，重要！！！
                test: /node_modules/,
                minSize: 0,  // 大小限制
                minChunks: 1  // 最少复用过几次
            },

            // 公共的模块
            common: {
                name: 'common', // chunk 名称
                priority: 0, // 优先级
                minSize: 0,  // 公共模块的大小限制
                minChunks: 2  // 公共模块最少复用过几次
            }
        }
    }
}
```

```js
plugins: [
    // new HtmlWebpackPlugin({
    //     template: path.join(srcPath, 'index.html'),
    //     filename: 'index.html'
    // })

    // 多入口 - 生成 index.html
    new HtmlWebpackPlugin({
        template: path.join(srcPath, 'index.html'),
        filename: 'index.html',
        // chunks 表示该页面要引用哪些 chunk （即上面的 index 和 other），默认全部引用
        chunks: ['index', 'vendor', 'common']  // 要考虑代码分割
    }),
    // 多入口 - 生成 other.html
    new HtmlWebpackPlugin({
        template: path.join(srcPath, 'other.html'),
        filename: 'other.html',
        chunks: ['other', 'common']  // 考虑代码分割
    })
]
```

### 懒加载

```
import('url')
.then(res=>{
	console.log(res.default.message)
})	//注意这里的default
```

### 处理JSX

在用babel-loader基础上在.babelrc文件中配置

```
{
    "presets": ["@babel/preset-react"],
}
```

### 处理Vue

```
module: {
    rules: [
        {
            test: /\.vue$/,
            loader: ['vue-loader'],
            include: srcPath
        },
    ]
}
```

## module、chunk、bundle的区别

+ module - 各个源码文件（能被引用的），webpack中一切皆模块，（可以没有依赖，独立）
+ chunk - 多模块合并成的，如entry    import()    splitChunk，（每个模块有依赖）
+ bundle - 最终的输出文件

## webpack性能优化

### webpack构建速度

#### 优化babel-loader

![image-20201128204156155](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201128204156155.png)

#### happyPack多进程打包

+ JS单线程，开启多进程打包
+ 提高构建速度（特别是多核CPU）

```
module: {
    rules: [
        // js
        {
            test: /\.js$/,
            // 把对 .js 文件的处理转交给 id 为 babel 的 HappyPack 实例
            use: ['happypack/loader?id=babel'],
            include: srcPath,
            // exclude: /node_modules/
        }
    ]
}

plugins: [
    // happyPack 开启多进程打包
    new HappyPack({
        // 用唯一的标识符 id 来代表当前的 HappyPack 是用来处理一类特定的文件
        id: 'babel',
        // 如何处理 .js 文件，用法和 Loader 配置中一样
        loaders: ['babel-loader?cacheDirectory']
    })
]
```

#### ParallelUglifyPlugin多进程压缩JS

+ webpack内置Uglify工具压缩JS
+ JS单线程，开启多线程压缩更快
+ 和happyPack同理

```
plugins: [
    // 使用 ParallelUglifyPlugin 并行压缩输出的 JS 代码
    new ParallelUglifyPlugin({
        // 传递给 UglifyJS 的参数
        // （还是使用 UglifyJS 压缩，只不过帮助开启了多进程）
        uglifyJS: {
            output: {
                beautify: false, // 最紧凑的输出
                comments: false, // 删除所有的注释
            },
            compress: {
                // 删除所有的 `console` 语句，可以兼容ie浏览器
                drop_console: true,
                // 内嵌定义了但是只用到一次的变量
                collapse_vars: true,
                // 提取出出现多次但是没有定义成变量去引用的静态值
                reduce_vars: true,
            }
        }
    })
]
```

#### 关于开启多进程

+ 项目较大，打包较慢，开启多进程能提高速度
+ 项目较小，打包很快，开启多进程会降低速度（进程开销）
+ 按需使用

#### 自动刷新（不可用于生产环境）

![image-20201128205825700](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201128205825700.png)

#### 热更新（不可用于生产环境）

+ 自动刷新：整个网页全部刷新，速度较慢
+ 自动刷新：整个网页全部刷新，状态会丢失
+ 热更新：新代码生效，页面不刷新，状态不丢失

```js
entry: {
    // index: path.join(srcPath, 'index.js'),
    index: [
        'webpack-dev-server/client?http://localhost:8080/',
        'webpack/hot/dev-server',
        path.join(srcPath, 'index.js')
    ],
    other: path.join(srcPath, 'other.js')
}
```

```js
plugins: [
    new HotModuleReplacementPlugin()
]
```

```js
devServer: {
    port: 8080,
    progress: true,  // 显示打包的进度条
    contentBase: distPath,  // 根目录
    open: true,  // 自动打开浏览器
    compress: true,  // 启动 gzip 压缩

    hot: true,//这里设置开启热更新

    // 设置代理
    proxy: {
        // 将本地 /api/xxx 代理到 localhost:3000/api/xxx
        '/api': 'http://localhost:3000',

        // 将本地 /api2/xxx 代理到 localhost:3000/xxx
        '/api2': {
            target: 'http://localhost:3000',
            pathRewrite: {
                '/api2': ''
            }
        }
    }
}
```

index.js

```js
// 增加，开启热更新之后的代码逻辑
if (module.hot) {
    module.hot.accept(['./math'], () => {
        const sumRes = sum(10, 30)
        console.log('sumRes in hot', sumRes)
    })
}
```

#### DllPlugin动态链接库插件（不可用于生产环境）

+ 前端框架如vue React，体积大，构建慢
+ 较稳定，不尝升级版本
+ 同一个版本只构建一次即可，不用每次都重新构建
+ webpack已内置Dllplugin支持
+ Dllplugin - 打包出dll文件
+ DllReferencePlugin - 使用dll文件

尤雨溪：dll 配置将会被移除，因为 Webpack 4 的打包性能足够好的，dll 没有在 Vue ClI 里继续维护的必要了。

### webpack产出代码

+ 体积更小
+ 合理分包，不重复加载
+ 速度更快，内存使用更小

#### 小图片base64编码

#### bundle加hash

#### 懒加载

#### 提取公因代码

#### 使用CDN加速

#### 使用production（webpack4之后）

+ 自动开启代码压缩
+ Vue React等会自动删除调试代码（如开发环境的warning）
+ 启动Tree - Shaking

#### Tree - Shaking

1、在开发环境(mode: 'development')下, 配置：

```js
optimization: {
  usedExports: true
}
```

webpack4.x 默认开启treeshaking, 当开启(mode: 'production')生产环境模式，可以注释掉这个配置。此配置在打包好的文件里可以看到以下信息：

```
/*! exports used: Machine */
```

2、在package.json里加入以下配置，告知模块打包时本模块可以被shake。

sideEffects 表示有副作用的函数（模块）。如果设置为 false，表示本模块没有副作用，可以shake掉了。

```json
"sideEffects": false,
```

3、如果在package.json里没有设置sideEffects，那么该模块不能被shake。
我们也可以定义模块某些导入的模块不能shake，如css文件。则做如下配置：

```json
"sideEffects": [
  "*.css"
],
```

ES6 Module可以Tree - Shaking，Commonjs不能

+ ES6 Module静态引用，编译时引用
+ Commonjs动态引入，执行时引入

#### Scope Hosting

+ 代码体积更小
+ 创建函数作用域更少
+ 代码可读性更好

![image-20201128220337770](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201128220337770.png)

![image-20201128220400597](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201128220400597.png)

![image-20201128220511758](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201128220511758.png)

## babel

+ 前端开发环境必备工具
+ 同webpack，需要了解基本的配置和使用

#### 环境搭建

package.json

```
"devDependencies": {
    "@babel/cli": "^7.7.5",
    "@babel/core": "^7.7.5",
    "@babel/plugin-transform-runtime": "^7.7.5",
    "@babel/preset-env": "^7.7.5"
},
"dependencies": {
    "@babel/polyfill": "^7.7.0",
    "@babel/runtime": "^7.7.5"
}
```

.babelrc

```js
{
    "presets": [
        [
            "@babel/preset-env",
            {
                "useBuiltIns": "usage",//按需引用babel-polyfill
                "corejs": 3
            }
        ]
    ],
    "plugins": [
        [
            "@babel/plugin-transform-runtime",
            {
                "absoluteRuntime": false,
                "corejs": 3,
                "helpers": true,
                "regenerator": true,
                "useESModules": false
            }
        ]
    ]
}
```

#### babel-polyfill（补丁）

+ 什么是polifill

也就是 babel6 中的 babel-polyfill , 使用 preset-env 能将最新的语法转换为ecmascript5的写法，当我们需要使用新增的全局函数(比如promise, Array.from)和实例方法(比如 Array.prototype.includes )时就需要引入 polyfill ，一般用法 在 index.js文件的最上层引入，或是在打包文件的entry 入口处引入，这样做的缺点是会全局引入整个 polyfill包，比如promise 会全局引入，污染全局环境。

+ core-js（新语法的函数库）和regenerator（解决无法解析generator）
+ babel-polyfill是两者的集合

+ Babel7.4之后弃用babel-polifill
+ 推荐直接使用core-js和regenerator

#### babel-runtime

promise 会全局引入，污染全局环境

重新给编译函数取个名字，就不会污染全局环境

## 前端为何要进行打包和构建？

+ 体积更小（Tree-Shaking、压缩、合并），加载更快
+ 编译高级语言或语法（TS，ES6+，模块化，scss）
+ 兼容性和错误检查（Polyfill，postcss，eslint）
+ 统一、高效的开发环境
+ 统一的构建环境和产出标准
+ 继承公司构建规范（提测、上线等）

## babel和webpack的区别

+ babel - JS新语法编译工具，不关心模块化
+ webpack - 打包构建工具，是多个loader、plugin的集合

## polifill和runtime的区别

+ polifill会污染全局变量
+ runtime不会污染全局遍历
+ 产出第三方lib要用babel-runtime

## 如何产出一个lib

![image-20201128225136485](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201128225136485.png)

## webpack如何实现懒加载

![image-20201128225203433](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201128225203433.png)

## 为何Proxy不能被Ployfill？

![image-20201128225252547](C:\Users\86252\AppData\Roaming\Typora\typora-user-images\image-20201128225252547.png)