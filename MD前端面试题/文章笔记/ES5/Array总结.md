#### 一、声明数组

1.通过new创建数组

~~~javascript
let arr=new Array(2);//2 代表创建数组的大小
arr[0]="asda";
arr[1]="adsa";
~~~

2.省略new创建数组

~~~javascript
let  arr=new Array("a","ad","fsgf");
~~~

3.数组常量进行赋值

~~~javascript
let arr=["a","ad","fsgf"];
~~~

#### 二、常用方法

##### 1、栈结构

栈结构是一种FILO(先进后出)的数据结构

​       1)  push 从数组的末尾添加一个元素，并返回其长度

~~~javascript
let arr=["asd","adad","22","a"];
console.log(arr.push("15"));
//结果为5
~~~

​       2)  pop  从数组中删除最后一个元素，并返回该元素的值

~~~javascript
let arr=["asd","adad","22","a"];
console.log(arr.pop());
//结果为a
~~~

##### 2、队列

队列结构是一种FIFO(先进先出)的数据结构

​       1)  shift  删除数组的第一个元素，并返回该元素的值

~~~javascript
let arr=["asd","adad","22","a"];

console.log(arr.shift());
// 结果为asd
~~~

​       2)  unshift 将一个或者多个元素添加到数组的开头，并返回其长度

~~~javascript
let arr=["asd","adad","22","a"];

console.log(arr.unshift("ff","he"));
//返回结果为6
~~~



##### 3、排序

​     1）reverse

​          将数组中的元素倒序排列，会改变原数组

~~~javascript
let arr=["asd","adad","22","a"];
arr.reverse();
console.log(arr);
//结果为[ 'a', '22', 'adad', 'asd' ]
~~~

​     2）sort

​          该方法是对数组内的元素进行排序，并且返回改变后的数组。排序规则默认是将元素转换成字符串进行字典序排序。

~~~javascript
let arr=["de","as","few","bdgd"];
arr.sort();
console.log(arr);
//结果为：[ 'as', 'bdgd', 'de', 'few' ]

const array1 = [1, 30, 4, 21, 100000];
arr.sort();
console.log(array1);
//[1, 100000, 21, 30, 4]
~~~

​          该方法内还可以设定排序规则，可以将一个比较两个数优先级的一个函数传递进去。

~~~javascript
//比较数字
var numbers = [4, 2, 5, 1, 3];
numbers.sort(function(a, b) {
  return a - b;
});
console.log(numbers);

//也可以写成：
var numbers = [4, 2, 5, 1, 3]; 
numbers.sort((a, b) => a - b); 
console.log(numbers);
// [1, 2, 3, 4, 5]

//比较对象
var items = [
  { name: 'Edward', value: 21 },
  { name: 'Sharpe', value: 37 },
  { name: 'And', value: 45 },
  { name: 'The', value: -12 },
  { name: 'Magnetic' },
  { name: 'Zeros', value: 37 }
];

// sort by value
items.sort(function (a, b) {
  return (a.value - b.value)
});

// sort by name
items.sort(function(a, b) {
  var nameA = a.name.toUpperCase(); // ignore upper and lowercase
  var nameB = b.name.toUpperCase(); // ignore upper and lowercase
  if (nameA < nameB) {
    return -1;
  }
  if (nameA > nameB) {
    return 1;
  }
  return 0;
});

//对于非ASCII字符排序，需要用到String.localeCompare
var items = ['réservé', 'premier', 'cliché', 'communiqué', 'café', 'adieu'];
items.sort(function (a, b) {
  return a.localeCompare(b);
});

// items is ['adieu', 'café', 'cliché', 'communiqué', 'premier', 'réservé']

//使用映射来改善排序，当元素较为复杂，需要比较的规则也比较复杂的时候，可以先将元素中要比较的值先提取出来进行基表
// 需要被排序的数组
var list = ['Delta', 'alpha', 'CHARLIE', 'bravo'];

// 对需要排序的数字和位置的临时存储
var mapped = list.map(function(el, i) {
  return { index: i, value: el.toLowerCase() };
})

// 按照多个值排序数组
mapped.sort(function(a, b) {
  return +(a.value > b.value) || +(a.value === b.value) - 1;
});

// 根据索引得到排序的结果
var result = mapped.map(function(el){
  return list[el.index];
});
~~~

##### 4、数组转换

​      1）`Array.from()`  将类数组或者可迭代对象按照一定的规则转换成一个数组。类数组对象最基本的要求就是具有length属性的对象。可迭代对象就是可以遍历获取对象中的元素,如 Map和 Set 等。

​        该方法中可以传递三个参数`Array.from(arrayLike[, mapFn[, thisArg]])`，第一个参数是必选，这个参数传递的是类数组或者可迭代对象。第二个参数可选，这个参数传递的是一个回调函数，一旦传递，就会为新数组的每一个元素执行该函数。第三个参数可选，这个参数是执行回调函数时的this对象，该方法会返回一个新的数组。

~~~javascript
//1.将类数组转换成数组
let array = {
    0: 'name', 
    1: 'age',
    2: 'sex',
    3: ['user1','user2','user3'],
    'length': 4
}
let arr = Array.from(array)
console.log(arr) // ['name','age','sex',['user1','user2','user3']]

//将length去掉之后
let array = {
    0: 'name', 
    1: 'age',
    2: 'sex',
    3: ['user1','user2','user3'],
}
let arr = Array.from(array)
console.log(arr)  // [ ]

//将键名改为字符串
let array = {
    'name': 'name', 
    'age': 'age',
    'sex': 'sex',
    'user': ['user1','user2','user3'],
    'length': 4
}
let arr = Array.from(array)
console.log(arr)  // [ undefined, undefined, undefined, undefined ]
//由此可见，要将一个类数组对象转换成数组，必须具备以下条件
//该类数组对象必须具有length属性，用于指定新数组的长度，如果没有数组的长度，转换成数组后会是一个空对象
//该类数组对象的属性名必须为数值型或者字符串类型的数字，该数组对象的属性名可以加引号，也可以不加引号

//2.将可迭代对象转换成数组
//set
let arr = [1,2,3,4,5,6,7,8,9]
let set = new Set(arr)
console.log(Array.from(set))  // [1,2,3,4,5,6,7,8,9]
//也可以传递一个回调函数
console.log(Array.from(set,x=>x+1));//[2,3,4,5,6,7,8,9,10]
//字符串
let  string="hello world";
console.log(Array.from(str)) // ["h", "e", "l", "l", "o", " ", "w", "o", "r", "l", "d", "!"]

//map
const map = new Map([[1, 2], [2, 4], [4, 8]]);
Array.from(map);
// [[1, 2], [2, 4], [4, 8]]

const mapper = new Map([['1', 'a'], ['2', 'b']]);
Array.from(mapper.values());
// ['a', 'b'];

Array.from(mapper.keys());
// ['1', '2'];


//在Array中使用箭头函数
Array.from({length: 5}, (v, i) => i);
// [0, 1, 2, 3, 4]
~~~

2）`Array.isArray(obj)`  用于确定传递的参数值是否是一个数组

3）`Array.of()`  创建一个具有可变数量参数的新数组实例，它与Array的区别在于处理单个的整数参数。

~~~javascript
Array.of(7);//[7]
Array(7);//[,,,,,,] 一个空的大小为7的数组
~~~

##### 5、数组的拷贝

slice不会改变原数组，会生成一个新的数组，copyWithin不会生成新的数组，而是将该数组的一部分复制到同一个数组的另外一个位置。

1）`slice()` 方法返回一个新的数组对象，这一对象是一个由 `begin` 和 `end` 决定的原数组的**浅拷贝**（包括 `begin`，不包括`end`）。原始数组不会被改变，返回的新数组包含begin索引代表的元素，但不包含end索引代表的元素。

​        如果begin为负数，则表示从原数组中的倒数第几个元素开始提取，`slice(-2)` 表示提取原数组中的倒数第二个元素到最后一个元素（包含最后一个元素）。如果省略 `begin`，则 `slice` 从索引 `0` 开始。如果 `begin` 大于原数组的长度，则会返回空数组。

​        如果该参数为负数， 则它表示在原数组中的倒数第几个元素结束抽取。 `slice(-2,-1)` 表示抽取了原数组中的倒数第二个元素到最后一个元素（不包含最后一个元素，也就是只有倒数第二个元素）。如果 `end` 被省略，则 `slice` 会一直提取到原数组末尾。如果 `end` 大于数组的长度，`slice` 也会一直提取到原数组末尾。

```javascript
//返回现有数组的一部分
var fruits = ['Banana', 'Orange', 'Lemon', 'Apple', 'Mango'];
var citrus = fruits.slice(1, 3);

// fruits contains ['Banana', 'Orange', 'Lemon', 'Apple', 'Mango']
// citrus contains ['Orange','Lemon']
```

2）`copyWithin()` 方法浅复制数组的一部分到同一数组中的另一个位置，并返回它，不会改变原数组的长度。该方法有三个参数，`arr.copyWithin(target[, start[, end]])`。

target：目的起始位置。

start：复制源的起始位置。

end：复制源的结束位置。

这三个参数，第一个参数必选，剩下两个可选。参数如果是负数，代表从末尾开始计算。

```javascript
[1, 2, 3, 4, 5].copyWithin(-2)//代表 复制的目的地从后往前数下标为-2的位置开始，复制源从0下标开始复制，直到数组的最大长度
// [1, 2, 3, 1, 2]

[1, 2, 3, 4, 5].copyWithin(0, 3)
// [4, 5, 3, 4, 5]

[1, 2, 3, 4, 5].copyWithin(0, 3, 4)
// [4, 2, 3, 4, 5]

[1, 2, 3, 4, 5].copyWithin(-2, -3, -1)
// [1, 2, 3, 3, 4]

[].copyWithin.call({length: 5, 3: 1}, 0, 3);
// {0: 1, 3: 1, length: 5}

// ES2015 Typed Arrays are subclasses of Array
var i32a = new Int32Array([1, 2, 3, 4, 5]);

i32a.copyWithin(0, 2);
// Int32Array [3, 4, 5, 4, 5]

// On platforms that are not yet ES2015 compliant: 
[].copyWithin.call(new Int32Array([1, 2, 3, 4, 5]), 0, 3, 4);
// Int32Array [4, 2, 3, 4, 5]
```

##### 6、数组元素的删除

splice会改变元素的长度和被删除元素之后的索引，delete不会改变元素的长度，也不会改变元素的索引。

1）`splice()`方法通过删除或替换现有元素或者原地添加新的元素来修改数组,并以数组形式返回被修改的内容。此方法会改变原数组。

该方法会传递三个参数start、deleteCount、items。

start代表指定修改位置的开始，如果超出了数组长度，就从数组末尾开始添加内容，如果是负值，则表示从末尾开始第几位，如果负数的绝对值大于数组的长度，则表示开始位置为第0位。

deleteCount代表要删除的数组元素的个数。为0或者负数时代表不删除元素，至少添加一个元素。为正整数时，则代表从start位置开始，删除n个元素。

items是一些数组元素，从start开始，在被删除元素的位置添加进去新的元素，如果不指定，则代表只删除元素。

返回值是被删除元素组成的一个数组，如果没有删除元素，则返回空数组。

```javascript
//从第 2 位开始删除 0 个元素，插入“drum”
var myFish = ["angel", "clown", "mandarin", "sturgeon"];
var removed = myFish.splice(2, 0, "drum");

// 运算后的 myFish: ["angel", "clown", "drum", "mandarin", "sturgeon"]
// 被删除的元素: [], 没有元素被删除

//从第 2 位开始删除 0 个元素，插入“drum” 和 "guitar"
var myFish = ['angel', 'clown', 'mandarin', 'sturgeon'];
var removed = myFish.splice(2, 0, 'drum', 'guitar');

// 运算后的 myFish: ["angel", "clown", "drum", "guitar", "mandarin", "sturgeon"]
// 被删除的元素: [], 没有元素被删除

//从第 3 位开始删除 1 个元素
var myFish = ['angel', 'clown', 'drum', 'mandarin', 'sturgeon'];
var removed = myFish.splice(3, 1);

// 运算后的 myFish: ["angel", "clown", "drum", "sturgeon"]
// 被删除的元素: ["mandarin"]

//从第 2 位开始删除 1 个元素，插入“trumpet”
var myFish = ['angel', 'clown', 'drum', 'sturgeon'];
var removed = myFish.splice(2, 1, "trumpet");

// 运算后的 myFish: ["angel", "clown", "trumpet", "sturgeon"]
// 被删除的元素: ["drum"]
```

2）delete arr[index]，不会改变原数组的长度，只会将对应位置的元素删除，并变为空元素,原来数组的索引依旧不变。

```javascript
let arr=[1,2,3,4,5,6,7];
delete arr[3];
console.log(arr);
//打印结果为[1,2,3,,5,6,7]
```

##### 6、Iterator对象数组

1）`entries()` 方法返回一个新的**Array Iterator**对象，该对象包含数组中每个索引的键/值对

~~~javascript
var arr = ["a", "b", "c"]; 
var iterator = arr.entries();
console.log(iterator.next());

/*
结果为一个Object对象：
 Object{
    done:false,
    value:
    _proto_:Object
 }
 当done为tue的时候 代表遍历结束
 
iterator.next()返回一个对象，对于有元素的数组，是next{ value: Array(2), done: false }；
next.done 用于指示迭代器是否完成：在每次迭代时进行更新而且都是false，直到迭代器结束done才是true。
next.value是一个["key","value"]的数组，是返回的迭代器中的元素值。
*/

var arr = ["a", "b", "c"];
var iter = arr.entries();
var a = [];

// for(var i=0; i< arr.length; i++){   // 实际使用的是这个 
for(var i=0; i< arr.length+1; i++){    // 注意，是length+1，比数组的长度大
    var tem = iter.next();             // 每次迭代时更新next
    console.log(tem.done);             // 这里可以看到更新后的done都是false
    if(tem.done !== true){             // 遍历迭代器结束done才是true
        console.log(tem.value);
        a[i]=tem.value;
    }
}
~~~

2） `keys() `方法返回一个包含数组中每个索引键的`**Array Iterator**`对象。

**`values()`** 方法返回一个新的 **`Array Iterator`** 对象，该对象包含数组每个索引的值

```
//索引迭代器会包含那些没有对应元素的索引
var arr = ["a", , "c"];
var sparseKeys = Object.keys(arr);
var denseKeys = [...arr.keys()];
console.log(sparseKeys); // ['0', '2']
console.log(denseKeys);  // [0, 1, 2]

//使用 for...of 循环进行迭代
//Array.prototype.values 是 Array.prototype[Symbol.iterator] 的默认实现。
let arr = ['w', 'y', 'k', 'o', 'p'];
let eArr = arr.values();

for (let letter of eArr) {
  console.log(letter);
} //"a" "b" "c" "d"
//使用 .next() 迭代
var arr = ['a', 'b', 'c', 'd', 'e'];
var iterator = arr.values(); 
iterator.next();               // Object { value: "a", done: false }
iterator.next().value;         // "b"
iterator.next()["value"];      // "c"
iterator.next();               // Object { value: "d", done: false }
iterator.next();               // Object { value: "e", done: false }
iterator.next();               // Object { value: undefined, done: true } 
iteraroe.next().value;         // undefined
```

##### 7、测试数组

1）`every()`方法测试一个数组内的所有元素是否能通过某个指定函数的测试，它返回一个布尔值。它可以接收三个参数

~~~javascript
const isBelowThreshold = (currentValue) => currentValue < 40;

const array1 = [1, 30, 39, 29, 10, 13];

console.log(array1.every(isBelowThreshold));
// expected output: true
~~~

2）`some()` 方法测试数组中是不是至少有1个元素通过了被提供的函数测试。它返回的是一个Boolean类型的值。

~~~javascript
//测试数组元素的值
function isBiggerThan10(element, index, array) {
  return element > 10;
}

[2, 5, 8, 1, 4].some(isBiggerThan10);  // false
[12, 5, 8, 1, 4].some(isBiggerThan10); // true
//使用箭头函数测试数组元素的值
[2, 5, 8, 1, 4].some(x => x > 10);  // false
[12, 5, 8, 1, 4].some(x => x > 10); // true
//判断数组元素中是否存在某个值  此例中为模仿 includes()  方法, 若元素在数组中存在, 则回调函数返回值为 true 
var fruits = ['apple', 'banana', 'mango', 'guava'];

function checkAvailability(arr, val) {
  return arr.some(function(arrVal) {
    return val === arrVal;
  });
}

checkAvailability(fruits, 'kela');   // false
checkAvailability(fruits, 'banana'); // true
~~~

##### 8、展开数组

1）`flat()` 方法会按照一个可指定的深度递归遍历数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回。参数为depth，可选，默认为1，代表要提取的嵌套数组的结构深度，返回值是一个包含将数组与子数组中所有元素的新数组。

~~~javascript
//扁平化嵌套数组
var arr1 = [1, 2, [3, 4]];
arr1.flat(); 
// [1, 2, 3, 4]

var arr2 = [1, 2, [3, 4, [5, 6]]];
arr2.flat();
// [1, 2, 3, 4, [5, 6]]

var arr3 = [1, 2, [3, 4, [5, 6]]];
arr3.flat(2);
// [1, 2, 3, 4, 5, 6]

//使用 Infinity，可展开任意深度的嵌套数组
var arr4 = [1, 2, [3, 4, [5, 6, [7, 8, [9, 10]]]]];
arr4.flat(Infinity);
// [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]


//扁平化与数组空项  flat() 方法会移除数组中的空项:
var arr4 = [1, 2, , 4, 5];
arr4.flat();
// [1, 2, 4, 5]


//替代方案
//使用 reduce 与 concat
var arr = [1, 2, [3, 4]];

// 展开一层数组
arr.flat();
// 等效于
arr.reduce((acc, val) => acc.concat(val), []);
// [1, 2, 3, 4]

// 使用扩展运算符 ...
const flattened = arr => [].concat(...arr);

//reduce + concat + isArray + recursivity
// 使用 reduce、concat 和递归展开无限多层嵌套的数组
var arr1 = [1,2,3,[1,2,3,4, [2,3,4]]];

function flatDeep(arr, d = 1) {
   return d > 0 ? arr.reduce((acc, val) => acc.concat(Array.isArray(val) ? flatDeep(val, d - 1) : val), [])
                : arr.slice();
};

flatDeep(arr1, Infinity);
// [1, 2, 3, 1, 2, 3, 4, 2, 3, 4]


//forEach+isArray+push+recursivity
// forEach 遍历数组会自动跳过空元素
const eachFlat = (arr = [], depth = 1) => {
  const result = []; // 缓存递归结果
  // 开始递归
  (function flat(arr, depth) {
    // forEach 会自动去除数组空位
    arr.forEach((item) => {
      // 控制递归深度
      if (Array.isArray(item) && depth > 0) {
        // 递归数组
        flat(item, depth - 1)
      } else {
        // 缓存元素
        result.push(item)
      }
    })
  })(arr, depth)
  // 返回递归结果
  return result;
} 

// for of 循环不能去除数组空位，需要手动去除
const forFlat = (arr = [], depth = 1) => {
  const result = [];
  (function flat(arr, depth) {
    for (let item of arr) {
      if (Array.isArray(item) && depth > 0) {
        flat(item, depth - 1)
      } else {
        // 去除空元素，添加非undefined元素
        item !== void 0 && result.push(item);
      }
    }
  })(arr, depth)
  return result;
}


//使用堆栈stack
// 无递归数组扁平化，使用堆栈
// 注意：深度的控制比较低效，因为需要检查每一个值的深度
// 也可能在 shift / unshift 上进行 w/o 反转，但是末端的数组 OPs 更快
var arr1 = [1,2,3,[1,2,3,4, [2,3,4]]];
function flatten(input) {
  const stack = [...input];
  const res = [];
  while (stack.length) {
    // 使用 pop 从 stack 中取出并移除值
    const next = stack.pop();
    if (Array.isArray(next)) {
      // 使用 push 送回内层数组中的元素，不会改动原始输入
      stack.push(...next);
    } else {
      res.push(next);
    }
  }
  // 反转恢复原数组的顺序
  return res.reverse();
}
flatten(arr1);// [1, 2, 3, 1, 2, 3, 4, 2, 3, 4]

// 递归版本的反嵌套
function flatten(array) {
  var flattend = [];
  (function flat(array) {
    array.forEach(function(el) {
      if (Array.isArray(el)) flat(el);
      else flattend.push(el);
    });
  })(array);
  return flattend;
}

//使用 Generator 函数
function* flatten(array) {
    for (const item of array) {
        if (Array.isArray(item)) {
            yield* flatten(item);
        } else {
            yield item;
        }
    }
}

var arr = [1, 2, [3, 4, [5, 6]]];
const flattened = [...flatten(arr)];
// [1, 2, 3, 4, 5, 6]

~~~

2）`flatMap()` 方法首先使用映射函数映射每个元素，然后将结果压缩成一个新数组。它与 [map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) 连着深度值为1的 [flat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat) 几乎相同，但 `flatMap` 通常在合并成一种方法的效率稍微高一些。

~~~javascript
//var arr1 = [1, 2, 3, 4];

arr1.map(x => [x * 2]); 
// [[2], [4], [6], [8]]

arr1.flatMap(x => [x * 2]);
// [2, 4, 6, 8]

// only one level is flattened
arr1.flatMap(x => [[x * 2]]);
// [[2], [4], [6], [8]]
var arr1 = [1, 2, 3, 4];


//更直观的例子
let arr1 = ["it's Sunny in", "", "California"];

arr1.map(x => x.split(" "));
// [["it's","Sunny","in"],[""],["California"]]

arr1.flatMap(x => x.split(" "));
// ["it's","Sunny","in", "", "California"]



let a = [5, 4, -3, 20, 17, -33, -4, 18];
a.flatMap( (n) =>
  (n < 0) ?      [] :
  (n % 2 == 0) ? [n] :
                 [n-1, 1]
)
~~~

##### 9、数组高阶函数

1）`filter()` 方法创建一个新数组, 其包含通过所提供函数实现的测试的所有元素。

```javascript
//下例使用 filter 创建了一个新数组，该数组的元素由原数组中值大于 10 的元素组成
function isBigEnough(element) {
  return element >= 10;
}
var filtered = [12, 5, 8, 130, 44].filter(isBigEnough);
//使用 filter() 创建具有非零 id 的元素的 json。
var arr = [
  { id: 15 },
  { id: -1 },
  { id: 0 },
  { id: 3 },
  { id: 12.2 },
  { },
  { id: null },
  { id: NaN },
  { id: 'undefined' }
];

var invalidEntries = 0;

function isNumber(obj) {
  return obj !== undefined && typeof(obj) === 'number' && !isNaN(obj);
}

function filterByID(item) {
  if (isNumber(item.id) && item.id !== 0) {
    return true;
  } 
  invalidEntries++;
  return false; 
}

var arrByID = arr.filter(filterByID);

console.log('Filtered Array\n', arrByID); 
// Filtered Array
// [{ id: 15 }, { id: -1 }, { id: 3 }, { id: 12.2 }]

console.log('Number of Invalid Entries = ', invalidEntries); 
// Number of Invalid Entries = 5



//使用 filter() 根据搜索条件来过滤数组内容。
var fruits = ['apple', 'banana', 'grapes', 'mango', 'orange'];

/**
 * Array filters items based on search criteria (query)
 */
function filterItems(query) {
  return fruits.filter(function(el) {
      return el.toLowerCase().indexOf(query.toLowerCase()) > -1;
  })
}

console.log(filterItems('ap')); // ['apple', 'grapes']
console.log(filterItems('an')); // ['banana', 'mango', 'orange']



//简化过滤数组内容
const fruits = ['apple', 'banana', 'grapes', 'mango', 'orange'];

/**
 * Array filters items based on search criteria (query)
 */
const filterItems = (query) => {
  return fruits.filter((el) =>
    el.toLowerCase().indexOf(query.toLowerCase()) > -1
  );
}

console.log(filterItems('ap')); // ['apple', 'grapes']
console.log(filterItems('an')); // ['banana', 'mango', 'orange']
```

2）`map()` 方法创建一个新数组，其结果是该数组中的每个元素都调用一次提供的函数后的返回值。

~~~javascript
//求数组中每个元素的平方根
var numbers = [1, 4, 9];
var roots = numbers.map(Math.sqrt);
// roots的值为[1, 2, 3], numbers的值仍为[1, 4, 9]

//使用 map 重新格式化数组中的对象
var kvArray = [{key: 1, value: 10}, 
               {key: 2, value: 20}, 
               {key: 3, value: 30}];

var reformattedArray = kvArray.map(function(obj) { 
   var rObj = {};
   rObj[obj.key] = obj.value;
   return rObj;
});

// reformattedArray 数组为： [{1: 10}, {2: 20}, {3: 30}], 

// kvArray 数组未被修改: 
// [{key: 1, value: 10}, 
//  {key: 2, value: 20}, 
//  {key: 3, value: 30}]


//使用一个包含一个参数的函数来mapping(构建)一个数字数组
var numbers = [1, 4, 9];
var doubles = numbers.map(function(num) {
  return num * 2;
});

// doubles数组的值为： [2, 8, 18]
// numbers数组未被修改： [1, 4, 9]


//如何在一个 String  上使用 map 方法获取字符串中每个字符所对应的 ASCII 码组成的数组
var map = Array.prototype.map
var a = map.call("Hello World", function(x) { 
  return x.charCodeAt(0); 
})
// a的值为[72, 101, 108, 108, 111, 32, 87, 111, 114, 108, 100]
~~~

3）`reduce()` 方法对数组中的每个元素执行一个由您提供的**reducer**函数(升序执行)，将其结果汇总为单个返回值。

`reduce`为数组中的每一个元素依次执行`callback`函数，不包括数组中被删除或从未被赋值的元素，接受四个参数：

1. Accumulator (acc) (累计器)
2. Current Value (cur) (当前值)
3. Current Index (idx) (当前索引)
4. Source Array (src) (源数组)

**callback**执行数组中每个值 (如果没有提供 `initialValue则第一个值除外`)的函数，包含四个参数：

`accumulator`：累计器累计回调的返回值; 它是上一次调用回调时返回的累积值，或`initialValue`

`currentValue`：数组中正在处理的元素。

`index` 可选 ：数组中正在处理的当前元素的索引。 如果提供了`initialValue`，则起始索引号为0，否则从索引1起始。

`array`可选：调用`reduce()`的数组

`initialValue`可选：作为第一次调用 `callback`函数时的第一个参数的值。 如果没有提供初始值，则将使用数组中的第一个元素。 在没有初始值的空数组上调用 reduce 将报错。

~~~javascript
var sum = [0, 1, 2, 3].reduce(function (accumulator, currentValue) {
  return accumulator + currentValue;
}, 0);
// 和为 6

//箭头函数简化
var total = [ 0, 1, 2, 3 ].reduce(
  ( acc, cur ) => acc + cur,
  0
);
//累加对象数组里的值
var initialValue = 0;
var sum = [{x: 1}, {x:2}, {x:3}].reduce(function (accumulator, currentValue) {
    return accumulator + currentValue.x;
},initialValue)

console.log(sum) // logs 6
//箭头函数简化
var initialValue = 0;
var sum = [{x: 1}, {x:2}, {x:3}].reduce(
    (accumulator, currentValue) => accumulator + currentValue.x
    ,initialValue
);

console.log(sum) // logs 6

//将二维数组转换为一维数组
var flattened = [[0, 1], [2, 3], [4, 5]].reduce(
  function(a, b) {
    return a.concat(b);
  },
  []
);
// flattened is [0, 1, 2, 3, 4, 5]
//箭头函数简化
var flattened = [[0, 1], [2, 3], [4, 5]].reduce(
 ( acc, cur ) => acc.concat(cur),
 []
);

//计算数组中每个元素出现的次数
var names = ['Alice', 'Bob', 'Tiff', 'Bruce', 'Alice'];

var countedNames = names.reduce(function (allNames, name) { 
  if (name in allNames) {
    allNames[name]++;
  }
  else {
    allNames[name] = 1;
  }
  return allNames;
}, {});
// countedNames is:
// { 'Alice': 2, 'Bob': 1, 'Tiff': 1, 'Bruce': 1 }
~~~

4）`reduceRight()` 方法接受一个函数作为累加器（accumulator）和数组的每个值（从右到左）将其减少为单个值。

~~~javascript
const array1 = [[0, 1], [2, 3], [4, 5]].reduceRight(
  (accumulator, currentValue) => accumulator.concat(currentValue)
);

console.log(array1);
// expected output: Array [4, 5, 2, 3, 0, 1]
~~~

##### 10、数组转换为字符串

1）`toString()`和`toLcoaleString()`都是将数组转换为字符串。

- toLocalString()是调用每个数组元素的 toLocaleString() 方法，然后使用地区特定的分隔符把生成的字符串连接起来，形成一个字符串。
- toString()方法获取的是String(传统字符串),而toLocaleString()方法获取的是LocaleString(本地环境字符串)。
- 如果你开发的脚本在世界范围都有人使用，那么将对象转换成字符串时请使用toString()方法来完成。
- LocaleString()会根据你机器的本地环境来返回字符串，它和toString()返回的值在不同的本地环境下使用的符号会有微妙的变化。
- 所以使用toString()是保险的，返回唯一值的方法,它不会因为本地环境的改变而发生变化。如果是为了返回时间类型的数据，推荐使用LocaleString()。若是在后台处理字符串，请务必使用toString()。

2）`join()` 方法将一个数组（或一个[类数组对象](https://developer.mozilla.org/zh-CN//docs/Web/JavaScript/Guide/Indexed_collections#Working_with_array-like_objects)）的所有元素连接成一个字符串并返回这个字符串。如果数组只有一个项目，那么将返回该项目而不使用分隔符。

`arr.join([separator])``separator` 可选，指定一个字符串来分隔数组的每个元素。如果需要，将分隔符转换为字符串。如果缺省该值，数组元素用逗号（`,`）分隔。如果`separator`是空字符串(`""`)，则所有元素之间都没有任何字符。

返回值：一个所有数组元素连接的字符串。如果 `arr.length` 为0，则返回空字符串。

~~~javascript
//使用四种不同的分隔符连接数组元素
var a = ['Wind', 'Rain', 'Fire'];
var myVar1 = a.join();      // myVar1的值变为"Wind,Rain,Fire"
var myVar2 = a.join(', ');  // myVar2的值变为"Wind, Rain, Fire"
var myVar3 = a.join(' + '); // myVar3的值变为"Wind + Rain + Fire"
var myVar4 = a.join('');    // myVar4的值变为"WindRainFire"

//连接类数组对象
function f(a, b, c) {
  var s = Array.prototype.join.call(arguments);
  console.log(s); // '1,a,true'
}
f(1, 'a', true);
~~~

##### 11、填充、替换数组

fill和splice都可以做数组元素的替换，splice基于先删除后添加的思想，fill是直接覆盖。

`fill()`方法用固定值填充一个数组中从起始索引到终止索引内的全部元素，不包括终止索引。该方法可传递三个参数`arr.fill(value[, start[, end]])`，value代表用来填充数组元素的值，start代表起始位置，end代表终止位置。

~~~javascript
[1, 2, 3].fill(4);               // [4, 4, 4]
[1, 2, 3].fill(4, 1);            // [1, 4, 4]
[1, 2, 3].fill(4, 1, 2);         // [1, 4, 3]
[1, 2, 3].fill(4, 1, 1);         // [1, 2, 3]
[1, 2, 3].fill(4, 3, 3);         // [1, 2, 3]
[1, 2, 3].fill(4, -3, -2);       // [4, 2, 3]
[1, 2, 3].fill(4, NaN, NaN);     // [1, 2, 3]
[1, 2, 3].fill(4, 3, 5);         // [1, 2, 3]
Array(3).fill(4);                // [4, 4, 4]
[].fill.call({ length: 3 }, 4);  // {0: 4, 1: 4, 2: 4, length: 3}

// Objects by reference.
var arr = Array(3).fill({}) // [{}, {}, {}];
// 需要注意如果fill的参数为引用类型，会导致都执行都一个引用类型
// 如 arr[0] === arr[1] 为true
arr[0].hi = "hi"; // [{ hi: "hi" }, { hi: "hi" }, { hi: "hi" }]
~~~

##### 12、查找满足条件的元素

1）`find()`方法返回数组中满足提供的测试函数的第一个元素的值。否则返回undefined。

补充：在第一次调用 `callback`函数时会确定元素的索引范围，因此在 `find`方法开始执行之后添加到数组的新元素将不会被 `callback`函数访问到。如果数组中一个尚未被`callback`函数访问到的元素的值被`callback`函数所改变，那么当`callback`函数访问到它时，它的值是将是根据它在数组中的索引所访问到的当前值。被删除的元素仍旧会被访问到，但是其值已经是undefined了。

~~~javascript
//用对象属性查找数组里的对象
var inventory = [
    {name: 'apples', quantity: 2},
    {name: 'bananas', quantity: 0},
    {name: 'cherries', quantity: 5}
];

function findCherries(fruit) { 
    return fruit.name === 'cherries';
}

console.log(inventory.find(findCherries)); // { name: 'cherries', quantity: 5 }


//寻找数组中的质数
function isPrime(element, index, array) {
  var start = 2;
  while (start <= Math.sqrt(element)) {
    if (element % start++ < 1) {
      return false;
    }
  }
  return element > 1;
}

console.log([4, 6, 8, 12].find(isPrime)); // undefined, not found
console.log([4, 5, 8, 12].find(isPrime)); // 5

~~~

2）`findIndex()`方法返回数组中满足提供的测试函数的第一个元素的**索引**。否则返回-1。

~~~javascript
const array1 = [5, 12, 8, 130, 44];

const isLargeNumber = (element) => element > 13;

console.log(array1.findIndex(isLargeNumber));
// expected output: 3
~~~

##### 13、合并数组

`concat()` 方法用于合并两个或者多个数组，该方法不会改变现有数组，只会返回一个新数组

```
let a1=['a', 'b', 'c'];
let a2=['d', 'e', 'f'];
let a3=a1.concat(a2);
//Array ["a", "b", "c", "d", "e", "f"]
```

##### 14、判断是否包含某元素

`includes()` 方法用来判断一个数组是否包含一个指定的值，根据情况，如果包含则返回 true，否则返回false。`arr.includes(valueToFind[, fromIndex])` valueToFind代表需要查找的元素值，fromIndex代表查找起始位的索引，如果是负值，则从末尾开始往前跳 `fromIndex` 的绝对值个索引，然后往后搜寻。

~~~javascript
[1, 2, 3].includes(2);     // true
[1, 2, 3].includes(4);     // false
[1, 2, 3].includes(3, 3);  // false
[1, 2, 3].includes(3, -1); // true
[1, 2, NaN].includes(NaN); // true
~~~

##### 15、位置方法

1）`indexOf()`方法返回在数组中可以找到一个给定元素的第一个索引，如果不存在，则返回-1。同样，也可以传递fromIndex参数。

~~~javascript
var array = [2, 5, 9];
array.indexOf(2);     // 0
array.indexOf(7);     // -1
array.indexOf(9, 2);  // 2
array.indexOf(2, -1); // -1
array.indexOf(2, -3); // 0
~~~

2）`lastIndexOf()` 方法返回指定元素（也即有效的 JavaScript 值或变量）在数组中的最后一个的索引，如果不存在则返回 -1。从数组的后面向前查找，从 `fromIndex` 处开始。

~~~javascript
var array = [2, 5, 9, 2];
var index = array.lastIndexOf(2);
// index is 3
index = array.lastIndexOf(7);
// index is -1
index = array.lastIndexOf(2, 3);
// index is 3
index = array.lastIndexOf(2, 2);
// index is 0
index = array.lastIndexOf(2, -2);
// index is 0
index = array.lastIndexOf(2, -1);
// index is 3


//下例使用 lastIndexOf 查找到一个元素在数组中所有的索引（下标），并使用 push 将所有添加到另一个数组中。
var indices = [];
var array = ['a', 'b', 'a', 'c', 'a', 'd'];
var element = 'a';
var idx = array.lastIndexOf(element);

while (idx != -1) {
  indices.push(idx);
  idx = (idx > 0 ? array.lastIndexOf(element, idx - 1) : -1);
}

console.log(indices);
// [4, 2, 0];
~~~

##### 16、数组的遍历

`forEach()` 方法对数组的每个元素执行一次给定的函数。

~~~javascript
//不对未初始化的值进行任何操作（稀疏数组）
const arraySparse = [1,3,,7];
let numCallbackRuns = 0;

arraySparse.forEach(function(element){
  console.log(element);
  numCallbackRuns++;
});

console.log("numCallbackRuns: ", numCallbackRuns);

// 1
// 3
// 7
// numCallbackRuns: 3

//如果数组在迭代时被修改了，则其他元素会被跳过。
var words = ['one', 'two', 'three', 'four'];
words.forEach(function(word) {
  console.log(word);
  if (word === 'two') {
    words.shift();
  }
});
// one
// two
// four
~~~

