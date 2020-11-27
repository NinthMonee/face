# webpack

### 1 优化打包速度

- 减少文件搜索范围
    - 比如通过别名
    - `loader` 的 `test`，`include & exclude`
- `Webpack4` 默认压缩并行
- `Happypack` 并发调用
- `babel` 也可以缓存编译

### 2 Babel 原理

- 本质就是编译器，当代码转为字符串生成 `AST`，对 `AST` 进行转变最后再生成新的代码
- 分为三步：词法分析生成 `Token`，语法分析生成 `AST`，遍历 `AST`，根据插件变换相应的节点，最后把 `AST`转换为代码

### 3 如何实现一个插件

- 调用插件 `apply` 函数传入 `compiler` 对象
- 通过 `compiler` 对象监听事件

比如你想实现一个编译结束退出命令的插件

```js
apply (compiler) {
  const afterEmit = (compilation, cb) => {
    cb()
    setTimeout(function () {
      process.exit(0)
    }, 1000)
  }

  compiler.plugin('after-emit', afterEmit)
}
}

module.exports = BuildEndPlugin
```