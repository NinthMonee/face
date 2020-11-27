# 01-Webpack 基础

Webpack 基础配置。

```js
const path = require('path')

const HtmlWebpackPlugin = require('html-webpack-plugin')
const MiniCssExtractPlugin = require('mini-css-extract-plugin')

const VueLoaderPlugin = require('vue-loader/lib/plugin')

module.exports = {
  mode: 'development',

  entry: {
    'scripts/app': './src/scripts/app.js',
    // 'scripts/app': ["@babel/polyfill", "./src/scripts/app.js"],
    'scripts/index': './src/scripts/index.js'
  },

  output: {
    path: path.resolve(__dirname, '../dist'),
    publicPath: 'http://localhost:9000/',
    filename: '[name].js'
  },

  devtool: 'inline-source-map',

  resolve: {
    alias: {
      '@': path.resolve(__dirname, '../component')
    },

    extensions: [".js", ".json", ".jsx"]
  },

  module: {
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader',
        options: {
          extractCSS: true
        }
      },
      {
        test: /\.m?jsx?$/,
        exclude: /(node_modules|bower_components)/,
        use: {
          loader: 'babel-loader',
          options: {
            "presets": [
              [
                "@babel/preset-env",
                // babel 7.3
                // {
                //   "targets": {
                //     "chrome": "58",
                //     "ie": "11"
                //   },
                //   "useBuiltIns": "usage"
                // }

                // babel 7.4
                {
                  useBuiltIns: "entry", // or "usage"
                  corejs: 3,
                }
              ],
              "@babel/preset-react"
            ],
            "plugins": [
              ["@babel/plugin-proposal-decorators", { "legacy": true }],
              ["@babel/plugin-proposal-class-properties", { "loose": true }]
            ]
          }
        }
      },
      {
        test: /\.s?css$/,
        use: [
          {
            loader: MiniCssExtractPlugin.loader,
            options: {
              hmr: process.env.NODE_ENV === 'development',
            },
          }, 

          {
            loader: "css-loader"
          }, 
          {
            loader: "postcss-loader",
            options: { 
              ident: "postcss", 
              plugins: () => [require("postcss-preset-env")()]
            }
          },
          {
            loader: "sass-loader"
          }
        ]
      },
      {
        test: /\.(jpg|jpeg|png|gif)$/,
        use: [
          {
            loader: 'url-loader',
            options: {
              limit: 800,
              name: '[name]-[hash:6].[ext]',
              outputPath: 'images/'
            }
          }
        ]
      }
    ]
  },

  plugins: [
    new HtmlWebpackPlugin({
      title: 'webpack demo',
      filename: 'index.html',
      template: './src/index.ejs',
    }),

    new MiniCssExtractPlugin({
      moduleFilename: ({ name }) => `${name.replace('scripts', 'styles')}-[hash:6].css`,
      ignoreOrder: false,
    }),

    new VueLoaderPlugin()
  ],

  devServer: {
    contentBase: './dist',
    compress: true,
    port: 9000
  }
}
```

### 一、Babel7.3- 中 @babel/preset-env 的使用

##### 1、 基本用法介绍

是一系列插件的集合，包含了我们在babel6中常用的es2015,es2016, es2017等最新的语法转化插件，允许我们使用最新的js语法，比如 let，const，箭头函数等等，但不包括stage-x阶段的插件。

##### 2、 @babel/polyfill

也就是 babel6 中的 babel-polyfill , 使用 preset-env 能将最新的语法转换为ecmascript5的写法，当我们需要使用新增的全局函数(比如promise, Array.from)和实例方法(比如 Array.prototype.includes )时就需要引入 polyfill ，一般用法 在 index.js文件的最上层引入，或是在打包文件的entry 入口处引入，这样做的缺点是会全局引入整个 polyfill包，比如promise 会全局引入，污染全局环境。

##### 3、 @babel/polyfill 和 @babel/preset-env 的关系

@babel/preset-env 中与 @babel/polyfill 的相关参数有 targets 和 useBuiltIns 两个

**targets: 支持的目标浏览器的列表**

```json
"targets": {
  "chrome": "58",
  "ie": "11"
}
```

参考：https://www.cnblogs.com/bai1218/p/12392180.html

**useBuiltIns: 参数有 “entry”、”usage”、false 三个值**

默认值是false，此参数决定了babel打包时如何处理@babel/polyfilll 语句。

“entry”: 会将文件中 `import @babel/polyfilll` 语句 结合 targets ，转换为一系列引入语句，去掉目标浏览器已支持的 polyfilll 模块，不管代码里有没有用到，只要目标浏览器不支持都会引入对应的 polyfilll 模块。

“usage”: 不需要手动在代码里写 `import @babel/polyfilll`，打包时会自动根据实际代码的使用情况，结合 targets 引入代码里实际用到部分 polyfilll 模块

false: 对 import‘@babel/polyfilll’不作任何处理，也不会自动引入 polyfilll 模块。

需要注意的是在 webpack 打包文件配置的 entry 中引入的 @babel/polyfill 不会根据 useBuiltIns 配置任何转换处理。

##### 总结：

在业务项目中需要用到polyfill时, 可以使用和 @babel/preset-env 的 targets 和 useBuiltIns: usage 来根据目标浏览器的支持情况，按需引入用到的 polyfill 文件。

不建议使用 `import @babel/polyfilll` 一次性引入所有的 polyfill 文件。

### 二、 babel7.4 中 @babel/preset-env 的使用

##### core-js介绍

其实core-js是我们能够使用新的API的最重要的包，然而一般情况它隐藏在webpack编译后的代码中，我们一般不会去查看，所以容易被遗忘，我们在webpack生成环境下，查看编译后的代码，可以看到例如includes就是从core-js导出到我们的代码去的。

![img](https://s2.ax1x.com/2019/10/07/uR9QE9.png)

##### core-js是什么

- 它是JavaScript标准库的polyfill
- 它尽可能的进行模块化，让你能选择你需要的功能
- 它可以不污染全局空间
- 它和babel高度集成，可以对core-js的引入进行最大程度的优化

##### 升级core-js@3动机

目前我们使用的都默认是core-js@2，它在 2018年 之前就封锁了分支，至此之后的特性都只会添加到core-js@3，这里有一个生产例子，使用了core-js@2不支持的新特性，导致错误

- core-js@2出现的问题
- Vue-cli使用flat报错
- vue-cli也会在V4升级core-js
- Roadmap for Vue-cli4
- core-js@3添加的特性

##### core-js@3 特性概览

- 支持ECMAScript稳定功能，引入core-js@3冻结期间的新功能，比如flat
- 加入到ES2016-ES2019中的提案，现在已经被标记为稳定功能
- 更新了提案的实现，增加了proposals配置项，由于提案阶段不稳定，需要谨慎使用
- 增加了对一些web标准的支持，比如URL 和 URLSearchParams
- 现在支持原型方法，同时不污染原型
- 删除了过时的特性

##### core-js@3 与 babel

以前我们实现API的时候，会引入整个polyfill,其实polyfill只是包括了以下两个包

```
core-js
regenerator-runtime
```

core-js@3 升级之后弃用了@babel/polyfill，以下是等价实现

```js
// babel.config.js
presets: [
  ["@babel/preset-env", {
    useBuiltIns: "entry", // or "usage"，见如下说明
    corejs: 3,
  }]
]

// 如果 useBuiltIns: "entry" 需要在打包入口文件导入两个包：
import "core-js/stable"
import "regenerator-runtime/runtime"

// 如果 useBuiltIns: "usage"，不需要在入口文件里导入包了
```

##### 总结

core-js@3 废弃了 babel-polyfill，实现了完全无污染的API转译，非常有潜力，但是其暂时会增加打包体积，这个还得看未来普及度上来之后的权衡



# 02-Tree Shaking

https://v.youku.com/v_show/id_XMTM0NjA1OTM3Ng==.html

### 1、准备一个PickApples.js文件

```js
export class Machine {
  name = 'CNMO'

  start() {
    console.log('press the start button please.')
  }
}

export class Appletree {
  type = 'guoguang'

  shake() {
    console.log('I am shaking...')
  }
}
```

### 2、在app.js里引用部分类

```js
// tree shaking 只支持 ES Module
import { Machine } from './PickApples'
Machine.start()
```

### 3、在开发环境(mode: 'development')下, 配置：

```js
optimization: {
  usedExports: true
}
```

webpack4.x 默认开启treeshaking, 当开启(mode: 'production')生产环境模式，可以注释掉这个配置。此配置在打包好的文件里可以看到以下信息：

```
/*! exports used: Machine */
```

### 4、在package.json里加入以下配置，告知模块打包时本模块可以被shake。

sideEffects 表示有副作用的函数（模块）。如果设置为 false，表示本模块没有副作用，可以shake掉了。

```json
"sideEffects": false,
```

如果在package.json里没有设置sideEffects，那么该模块不能被shake。
我们也可以定义模块某些导入的模块不能shake，如css文件。则做如下配置：

```json
"sideEffects": [
  "*.css"
],
```

### 有关 sideEffects

https://github.com/happylindz/blog/issues/15



# 03-生产环境和开发环境

区分 `webpack` 生产环境和开发环境，通过 `webpack-merge` 模块来合并配置：

webpack merge提供了一个合并函数，用于连接数组和合并对象，从而创建一个新对象。如果遇到函数，它将执行它们，在算法中运行结果，然后再次将返回的值包装在函数中。这种行为在配置webpack时特别有用。

##### 局限性

请注意，不支持 Promise ！如果要返回一个封装在 Promise 中的配置，请在其中合并。例子：

```js
Promise.resolve(merge({ ... }, { ... }))
```

### merge

`merge` 是API的核心，也是最重要的思想。

```js
const { merge } = require('webpack-merge');
 
// Default API
const output = merge(object1, object2, object3, ...);
 
// You can pass an array of objects directly.
// This works with all available functions.
const output = merge([object1, object2, object3]);
 
// Keys matching to the right take precedence:
const output = merge(
  { fruit: "apple", color: "red" },
  { fruit: "strawberries" }
);
console.log(output);
// { color: "red", fruit: "strawberries"}
```

### webpack.config.js

```js
const commonConfig = { ... };
 
const productionConfig = { ... };
 
const developmentConfig = { ... };
 
module.exports = env => {
  switch(env) {
    case 'development':
      return merge(commonConfig, developmentConfig);
    case 'production':
      return merge(commonConfig, productionConfig);
    default:
      throw new Error('No matching configuration was found!');
  }
}
```



# 04-代码分割

## 方案一：全局挂载

假设有模块 `module`, 将 `module` 挂载到 `window` 上，各模块均可使用。

**第一步**

```js
window.module = module
```

**第二步**

将 `module` 放源代码中，然后在配置文件的 `entry` 处加一个 `chunk`

```js
entry: {
  module: path.resove(__dirname + './src/module.js')
}
```



## 方案二：externals

externals 配置选项提供了「从输出的 bundle 中排除依赖」的方法。相反，所创建的 bundle 依赖于那些存在于用户环境(consumer's environment)中的依赖。此功能通常对 library 开发人员来说是最有用的，然而也会有各种各样的应用程序用到它。

防止将某些 import 的包(package)打包到 bundle 中，而是在运行时(runtime)再去从外部获取这些扩展依赖(external dependencies)。

例如，从 CDN 引入 jQuery，而不是把它打包：

**index.html**

```html
<script
  src="https://code.jquery.com/jquery-3.1.0.js"
  integrity="sha256-slogkvB1K3VOkzAI8QITxV3VzpOnkeNVsKvtkYLMjfk="
  crossorigin="anonymous">
</script>
```

**webpack.config.js**

```js
externals: {
  jquery: 'jQuery'
}
```

这样就剥离了那些不需要改动的依赖模块，换句话，下面展示的代码还可以正常运行：

```js
import $ from 'jquery'
$('.my-element').animate(...)
```



## 方案三：SplitChunksPlugin

### 一、SplitChunksPlugin的概念

在工程化地使用webpack时，公共代码抽离是不可或缺的，4.x版本之后，commonsChunkPlugin已经被去掉，splitChunksPlugins登上舞台，并且优化了很多配置选项。

在使用splitChunksPlugins之前，首先要知道splitChunksPlugins是webpack主模块中的一个细分模块，无需npm引入。功能上，splitChunksPlugins只能用于如何抽离公用的代码，也就是抽离公用代码的规则，要记住，除了这个功能之外，splitChunksPlugins再无其他功能。

### 二、SplitChunksPlugin的默认配置

以下是SplitChunksPlugin的默认配置：

```js
module.exports = {
  //...
  optimization: {
    splitChunks: {
      chunks: 'async',
      minSize: 20000,
      maxSize: 0,
      minChunks: 1,
      maxAsyncRequests: 30,
      maxInitialRequests: 30,
      automaticNameDelimiter: '~',
      enforceSizeThreshold: 50000,
      cacheGroups: {
        defaultVendors: {
          test: /[\\/]node_modules[\\/]/,
          priority: -10,
          reuseExistingChunk: true
        },
        default: {
          minChunks: 2,
          priority: -20,
          reuseExistingChunk: true
        }
      }
    }
  }
};
```

#### 1、小试牛刀

以下配置可以自动实现代码分割：

```js
module.exports = {
  //...
  optimization: {
    splitChunks: {
      chunks: 'all',
    }
  }
  //...
}
```

#### 2、异步代码分割

**异步代码：**

```js
function getComponent() {
  return import(/* webpackChunkName: "lodash" */ 'lodash').then(_ => {
    var element = document.createElement('div')
    element.innerHTML = _.join(['Hello', 'webpack'], ' ')
    return element
  }).catch(error => 'An error occurred while loading the component')
}

getComponent().then(component => {
  document.body.appendChild(component);
})
```

此处 `/* webpackChunkName: "lodash" */` 为魔法注释，这个要生效除了上述插件外，还需要配置，这样导出的文件名即为 `lodash.js`

```js
module.exports = {
  //...
  optimization: {
    splitChunks: {
      chunks: 'all',
    },
    cacheGroups: {
      defaultVendors: false,
      default: false
    }
  }
  //...
}
```

#### 3、配置项

##### 3.1 默认配置

以下默认配置等同上述的 `SplitChunksPlugin的默认配置`

```js
module.exports = {
  //...
  optimization: {
    splitChunks: {}
  }
  //...
}
```

##### 3.2 chunks

这表示将选择哪些块进行优化。提供字符串时，有效值为all、async和c。提供all功能特别强大，因为这意味着即使在异步和非异步块之间也可以共享块。

**同步代码：**

```js
import _ from 'lodash';
function component() {
  var element = document.createElement('div');

  // Lodash, now imported by this script
  element.innerHTML = _.join(['Hello', 'webpack'], ' ');
  
  return element
}

document.body.appendChild(component());
```

**异步代码：**

```js
function getComponent() {
  return import(/* webpackChunkName: "lodash" */ 'lodash').then(_ => {
    var element = document.createElement('div')
    element.innerHTML = _.join(['Hello', 'webpack'], ' ')
    return element
  }).catch(error => 'An error occurred while loading the component')
}

getComponent().then(component => {
  document.body.appendChild(component);
})
```

**配置**

```js
module.exports = {
  //...
  optimization: {
    splitChunks: {
      chunks: 'initial',
    },
    cacheGroups: {
      defaultVendors: {
        test: /[\\/]node_modules[\\/]/,
        priority: -10,
        filename: 'venders.js'
      },
      default: false
    }
  }
  //...
}
```

##### 3.3 minSize 配置

需要打包的文件生成块的最小大小（字节）。

##### 3.4 maxSize 配置

对需要打包文件进行二次分割

##### 3.5 minChunks 配置

当一个模块被用了多少次的时候再进行代码分割

##### 3.6 maxAsyncRequests 配置

当整个项目打包完之后，一个按需加载的包最终被拆分成 n 个包，maxAsyncRequests 就是用来限制 n 的最大值

##### 3.7 maxInitialRequests 配置

入口点的最大并行请求数。

##### 3.8 automaticNameDelimiter 配置

默认情况下，webpack将使用块的来源和名称生成名称（例如vendors~main.js). 此选项允许您指定要用于生成的名称的分隔符。

##### 3.9 reuseExistingChunk 配置

复用之前打包好的模块

##### 3.10 minRemainingSize 配置

splitChunks.minRemainingSize在webpack5中引入了选项，通过确保拆分后剩余的块的最小大小超过限制，从而避免零大小的模块。在“开发”模式下默认为0。其他情况下splitChunks.minRemainingSize默认值为splitChunks.minSize。因此，除了极少数需要深度控制的情况外，不需要手动指定它。

##### 3.11 enforceSizeThreshold 配置

执行拆分的大小阈值，忽略其他限制（minRemainingSize、maxAsyncRequests、maxInitialRequests）。



# 05-Lazy Loading 懒加载

异步加载的模块，会先被打包，然后根据某些条件异步加载，比如单击页面。

```js
function getComponent() {
  return import(/* webpackChunkName: "lodash" */ 'lodash').then(_ => {
    var element = document.createElement('div')
    element.innerHTML = _.join(['Hello', 'webpack'], ' ')
    return element
  }).catch(error => 'An error occurred while loading the component')
}

document.addEventListener('click', () => {
  getComponent().then(component => {
    document.body.appendChild(component);
  })
})
```

或者使用异步函数的写法：

```js
async function getComponent() {
  const { default: _ } = await import(/* webpackChunkName: "lodash" */ 'lodash')
  var element = document.createElement('div')
  element.innerHTML = _.join(['Hello', 'webpack'], ' ')
  return element
}

document.addEventListener('click', () => {
  getComponent().then(component => {
    document.body.appendChild(component);
  })
})
```

在浏览器的 Network 点击页面，异步加载的模块才被加载出来。



# 06-代码覆盖率（代码利用率）

### 打包分析

**生成用于分析的json文件**

进入 package.json：

```
webpack --profile --json > stats.json --config ./config/webpack.prod.js
```

**分析网站：**

http://www.github.com/webpack/analyse

**第三方分析工具**

https://webpack.js.org/guides/code-splitting/#bundle-analysis

### Prefetching

提高首屏的打开速度！！！

思考，以下代码还有没有优化空间？

```js
document.addEventListener('click', () => {
  var element = document.createElement('div')
  element.innerHTML = _.join(['Hello', 'webpack'], ' ')
  document.body.appendChild(element)
})  
```

在浏览器里输入 cmd+shift+p, 输入 Show Coverage, 查看文件的代码利用率（或覆盖率）。
改造方法：拆分成文件异步加载

**AsyncPack.js**

```js
import _ from 'lodash'
export default () => {
  var element = document.createElement('div')
  element.innerHTML = _.join(['Hello', 'webpack'], ' ')
  document.body.appendChild(element)
}
```

**index.js**
注意魔法注释

```js
document.addEventListener('click', () => {
  import(/* webpackPrefetch: true */ './component/AsyncPack.js').then( ({default: fun}) => {
    fun()
  })
})
```

### Preloading

Prefetching 在页面主要JS加载完毕，再加载异步JS，而 Preloading 是和主要JS同时加载。



# 07-wepack 优化

### 1、版本升级

与Webpack相关的工具版本要升级，如 Node.js, npm, yarn 等。

### 2、在loader上尽可能少的应用模块

比如在loader上使用 include, exclude 等，让loader的作用范围尽量小。

### 3、plugin 的使用

plugin 尽可能精简，并推荐使用官方的 plugin, 或官方推荐的 plugin。

### 4、合理的使用 resolve

```
  resolve: {
    extentions: ['.jsx', '.js'],
    mainFiles: ['index', 'main'],
    alias: {
      @: path.resolve(__dirname, './src/components/')
    }
  }
```

所有的配置都涉及到文件的查找，损耗性能，需要合理使用。

### 5、使用 DLLPlugin 提高打包速度

https://www.cnblogs.com/skychx/p/webpack-dllplugin.html

### 6、happypack 的使用

https://www.jianshu.com/p/56463890f7cd

# 08-loader-plugin

https://segmentfault.com/a/1190000021444966

