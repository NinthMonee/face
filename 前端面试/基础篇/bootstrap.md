# bootstrap

### 1 什么是Bootstrap？以及为什么要使用Bootstrap？

> `Bootstrap` 是一个用于快速开发 `Web`应用程序和网站的前端框架。`Bootstrap`是基于 `HTML`、`CSS`、`JAVASCRIPT` 的

- `Bootstrap`具有移动设备优先、浏览器支持良好、容易上手、响应式设计等优点，所以`Bootstrap`被广泛应用

### 2 使用Bootstrap时，要声明的文档类型是什么？以及为什么要这样声明？

- 使用`Bootstrap`时，需要使用 `HTML5` 文档类型（`Doctype`）。`<!DOCTYPE html>`
- 因为`Bootstrap`使用了一些 `HTML5` 元素和 `CSS` 属性，如果在 `Bootstrap`创建的网页开头不使用 `HTML5` 的文档类型（`Doctype`），可能会面临一些浏览器显示不一致的问题，甚至可能面临一些特定情境下的不一致，以致于代码不能通过 `W3C` 标准的验证

### 3 什么是Bootstrap网格系统

> `Bootstrap` 包含了一个响应式的、移动设备优先的、不固定的网格系统，可以随着设备或视口大小的增加而适当地扩展到 `12` 列。它包含了用于简单的布局选项的预定义类，也包含了用于生成更多语义布局的功能强大的混合类

- 响应式网格系统随着屏幕或视口（`viewport`）尺寸的增加，系统会自动分为最多`12`列。

### 4 Bootstrap 网格系统（Grid System）的工作原理

- （1）行必须放置在 `.container class` 内，以便获得适当的对齐（`alignment`）和内边距（`padding`）。
- （2）使用行来创建列的水平组。
- （3）内容应该放置在列内，且唯有列可以是行的直接子元素。
- （4）预定义的网格类，比如 `.row` 和 `.col-xs-4`，可用于快速创建网格布局。`LESS` 混合类可用于更多语义布局。
- （5）列通过内边距（`padding`）来创建列内容之间的间隙。该内边距是通过 `.rows` 上的外边距（`margin`）取负，表示第一列和最后一列的行偏移。
- （6）网格系统是通过指定您想要横跨的十二个可用的列来创建的。例如，要创建三个相等的列，则使用三个 `.col-xs-4`

### 5 对于各类尺寸的设备，Bootstrap设置的class前缀分别是什么

- 超小设备手机（`<768px`）：`.col-xs-*`
- 小型设备平板电脑（`>=768px`）：`.col-sm-*`
- 中型设备台式电脑（`>=992px`）：`.col-md-*`
- 大型设备台式电脑（`>=1200px`）：`.col-lg-*`

### 6 Bootstrap 网格系统列与列之间的间隙宽度是多少

> 间隙宽度为`30px`（一个列的每边分别是`15px`）

### 7 如果需要在一个标题的旁边创建副标题，可以怎样操作

> 在元素两旁添加`<small>`，或者添加`.small`的`class`

### 8 用Bootstrap，如何设置文字的对齐方式？

- `class="text-center"` 设置居中文本
- `class="text-right"` 设置向右对齐文本
- `class="text-left"` 设置向左对齐文本

### 9 Bootstrap如何设置响应式表格？

> 增加`class="table-responsive"`

### 10 使用Bootstrap创建垂直表单的基本步骤？

- （1）向父`<form>`元素添加`role="form"`；
- （2）把标签和控件放在一个带有`class="form-group"`的`<div>`中，这是获取最佳间距所必需的；
- （3）向所有的文本元素`<input>`、`<textarea>`、`<select>`添加`class="form-control"`

### 11 使用Bootstrap创建水平表单的基本步骤？

- （1）向父`<form>`元素添加`class="form-horizontal"`；
- （2）把标签和控件放在一个带有`class="form-group"`的`<div>`中；
- （3）向标签添加`class="control-label"`。

### 12 使用Bootstrap如何创建表单控件的帮助文本？

> 增加`class="help-block"`的`span`标签或`p`标签。

### 13 使用Bootstrap激活或禁用按钮要如何操作？

- 激活按钮：给按钮增加`.active`的`class`
- 禁用按钮：给按钮增加`disabled="disabled"`的属性

### 14 Bootstrap有哪些关于![img]()的class？

- （1）`.img-rounded` 为图片添加圆角
- （2）`.img-circle` 将图片变为圆形
- （3）`.img-thumbnail` 缩略图功能
- （4）`.img-responsive` 图片响应式 (将很好地扩展到父元素)

### 15 Bootstrap中有关元素浮动及清除浮动的class？

- （1）`class="pull-left"` 元素浮动到左边
- （2）`class="pull-right"` 元素浮动到右边
- （3）`class="clearfix"` 清除浮动

### 16 除了屏幕阅读器外，其他设备上隐藏元素的class？

> `class="sr-only"``

### 17 Bootstrap如何制作下拉菜单？

- （1）将下拉菜单包裹在`class="dropdown"`的`<div>`中；
- （2）在触发下拉菜单的按钮中添加：`class="btn dropdown-toggle" id="dropdownMenu1" data-toggle="dropdown"`
- （3）在包裹下拉菜单的ul中添加：`class="dropdown-menu" role="menu" aria-labelledby="dropdownMenu1"`
- （4）在下拉菜单的列表项中添加：`role="presentation"`。其中，下拉菜单的标题要添加`class="dropdown-header"`，选项部分要添加`tabindex="-1"`。

### 18 Bootstrap如何制作按钮组？以及水平按钮组和垂直按钮组的优先级？

- （1）用`class="btn-group"`的`<div>`去包裹按钮组；`class="btn-group-vertical"`可设置垂直按钮组。
- （2）`btn-group`的优先级高于`btn-group-vertical`的优先级。

### 19 Bootstrap如何设置按钮的下拉菜单？

> 在一个 `.btn-group` 中放置按钮和下拉菜单即可。

### 20 Bootstrap中的输入框组如何制作？

- （1）把前缀或者后缀元素放在一个带有`class="input-group"`中的`<div>`中
- （2）在该`<div>`内，在`class="input-group-addon"`的`<span>`里面放置额外的内容；
- （3）把`<span>`放在`<input>`元素的前面或后面。

### 21 Bootstrap中的导航都有哪些？

- （1）导航元素：有`class="nav nav-tabs"`的标签页导航，还有`class="nav nav-pills"`的胶囊式标签页导航；
- （2）导航栏：`class="navbar navbar-default" role="navigation"`；
- （3）面包屑导航：`class="breadcrumb"`

### 22 Bootstrap中设置分页的class？

- 默认的分页：`class="pagination"`
- 默认的翻页：`class="pager"`

### 23 Bootstrap中显示标签的class？

> ```
> class="label"
> ```

### 24 Bootstrap中如何制作徽章？

> ```
> <span class="badge">26</span>
> ```

### 25 Bootstrap中超大屏幕的作用是什么？

> 设置`class="jumbotron"`可以制作超大屏幕，该组件可以增加标题的大小并增加更多的外边距