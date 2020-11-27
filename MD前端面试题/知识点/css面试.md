### 1、`<img>`的title和alt有什么不同

- title通常是鼠标滑动到元素上的时候显示
- alt是img的特有元素，是图片内容的等价描述，用于图片无法加载时显示。可以提高图片的高可访问性，除了纯装饰图片外都必须设置有意义的值，搜索引擎会重点分析

### 2、Cnavas和SVG的区别

svg绘制出来的每一个图片的元素都是独立的DOM节点，能够方便的绑定事件或用来修改

svg输出的图片是矢量图片，后期可以修改参数来自由放大缩小，不会失真和锯齿，而canvas输出标量画布，就像一张图片图片一样，放大会失真或者锯齿。

### 3、VW 和em、rem的区别

vw是页面视口1vw 就是1%的视口宽度

em相对父元素的字体大小

rem相对根元素的字体大小



### 4、移动端的1px被渲染为2px的问题

首先要处理移动端1px会比较粗的问题，我们就得知道为什么会有这个问题的出现？

出现这个问题的主要原因是逻辑像素与设备像素的比例不同导致的。我们的UI设计稿通常都是以iphone6/7/8的来设计的，iphone6/7/8的DPR是2，这个机型的DPR是比较标准的，所以UI设计师设计图的时候大多采用这个机型来设计，而UI说的1px其实是物理像素的1px，css是逻辑像素。DPR=物理像素/逻辑像素

1、 媒体查询利用设备像素比缩放，设置小数像素；

​		优点：简单，好理解
​				缺点：兼容性差，目前之余IOS8+才支持，在IOS7及其以下、安卓系统都是显示0px。

- IOS8+下已经支持带小数的px值，media query 对应 devicePixelRatio 有个查询值 -webkit-min-device-pixel-ratio；

```css
.border { border: 1px solid #999 }
@media screen and (-webkit-min-device-pixel-ratio: 2) {
    .border { border: 0.5px solid #999 }
}
@media screen and (-webkit-min-device-pixel-ratio: 3) {
    .border { border: 0.333333px solid #999 }
}
```

2、设置 border-image 方案

缺点：需要制作图片，圆角可能出现模糊

```css
.border-image-1px {
    border-width: 1px 0px;
    -webkit-border-image: url("border.png") 2 0 stretch;
    border-image: url("border.png") 2 0 stretch;
}
border-image：该例意为：距离图片上方2px（属性值上没有单位）裁剪边框图片作为上边框，下方2px裁剪作为下边框。距离左右0像素裁剪图片即没有边框，以拉伸方式展示
```

3、使用**background-img渐变**

除了使用图片外，当然也能使用纯css来实现，百度糯米团就是采用的这种方案。
		缺点：因为每个边框都是线性渐变颜色实现，因此无法实现圆角。

​	设置1px的渐变背景，50%有颜色，50%透明

```css
.border {
    background:
    linear-gradient(180deg, black, black 50%, transparent 50%) top    left  / 100% 1px no-repeat,
    linear-gradient(90deg,  black, black 50%, transparent 50%) top    right / 1px 100% no-repeat,
    linear-gradient(0,      black, black 50%, transparent 50%) bottom right / 100% 1px no-repeat,
    linear-gradient(-90deg, black, black 50%, transparent 50%) bottom left  / 1px 100% no-repeat;
}
```

原理：将原本1个物理像素的边框大小利用线性渐变分割成几个部分（百分比控制），实现小于1像素效果。

`linear-gradient`：指定线性渐变，接受大于等于3个参数，第一个为渐变旋转角度，第二个开始为渐变的颜色和到哪个位置（百分比）全部变为该颜色，该例子中，第一句就是，渐变方向旋转180度，即从上往下（默认为0度从下往上），从红色开始渐变，到50%的位置还是红色，再渐变为继承父元素颜色。

4、box-shadow

利用阴影也可以实现，优点是没有圆角问题，缺点是颜色不好控制

```css
div {
    -webkit-box-shadow: 0 1px 1px -1px rgba(0, 0, 0, 0.5);
}
```

- box-shadow属性的用法：`box-shadow: h-shadow v-shadow [blur] [spread] [color] [inset]`

`参数分别表示: 水平阴影位置，垂直阴影位置，模糊距离， 阴影尺寸，阴影颜色，将外部阴影改为内部阴影，后四个可选。该例中为何将阴影尺寸设置为负数？设置成-1px 是为了让阴影尺寸稍小于div元素尺寸，这样左右两边的阴影就不会暴露出来，实现只有底部一边有阴影的效果。从而实现分割线效果（单边边框）。`

5、viewport+rem

该方案是对上述方案的优化，整体思路就是利用viewport + rem + js 动态的修改页面的缩放比例，实现小于1像素的显示。

缺点：因为缩放涉及全局的rem单位，比较适合新项目，对于老项目可能要涉及到比较多的改动。

6、transform: scale(0.5) 方案 - 推荐: 很灵活（推荐）

​	1) 设置height: 1px，根据媒体查询结合transform缩放为相应尺寸。

```css
div {
    height:1px;
    background:#000;
    -webkit-transform: scaleY(0.5);
    -webkit-transform-origin:0 0;
    overflow: hidden;
}
```

2) 用::after和::befor,设置border-bottom：1px solid #000,然后在缩放-webkit-transform: scaleY(0.5);可以实现两根边线的需求

```css
div::after{
    content:'';width:100%;
    border-bottom:1px solid #000;
    transform: scaleY(0.5);
}
```

3) 用::after设置border：1px solid #000; width:200%; height:200%,然后再缩放scaleY(0.5); 优点可以实现圆角，京东就是这么实现的，缺点是按钮添加active比较麻烦。

```css
.div::after {
    content: '';
    width: 200%;
    height: 200%;
    position: absolute;
    top: 0;
    left: 0;
    border: 1px solid #bfbfbf;
    border-radius: 4px;
    -webkit-transform: scale(0.5,0.5);
    transform: scale(0.5,0.5);
    -webkit-transform-origin: top left;
}
```

7、媒体查询 + transfrom 对方案1的优化（推荐）

```css
/* 2倍屏 */
@media only screen and (-webkit-min-device-pixel-ratio: 2.0) {
    .border-bottom::after {
        -webkit-transform: scaleY(0.5);
        transform: scaleY(0.5);
    }
}
/* 3倍屏 */
@media only screen and (-webkit-min-device-pixel-ratio: 3.0) {
    .border-bottom::after {
        -webkit-transform: scaleY(0.33);
        transform: scaleY(0.33);
    }
}
```

### 5、渐进增强和优雅降级

渐进增强：首先针对低版本浏览器构建页面，保证能实现其最基本的功能，然后再根据高级浏览器进行效果、交互德国改进和追加功能达到更好的用户体验

优雅降级：一开始就构建完整的功能，然后再针对低版本浏览器进行兼容

区别：优雅降级是从复杂的现状开始，并试图减少用户体验的供给，而渐进增强则是从一个非常基础的，能够起作用的版本开始，并不断扩充，以适应未来环境的需要。降级（功能衰减）意味着往回看；而渐进增强则意味着朝前看，同时保证其根基处于安全地带

### 6、简述一下src与href的区别

src是source的缩写，指向的是外部资源的位置，指向的内容将会被嵌入到文档中当前标签所在位置。在请求资源时会将指向的资源下载并应用到文档内，且替换当前元素。

href指向的是网络资源的所在位置，用于当前文档和引用资源之间建立联系。它并不会替换当前元素，

### 7、line-height 三种赋值方式有何区别

- 带单位：px是固定值，而em会参考父元素，font-size值计算自身的行高
- 纯数字：会把比例传递给后代，例如父元素行高1.5  子元素字体为18px、则子元素的行高为1.5*18=27px
- 百分比：将计算后的值传递给后代

### 8、重绘和回流是什么？

- 重绘：当渲染树中的元素外观发生改变，不影响布局时，产生重绘
- 回流：当渲染树中的元素的布局发生改变时，产生回流
- 注意：JS获取布局相关的属性值时（例如：offsetLeft、scrollTop、getComputedStyle）也会引起回流，因为浏览器通过回流计算最新值
- 回流一定会引起重绘，重绘不一定会引起回流

`如何最小化重绘和回流？`

- 需要对元素进行复杂操作时，可以通过display:none先隐藏该元素，操作完成之后再进行显示
- 需要创建多个DOM节点时，使用DcoumentFragment创建完成后一次性的加入document
- 缓存布局相关的属性值：如var  left=elem.offsetLeft，这样做的好处是多次使用left只产生一次回流
- 尽量避免使用table布局,table元素一旦触发回流，就会导致table内其他元素也产生回流
- 避免使用css表达式，因为每次调用都会重新计算值，包括加载页面
- 尽量使用css属性简写
- 修改样式的时候，尽可能的批量修改。

### 9、什么是视差滚动？如何给每页做不同的效果？

- 视差滚动是指多层背景以不同的速度滚动，形成立体的运动效果
- 一般把网页解刨为背景层，内容层，悬浮层。当滚动鼠标滚轮时，各个图层以不同的速度移动，形成视差
- 实现原理
    - 以页面滚动条作为视差动画进度条
    - 以滚动刻度作为动画帧度去播放动画
    - 监听mousewheel，事件被触发即播放动画，实现翻页效果

### 10、伪元素和伪类的区别和作用

伪元素：在内容元素的前后插入额外的元素和样式，但是这些元素实际上并不在文档中生成

他们在外部显示可见，但不会在文档中的源代码中找到他们，所以叫做伪元素

伪类：将特殊的效果添加到特定选择器上，他是已有元素上添加类别的，不会产生新的元素

### 11、css hack原理和常用hack

原理：利用不同的浏览器对css的支持和解析结果不一致编写针对特定浏览器样式

常见的hack有：

- 属性hack
- 选择器hack
- IE条件注释

### 12、如何实现小于12px的字体效果

`transform:scale()`这个属性只可以缩放可以定义宽高的元素，而行内元素是没有宽高的，我们可以加上一个`display:inline-block`;

### 13、水平居中的方法

- 行内元素水平居中

    - 元素为inline、inline-block、inline-table、inline-flex元素的 ，设置父元素为text-align；center

    ```css
    .box{
        width: 100%;
        height: 500px;
        background-color: #ccc;
        /*css代码核心*/
        text-align: center;
    }
    <div class="box">水平居中</div>
    ```

- 块级元素的水平居中

    - 通过把固定宽度块级元素的margin-left和margin-right设成auto，就可以使块级元素水平居中。（行内块元素不适用）

    ```css
    .box{
        width: 500px;
        height: 500px;
        background-color: #ccc;
        /*css代码核心*/
        margin:0 auto;
    }
    <div class="box">水平居中</div>
    ```

    - 如果是行内块元素居中，可以给父级元素设置text-align：center

    ```css
    .box{
        display: inline-block;
        width: 500px;
        height: 500px;
        background-color: #ccc;
    }
    .box1{
        width: 1000px;
        height: 1000px;
        background-color: #000;
        /*css代码核心*/
    
        text-align: center;
    }
    
    <div class="box1"><div class="box">水平居中</div></div>
    ```

    - 如果元素宽度固定：可以设置左右margin为auto

    

- 使用table+margin  先将子元素设置为块级表格来显示  再将其设置为水平居中，display:table类似于block，但是宽度为内容宽。

    ```css
    .box{
        width: 500px;
        height: 500px;
        background-color: #ccc;
        /*css代码核心*/
        display: table;
        margin:0 auto;
    }
    .box1{
        width: 1000px;
        height: 1000px;
        background-color: #000;
    }
    <div class="box1"><div class="box">水平居中</div></div>
    ```

- 使用绝对定位+transform或者margin先将父元素设置为相对定位，再将子元素设置为绝对定位，向右移动子元素，移动距离为父容器的一半，最后通过向左移动子元素的一半宽度以达到水平居中

    - 在不知道自身大小的情况下

    ```css
    <div class="box">
      <div class="box1">Demo</div>
    </div>
    <style>
      .box1 {
        position:absolute;
        left:50%;
        transform:translateX(-50%);
      }
      .box {
        position:relative;
      }
    </style>
    ```

    - 在知道自身大小情况下

    ```css
    .box{
        width: 500px;
        height: 500px;
        background-color: #ccc;
        /*css代码核心*/
        position:absolute;
        left:50%;/*这里也可以用具体数值*/
        margin-left: -250px;
    }
    .box1{
        width: 1000px;
        height: 1000px;
        background-color: #000;
        /*css代码核心*/
        position:relative;
    }
    <div class="box1"><div class="box">水平居中</div></div>
    ```

- 使用flex+justify-content通过给父元素设置flex：justify-content

    ```css
    <div class="box1">
      <div class="box">Demo</div>
    </div>
    <style>
      .box1 {
        display: flex;
        justify-content:center;
      }
    </style>
    ```

- 使用flex+margin 通过flex将父容器设置为为Flex布局，再设置子元素居中

    ```css
    <div class="box1">
      <div class="box">Demo</div>
    </div>
    
      .box1 {
        display: flex;
      }
      .box {
        margin:0 auto;
      }
    
    ```

- 多块级元素水平居中

    - 利用flex布局 实现水平居中，其中justify-content 用于设置弹性盒子元素在主轴（默认横轴）方向上的对齐方式，本例中设置子元素水平居中显示

    ```css
     #container {
        display: flex;
        justify-content: center;
    }
    ```

    - 利用inline-block 将要水平排列的块状元素设为display:inline-block，然后在父级元素上设置text-align:center，达到与上面的行内元素的水平居中一样的效果。

    ```css
    .container {
    text-align: center;
    }
    .inline-block {
    display: inline-block;
    }
    ```

- `浮动元素设置水平居中`

    - 如果定宽的父元素宽度，通过子元素 设置relative+负的margin或者realative+transform

    ```css
    .box{
        width: 500px;
        height: 500px;
        background-color: #ccc;
        float: left;
        /*css代码核心*/
        position:relative;
        left:50%;
        /*margin-left:-250px;*/
        transform: translateX(-50%);
    
    }
    .box1{
        width: 1000px;
        height: 1000px;
        background-color: #000;
    }
    <div class="box1"><div class="box">水平居中</div></div>
    ```

    - 不定宽的浮动元素：父子元素都需要设置相对定位，并且清除浮动，给父元素加float

    ```css
    <div class="box">
        <p>我是浮动的</p>
        <p>我也是居中的</p>
    </div>
    .box{
        float:left;
        position:relative;
        left:50%;
    }
    p{
        float:left;
        position:relative;
        right:50%;
    }
    ```

    - 通用办法flex布局(不管是定宽还是不定宽) 利用弹性布局(flex)的justify-content属性，实现水平居中。

    ```css
    .parent {
        display:flex;
        justify-content:center;
    }
    .chlid{
        float: left;
        width: 200px;//有无宽度不影响居中
    }
    <div class="parent">
      <span class="chlid">我是要居中的浮动元素</span>
    </div>
    ```

- 绝对元素水平居中，通过子元素绝对定位，外加margin：0来实现而且left、right都为0

    ```css
    <div class="parent">
        <div class="child">让绝对定位的元素水平居中对齐。</div>
    </div>
      .parent{
            position:relative;
        }
       .child{
             position: absolute; /*绝对定位*/
             width: 200px;
             height:100px;
             background: yellow;
             margin: 0 auto; /*水平居中*/
             left: 0; /*此处不能省略，且为0*/
             right: 0;/*此处不能省略，且为0*/
        }
    
    ```

### 14、垂直居中的方法

- 单行内联元素垂直居中

    - 在父级远古三height有确定值下设置行高等于高

        ```css
        <div id="box">
             <span>单行内联元素垂直居中。</span>。
        </div>
        <style>
         #box {
            height: 120px;
            line-height: 120px;
            border: 2px dashed #f69c55;
            }
        </style>
        
        ```

- 多行内联元素垂直居中

    - 利用flex布局其中flex-direction: column定义主轴方向为纵向

        ```css
        <div class="parent">
            <p>利用flex布局（flex）其中flex-direction: column定义主轴方向为纵向。这种方式在较老的浏览器存在兼容性问题。</p>
        </div>
        <style>
            .parent { 
                height: 140px;
                display: flex;
                flex-direction: column;
                justify-content: center;
            }
        </style>
        
        ```

    - 利用表布局利用表布局vertical-align: middle可以实现子元素的垂直居中

        ```css
        <div class="parent">
            <p class="child">利用表布局（table）利用表布局的vertical-align: middle可以实现子元素的垂直居中</p>
        </div>
         <style>
            .parent {
                display: table;
                height: 140px;
                border: 2px dashed #f69c55;
            }
            .child {
                display: table-cell;
                vertical-align: middle;
            }
        </style>
        
        ```

- 块级元素垂直居中

    - 使用absolute+负margin(已知高度宽度)或者使用absolute+transform

    - 在已经知道高度情况下通过绝对定位元素距离顶部50%，并设置margin-top向上偏移元素高度的一半

        ```css
        <div class="parent">
            <div class="child">元素垂直居中</div>
        </div>
        .parent {
        position: relative;
        }
        .child {
        position: absolute;
        top: 50%;
        transform: translateY(-50%);
        }
        ```

    - 在不知道高度下当可以借助CSS3中的transform属性向Y轴反向偏移50%的方法实现垂直居中

        ```css
        <div class="parent">
            <div class="child">元素垂直居中</div>
        </div>
        .parent {
        position: relative;
        }
        .child {
        position: absolute;
        top: 50%;
        transform: translateY(-50%);
        }
        ```

    - 使用flex+align-items通过设置flex布局中的属性align-items，使子元素垂直居中。

        ```css
        <div class="parent">
            <div class="child">元素垂直居中</div>
        </div>
        .parent {
            display:flex;
            align-items:center;
        }
        ```

    - 使用table-cell+vertical-align通过将父元素转化为一个表格单元格显示（类似 和 ），再通过设置 vertical-align属性，使表格单元格内容垂直居中

        ```css
        <div class="parent">
          <div class="child">元素垂直居中</div>
        </div>
        <style>
          .parent {
            display: table-cell;
            vertical-align: middle;
          }
        </style>
        ```

    - 利用flex布局实现垂直居中，其中flex-direction: column定义主轴方向为纵向

        ```css
        .center-flex {
            display: flex;
            flex-direction: column;
            justify-content: center;
        }
        ```

    - 利用“精灵元素”(ghost element)技术实现垂直居中，即在父容器内放一个100%高度的伪元素，让文本和伪元素垂直对齐，从而达到垂直居中的目的

        ```css
        .ghost-center {
            position: relative;
        }
        .ghost-center::before {
            content: " ";
            display: inline-block;
            height: 100%;
            width: 1%;
            vertical-align: middle;
        }
        .ghost-center p {
            display: inline-block;
            vertical-align: middle;
            width: 20rem;
        }
        
        ```

### 15、水平垂直都居中的方法

- 在明确高度和宽度情况下

    - 绝对定位与负margin实现 兼容所有浏览器

        ```css
        .box1{
            width: 1000px;
            height: 1000px;
            background-color: #000;
            /*css核心代码*/
            position: relative;
        }
        .box{
            width: 100px;
            height: 100px;
            background-color: #ccc;
            /*css核心代码*/
            position: absolute;
            top: 50%;
            left: 50%;
            margin: -50px 0 0 -50px;
        }
        <div class="box1"><div class="box">水平居中</div></div>
        ```

    - 绝对定位与margin:auto 这种方式无需知道被垂直居中元素的高和宽，但不能兼容低版本的IE浏览器

        ```css
        .box1{
            width: 1000px;
            height: 1000px;
            background-color: #000;
            /*css核心代码*/
            position: relative;
        }
        .box{
            width: 100px;
            height: 100px;
            background-color: #ccc;
            /*css核心代码*/
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            margin: auto;
        }
        <div class="box1"><div class="box">水平居中</div></div>
        ```

- 在不知道高度宽度情况下（当然在高度和宽度已知也可用）

    - 绝对定位+transform CSS3的transform固然好用，但在项目的实际运用中必须考虑兼容问题，大量的hack代码可能会导致得不偿失。

        ```css
        #container {
              position: relative;
            }
        #center {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
        }
        ```

    - flex布局 利用flex布局，其中justify-content 用于设置或检索弹性盒子元素在主轴（横轴）方向上的对齐方式；而align-items属性定义flex子项在flex容器的当前行的侧轴（纵轴）方向上的对齐方式。不能兼容低版本的IE浏览器。

        ```css
        #container {//直接在父容器设置即可
            height: 100vh;//必须有高度
            display: flex;
            justify-content: center;
            align-items: center;
        }
        
        ```

    - flex/grid与margin:auto(最简单写法)容器元素设为 flex 布局或是grid布局，子元素只要写 margin: auto 即可,不能兼容低版本的IE浏览器。

        ```css
          #container {
              height: 100vh;//必须有高度
              display: grid;
            }
          #center {
              margin: auto;
            }
        
        ```

    - 当要被居中的元素是inline或者inline-block的时候，可以巧妙的将父级容器设置为display:table-cell，配合text-align:center和vertical-align:middle即可以实现水平垂直居中。

        ```css
        #container{//父级元素加
            display:table-cell;
            text-align:center;
            vertical-align:middle;
        }
        ```

    - 屏幕上水平垂直居中十分常用，常规的登录及注册页面都需要用到。要保证较好的兼容性，还需要用到表布局。

        ```html
            <style>
                .outer {
                    display: table;
                    position: absolute;
                    height: 100%;
                    width: 100%;
                }
                .middle {
                    display: table-cell;
                    vertical-align: middle;
                }
                .inner {
                    margin-left: auto;
                    margin-right: auto;
                    background: #2b2b2b;
                    color:#fff;
                    padding: 2rem;
                    max-width: 320px;
                }
            </style>
        
        <div class="outer">
            <div class="middle">
                <div class="inner">
                    <p>水平垂直居中</p>
                </div>
            </div>
        </div>
        
        ```

### 16、你知道的网页制作会用到的图片格式有哪些？

- png-8
- png-24
- jpeg
- gif
- svg
- webp
- Apng

### 17、一个页面上有大量的图片（大型电商网站），加载很慢，你有哪些方法优化这些图片的加载，给用户更好的体验。

- 图片懒加载，在页面上的未可视区域可以添加一个滚动事件，判断图片位置与浏览器顶端的距离与页面的距离，如果前者小于后者，优先加载。
- 如果为幻灯片、相册等，可以使用图片预加载技术，将当前展示图片的前一张和后一张优先下载。
- 如果图片为css图片，可以使用`CSSsprite`，`SVGsprite`，`Iconfont`、`Base64`等技术。
- 如果图片过大，可以使用特殊编码的图片，加载时会先加载一张压缩的特别厉害的缩略图，以提高用户体验。
- 如果图片展示区域小于图片的真实大小，则因在服务器端根据业务需要先行进行图片压缩，图片压缩后大小与展示一致。

### 18、移动端布局

- 百分百布局

    使用非固定像素来定义网页内容，也就是百分比布局，(特别适合于电商网站的布局)通过盒子的宽度设置成百分比来根据屏幕的宽度来进行伸缩，不受固定像素的限制，内容向两侧填充。

    **屏幕尺寸越大显示的内容越多;**

    ```js
    <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0" id="vp"/>
    <script type="text/javascript">
    // 把屏幕的倍率缩小到N分之一（N是window.devicePixelRatio）
    	var scale = 1/window.devicePixelRatio;
    	var mstr = 'initial-scale='+ scale +', maximum-scale='+ scale +', minimum-scale='+ scale +', user-scalable=no';
    	document.getElementById("vp").content = mstr;
    </script>
    ```

    `body, html { height : 100%; }`
    （1）整个盒子高度100%，设置成弹性盒，主轴垂直向下
    （2）`main`部分高度不确定，所以剩余高度都给`main`，`flex:1`;由于`main`内容很多，所以还要添加`overflow-x:hidden;`从而`overflow-y:auto;`（可以省略），这一部分可以垂直滚动。

- rem布局

    rem布局方案（用rem这个单位来做不同手机的适配）是现在做手机端页面最好的。

    **不同屏幕尺寸，显示的内容大致一样;**

    ```js
    
    <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0" id="vp"/>
    
    <script type="text/javascript">
        // 把尺寸放大N倍（N是window.devicePixelRatio）
        var wd = document.documentElement.clientWidth*window.devicePixelRatio/10;
        //物理像素*设备像素比=真实像素
        document.getElementsByTagName("html")[0].style.fontSize = wd + "px";
        // 把屏幕的倍率缩小到N分之一（N是window.devicePixelRatio）
        var scale = 1/window.devicePixelRatio;
        var mstr = 'initial-scale='+ scale +', maximum-scale='+ scale +', minimum-scale='+ scale +', user-scalable=no';
        document.getElementById("vp").content = mstr;
    </script>
    
    ```

    把单位直接写成`rem`。（可以通过插件转换，插件里面`px`转`rem`的值写成：你的开发平台的根部字体大小）
    （1）整个盒子高度`100%`，设置成弹性盒，主轴垂直向下
    （2）`main`部分高度不确定，所以剩余高度都给`main`，`flex:1;`由于`main`内容很多，所以还要添加`overflow-x:hidden;`从而`overflow-y:auto;`（可以省略），这一部分可以垂直滚动。

- 响应式布局

    使用媒体查询来使得一个网站兼容多个中端，好处是可以精确控制每个设备的显示内容，坏处就是需要写大量的css

    

###  19、 display:inline-block 什么时候会显示间隙

- 相邻的 `inline-block` 元素之间有换行或空格分隔的情况下会产生间距
- 非 `inline-block` 水平元素设置为 `inline-block` 也会有水平间距
- 可以借助 `vertical-align:top;` 消除垂直间隙
- 可以在父级加 `font-size：0;` 在子元素里设置需要的字体大小，消除垂直间隙
- 把 `li` 标签写到同一行可以消除垂直间隙，但代码可读性差

### 20、CSS优化、提高性能的方法有哪些

- 多个`css`合并，尽量减少`HTTP`请求
- 将`css`文件放在页面最上面
- 移除空的`css`规则
- 避免使用`CSS`表达式
- 选择器优化嵌套，尽量避免层级过深
- 充分利用`css`继承属性，减少代码量
- 抽象提取公共样式，减少代码量
- 属性值为`0`时，不加单位
- 属性值为小于`1`的小数时，省略小数点前面的0
- `css`雪碧图

### 21、经常遇到的浏览器的JS兼容性有哪些？解决方法是什么

- 当前样式：`getComputedStyle(el, null) VS el.currentStyle`
- 事件对象：`e VS window.event`
- 鼠标坐标：`e.pageX, e.pageY VS window.event.x, window.event.y`
- 按键码：`e.which VS event.keyCode`
- 文本节点：`el.textContent VS el.innerText`

### 22、外边距折叠(collapsing margins)

- 毗邻的两个或多个 `margin` 会合并成一个`margin`，叫做外边距折叠。规则如下：
    - 两个或多个毗邻的普通流中的块元素垂直方向上的`margin`会折叠
    - 浮动元素或`inline-block`元素或绝对定位元素的`margin`不会和垂直方向上的其他元素的margin折叠
    - 创建了块级格式化上下文的元素，不会和它的子元素发生margin折叠
    - 元素自身的`margin-bottom`和`margin-top`相邻时也会折叠

### 23、 li与li之间有看不见的空白间隔是什么原因引起的？有什么解决办法

行框的排列会受到中间空白（回车\空格）等的影响，因为空格也属于字符,这些空白也会被应用样式，占据空间，所以会有间隔，把字符大小设为0，就没有空格了

### 24、为什么要初始化CSS样式

因为浏览器的兼容问题，不同浏览器对有些标签的默认值是不同的，如果没对CSS初始化往往会出现浏览器之间的页面显示差异

### 25、请列举几种隐藏元素的方法	

- `visibility: hidden;` 这个属性只是简单的隐藏某个元素，但是元素占用的空间任然存在
- `opacity: 0;` `CSS3`属性，设置`0`可以使一个元素完全透明
- `position: absolute;` 设置一个很大的 `left` 负值定位，使元素定位在可见区域之外
- `display: none;` 元素会变得不可见，并且不会再占用文档的空间。
- `transform: scale(0);` 将一个元素设置为缩放无限小，元素将不可见，元素原来所在的位置将被保留
- `<div hidden="hidden">` HTML5属性,效果和`display:none;`相同，但这个属性用于记录一个元素的状态
- `height: 0;` 将元素高度设为 `0` ，并消除边框
- `filter: blur(0);` CSS3属性，将一个元素的模糊度设置为`0`，从而使这个元素“消失”在页面中

### 26、rgba() 和 opacity 的透明效果有什么不同

- `opacity` 作用于元素以及元素内的所有内容（包括文字）的透明度
- `rgba()` 只作用于元素自身的颜色或其背景色，子元素不会继承透明效果