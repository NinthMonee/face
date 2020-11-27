## 一、数据类型

基本数据类型：

1. ES5及ES5之前的原始数据类型

   基本数据类型：

   - boolean:只有两个值true/false
   - number:只能安全的表示-(2^53-1)到2^53-1之间的整数
   - string：表示字符串数据
   - null：只有null值
   - undefined：变量没有赋值之前的默认值

   复杂数据类型：

   - object：表示基本对象的数据类型

2. ES6及ES6之后的新增的数据类型

   - symbol(es6)：唯一标识符，可用作对象的唯一属性名
   - bigInt(es10,V8引擎v6.7 默认启用对 BigInt 的支持)：支持大整数类型的计算，要表示BigInt，只需在整数之后加一个n，就可以调用BigInt的构造函数

## 二、数据类型转换的规则

|         值          |  转换为字符串   | 数字 | 布尔值 |         对象          |
| :-----------------: | :-------------: | :--: | :----: | :-------------------: |
|      undefined      |   "undefined"   | NaN  | false  |                       |
|        null         |     "null"      |  0   | false  |                       |
|        true         |     "true"      |  1   |        |   new Boolean(true)   |
|        false        |     "false"     |  0   |        |  new Boolean(false)   |
|    ""(空字符串)     |                 |  0   | false  |    new String("")     |
|  "1.2"(非空，数字)  |                 | 1.2  |  true  |   new String("1.2")   |
| "one"(非空，非数字) |                 | NaN  |  true  |   new String("one")   |
|          0          |       "0"       |      | fasle  |     new Number(0)     |
|         NaN         |      "NaN"      |      | false  |    new Number(NaN)    |
|      Infinity       |   "Infinity"    |      |  true  | new Number(Infinity)  |
|      -Infinity      |   "-Infinity"   |      |  true  | new Number(-Infinity) |
|         []          |       ""        |  0   |  true  |                       |
|         {}          | [object Object] |  0   | false  |                       |

### 1、数值转换

转换分为强制类型转换与隐式类型转换。

强制类型转换是自己手动调用方法去转换元素类型，隐式类型转换是利用运算符特征自动转换类型。

#### (1) 数值转换为字符串

将数值强制转换为字符串使用String();

而隐式类型转换则是调用toString()方法，toString()方法是Object对象的方法，任何类型都可以调用该方法，该方法会传递一个参数，用于指定要转换的数值类型的进制数。

```js
强制类型转换：
    var a=3;
    String(a);
隐式类型转换：
a=a+"";//利用运算符特征自动转换类型
var color=0x0000FFFF;
div0.style.color="#"+(color<<8).toString(16);
```

补充：

```js
//toFixed()         转换为字符串  并且保留小数点位数  自动四舍五入
//toPrecision(5)    转换为字符串  保留有效数字，包括整数位
//toExponential(2)  转换为字符串  保留有效数字，不包括整数位，且必须是以科学计数法的形式展示
```

#### (2)数值转换为布尔值

将数值强制转换为布尔值Boolean();

除了0以外所有的数值转换布尔值都是true

```js
var a=0;
var res=Boolean(a);//false
```

#### (3)数值转换为对象

将数值强制转换为对象类型，并将数据保存在堆中。

具备数值的特征，但是===不相等，有引用地址，必须赋值为null才可以被清理

```js
var a=10;
var res=Object(res);
console.log(typeof res);//object
```

### 2、字符串转换

(1)字符串转数值

字符串转数值有三种方法。

- Number()

  - 只能转换内部是数值的字符串，如果有其他字符串，则转换失败，返回NaN
  - 强制转换和隐式转换都遵从该方法
  - 会自动忽略数最前面和最后面的空格

  ```js
  var a="1545";
  var res=Number(a);
  console.log(res);//1545
  var b="15sd";
  var res2=Number(b);
  console.log(res2);//NaN
  var c=" 15 ";
  var res3=Numbe(c);
  console.log(res3);//15
  ```

- parseInt()

  - 可以转换数值和其余字符混合而成的字符串，当遇到第一个非数值的字符，则转换停止
  - 第一个参数是要转换的数值，第二个参数是要转换的数值的进制类型

  ```js
  var  color="FF0000";
  color=parseInt(color,16);
  ```

- parseFloat()

  - 与parseFloat，可以转换整数，也可以转换小数
  - 只能传递一个参数，不能指定进制类型

  ```js
  var a=parseFloat("1.2");
  console.log(a);
  ```

(2)字符串布尔值

*仅空字符串转换为布尔值，是false 除此之外，全都是true*

```js
var str="";
jsstr=Boolean(str);
```

(3)字符串转对象

```js
var  str="aaa";
str=Object(str);
```

### 3、布尔值转换

(1)布尔值转数值

- true为1

- false为0

(2)布尔值转字符串

- true转换后为“true”
- false转换后为“false”

(3)布尔值转对象

- Obejct()，转换过后就是布尔值对象

### 4、总结

1. 只有空字符串、0、NaN、false、null、undefined这六种转换布尔值是false，其他都是true
2. undefined只与null在做比较时相等，这二者与其他任何值相比都为false
3. 如果一个操作数是布尔值，则在比较相等性之前会先将其转换为数值，false转换为0，而true转换为1
4. 如果一个操作数是字符串，另一个操作数是数值，在比较前会先将字符串转换为数值。
5. 如果一个操作数是对象，另一个操作数不是，则调用对象的valueOf方法，用得到的基本类型值按照前面的规则进行比较，如果对象没有valueOf()方法，则调用toString()

### 5、案例练习 

```js
1、console.log(![]==[]);
解释：首先按照运算符优先级的关系来转换，!的优先级大于==，所以会先执行![],!可将变量转换为boolean类型，null,undefined,NaN,空字符串"",0,空对象{}转换为布尔值都是flase,其余都是true。所以[]转换为布尔值为true,取反得false, 而false如果在等号两边要做比较，那么会先转换为数值0，这时候判断的就是0==[]，[]是对象，会首先去找对象的valueOf()，空数组没有valueOf()方法，就会去找toString()方法，[]转换为空字符串，字符串与数值在做比较时，会自动隐式转换为数值，所以空字符串隐式转化为0
2、console.log([]=="");
解释：首先执行规则5，[]先调用toString()方法转换为空字符串,再转换为0，然后字符串与数值比较，则会将字符串转换为数值0，所以而知相等
3、console.log([]==false);
解释：[]  false都会隐式转换为0
4、console.log([]==0);
解释:[]会先转换为字符串然后转换为数值，最后[]会转换为0，所以返回true
```

