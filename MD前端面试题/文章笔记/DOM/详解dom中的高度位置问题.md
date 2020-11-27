# 详解DOM中的宽高、位置问题

DOM中的计算宽高位置的方法，主要分为三大类。

- client   客户端

- offset   偏移量

- scroll   滚动内容

  ![](D:\Program Files (x86)\OneDrive文件\OneDrive\桌面\client.png)

![](D:\Program Files (x86)\OneDrive文件\OneDrive\桌面\offset.png)

![](D:\Program Files (x86)\OneDrive文件\OneDrive\桌面\scroll.png)

## 一、宽高问题

### 计算宽高的三类属性：

- clientWidth    clientHeight
- offsetWidth   offsetHeight
- scrollWitdh    scrollWidth  

### 1、普通DOM元素

- clientWidth/clientHeight   计算的是元素自身的宽高+padding-滚动条的宽度
- offsetWidth/clientHeight    计算的是元素自身的宽高+padding+border的宽度-滚动条的宽度
- scrollWidth/clientHeight    
  - 如果没有滚动条，等同于client
  - 如果有，那么计算的是元素实际内容所占据的宽高-滚动条的宽度

### 2、body

body的宽度永远都是与html的宽度一样

- clientWidth/clientHeight   计算的是body实际的宽高-滚动条的宽高
- offsetWidth/clientHeight   计算的是body实际内容的宽高-滚动条的宽高
- scrollWidth/clientHeight    计算的是实际body中内容的宽高

### 3、html

body元素自带8px的margin ，而这部分margin对于html来说是内边距，所以即便body的高度为0，html也有16px的offsetHeight

- clientWidth/clientHeight 当前文档的可视宽高-(如果有滚动条，减去滚动条宽高)
- offsetWidth/clientHeight 当前文档的内容宽高-(如果有滚动条，减去滚动条宽高)
- scrollWidth/clientHeight  当前文档的可视宽高

## 二、位置问题

### 计算位置的两类方法：

- offsetLeft   offsetTop
- scrollLeft    scrollTop
- clientLeft   clientTop

### 1、普通dom元素

- offsetLeft/offsetTop   该元素左边框距离父元素左上角的位置（如果父级没有定位，就是相对于浏览器窗口。如果有定位，是对有定位的父级元素）它与position的宽高是相等的

- clientLeft/clientTop  其实就是该元素边框的的宽高

- scrollLeft/scrollTop  

  - 当前元素滚动条的位置，这两个元素只能用于设置了overflow的元素中，否则就没有任何意义。
  - 以上所有元素都是只读，只有这个元素是可以修改的

  ```js
  补充：getBoundingClientRect()用于获得页面中某个元素的左，上，右和下分别相对浏览器视窗的位置。 
  getBoundingClientRect()是DOM元素到浏览器可视范围的距离（不包含文档卷起的部分）。 
  
  
  <div id="box"></div>
  var object=document.getElementById('box');  
  rectObject = object.getBoundingClientRect();
   
  	rectObject.top：元素上边到视窗上边的距离;
  	rectObject.right：元素右边到视窗左边的距离;
  	rectObject.bottom：元素下边到视窗上边的距离;
  	rectObject.left：元素左边到视窗左边的距离;
  	rectObject.width：是元素自身的宽
  	rectObject.height是元素自身的高
  ```

### 2、body

对于html和body的clientLeft ,offsetLeft 没有任何意义，所以不考虑。

### 3、html

