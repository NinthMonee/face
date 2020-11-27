# 概览

> Less （Leaner Style Sheets 的缩写） 是一门向后兼容的 CSS 扩展语言。这里呈现的是 Less 的官方文档（中文版），包含了 Less 语言以及利用 JavaScript 开发的用于将 Less 样式转换成 CSS 样式的 Less.js 工具。

因为 Less 和 CSS 非常像，因此很容易学习。而且 Less 仅对 CSS 语言增加了少许方便的扩展，这就是 Less 如此易学的原因之一。

在 Node.js 环境中使用 Less ：

```
npm install -g less
```

 在浏览器环境中使用 Less ： 

```
<link rel="stylesheet/less" type="text/css" href="styles.less" />
<script src="//cdnjs.cloudflare.com/ajax/libs/less.js/3.10.0-beta/less.min.js" ></script>
```

Less 到底为 CSS 添加了什么功能？以下就是这些新加功能的概览。

### 二、LESS基本用法

#### 2.1变量的设置与使用

设置变量，封装样式：

```
@main-color: #5B83AD;
```


使用变量：

使用变量：

```
body{
    background: @bg-color;
}
```


渲染结果如下：

```
body {
  background: #FFFFFF;
}
```

#### 2.2混合使用

设置混合使用的.class样式，封装样式：

```
.padding{
    padding: 0.5rem 0.625rem;
}
```


调用所需的.class样式

```
.nav-wrapper{
    .padding;//使用less混合功能，仅需在所需要的.class类(.nav-wrapper)中声明.+class类名(.padding)即可直接引入该样式
    background-color: #EFF3F7;
    font-size: 0;
}
```


渲染结果如下：

```
.nav-wrapper {
  padding: 0.5rem 0.625rem;
  background-color: #EFF3F7;
  font-size: 0;
}
```



#### 2.3混合使用延伸1（带参使用）

设置所需的混合参数以及变量，封装样式：

```
.padding{
    padding: 0.5rem 0.625rem;
}
.nav-wrapper(@bgcolor) {
  .padding;
  background-color: @bgcolor;
  font-size: 0;
}
```


混合模式带参使用：

```
.nav{
    .nav-wrapper(#EFF3F7);
}
```


渲染结果如下：

```
.nav {
  padding: 0.5rem 0.625rem;
  background-color: #EFF3F7;
  font-size: 0;
}
```



#### 2.3混合使用延伸2（带默认参数使用）

设置所需的混合参数以及变量，封装样式：

```
.padding{
    padding: 0.5rem 0.625rem;
}
.nav-wrapper(@bgcolor:#EEEEEE) {
  .padding;
  background-color: @bgcolor;
  font-size: 0;
}
```


混合模式带参使用：

```
.nav{
    .nav-wrapper();
}
```


渲染结果如下：

```
.nav {
  padding: 0.5rem 0.625rem;
  background-color: #EEEEEE;
  font-size: 0;
}
```

#### 2.4@_ 匹配模式

栗子：绘制三角形
配置基础样式，封装样式：

```
.triangle(top, @width: 5px, @color: #ccc) {
    border-width: @width;
    border-color: @color transparent transparent transparent;
    border-style: solid dashed dashed dashed;
}

.triangle(right, @width: 5px, @color: #ccc) {
    border-width: @width;
    border-color: transparent @color transparent transparent;
    border-style: dashed solid dashed dashed;
}

.triangle(bottom, @width: 5px, @color: #ccc) {
    border-width: @width;
    border-color: transparent transparent @color transparent;
    border-style: dashed dashed solid dashed;
}

.triangle(left, @width: 5px, @color: #ccc) {
    border-width: @width;
    border-color: transparent transparent transparent @color;
    border-style: dashed dashed dashed solid;
}

.triangle(@_, @width: 5px, @color: #ccc) {
    // @_ 是固定格式，表示不管你匹配到谁，都会带上这些内容
    width: 0;
    height: 0;
    overflow: hidden;
}
```


调用集成样式，如制作顶部倒三角

```
.triangle-top {
    .triangle(top, 30px);
}
```


渲染效果如下：

```
.triangle-top {
  border-width: 30px;
  border-color: #ccc transparent transparent transparent;
  border-style: solid dashed dashed dashed;
  width: 0;
  height: 0;
  overflow: hidden;
}
```

#### 2.5运算

包含加减乘的运算，需要用括号拼接，操作的对象主要是数字、颜色、变量等。(颜色一般情况下很少用运算)
配置基础样式，封装样式：

```
@width：3rem;
```


调用样式：

```
.wrapper{
    width：(@width-0.5)*1;//单位只要有一个带即可
}
```



#### 2.6嵌套规则

嵌套关系主要是父子级的关系层，但是不建议大家使用，该方式不便代码的维护管理，尽可能使用.class来规划业务层级问题。
LESS嵌套参考案例：

```
.list {
    width: 10rem;
    margin: 3rem auto;
    padding: 0;
    font-size: 16px;
    li {
        height: 1.5rem;
        line-height: 1.5rem;
        margin-bottom: 0.375rem;
        padding: 0 0.375rem;
    }
    p {
        float: left;
        color: #333;
        &:hover {
            //&代表上一层选择器
            color: red;
        }
    }
    span {
        float: right;
    }
}
```


渲染结果：

```
.list {
  width: 10rem;
  margin: 3rem auto;
  padding: 0;
  font-size: 16px;
}
.list li {
  height: 1.5rem;
  line-height: 1.5rem;
  margin-bottom: 0.375rem;
  padding: 0 0.375rem;
}
.list p {
  float: left;
  color: #333;
}
.list p:hover {
  color: red;
}
.list span {
  float: right;
}
```

#### 2.7懒人必备arguments变量

使用arguments的好处的就可以帮你自动填充所有变量。
封装样式：

```
.border_arg(@width: 1.75rem, @color: #e6e6e6, @style: solid) {
    border: @arguments
}
```


调用样式：

```
.border_arguments {
    .border_arg(0.375rem); //传参的思想
}
```


渲染结果：

```
.border_arguments {
  border: 0.375rem #e6e6e6 solid;
}
```



### 三、less小技巧

#### 3.1注释问题

```
/*编译后会被保留*/
//编译后不会被保留
```



#### 3.2适用 !important

```
.nav{
    .nav-wrapper(#EFF3F7)!important;//nav-wrapper所有的样式都带上!important
}
```



#### 3.3避免编译符号 ~

在字符串前加上一个~即可使用一些less不认识的专有语法或者一些不正确的css语法。
