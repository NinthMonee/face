## 一、DOM节点

### 1、什么是DOM节点？

HTML 文档中的每个成分都是一个节点。

#### （1）节点类型

- DOM 是这样规定的：

- 整个文档是一个文档节点	 

- 每个 HTML 标签是一个元素节点 

- 包含在 HTML 元素中的文本是文本节点 

- 每一个 HTML 属性是一个属性节点 

- 注释属于注释节点


#### （2）节点层次关系-节点彼此都有等级关系。

HTML 文档中的所有节点组成了一个文档树（或节点树）。HTML 文档中的每个元素、属性、文本等都代表着树中的一个节点。树起始于文档节点，并由此继续伸出枝条，直到处于这棵树最低级别的所有文本节点为止。

<head> 和 <body> 的父节点是 <html> 节点，文本节点 "Hello world!" 的父节点是 <p> 节点。

<head> 节点有一个子节点：<title> 节点。<title> 节点也有一个子节点：文本节点 "DOM Tutorial"。

当节点分享同一个父节点时，它们就是同辈（同级节点）。比方说，<h1> 和 <p>是同辈，因为它们的父节点均是 <body> 节点。

节点也可以拥有后代，后代指某个节点的所有子节点，或者这些子节点的子节点。

节点也可以拥有先辈。先辈是某个节点的父节点，或者父节点的父节点，以此类推。比方说，所有的文本节点都可把 <html> 节点作为先辈节点。

#### （3）节点属性

每个节点都拥有包含着关于节点某些信息的属性。这些属性是：

##### nodeName（节点名称） 

- 元素节点的 nodeName 是标签名称 

- 文本节点的 nodeName 永远是 #text 

- 注释节点的 nodeName 永远是 #comment


##### nodeValue（节点值） 

- 文本节点，nodeValue 属性包含文本。

- 元素节点nodeValue不可用

- 注释节点nodeValue包括注释内容


##### nodeType（节点类型）

|                节点                | 节点类型 |
| :--------------------------------: | :------: |
|         元素节点(Element)          |    1     |
|        属性节点(Attribute)         |    2     |
|        文本节点(TEXT_Node)         |    3     |
|         注释节点(comment)          |    8     |
|         文档节点(Document)         |    9     |
|    文档类型节点(Document_type)     |    10    |
| 文档片段节点(游离文档节点Fragment) |    11    |

|                     判断节点类型                      | 值   |
| :---------------------------------------------------: | ---- |
|       document.body.firstChild.nodeName==="DIV"       | true |
| document.body.firstChild.constructor===HTMLDivElement | true |
|           document.body.firstChild.nodeType           | 1    |

## 二、DOM的创建和获取

### 1、节点的创建

|              创建节点               |     解释     |
| :---------------------------------: | :----------: |
|     document.createElement(“”)      | 创建元素节点 |
|    document.createAttribute(“”)     | 创建属性节点 |
|     document.createComment("")      | 创建注释节点 |
|     document.createTextNode("")     | 创建文本节点 |
| document.createDocumentFragment("") | 创建游离节点 |

### 2、节点的获取

|              获取方式               |                     解释                      |
| :---------------------------------: | :-------------------------------------------: |
|     document.getElementById(“”)     |       根据标签的id获取元素,返回node对象       |
|  document.getElementsByTagName(“”)  |       根据标签名获取元素，返回nodeList        |
|   document.getElementsByName(“”)    |       根据 标签名获取元素，返回nodeList       |
| document.getElementsByClassName("") |       根据class名获取元素，返回nodeList       |
|     document.querySelector( "")     |   根据css选择器获取第一个元素，返回node对象   |
|   document.querySelectorAll( "")    | 根据css选择器获取对应的所有元素，返回node对象 |

## 四、DOM节点的插入复制删除替换

## 五、DOM属性

## 六、DOM的样式

## 七、DOM的属性

