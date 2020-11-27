## 一、BOM的概念

###  0、XMind思维导图

###  1、什么是BOM？

```js
BOM的全称是Brower Object Model，译为浏览器对象模型。它是用来操作的浏览器行为，并与浏览器进行互动的一种结构模式。`
ECMAScript 是 JavaScript 的核心，但如果要在 Web 中使用 JavaScript，那么 BOM（浏览器对象模型）

则无疑才是真正的核心。BOM 提供了很多对象，用于访问浏览器的功能，这些功能与任何网页内容无关。多年来，

缺少事实上的规范导致 BOM 有很多问题，因为浏览器提供商会按照各自的想法随意去扩展它。W3C 为了把浏览器中

JavaScript 最基本的部分标准化，已经将 BOM 的主要方面纳入了 HTML5 的规范中
```

###  2、BOM的组成

```js
`BOM是由多个对象组成，其中代表浏览器窗口的Window对象是BOM的顶层对象，其他对象都是该对象的子对象。BOM提供了一些访问窗口对象的一些方法，我们可以用它来移动窗口位置，改变窗口大小，打开新窗口和关闭窗口，弹出对话框，进行导航以及获取客户的一些信息如：浏览器品牌版本，屏幕分辨率。但BOM最强大的功能是它提供了一个访问HTML页面的一入口——document对象，以使得我们可以通过这个入口来使用DOM的强大功能！！！`
```

## 二、BOM对象

### 1、window

#### 描述：

```js
window作为BOM中的顶级对象。window对象是BOM的顶层(核心)对象，所有对象都是通过它延伸出来的，也可以称为window的子对象。由于window是顶层对象，因此调用它的子对象时可以不显示的指明window对象，例如下面两行代码是一样的：

document.write("BOM"); 

window.document.write("BOM");

window对象表示整个浏览器窗口。

JavaScript中的任何一个全局函数或变量都是window的成员。全局变量是 window 对象的属性。全局函数是 window 对象的方法。例如：alert，定时器都是window对象的方法。

BOM 的核心对象是 window，它表示浏览器的一个实例。在浏览器中，window 对象有双重角色，
它既是通过 JavaScript 访问浏览器窗口的一个接口，又是 ECMAScript 规定的 Global 对象。
这意味着在网页中定义的任何一个对象、变量和函数，都以 window 作为其 Global 对象，因此有权访问 
isNaN()、isFinite()、parseInt()、parseFloat() 等方法。
```

###   全局变量与windows对象属性的差别

 ```js
抛开全局变量会成为 window 对象的属性不谈，定义全局变量与在 window 对象上直接定义属性还是有一点差别：
全局变量不能通过 delete 运算符删除，而直接在 window 对象上的定义的属性可以
 ```

#### 打开、关闭方法：

```js
//不用，浏览器都有弹窗拦截功能
open("要打开的网页","网页的名字","打开的网页设置");

close();//关闭打开的页面

document.onmousemove=function(){
   close();//关闭
   open("http://www.163.com","网易","width=400,height=400");//打开
 }
```

#### 浏览器内部宽/高度方法：

```js
//innerHeight  innerWidth
//兼容所有浏览器
//且包含滚动条
```

#### 整个浏览器宽/高度方法：

```js
//outerHeight outerWidth
//可以获取到整个浏览器窗口的整个宽高
```

#### 浏览器距屏幕位置方法：

```js
//screenLeft  screenTop
//screenX   screenY
window.screenLeft火狐不支持，火狐得使用window.screenX
window.screenX   ie不支持，ie得使用window.screenLeft
```

### 2、location

#### 重载页面

```js
//重新加载、刷新当前页面
localtion.reload();
```

#### 跳转页面

```js
//href  可以获取到当前页面的地址，且会产生历史记录
location.href("http://www.163.com")
//assign 无法获取当前页面地址，且会产生历史记录
location.assign("http://www.163.com")
//repalce   不产生历史记录
location.repalce("http://www.163.com")
```

#### 获取URL

```js
//hash 获取url#之后的字符串(包括#)，如果没有，则返回空字符串
location.hash

//search 获取URL中?之后的参数
location.search

//hostname 获取web主机的域名
location.hostname

//pathname  获取当前页面的路径很文件名
location.pathname

//port  获取当前URL地址的端口
location.port

//protocol 获取当前URL使用的协议
loocation.pprotocol
```

### 3、history

#### 历史记录的前进后退

```js
//后退
history.back()
//前进
history.forward()
//跳转 既可以前进也可以后退  
/*
   该方法中既可以传递number整数类型的参数，也可以传递string类型参数
   当传递number整数类型的参数的时候
        0       刷新当前页面
        正整数n   前进n页
        负整数n   后退n页
    当传递string类型的参数的时候
        此时浏览器会跳转到历史记录中包含该字符串的第一个位置，可能前进，也可能后退。
        如果历史记录中不包含该字符串，那么该方法什么也不做
*/
history.go()
```

#### 添加、更新历史记录

```js
/*
添加历史记录
history.pushState(state, title[, url])


更新历史记录
history.replaceState(state, title, url)


当活动历史记录条目更改时，将触发popstate事件。如果被激活的历史记录条目是通过对history.pushState（）的调用创建的，或者受到对history.replaceState（）的调用的影响，popstate事件的state属性包含历史条目的状态对象的副本。

补充:查看历史记录的长度 history.length
*/
```

### 4、screen

#### 不包含任务栏的屏幕宽高

```js
/*
   availHeight和availWidth
*/
```

#### 包含任务栏的屏幕宽高

```js
/*
   width和height
*/
```

### 5、navigator

#### userAgent

```js
/*
  userAgent返回由客户机发送服务器的 user-agent 头部的值
*/
console.log(navigatro.userAgent);
```

#### appName

```js
/*
  appName  是用来获取浏览器的名称的
*/
```

#### appVersion

```js
/*
 appVersion   获取浏览器的平台和版本信息
*/
```

#### platform

```js
/*
返回运行浏览器的操作系统的平台
*/
```

## 案例_历史列表

实现了不刷新浏览器页面跳转

```js
<div>水果</div>
    <div>蔬菜</div>
    <div>零食</div>
    <div>饮料</div>
    <br>
    <br>
    <p>猕猴桃
        苹果
        梨</p>
    <p>白菜
        土豆
        地瓜</p>
    <p>辣条
        牛肉干
        薯片</p>
    <p>可乐
        雪碧
        果汁</p>
    <script>
        var arr,divs;
        init()
        function init(){
            // 当历史前进或者后退时就会收到这个事件
            window.onpopstate=popStateHandler;
            arr=Array.from(document.getElementsByTagName("p"));
            divs=Array.from(document.getElementsByTagName("div"));
            arr[0].style.display="block";
            for(var i=0;i<divs.length;i++){
                divs[i].onclick=clickHandler;
            }
            
        }


        function clickHandler(){
           var index=divs.indexOf(this);
        //    history.pushState({state:1},"","#"+this.innerHTML);
            // 在历史记录列表中增加数据，后面的#内容标示当前跳转部分
             history.pushState({index:index}, "", "#" +this.innerHTML);
             changeMenu(index);
        }

        function popStateHandler(){
            console.log(history.state);
            changeMenu(history.state.index)
        }

       function changeMenu(index){
        for(var i=0;i<arr.length;i++){
                if(i===index){
                    arr[i].style.display="block";
                }else{
                    arr[i].style.display="none";
                }
            }
       }
    </script>
```

