# node

### 00_关于模块化

1. `什么是模块`

   => 一个 js 文件就是一个模块

   => 我们把一类方法放在一个 js 文件里面, 这个 js 文件就变成了一个模块

   => 再需要哪一类方法的时候, 引入这个 js 文件就好了

2. `什么是模块化`

   => 再开发的过程中, 尽可能把开发方式趋近于模块的方式

   => 把我们所有的内容都按照类别分好文件

   => 按需引入

3. `模块化的分类`

   `3-1. 自定义模块`

​    -> 我们自己按照模块化的语法, 开发的 js 文件

​    -> 自己定义模块, 自己再需要的时候导入模块使用

​    -> 自己决定模块内哪些内容向外暴露

  `3-2. 内置模块`

​    -> node 这个环境天生自带的一些模块("一个一个的 js 文件")

​    -> 你需要使用的时候, 直接引入使用就好了

  ` 3-3. 第三方模块`

### 00_自定义模块：

​     在每一个js文件里面

​     天生自带一个变量，叫做module，表示模块

```js
// console.log(module)
/*
  Module {
    id: '.',
    path: 'E:\\BK_GP_19\\01_第一周\\01_DAY\\01_代码\\02_自定义模块',
    // 我这个模块向外导出了哪些内容, 我这个文件里面哪些方法别的文件可以使用
    exports: {},
    // 我被哪些模块导入了, 谁再使用我的内容
    parent: null,
    filename: 'E:\\BK_GP_19\\01_第一周\\01_DAY\\01_代码\\02_自定义模块\\01_定义模块.js',
    loaded: false,
    // 我引用着哪些模块
    children: [],
    // 查找第三方模块的时候的顺序
    paths: [
      'E:\\BK_GP_19\\01_第一周\\01_DAY\\01_代码\\02_自定义模块\\node_modules',
      'E:\\BK_GP_19\\01_第一周\\01_DAY\\01_代码\\node_modules',
      'E:\\BK_GP_19\\01_第一周\\01_DAY\\node_modules',
      'E:\\BK_GP_19\\01_第一周\\node_modules',
      'E:\\BK_GP_19\\node_modules',
      'E:\\node_modules'
    ]
  }
*/
```

   用来描述这个文件模块的 我们这个文件定义好以后，你想让别人使用的东西，就必须导出该模块，我们有三种带出模块的方式。 

 在设计的时候，除了标准导出模块的方式外，给我们提供了两种其他方式

​        1、给module.exports重新赋值

​              赋值为一个新的对象或者数组或者任何其他数据类型

​        2、exports 也是每一个js文件天生自带的一个变量

​              每一个js文件里面相当于有一句代码 var exports=module.exports

​              注意: 如果你需要给 module.exports 重新赋值, 那么不能使用 exports

`1) 标准导出`

```js
// 我自己的变量
const num = 100
const str = 'hello world'

// 我自己的一个方法
function fn1() {
  console.log('我是 fn1 方法')
}

// 导出一个内容
module.exports = {
  fn1: fn1,
}


module.exports.fn1=fn1
module.exports.num=100
```

`2) 给module.exports重新赋值`

```js
module.exports={
    fn1,
    num:100
}
```

`3) 利用exports向外暴露模块`

```js
exports.fn1=fn1
exports.num=200
exports.str="hello world"
//以下是错误的示例
//不能这么写
exports={
    fn1,
    num:100,
    str:"hello world"
}
```

`问题：为什么exports重新赋值的时候不好使？`

```js
// 使用 obj 模拟 module
 var obj = {   
     exports: {}
 }

// 使用 exp 模拟 exports
var exp = obj.exports

// 需求: 想 exports 里面添加一个 num 一个 str
// 1.
obj.exports.num = 100
obj.exports.str = 'hello'

// 2.
obj.exports = {
    num: 100,
    str: 'hello'
 }

// 3.
// 本身 exp 代表的是 obj.exports
// 现在 exp 代表一个新的对象, 不再代表 obj.exports
// 你现在只是向一个对象里面添加内容
// 但是向外导出的还是 obj.exports
exp = {}
console.log(obj)
```



### 01_内置模块

####       01_1、fs模块



   *1. readFile()*

  *=> 异步读取文件*

  *=> 语法: fs.readFile(路径, 读取格式, 回调函数)*

   *-> 路径: 你要读取的文件的路径, 如果路径不存在, 会出现错误*

   *-> 读取格式: 选填, 默认是 buffer, 我们可以选填 'utf8'*

   *-> 回调函数: 读取成功以后会执行的函数*

```js
fs.readFile('./test.txt', 'utf8', function (err, data) {
// err 就是读取失败的时候的错误信息
// data 就是读取成功的时候的读取内容
if (err) return console.log(err)
 console.log('读取成功')
 console.log(data)
 //<Buffer e4 bd a0 e5 a5 bd e5 95 8a>
 //它是以16进制的形式来表示2进制的8位，能在同样的内存空间下存储更多的数据
})
```

 *2. readFileSync()*

  *=> 同步读取文件*

  *=> 语法: fs.readFileSync(路径, 读取格式)*

  *=> 返回值: 读取的文件内容*

  *=> 注意: 读取路径不存在, 或者没有权限的时候, 会报错, 打断程序的继续执行*

```js
const res = fs.readFileSync('./test1.txt', 'utf8')
console.log('读取完成')
console.log(res)
```

 *3. writeFile()*

  *=> 异步写入文件*

  *=> 语法: fs.writeFile(路径, 要写入的内容, 回调函数)*

   *-> 路径: 你要写入文件的路径, 如果路径文件不存在, 会创建一个这个文件再写入*

   *-> 写入的内容: 你自己定义*

   *-> 回调函数: 写入成功以后执行的函数, 必填*

   *->没有返回值，准确的说返回值是undefined*

  *=> 注意: 完全覆盖式的写入*

```js
fs.writeFile('./test.txt', '你好 世界', function () {
 console.log('写入完成')
})
```

 *4. writeFileSync()*

  *=> 同步写入文件*

  *=> 语法: fs.writeFileSync(路径, 要写入的内容)*

  *=> 注意: 完全覆盖式的写入*

```js
fs.writeFileSync('./test.js', 'hello node')
console.log('写入完成')
```

 *5. appendFile()*

  *=> 异步追加内容*

  *=> 语法: fs.appendFile(路径, 追加的内容, 回调函数)*

   *-> 路径: 写入文件的路径, 没有该路径, 自己创建*

   *-> 追加的内容*

   *-> 回调函数: 必填*

```js
 fs.appendFile('./test1.txt', 'hello world', () => {})
```

 *6. appendFileSync()*

  *=> 同步追加内容*

  *=> 语法: fs.appendFileSync(路径, 追加的内容)*

```js
fs.appendFileSync('./test1.txt', '你好 世界')
```

 *7. readdir()*

  *=> 异步读取文件夹*

  *=> 语法: fs.readdir(路径, 回调函数)*

```js
fs.readdir('../02_自定义模块', (err, data) => {
if (err) return console.log(err)
 console.log('读取文件夹成功')
 console.log(data)
})
```

 *8. readdirSync()*

  *=> 同步读取文件夹*

  *=> 语法: fs.readdirSync(路径)*

```js
const res = fs.readdirSync('../02_自定义模块')
console.log(res)
```

 *9. mkdir()*

  *=> 异步创建文件夹*

  *=> 语法: fs.mkdir(路径, 回调函数)*

```js
fs.mkdir('./a', (err) => {
if (err) return console.log(err)

console.log('创建文件夹成功')
})
```

 *10. mkdirSync()*

  *=> 同步创建文件夹*

  *=> 语法: fs.mkdirSync(路径)*

```js
 fs.mkdirSync('./b')
```

 *11. rmdir()*

  *=> 异步删除文件夹*

  *=> 语法: fs.rmdir(路径, 回调函数)*

```js
fs.rmdir('./a', err => {
 if (err) return console.log(err)
})
```

 *12. rmdirSync()*

  *=> 同步删除文件夹*

  *=> 语法: fs.rmdirSync(路径)*

```js
fs.rmdirSync('./b')
```

 *13. unlink()*

  *=> 异步删除文件*

  *=> 语法: fs.unlink(路径, 回调函数)*

```js
fs.unlink('./test1.txt', err => {
if (err) return console.log(err)
})
```

 *14. unlinSync()*

  *=> 同步删除文件*

  *=> 语法: fs.unlinkSync(路径)*

```js
fs.unlinkSync('./test123.txt')
```

 *15. stat()*

  *=> 异步查看*

  *=> 语法: fs.stat(路径, 回调函数)*

   *-> 回调函数里面可以接收一个 data*

   *-> 表示你查看的这个路径的内容*

```js
 fs.stat('./test.txt', (err, data) => {
  if (err) return console.log(err)
  console.log(data)
  console.log(data.isFile())
  console.log(data.isDirectory())
})
```

 *16. statSync()*

  *=> 同步查看*

  *=> 语法: fs.statSync(路径)*

  *=> 返回值: 查看该路径的结果*

```js
const res = fs.statSync('../02_自定义模块')
console.log(res)
```

 *17. isFile()*

  *=> 注意: 只有 stats 可以调用*

   *-> stat 的 回调函数中的 data*

   *-> statSync 的返回值*

  *=> 语法: stats.isFile()*

  *=> 返回值: 一个布尔值*

```js
console.log(res.isFile())
```

 *18: isDirectory()*

  *=> 注意: 只有 stats 可以调用*

   *-> stat 的 回调函数中的 data*

   *-> statSync 的返回值*

  *=> 语法: stats.isDirectory()*

  *=> 返回值: 一个布尔值*

```js
console.log(res.isDirectory())
```

#### 01_2、path模块

*path 模块*

  *+ node 自带的一个内置模块*

  *+ 里面封装的都是一些操作路径的或者和路径相关的方法*

  *+ 使用的时候直接导入就可以了*



```js
console.log(path)
/*
   resolve: [Function: resolve],
  normalize: [Function: normalize],
  isAbsolute: [Function: isAbsolute],
  join: [Function: join],
  relative: [Function: relative],
  toNamespacedPath: [Function: toNamespacedPath],
  dirname: [Function: dirname],
  basename: [Function: basename],
  extname: [Function: extname],
  format: [Function: bound _format],
  parse: [Function: parse],
  sep: '\\',
  delimiter: ';',
  win32: [Circular *1],
  posix: <ref *2> {
    resolve: [Function: resolve],
    normalize: [Function: normalize],
    isAbsolute: [Function: isAbsolute],
    join: [Function: join],
    relative: [Function: relative],
    toNamespacedPath: [Function: toNamespacedPath],
    dirname: [Function: dirname],
    basename: [Function: basename],
    extname: [Function: extname],
    format: [Function: bound _format],
    parse: [Function: parse],
    sep: '/',
    delimiter: ':',
    win32: [Circular *1],
    posix: [Circular *2],
    _makeLong: [Function: toNamespacedPath]
  },
  _makeLong: [Function: toNamespacedPath]
}

  }
*/

```



 *1. join()*

  *+ 拼接相对路径*

  *+ 语法: path.join(路径1, 路径2, 路径3, ...)*

  *+ 返回值: 拼接好的路径结果*

  *+ 注意:*

   *=> 拼接规则是后一个参数是前一个参数的子级*

   *=> 除非你写 ../*

```js
const path1 = path.join('a', './b', '/c', 'd')
console.log(path1)

const path2 = path.join('a', './b', '/c', 'd', '../e')
console.log(path2)
```

 *2. resolve()*

  *+ 拼接绝对路径*

  *+ 语法: path.resolve(路径1, 路径2, 路径3, ...)*

  *+ 返回值: 拼接好的绝对路径*

  *+ 注意:*

   *=> 拼接规则是每一个参数都是相对独立的一个参数*

   *=> 如果你写 /xx, 直接回到根目录*

​    *-> xx 当前目录下*

​    *-> ./xx 当前目录下*

​    *-> ../xx 上一级目录*

​    *-> /xx 根目录*

```js
const path1 = path.resolve('a', './b')
console.log(path1)
const path2 = path.resolve('a', './b', '/c')
console.log(path2)
```

 *3. extname()*

  *+ 获取文件后缀名*

  *+ 语法: path.extname(文件名)*

  *+ 返回值: 该文件的后缀名*

```js
const ext = path.extname('abc.html')
console.log(ext)
```

 *4. isAbsolute()*

  *+ 判断路径是不是绝对路径*

  *+ 语法: path.isAbsolute(路径)*

  *+ 返回值: 一个布尔值*

  *+ 路径区分:*

   *=> 绝对路径: 从根目录开始的路径*

   *=> 相对路径: 不是从根目录开始的路径*

```js
const res = path.isAbsolute('/a/b')
 console.log(res)
```

 *5. parse()*

  *+ 解析路径信息*

  *+ 语法: path.parse(路径)*

  *+ 返回值: 一个对象, 里面包含该路径的所有信息*

```js
const res = path.parse(__filename)
console.log(res)
/*
  {
    root: 'E:\\',
    dir: 'E:\\BK_GP_19\\01_第一周\\01_DAY\\01_代码\\03_内置模块',
    base: '02_path模块.js',
    ext: '.js',
    name: '02_path模块'
  }
*/
```

 *每一个 js 文件里面自带两个变量*

  *1. __dirname: 表示该文件所在的文件夹的绝对路径*

  *2. __filename: 表示该文件的绝对路径*

#### 01_3、url内置模块

*url 内置模块*

  *+ node 自带的一个内置模块*

  *+ 里面封装的都是和 url 地址栏相关的方法*

  *+ 使用的时候直接导入使用*



 *1. parse()*

  *+ 解析 url 地址的方法*

  *+ 语法: url.parse(url 地址, 是否解析查询字符串)*

   *-> 地址: 要解析的地址*

   *-> 是否解析查询字符串: 默认是 false, 选填 true*

​    *=> 会把地址里面的查询字符串解析成一个对象的形式*

  *+ 返回值: 是一个对象, 里面包含整个 url 地址的所有信息*

```js
const url = require('url')

// 1. parse()
const res = url.parse('http://www.guoxiang.com:8080/a/b/c/hello.html?a=100&b=200#abc', true)
 console.log(res)
/*
  {
    protocol: 'http:',
    slashes: true,
    auth: null,
    host: 'www.guoxiang.com:8080',
    port: '8080',
    hostname: 'www.guoxiang.com',
    hash: '#abc',
    search: '?a=100&b=200',
    query: 'a=100&b=200',
    pathname: '/a/b/c/hello.html',
    // 路径标识符, 前后端配套的一个标识符(暗号)
    path: '/a/b/c/hello.html?a=100&b=200',
    href: 'http://www.guoxiang.com:8080/a/b/c/hello.html?a=100&b=200#abc'
  }
*/



// post 请求是在请求体携带参数
//   a=100&b=200&c=300
const str = 'a=100&b=200&c=300'
const res = url.parse('?' + str, true)
console.log(res)
```

#### 01_4、http模块

*http 模块*

  *+ 也是 node 内置的一个 模块*

  *+ 主要是用来创建一个 http 服务的模块*

  *+ 需要使用的时候直接导入使用*



 *1. createServer()*

  *=> 创建服务的方法*

  *=> 语法: http.createServer(function () {})*

   *-> 每一个请求进来的时候, 都会触发函数*

  *=> 返回值: 一个服务*



 *2. listen()*

  *=> 注意: 需要使用 服务(createServer 的返回值) 来调用*

  *=> 语法: 服务.listen(端口号, 回调函数)*

   *-> 端口号: 0 ~ 65535, 尽量不使用 1024 以下*

   *-> 域名: 默认是本机 localhost || 127.0.0.1*

  *=> 当 listen 执行以后*

   *-> 你的 命令行, 就会被变成了一个服务器*

   *-> 一个什么都没有的服务器*

   *-> 当你访问 localhost:8080 的时候, createServer 后面的函数就会执行*



假设：

 *开发文档:*

  *1. /abc*

   *=> 表示 index.html*

  *+ 前端: 如果我想要 index.html 文件*

   *=> 发送请求: path 位置书写 /abc*

  *+ 后端: 当我接收到 /abc 这个请求的时候*

   *=> 我要去读取 index.html 文件, 把内容返回给前端*

```js
// 0. 导入 http 模块
const http = require('http')
const fs = require('fs')

// 1. createServer()
const server = http.createServer(function (req, res) {
  // 这个函数接收两个参数
  // req: request 本次请求的所有信息
  //     req. 里面有一个信息叫做 url 就是路径信息
  // res: response 本次的响应对象
  //     res 里面有一个方法叫做 end()
  //     专门给前端返回信息的方法
  // console.log('有请求进来了')

  // console.log(req.url)
  if (req.url === '/abc') {
    // 读取 index.html
    fs.readFile('./index.html', (err, data) => {
      if (err) return console.log(err)

      // 把 data 返回给前端
      res.end(data)
    })
  }



})

// 2. 监听端口号
server.listen(8080, () => {
  console.log('running at port 8080! ^_^ ')
})
```

### 02_第三方模块

#### 02_1 moment模块

*moment 是一个第三方插件*

  *+ 专门用来格式化时间*

  *+ 官方: https://momentjs.com/*



 *使用*

  *+ moment(时间对象).format(格式化规则)*

  *+ 在你导入包以后, 使用方法之前, 设置一下语言环境*

```js
const moment = require('moment')
// 把格式化包换成中文
moment.locale('zh-cn')

// 拿到当前时间
const time = new Date()

// 把当前时间进行格式化
const res = moment(time).format('MMMM Do YYYY, h:mm:ss a')
console.log(time)
console.log(res)
```

#### 02_2 nodemailer模块

 *nodemailer 模块*

  *+ 第三方模块, 专门用来发邮件*

  *+ 下载使用*



 *使用:*

  *1. 使用 nodemailer 创建一个 发送器(邮差)*

   *=> 语法: nodemailer.createTransporter(配置文件{})*

​    *-> 参数位置: 需要一些内容*

​    *-> 对邮件发送放的配置*

​    *-> 直接在包里面找到 -> nodemailer -> lib -> well-known*

   *=> 返回值: 就是一个发送器*

  *2. 使用发送器去发送邮件*

   *=> sendmail(邮件的配置{}, 回调函数) 方法*

​    *-> from*

​    *-> to*

​    *-> subject: '标题'*

​    *-> text: '' 文本内容*

​    *-> html: 超文本内容*

```js
const nodemailer = require('nodemailer')

// 创建发送器
const transporter = nodemailer.createTransport({
  "host": "smtp.qq.com",
  "port": 465,
  "secure": true,
  // 发送方用户名和密码验证
  // 使用的 smtp 的用户名和密码
  auth: {
    user: '493405058@qq.com',
    pass: 'txsfmdffeznlbijh'
  }
})

transporter.sendMail({
  from: '493405058@qq.com',
  to: '493405058@qq.com',
  subject: '验证码',
  text: '', // 文本内容
  html: '<h1>本次的验证码是  1234    ^_^</h1>', // 超文本内容
}, function (err, info) {
  if (err) return console.log(err)
  console.log('发送成功')
  console.log(info)
})
```

#### 02_3 mysql模块

 *mysql 模块*

  *+ 专门用来连接 mysql 数据库*



 *使用*

  *+ mysql 包给我们提供了两个连接数据库的方式*

   *1. createConnection()*

​    *-> 语义趋近于单次连接单次使用*

​    *-> 连接, 执行 sql 语句, 关闭*

​    *-> 最好是执行一次 数据库操作关闭一次*

   *2. cretaePool()*

​    *-> 创建一个连接池*

​    *-> 语义趋近于打开就不关了*



 *使用 db 模块执行 sql 语句*

  *1. db.query(sql, 回调函数)*

  *2. db.query(sql, [], 回调函数)*

   *-> 在 sql 语句里面预埋好多 问号(?)*

   *-> 你在数组里面依次传递参数*

```js
// 0. 导入 mysql
const mysql = require('mysql')

// 1. 连接数据库
//    返回值就是连接好的模块
// const db = mysql.createConnection({
//   host: 'localhost', // 127.0.0.1
//   port: '3306',
//   user: 'root',
//   password: 'root',
//   database: 'gp19' // 操作哪一个库
// })


// 2. 使用 db 模块执行 sql 语句
// const username = 'guoxiang'
// const password = '123456'
// db.query('SELECT * FROM `users` WHERE `username`="' + username + '"', (err, data) => {
//   if (err) return console.log(err)

//   console.log(data)
// })

// db.query(
//   'SELECT * FROM `users` WHERE `username`=? AND `password`=?',
//   [username, password],
//   (err, data) => {
//     if (err) return console.log(err)

//     console.log(data)
//   })


// 3. 关闭数据库连接
// db.end()



// 1. 连接数据库
const db = mysql.createPool({
  host: '127.0.0.1',
  port: 3306,
  user: 'root',
  password: 'root',
  database: 'gp19'
})

// 2. 操作数据库
const sql = 'INSERT INTO `users` VALUES(null, ?, ?)'
db.query(sql, ['张思锐', '123456'], (err, data) => {
  if (err) return console.log(err)

  console.log(data)
})
```

