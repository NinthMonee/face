#### 1.object.assign(target,source1,source2...)

该方法主要是用于对象的合并，将源对象source的所有可枚举属性复制到目标对象，并将合并后的目标对象返回(不会生成新对象)。

~~~javascript
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };

const returnedTarget = Object.assign(target, source);

console.log(target);
// expected output: Object { a: 1, b: 4, c: 5 }

console.log(returnedTarget);
// expected output: Object { a: 1, b: 4, c: 5 }

//target与returnedTarget对象的值完全一致

const a1=Object.assign([1, 2, 3], [4, 5]) 
//[4,5,3]
const a2=Object.assign([1, 2, 3], [4, 5, 6]) 
//[4,5,6]
~~~

注意：

1.  该方法只会拷贝源对象的自身属性，对继承的属性不作拷贝
2.  该方法实行的是浅拷贝，不是深拷贝。如果源对象的某个属性值是对象，那么目标对象拷贝得到的是这个对象的引用，同名属性会进行替换。
3.  该方法只能进行值的复制，如果复制的是一个取值函数，那么将求值后再拷贝
4.  该方法可以处理数组，但是会把数组当成对象来处理,会将目标数组的元素从前到后按照同名属性的方法依次进行替换。

#### **2.object.create()**

该方法主要是用来创建一个新对象，使用现有的对象来提供新创建的对象的隐式原型对象_  proto _

~~~javascript
let parent={
    x:1,
    y:2
}
//z会成为新创建对象的属性
let child=Object.create(parent,{
    z:5
})
console.log(child);
//{z:5}  自身实例下x，y属性不存在
console.log(child.y)
//2   y在原型下
~~~

#### **3.Object.defineProperties(obj,props)**

该方法是直接在一个对象上定义新的属性或修改现有属性，并返回该对象。obj是在要定义或者修改的属性的对象，props是要定义或者修改的属性描述符的对象，该对象中存在的属性描述符主要有两种，数据描述符和访问器描述符。描述符具有以下键：

**configurable**：默认为false,为true的时候是该属性描述符可以被改变且可以从相应对象中删除。

**enumerable**：默认为false,为true的时候是当且仅当在枚举相应对象上的属性时该属性显现。

**value**：默认为undefined,是与属性关联的值，可以是任何有效的JavaScript值

**writable**：默认为false,当且仅当与该属性相关联的值可以用[assignment operator](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Assignment_Operators)改变时。

**get**：默认为undefined,作为该属性的 getter 函数，如果没有 getter 则为[`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)。函数返回值将被用作属性的值。

**set**：默认为undefined,作为属性的 setter 函数，如果没有 setter 则为[`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)。函数将仅接受参数赋值给该属性的新值。

~~~javascript
	var obj = {};
Object.defineProperties(obj, {
  'property1': {
    value: true,
    writable: true
  },
  'property2': {
    value: 'Hello',
    writable: false
  }
  // etc. etc.
});
console.log(obj)   // {property1: true, property2: "Hello"}
~~~

#### **4.Object.defineProperty(obj,prop,descriptor)**

在一个对象上定义一个新属性，或者修改一个对象的现有属性， 并返回这个对象。

~~~javascript
Object.defineProperty(Object, 'is', {
  value: function(x, y) {
    if (x === y) {
      // 针对+0 不等于 -0的情况
      return x !== 0 || 1 / x === 1 / y;
    }
    // 针对NaN的情况
    return x !== x && y !== y;
  },
  configurable: true,
  enumerable: false,
  writable: true 
}); 
 
// 注意不能同时设置(writable，value) 和 get，set方法，否则浏览器会报错 ： Invalid property descriptor. Cannot both specify accessors and a value or writable attribute
~~~

#### **5.Object.keys(obj)**

返回一个由一个给定对象的自身**可枚举**属性组成的数组，数组中属性名的排列顺序和使用 [`for...in`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...in) 循环遍历该对象时返回的顺序一致 （两者的主要区别是 一个 for-in 循环还会枚举其原型链上的属性）

~~~javascript
let arr = ["a", "b", "c"];
console.log(Object.keys(arr));
// ['0', '1', '2']
 
/* Object 对象 */
let obj = { foo: "bar", baz: 42 },
    keys = Object.keys(obj);
console.log(keys);
// ["foo","baz"]
~~~

#### **6.Object.values()**

方法返回一个给定对象自己的所有**可枚举**属性值的数组，值的顺序与使用[`for...in`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...in)循环的顺序相同 ( 区别在于 for-in 循环枚举原型链中的属性 )。

`Object.values`会过滤属性名为 Symbol 值的属性。

~~~javascript
var an_obj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.values(an_obj)); // ['b', 'c', 'a']
 
var obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.values(obj)); // ['a', 'b', 'c']
~~~

#### **7.Object.entries()**

返回一个给定对象自身**可枚举**属性的键值对数组，其排列与使用 [`for...in`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...in) 循环遍历该对象时返回的顺序一致（区别在于 for-in 循环也枚举原型链中的属性）。

~~~javascript
const obj = { foo: 'bar', baz: 42 };
console.log(Object.entries(obj)); // [ ['foo', 'bar'], ['baz', 42] ]
 
const simuArray = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.entries(simuArray)); // [ ['0', 'a'], ['1', 'b'], ['2', 'c'] ]
~~~

#### **8.hasOwnProperty()**

判断对象自身属性中是否具有指定的属性。

~~~javascript
obj.hasOwnProperty("name")
~~~

#### **9.Object.getOwnPropertyDescriptor(obj,prop)**

返回指定对象上一个自有属性对应的属性描述符。（自有属性指的是直接赋予该对象的属性，不需要从原型链上进行查找的属性）.

如果指定的属性存在于对象上，则返回其属性描述符对象（property descriptor），否则返回 undefined。 

~~~javascript
var xxx={x:1,y:2};
Object.getOwnPropertyDescriptor(xxx,'x')
//{value: 1, writable: true, enumerable: true, configurable: true}
~~~

#### **10.Object.getOwnPropertyDescriptors(obj)**

获取一个对象的所有自身属性的描述符。

~~~javascript
let test={
name:"zhangsan",
age:18
}
Object.getOwnPropertyDescriptors(test);
//age...
//name...
//_ proto _
~~~

#### **11.Object.getOwnPropertyNames()**

返回一个由指定对象的所有自身属性的属性名（**包括不可枚举属性**但不包括Symbol值作为名称的属性）组成的数组。

#### **12.Object.getOwnPropertySymbols()**

 返回一个给定对象自身的所有[ Symbol 属性](https://www.cnblogs.com/vickylinj/p/13037892.html)的数组。

#### **13.Object.getPrototypeOf()**

返回指定对象的原型（内部`[[Prototype]]`属性的值）

#### **14.isPrototypeOf()**

 判断一个对象是否存在于另一个对象的原型链上。

#### **15.Object.setPrototypeOf(obj,prototype)**

设置对象的原型对象

#### **16.Object.is()**

判断两个值是否相同。

如果下列任何一项成立，则两个值相同：

- 两个值都是 [`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)
- 两个值都是 [`null`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/null)
- 两个值都是 `true` 或者都是 `false`
- 两个值是由相同个数的字符按照相同的顺序组成的字符串
- 两个值指向同一个对象
- 两个值都是数字并且
  - 都是正零 `+0`
  - 都是负零 `-0`
  - 都是 [`NaN`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/NaN)
  - 都是除零和 [`NaN`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/NaN) 外的其它同一个数字

#### **17.Object.freeze()**

冻结一个对象，冻结指的是不能向这个对象添加新的属性，不能修改其已有属性的值，不能删除已有属性，以及不能修改该对象已有属性的可枚举性、可配置性、可写性。也就是说，这个对象永远是不可变的。该方法返回被冻结的对象。

#### **18.Object.isFrozen()**

判断一个对象是否被[冻结](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze) .

#### **19.Object.preventExtensions()**

对象不能再添加新的属性。可修改，删除现有属性，不能添加新属性。

#### **20.Object.isExtensible()** 

 判断对象是否是可扩展的，[`Object.preventExtensions`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/preventExtensions)，[`Object.seal`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/seal) 或 [`Object.freeze`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze) 方法都可以标记一个对象为不可扩展（non-extensible）

#### **21.Object.seal()**

Object.seal() 方法可以让一个对象密封，并返回被密封后的对象。密封一个对象会让这个对象变的**不能添加新属性**，且所有已有属性会变的不可配置。属性不可配置的效果就是属性变的**不可删除**，以及一个数据属性不能被重新定义成为访问器属性，或者反之。但**属性的值仍然可以修改**。尝试删除一个密封对象的属性或者将某个密封对象的属性从数据属性转换成访问器属性，结果会静默失败或抛出TypeError 异常. 不会影响从原型链上继承的属性。但 __proto__ ( ) 属性的值也会不能修改。

~~~javascript
var obj = {
    prop: function () {},
    foo: "bar"
  };
 
// 可以添加新的属性,已有属性的值可以修改,可以删除
obj.foo = "baz";
obj.lumpy = "woof";
delete obj.prop;
 
var o = Object.seal(obj);
 
assert(o === obj);
assert(Object.isSealed(obj) === true);
 
// 仍然可以修改密封对象上的属性的值.
obj.foo = "quux";
 
// 但你不能把一个数据属性重定义成访问器属性.
Object.defineProperty(obj, "foo", { get: function() { return "g"; } }); // 抛出TypeError异常
 
// 现在,任何属性值以外的修改操作都会失败.
obj.quaxxor = "the friendly duck"; // 静默失败,新属性没有成功添加
delete obj.foo; // 静默失败,属性没有删除成功
 
// ...在严格模式中,会抛出TypeError异常
function fail() {
  "use strict";
  delete obj.foo; // 抛出TypeError异常
  obj.sparky = "arf"; // 抛出TypeError异常
}
fail();
 
// 使用Object.defineProperty方法同样会抛出异常
Object.defineProperty(obj, "ohai", { value: 17 }); // 抛出TypeError异常
Object.defineProperty(obj, "foo", { value: "eit" }); // 成功将原有值改变
~~~

