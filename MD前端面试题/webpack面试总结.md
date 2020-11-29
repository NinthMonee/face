### 1、前端代码为何要进行构建和打包？

代码层面：

- 是为了让代码体积更小(Tree-Shaking、压缩、合并)，加载更快
- 编译高级语言或者语法(Ts、ES6+、模块化、scss)
- 兼容性和错误检查(Polyfill,postcss,eslint)

项目流程方面：

- 统一、高效的开发环境
- 统一的构建流程和产出标准
- 集成公司构建规范

### 2、module chunk bundle分别是什么意思 有何区别

### 3、loader和plugin的区别

### 4、webpack如何实现懒加载

### 5、webpack常见性能优化

### 6、babel-runtime和babel-ployfill的区别

### 7、拆分配置和merge

### 8、如何配置多入口？

在entry里面以对象的形式配置。

```js
const srcPath = path.join(__dirname, '..', 'src')
const distPath = path.join(__dirname, '..', 'dist'
//入口配置
entry:{
    index:path.join(srcPath,'index.js'),
    other:path.join(srcPath,'other.js')
}
//多入口生成index.html
new HtmlWebpackPlugin({
    template:path.join(srcPath,'index.html').
    filename:'index.html',
    chunks:['index']//配置chunk，只会引用index
})
new HtmlWebpackPlugin({
    template:path.join(srcPath,'index.html').
    filename:'other.html',
    chunks:['other']
})
```

小技巧：

​	在output中导出的文件中，加一个contenthash，这样的话，如内容不变，文件hash值不变，去请求资源的时候，就会命中缓存，如果内容改变，hash改变，就会重新请求资源，存储到缓存中。

```js
output: {
        filename: 'bundle.[contentHash:8].js',  // 打包代码时，加上 hash 戳
        path: distPath,
        // publicPath: 'http://cdn.abc.com'  // 修改所有静态文件 url 的前缀（如 cdn 域名），这里暂时用不到
    },
```

### 9、抽离css文件

使用MiniCssExtractPlugin

```js
new MiniCssExtractPlugin({
    filename:'css/main.[contenthash:8].css'
})
```

### 10、如何异步加载js

引用动态数据，就是懒加载。使用import()

### 11 、如何自动去除打印语句？

使用terser-webpack-plugin

### 12、module chunk bundle的区别

module  各个源码 文件  webpack中一切皆模块

chunk 多模块合并而成的，如entry import()  splitChunk

bundel  最终的输出文件

### 13、如何处理jsx

引入@babel/preset-react

### 14、如何抽离公共代码

```js
 optimization: {
        // 压缩 css
        minimizer: [new TerserJSPlugin({}), new OptimizeCSSAssetsPlugin({})],

        // 分割代码块
        splitChunks: {
            chunks: 'all',
            /**
             * initial 入口 chunk，对于异步导入的文件不处理
                async 异步 chunk，只对异步导入的文件处理
                all 全部 chunk
             */

            // 缓存分组
            cacheGroups: {
                // 第三方模块
                vendor: {
                    name: 'vendor', // chunk 名称
                    priority: 1, // 权限更高，优先抽离，重要！！！
                    test: /node_modules/,
                    minSize: 0,  // 大小限制
                    minChunks: 1  // 最少复用过几次
                },

                // 公共的模块
                common: {
                    name: 'common', // chunk 名称
                    priority: 0, // 优先级
                    minSize: 0,  // 公共模块的大小限制
                    minChunks: 2  // 公共模块最少复用过几次
                }
            }
        }
    }
```

### 15、webpack优化

- 优化打包构建速度  ---开发体验和效率

    - 优化babel-loader

    - 使用IgnorePlugin

        ```js
        以moment时间格式包为例子
        //忽略moment下的/locale目录
        new webpack.IgnorePlugin(/\.\/locale/,/moment/)
        
        
        ```

    - noParse  不去管理哪些模块

        ```js
        moudle.exports={
            module:{
                //对完整的`react.min.js` 文件就没有采用模块化
                //忽略对`react.min.js`文件的递归解析处理
                noParse:[/react/.min/.js$/]
            }
        }
        ```

        

    - happyPack  多进程打包工具 开启多进程 加快打包速度

        ```js
        new HappyPack({
            //用唯一标识符id来代表当前的HappyPack 是用来处理一类特定的文件
            id:'babel',
            //如何处理.js文件  用法和loader配置中一样
            loaders:['babel-loader?cancheDirectory']
        })
        ```

    - parallellUglifyPlugin 开启多进程  启动代码压缩

        wepack内置Uglify工具压缩js

        js单线程  开启多线程压缩更快

        和happyPack同理

        ```js
        
                // 使用 ParallelUglifyPlugin 并行压缩输出的 JS 代码
                new ParallelUglifyPlugin({
                    // 传递给 UglifyJS 的参数
                    // （还是使用 UglifyJS 压缩，只不过帮助开启了多进程）
                    uglifyJS: {
                        output: {
                            beautify: false, // 最紧凑的输出
                            comments: false, // 删除所有的注释
                        },
                        compress: {
                            // 删除所有的 `console` 语句，可以兼容ie浏览器
                            drop_console: true,
                            // 内嵌定义了但是只用到一次的变量
                            collapse_vars: true,
                            // 提取出出现多次但是没有定义成变量去引用的静态值
                            reduce_vars: true,
                        }
                    }
                })
        ```

        `注意：`

        - 项目较大，打包较慢，开启多进程能提高我们的速度
        - 项目较小，打包很快，开启多进程会降低速度(进程开销)

    - 自动刷新

        - 热更新 HotModuleReplacementPlugin

    - DllPlugin  针对一些比较大的库和插件，没有必要每一次都打包一次，事先打包好之后作为一个DLL来引用

        `但是注意这种方式已经不被使用了，vue和react的cli工具都已经放弃了使用该方法，是因为webpack4的打包速度，没必要花费大量的精力去维护这个配置的极其繁琐的插件`

        `但是有一个能替换该plugin的插件，那就是`hard-source-webpack-plugin,他的加速效果非常快。

- 优化产出代码---产品性能

    从三个方面考虑：

    - 体积更小
    - 合理分包，不重复加载
    - 速度更快，内存使用更小

    优化方法：

    - ​	小图片base64、大图片单独打包，可以少做一些网络请求

        ```js
        use:{
            loader:'url-loader',
            options:{
                limit:5*1024,
           }
        }
        ```

    - bundle 加上hash ,文件内容不变的情况，可以命中缓存

        ```js
        output:{
            filename:'[name].[contenthash:8].js'
        }
        ```

    - 懒加载

    - 提取公共代码

    - IngorePlugin

    - CDN加速

        js、css、图片都可以

        ```js
        outpath:{
            ...
            
            publicPath:'http://cdn.abc.com'//修改所有静态文件url的前缀
        }
        ```

    - 使用production

    - 开启tree-shaking

        - 但是只有ES6 Module 才能生效，Commonjs不生效

            这是因为ES6 module静态引入，编译时引入

            commonjs动态引入，执行时引入

            只有ES6 module才能静态分析，实现Tree-Shaking

    - 使用Scope Hosting

        把多个函数的内容放到一个函数中来

        创建函数作用域更少

        代码可读性更好 

        ```js
         引入插件 ModuleConcatenationPlugin
         
         module.exports={
             //针对npm中的第三方模块优先采用jsnext:main中指向的ES6模块化语法的文件
             mainFields:['jssnext:main','brower']
         }
        ```

        





