# 理解JS的执行过程

### 了解前置概念

#### 1、代码是怎么运行的？

代码最终是会由CPU去执行，但是CPU并不认识我们写的代码，它只认识二进制数据，但二进制数据对我们的可读性太差，所以为了可读性，就出现了汇编语言。

 汇编语言解决了二进制指令可读性不高的缺点，但是它也有很多问题。

- 编写的代码难以理解，不好维护
- 它只针对特定的体系结构和处理器进行优化
- 开发效率低，容易出现bug，不好调试

所以，这就有了高级语言。

#### 2、了解编译

因为高级语言更符合我们的人类的思维和阅读习惯，所以把它们叫做高级语言。计算机要执行这些高级语言的时候，必须先将高级语言转换为机器指令，cpu才可以执行，这个过程叫做编译。

##### 为什么需要编译

之所以需要编译的原因是因为在不同的操作系统上，代码和文件是不通用的，这些代码的底层通常都需要操作系统，API，内存，线程，进程等资源，但是不同的操作系统实现的方式也不一样，所以针对不同的操作系统我们需要使用不同的编译器去对他们进行编译。

#### 3、解释型语言和编译型语言的区别

编译型语言是在运行前将代码转换为机器指令。

解释性语言是在运行的时候才讲代码转换为机器指令。

### 了解JS的解析引擎

编译型语言都需要一个编译器来编译代码，而解释性语言，也都会需要一个引擎来编译代码。js最初是运行在浏览器上的语言，所以它的解析引擎也都内嵌于浏览器中，不同的浏览器对于引擎的实现也不相同，所以部分语法以及实现效果也不太一样。现在有了node，它是将V8引擎抽离出来作为编译器使用，以此来达到实现js运行在服务器的目的。

```
Mozilla浏览器 -----> 解析引擎为 Spidermonkey(由c语言实现的)
Chrome浏览器 ------> 解析引擎为 V8(它是由c++实现的)
Safari浏览器 ------> 解析引擎为 JavaScriptCore(c/c++)
IE and Edge ------> 解析引擎为 Chakra(c++)
Node.js     ------> 解析引擎为 V8
```

解析引擎会根据ECMAScript定义的语言标准来动态执行JS字符串。

#### 解析引擎是如何解析js的

解析JS分为两个阶段。第一个部分就是语法检查阶段，第二部分就是运行阶段。语法检查又分为词法分析和语法分析，运行阶段分为预解析和运行时。

`词法分析`：

​      词法分析会将js代码中的字符串分割为有意义的代码块，这些代码块我们可以称之为'词法单元'

比如：var a=1; 会被拆分为 var 、a、=、1，这些零散的语法会组成一个词法单元进行解析 

```json
[
    {
        "type": "Keyword",
        "value": "var"
    },
    {
        "type": "Identifier",
        "value": "a"
    },
    {
        "type": "Punctuator",
        "value": "="
    },
    {
        "type": "Numeric",
        "value": "1"
    }
]
```

可以看看babel编译器的代码，来理解js是如何封装词法分析的。

```js
<!DOCTYPE html>
<html>
<head>
  <title></title>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=0">
</head>
<body>
  <div id="app">
  </div>
  <script type="text/javascript">
      function tokenizer(input) {
        // 记录当前解析到词的位置
        var current = 0;
        // tokens 用来保存我们解析的token
        var tokens = [];
        // 利用循环进行解析
        while(current < input.length) {
            // 提取出当前要解析的字符
            var char = input[current];

            // 处理符号: 检查是否是符号
        var PUNCTUATOR = /[`~!@#$%^&*()_\-+=<>?:"{}|,.\/;'\\[\]·~！@#￥%……&*（）——\-+={}|《》？：“”【】、；‘’，。、]/im;
        if (PUNCTUATOR.test(char)) {
          // 创建变量用于保存匹配的符号
          var punctuators = char;
          // 判断是否是箭头函数的符号
          if (char === '=' && input[current + 1] === '>') {
              punctuators += input[++current];
          }    
          current++;
          // 最后把数据存入到tokens中
          tokens.push({
              type: 'Punctuator',
              value: punctuators
          });
          // 进入下一次循环
          continue;
        }
        // 下面是处理空格，如果是空格的话，则直接进入下一个循环
        var WHITESPACE = /\s/;
        if (WHITESPACE.test(char)) {
          current++;
          continue;    
        }
        // 处理数字，检查是否是数字
        var NUMBERS = /[0-9]/;
        if (NUMBERS.test(char)) {
          // 创建变量，用于保存匹配的数字
          var number = '';
          // 循环当前的字符及下一个字符，直到不是数字为止
          while(NUMBERS.test(char)) {
              number += char;
              char = input[++current];
          }
          // 最后我们把数据更新到tokens中
          tokens.push({
              type: 'Numeric',
              value: number
          });
          // 进入下一个循环
          continue;    
        }
        // 检查是否是字符
        var LETTERS = /[a-z]/i;
        if (LETTERS.test(char)) {
          // 创建一个临时变量保存该字符
          var value = '';
          // 循环遍历所有的字母
          while(LETTERS.test(char)) {
              value += char;
              char = input[++current];
          }
          // 判断当前的字符串是否是关键字
          var KEYWORD = /function|var|return|let|const|if|for/;
          if (KEYWORD.test(value)) {
              // 标记关键字
              tokens.push({
                type: 'Keyword',
                value: value    
              })
          } else {
              // 标记变量
              tokens.push({
                type: 'Identifier',
                value: value
              })
          }
          // 进入下一次循环
          continue;    
        }
        // 如果我们没有匹配上任何类型的token; 那么就抛出一个错误
        throw new TypeError('I dont konw what this character is:' + char);
        }    
        // 最后我们返回词法单元数组
        return tokens;
      }

      //代码调用
      var str = 'var a = 1';
      console.log(tokenizer(str));
  </script>
</body>
</html>
```

这个str字符长度为9，从0开始，从第一个字符开始解析，判断该字符是 否为符号、空格、数字、字母等操作。如果是字母的话，继续判断下一个字符是否是字母，依次类推，直到下一个字符不是字母的话，就获取该值，因此获取到的 value为 'var';
然后会判断该字符串是否为关键字，如关键字: var KEYWORD = /function|var|return|let|const|if|for/;这些其中的一个，如果是的话，直接标记为关键字，存入tokens数组中，如下代码：

```js
tokens.push({
  type: 'Keyword',
  value: value    
});

因此 tokens = [{ type: 'Keyword', value: 'var' }];
```

`语法分析`

  在这个过程中会将词法单元流转换成一颗 抽象语法树(AST)。比如 var a = 1; 的词法单元流就会被解析成下面的AST；

```json
{
    "type": "Program",
    "body": [
        {
            "type": "VariableDeclaration",
            "declarations": [
                {
                    "type": "VariableDeclarator",
                    "id": {
                        "type": "Identifier",
                        "name": "a"
                    },
                    "init": {
                        "type": "Literal",
                        "value": 1,
                        "raw": "1"
                    }
                }
            ],
            "kind": "var"
        }
    ],
    "sourceType": "script"
}
```

`转换` 生成AST树之后，我们还需要对其中的节点进行操作,比如我们需要将ES6代码转换为ES5。我们可以添加、移动、替换及删除AST抽象树的节点操作。

```js
/*
  为了修改AST抽象树，我们首先要对节点进行遍历
  @param AST语法树
  @param visitor定义转换函数，也可以使用visitor函数进行转换
*/
function traverser(ast, visitor) {
  // 遍历树中的每个节点
  function traverseArray(array, parent) {
      if (typeof array.forEach === 'function') {
        array.forEach(function(child) {
            traverseNode(child, parent);
        });    
      }
  }
  function traverseNode(node, parent) {
      // 看下 vistory中有没有对应的type处理函数
      var method = visitor[node.type];
      if (method) {
        method(node, parent);    
      }
      switch(node.type) {
        // 从顶层的Program开始
        case 'Program':
            traverseArray(node.body, node);
            break;
        // 如下的是不需要转换的
        case 'VariableDeclaration':
        case 'VariableDeclarator':
        case 'AssignmentExpression':
        case 'Identifier':
        case 'Literal':
            break;
        default:
            throw new TypeError(node.type)    
      }
  }
  traverseNode(ast, null)
}

/*
  下面是转换器，它用于遍历过程中转换数据，
  我们接收之前的AST树作为参数，最后会生成一个新的AST抽象树
*/
function transformer(ast) {
  // 创建新的ast抽象树
  var newAst = {
      type: 'Program',
      body: [],
      sourceType: 'script'
  };
  ast._context = newAst.body;
  // 我们把AST 和 vistor 作为参数传入进去
  traverser(ast, {
      VariableDeclaration: function(node, parent) {
        var variableDeclaration = {
            type: 'VariableDeclaration',
            declarations: node.declarations,
            kind: 'var'
        };
        // 把新的 VariableDeclaration 放入到context中
        parent._context.push(variableDeclaration);     
      }
  });
  // 最后返回创建号的新AST
  return newAst;
}

var ast = {
"type": "Program",
"body": [
    {
        "type": "VariableDeclaration",
        "declarations": [
            {
                "type": "VariableDeclarator",
                "id": {
                    "type": "Identifier",
                    "name": "a"
                },
                "init": {
                    "type": "Literal",
                    "value": 1,
                    "raw": "1"
                }
            }
        ],
        "kind": "const"
    }
],
"sourceType": "script"
}
console.log(ast);
console.log('转换后的-------');
console.log(transformer(ast));
```

运行阶段：包括预解析和运行时。

经过上述词法分析与语法分析之后，编译基本已经完成。现在我们需要js代码进行预解析。

预解析:在js文件或者script里面的代码在正式开始执行之前，会进行一些解析工作。比如会先在全局中找到var关键字声明的变量和通过function关键字声明的函数，找到之后，会进行变量声明提升和函数提升。

预解析需要注意的几个问题：

  1、预解析是全局预解析，函数体未调用时是不进行预解析的。

  2、只有var 和function会被提升

  3、只在所在作用域提升

 4、预解析结束后，会顺序执行代码、