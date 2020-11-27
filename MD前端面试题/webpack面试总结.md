### 1、前端代码为何要进行构建和打包？

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

