## $1~前言

:heart:作为头发最后守卫者(程序员)来说，无论学习哪种编程语言，数据结构和算法都是绕不开的门槛。如果说框架、技术等是程序员的武功秘籍的话，那么数据结构与算法就是程序员修行的内功心法。招式用的再好，功力不足，也难以摧金断石，搬山填海。

:heart:学习js数据结构之前，先来谈一谈我所认为的js，js是一门很奇怪的语言，在ES6之前，存储数据的结构只有数组和对象。ES6之后才有了Set、Map来存储数据。我一直认为，一门编程语言的灵魂就是数据结构，没有数据结构的语言，就像是没有骨骼的皮肉，难以组成一个完成的语言。但没有办法，js是前端程序员唯一的选择，那么既然它自己没有数据结构，我们就自己来利用它内部现成的数组、对象等封装属于我们自己的数据结构。

:heart:既然如此，今天就跟我一起来从最简单的数据结构开始学习吧。

## $2~栈

### $2.1~栈是什么？

:heart:栈是一种LIFO(Last In  First Out  后进先出)的、有序的数据结构。新添加的元素都位于栈结构的一端，叫做栈顶，而另一端叫做栈底。栈结构只能在栈顶添加或者删除元素。往栈结构中添加元素的方式叫做压栈/入栈，删除栈顶元素的方式叫做弹栈/出栈。

### $2.2~栈中的API

|    API     |               描述               |
| :--------: | :------------------------------: |
| push(elem) |    添加一个或者多个元素到栈顶    |
|   pop()    | 移出栈顶的元素，返回被移除的元素 |
|   peek()   |           查看栈顶元素           |
| isEmpty()  |          判断栈是否为空          |
|  clear()   |           清除栈内元素           |
|   size()   |        查看栈中元素的个数        |
| toString() |        打印栈中的所有元素        |

### $2.3~简单实现栈的三种方式

:heart:在学习数据结构的实现之前，我们需要先有数组与对象，Set/Map等知识的基础。

#### $2.3.1~ES5_数组方式实现

:heart:以下代码是我们用最简单的ES5中的数组结合原型链的方式实现的栈结构，当然，这种方式实现的栈结构会存在各种各样的问题，比如效率低，写法复杂，属性不私有等。别急，让我们慢慢往后看，你想要的都会有。

```js
function Stack() {
    //栈中的属性
    this.items = []
    //栈的相关操作 
    //1.压栈
    Stack.prototype.push = function (element) {
        this.items.push(element)
    }
    //2.从栈中取出元素
    Stack.prototype.pop = function () {
        return this.items.pop()
    }
    //3.查看栈顶元素
    Stack.prototype.peek = function () {
        return this.items[this.items.length - 1]
    }
    //4.判断栈是否为空
    Stack.prototype.isEmpty = function () {
        return this.items.length == 0
    }
    //5.获取栈中元素的个数
    Stack.prototype.size = function () {
        var count = 0;
        for (var i = 0; i < this.items.length; i++) {
            if (i in this.length[i]) {
                count++;
            }
        }
        return count;
    }
    //6.toString方法
    Stack.prototype.toString = function () {
        var resultString = ''
        for (var i = 0; i < this.items.length; i++) {
            resultString += this.items[i] + ' '
        }
        return resultString
    }
}
```

#### $2.3.2~ES6_数组方法实现

:heart:需要注意的是，在这个实现方式中，我没有将数组的length属性直接赋值给size，这是因为，length代表的是数组的长度，在数组中是可以存在空元素的，而size是栈中元素的个数，不包括空元素，这二者是有区别的。我们在使用数组封装栈结构的时候，尤其需要注意到这一点。

```js
/**
 * 利用ES6数组模拟栈数据结构的实现
 */
export default class Stack {
  constructor() {
    //创建一个空数组
    this.items = [];
  }
  /**
   * 压栈
   * @param {T} elem 
   */
  push(elem) {
    this.items.push(elem);
  }
  /**
   * 弹栈
   */
  pop() {
    return this.items.pop();
  }
  /**
   * 查看栈顶元素
   */
  peek() {
    return this.items[this.items.length - 1];
  }
  /**
   * 查看栈中元素的个数
   * size与length不同，length不会跳过空元素，而size会忽略过空元素
   */
  size() {
    let count = 0;
    for (let i of this.items) {
      count++;
    }
    return count;
  }
  /**
   * 判断栈是否为空
   */
  isEmpty() {
    return this.size() === 0;
  }
  /**
   * 清空栈
   */
  clear() {
  /*
   while(!this.isEmpty()){
       this.pop();
   }*/
    this.items = [];
  }
  /**
   * 打印栈元素
   */
  toString() {
    //可以使用for of的方法打印栈元素,也可以直接调用数组的toString()方法
    // let str = "";
    // for (let item of this.items) {
    //   str += item+" ";
    // }
    // return str;
    return this.items.toString();
  }
}
```

#### $2.3.3~ES6_对象方法实现

:heart:通过学习以上两种数组实现栈的结构的方式，相信我们已经对如何实现栈结构已经了有了一个大概的认知了。但是我们要知道，数组是一种紧密型的结构，数组内的所有元素都依靠下标来存储。在数组中添加、删除元素都会影响到别的元素，这样操作数据的效率是非常低的。而且在使用数组方法的时候，大部分方法的时间复杂度都是**O(n)**，在存储大量数据的时候，数组的效率更是会显得尤为的低下，而我们设计数据结构的目的，就是为了提高效率，数据存储数据，恰恰与我们的初衷正好相反。那么有没有更好的方式来存储数据了？

:heart:答案自然是肯定的，在js中，对象存储数据的方式是用key-value的形式来存储的，我们再去寻找一个数据的时候，无需去遍历，只需要寻找到key值对应的value就可以取到我们的数据，增加或者删除数据，都不会影响到别的数据，效率是非常高的。

```js
/**
 * 用ES6对象的方式来模拟实现栈
 */
export default class Stack {
  constructor() {
    //用count来记录栈的大小
    this.count = 0;
    this.items = {};
  }
  /**
   * 压栈
   * @param {*} elem 
   */
  push(elem) {
    this.items[this.count] = elem;
    this.count++;
  }
  /**
   * 弹栈
   * 先判断栈内元素是否为空
   * 如果不为空，则取出最后存入的数据
   * 也就是栈顶元素
   */
  pop() {
    if (this.isEmpty()) return;
    //栈元素数量-1
    this.count--;
    //取出最后一位的数组元素
    const result = this.items[this.count];
    //然后删除该元素
    //对象中直接使用delete删除就可以，对象长度也会随之减小
    delete this.items[this.count];
    //返回被删除的元素
    return result;
  }
  /**
   * 查看栈顶元素
   */
  peek() {
    if (this.isEmpty()) return;
    return this.items[this.count - 1];
  }
  /**
   * 判断栈是否为空
   */
  isEmpty() {
    return this.count === 0;
  }
  /**
   * 查看栈中元素的个数
   */
  size() {
    return this.count;
  }
  /**
   * 清空栈元素
   */
  clear(){
    /* while(!this.isEmpty){
      this.pop();
    } */
    this.items={};
    this.count=0;
  }
  /**
   * 打印栈元素
   */
  toString() {
    if (this.isEmpty()) {
      return "";
    }
    let objString = `${this.items[0]}`;
    for (let i = 1; i < this.count; i++) {
      objString = `${objString},${this.items[i]}`;
    }
    return objString;
  }
}
```

### $2.4~如何保证栈结构的数据安全性?

:heart:我们在设计栈结构的时候，会发现，外部是可以​不通过我们对外暴露的方法直接访问操作栈内的元素和数据的。这种情况严重的造成了我们的数据的不安全性，这是js语言的通病，没有设置私有属性和方法。但我们可以通过其他的方式来保证数据的安全性。

#### $2.4.1~基于开发者的自觉性

:heart: ​我们可以使用自己开发团队内部的约定，来设置属性的私有。比如可以使用_或者-来做一个标记，凡是在开发过程遇到的带有该标记的属性或者方法，就都是私有方法，除了内部提供暴露的方法之外，外部调用者，别用别的方法操作数据。不过，这种方式，就如我的小标题一样，全看开发者自觉不自觉了。:dizzy_face:

#### $2.4.2~基于ES6中的Symbol

:heart:ES6中新增了一种数据类型，叫做Symbol，它是用来保证属性的唯一性，是不可变的。那么我们可以利用该特性来声明items。我们只需声明一个Symbol类型的私有属性，将其作为类属性的key存储，给它的value值赋值一个{}或者[]，就可以做

```js
const _items=Symbol();
class Stack{
    constructor(){
        //为了方便，这里我们使用数组来作为存储栈的结构
        this[_items]=[];
   }
    //其他方法，与上述数组实现栈方法相同，只需要将所有的this.items替换为this[_items]即可
}
```

:heart:看似这种方法无法从外部直接访问_items，但是ES6中又提供了一种破坏该私有性的方法，Object.getOwnPropertySymnols()可以取到在类里面声明的所有Symbols属性。

```js
let s=new Stack();
s.push(5);
s.push(6);
s.push(7);
let ops=Object.getOwnPropertySymbols(s);
//返回的ops是一个存储着所有Symbol的数组
s[ops[0]].push(0);
console.log(s);
//5 6 7 0;
```

:heart:由于获取的是属性items是数组，所以它可以在不使用我暴露的方法下，可以随意的操作我们的数组​，而我们希望操作的是栈，栈不应该会出现这种情况，所以我们思考还有没有更好的解决方案。自然是有的。

#### $2.4.3~基于ES6中的WeakMap

:heart:在ES6中新引入了四种数据结构Set、Map、WeakSet、WeakMap，个人认为，正是因为ES6中引入种种非常有用的新特性，JS才不至于在日新月异高速发展的今天被抛弃和淘汰。话说回来，WeakMap是Map数据结构的弱引用类型，关于Map是什么，本文就不在这里做详细介绍，有需要者可自行查阅资料。WeakMap可以存储键值对，键可以存储对象，值可以是任意数据类型。

```js
const items=new WeakMap();

class Stack{
   constructor(){
        items.set(this,[]);
   }
   push(elem){
        let a=items.get(this);
   }
   pop(){
        let a=items.get(this);
        return a.pop();
   }
   peek(){
       let a=items.get(this);
       return a[a.length-1];
   }
   isEmpty(){
       let a=items.get(this);
       return a.length===0;
   }
   clear(){
       //items.get(this).length=0;
       items.set(this,[]);
   }
   size(){
       let count=0;
       let a=items.get(this);
       for(let i in a){
           count++
       }
       return count++;
   }
    toString(){
        let a=items.get(this);
        let str="";
        for(let i in a){
            str+=a[i]+" ";
        }
        return str;
    }
}

```

## $3~进制转换案例练习

### $3.1~十进制转二进制

```js
function dec2bin(decNumber) {
    //1.定义栈对象
    let stack = new Stack()
    //2.循环操作
    while (decNumber > 0) {
        //2.1 获取余数放入到栈中
        stack.push(decNumber % 2)
        //2.2 获取整除后的结果 作为下一次运行的数字
        decNumber = Math.floor(decNumber / 2)
    }
    //3.从栈中取出0 1 
    let binaryString = ''
    while (!stack.isEmpty()) {
        binaryString += stack.pop()
    }
    return binaryString
}
```

:heart:以上代码，就可以简单实现十进制转二进制，但是我们现在有一个需求，就是我们希望可以实现十进制转换任意进制。那么我们该怎么做？

思路：

​    1、首先我们会传入两个参数，一个是十进制数，一个要转换的进制值(这个进制值，我们规定在2~36之间，因为数字+字母只有36个)

​    2、然后我们应该有一个数组或者字符串存储0-9A-Z

​    3、我们取到传递进来的十进制数的每一个进制位上的数值，利用栈将其存储起来

​    4、然后按照按照存储的字符，将其一个个对应拼接起来

```js
function baseCover(decNumber,base){
    if(!(base>=2&&base<=36))return;
    let stack=new Stack();
    let digits=[0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F,G,H,I,J,K,L,M,,N,O,P,Q,R,S,T,U,V,W,X,Y,Z];
    let num=decNumber;
    while(num>0){
        stack.push(Math.floor(num%base));
        num=Math.floor(num/base);
    }
    let str="";
    while(!stack.isEmpty()){
        str+=digits[stack.pop()];
    }
    return str;
}
```

## $4~​ :upside_down_face: 写在最后

:heart:以上就是我所总结的栈的知识，希望可以帮助到像我一样在苦恼数据结构和算法的小伙伴们！！！

PS：最后，希望路过的大佬不吝指教！！！感谢！！！