                             ### node学习

#### node基本了解

##### 1.Node.js中的JavaScript

没有BOM、DOM
有Ecmascript
在node这个JavaScript执行环境中为JavaScript提供了一些服务器级别的操作API
    例如文件读写
    网络服务的构建
    网络通信
    http服务器
   等处理....

node中可以像@import（）一样来引用加载JavaScript脚本文件
浏览器中的JavaScript是没有文件操作的能力的
但是node中的JavaScript具有文件操作的能力

##### 2.什么是npm

npm是世界上最大的开源库生态系统
绝大多数JavaScript相关的包都存放在了npm上，这样做的目的是为了让开发人员更方便的去下载使用。

##### 3.一些资源

![1572932564888](C:\Users\bbk\AppData\Roaming\Typora\typora-user-images\1572932564888.png)

#### 用node操作文件

  必须引入fs这个核心模块，在这个核心模块中就提供了所有的文件操作相关的API

加载模块：var fs = require('fs')

读取文件：fs.readFild('路径包括文件名'，function（error,data){
                       console.log(data)
              }    //成功：data是数据，error为null   失败data null  error 错误对象

写文件：fs.writeFile('要写的路径包括文件名'，'文件内容'，function（error){ } 
                  //成功error是null，失败error是错误对象

因为不会自动报错所以：所以需要先判断是否有错误
加个错误判断         if （error）{    console.log('读取文件失败了')   return  }

#### 用node创建一个web服务器

node中提供了一个核心模块：http，可以用来编写服务器
步骤：
  1.加载http模块：var http = require('http')
  2.使用http.createServer()方法创建一个Web服务器 ，返回一个实例。
         var server = http.createServer()
  3.服务器要干嘛？(监听request请求事件，设置请求处理函数)
     提供服务：对 数据的服务
      发请求
     接收请求
     处理请求
     给个反馈（发送响应）
      注册 request 请求事件
       当客户端请求过来，就会自动触发服务器的 request 请求事件，然后执行第二个参数：回调处理 函数

~~~javascript
server.on('request', function () {
  console.log('收到客户端的请求了')
})
~~~

4. 绑定端口号，启动服务器

   ~~~javascript
   server.listen(3000, function () {
     console.log('服务器启动成功了，可以通过 http://127.0.0.1:3000/ 来进行访问')
   })
   ~~~

例子

~~~javascript
var http = require('http')

var server = http.createServer()

// request 请求事件处理函数，需要接收两个参数：
//    Request 请求对象
//        请求对象可以用来获取客户端的一些请求信息，例如请求路径
//    Response 响应对象
//        响应对象可以用来给客户端发送响应消息
server.on('request', function (request, response) {
  // http://127.0.0.1:3000/ /
  // http://127.0.0.1:3000/a /a
  // http://127.0.0.1:3000/foo/b /foo/b
  console.log('收到客户端的请求了，请求路径是：' + request.url)

  // response 对象有一个方法：write 可以用来给客户端发送响应数据
  // write 可以使用多次，但是最后一定要使用 end 来结束响应，否则客户端会一直等待
  response.write('hello')
  response.write(' nodejs')

  // 告诉客户端，我的话说完了，你可以呈递给用户了
  response.end()

  // 由于现在我们的服务器的能力还非常的弱，无论是什么请求，都只能响应 hello nodejs
  // 思考：
  //  我希望当请求不同的路径的时候响应不同的结果
  //  例如：
  //  / index
  //  /login 登陆
  //  /register 注册
  //  /haha 哈哈哈
})

server.listen(3000, function () {
  console.log('服务器启动成功了，可以通过 http://127.0.0.1:3000/ 来进行访问')
})

~~~

例子二：根据不同的请求路径发送不同的响应结果

1. 获取请求路径

​      req.url 获取到的是端口号之后的那一部分路径

​       也就是说所有的 url 都是以 / 开头的

2. 判断路径处理响应

   ~~~javascript
   var http = require('http')
   
   // 1. 创建 Server
   var server = http.createServer()
   
   // 2. 监听 request 请求事件，设置请求处理函数
   server.on('request', function (req, res) {
     console.log('收到请求了，请求路径是：' + req.url)
     console.log('请求我的客户端的地址是：', req.socket.remoteAddress, req.socket.remotePort)
   
     // res.write('hello')
     // res.write(' world')
     // res.end()
   
     // 上面的方式比较麻烦，推荐使用更简单的方式，直接 end 的同时发送响应数据
     // res.end('hello nodejs')
   
     // 根据不同的请求路径发送不同的响应结果
     // 1. 获取请求路径
     //    req.url 获取到的是端口号之后的那一部分路径
     //    也就是说所有的 url 都是以 / 开头的
     // 2. 判断路径处理响应
   
     var url = req.url
   
     if (url === '/') {
       res.end('index page')
     } else if (url === '/login') {
       res.end('login page')
     } else if (url === '/products') {
       var products = [{
           name: '苹果 X',
           price: 8888
         },
         {
           name: '菠萝 X',
           price: 5000
         },
         {
           name: '小辣椒 X',
           price: 1999
         }
       ]
   
       // 响应内容只能是二进制数据或者字符串
       // 不能是以下这几种
       //  数字
       //  对象
       //  数组
       //  布尔值
       res.end(JSON.stringify(products))
     } else {
       res.end('404 Not Found.')
     }
   })
   
   // 3. 绑定端口号，启动服务
   server.listen(3000, function () {
     console.log('服务器启动成功，可以访问了。。。')
   })
   
   ~~~

#### 补充核心模块概念

Node 中的 JavaScript

+ EcmaScript
  * 变量
  * 方法
  * 数据类型
  * 内置对象
  * Array
  * Object
  * Date
  * Math
+ 模块系统
  * 在 Node 中没有全局作用域的概念
  * 在 Node 中，只能通过 require 方法来加载执行多个 JavaScript 脚本文件
  * require 加载只能是执行其中的代码，文件与文件之间由于是模块作用域，所以不会有污染的问题
    - 模块完全是封闭的
    - 外部无法访问内部
    - 内部也无法访问外部
  * 模块作用域固然带来了一些好处，可以加载执行多个文件，可以完全避免变量命名冲突污染的问题
  * 但是某些情况下，模块与模块是需要进行通信的
  * 在每个模块中，都提供了一个对象：`exports`
  * 该对象默认是一个空对象
  * 你要做的就是把需要被外部访问使用的成员手动的挂载到 `exports` 接口对象中
  * 然后谁来 `require` 这个模块，谁就可以得到模块内部的 `exports` 接口对象
  * 还有其它的一些规则，具体后面讲，以及如何在项目中去使用这种编程方式，会通过后面的案例来处理
+ 核心模块
  * 核心模块是由 Node 提供的一个个的具名的模块，它们都有自己特殊的名称标识，例如
    - fs 文件操作模块
    - http 网络服务构建模块
    - os 操作系统信息模块
    - path 路径处理模块
    - 。。。。
  * 所有核心模块在使用的时候都必须手动的先使用 `require` 方法来加载，然后才可以使用，例如：
    - `var fs = require('fs')`

~~~javascript
// require 是一个方法
// 它的作用就是用来加载模块的
// 在 Node 中，模块有三种：
//    具名的核心模块，例如 fs、http
//    用户自己编写的文件模块
//      相对路径必须加 ./
//      可以省略后缀名
//      相对路径中的 ./ 不能省略，否则报错
//    在 Node 中，没有全局作用域，只有模块作用域
//      外部访问不到内部
//      内部也访问不到外部
//      默认都是封闭的
//    既然是模块作用域，那如何让模块与模块之间进行通信
//    有时候，我们加载文件模块的目的不是为了简简单单的执行里面的代码，更重要是为了使用里面的某个成员

var foo = 'aaa'

console.log('a start')

function add(x, y) {
  return x + y
}

// Error: Cannot find module 'b'
// require('b')

// 可以
// require('./b.js')

// 推荐：可以省略后缀名
require('./b')

console.log('a end')

console.log('foo 的值是：', foo)

~~~

#### 模块的加载与导出

~~~javascript
// require 方法有两个作用：
//    1. 加载文件模块并执行里面的代码
//    2. 拿到被加载文件模块导出的接口对象
//    
//    在每个文件模块中都提供了一个对象：exports
//    exports 默认是一个空对象
//    你要做的就是把所有需要被外部访问的成员挂载到这个 exports 对象中
var bExports = require('./b')
var fs = require('fs')

console.log(bExports.foo)

console.log(bExports.add(10, 30))

console.log(bExports.age)

bExports.readFile('./a.js')

fs.readFile('./a.js', function (err, data) {
  if (err) {
    console.log('读取文件失败')
  } else {
    console.log(data.toString())
  }
})

~~~

~~~javascript
var foo = 'bbb'

// console.log(exports)

exports.foo = 'hello'

exports.add = function (x, y) {
  return x + y
}

exports.readFile = function (path, callback) {
  console.log('文件路径：', path)
}

var age = 18

exports.age = age

function add(x, y) {
  return x - y
}

~~~

#### 关于乱码

~~~javascript
// require
// 端口号

var http = require('http')

var server = http.createServer()

server.on('request', function (req, res) {
  // 在服务端默认发送的数据，其实是 utf8 编码的内容
  // 但是浏览器不知道你是 utf8 编码的内容
  // 浏览器在不知道服务器响应内容的编码的情况下会按照当前操作系统的默认编码去解析
  // 中文操作系统默认是 gbk
  // 解决方法就是正确的告诉浏览器我给你发送的内容是什么编码的
  // 在 http 协议中，Content-Type 就是用来告知对方我给你发送的数据内容是什么类型
  // res.setHeader('Content-Type', 'text/plain; charset=utf-8')
  // res.end('hello 世界')

  var url = req.url

  if (url === '/plain') {
    // text/plain 就是普通文本
    res.setHeader('Content-Type', 'text/plain; charset=utf-8')
    res.end('hello 世界')
  } else if (url === '/html') {
    // 如果你发送的是 html 格式的字符串，则也要告诉浏览器我给你发送是 text/html 格式的内容
    res.setHeader('Content-Type', 'text/html; charset=utf-8')
    res.end('<p>hello html <a href="">点我</a></p>')
  }
})

server.listen(3000, function () {
  console.log('Server is running...')
})

~~~

#### content-type

content-type类型查询对应表：http://tool.oschina.net/commons

~~~javascript
// 1. 结合 fs 发送文件中的数据
// 2. Content-Type
//    http://tool.oschina.net/commons
//    不同的资源对应的 Content-Type 是不一样的
//    图片不需要指定编码
//    一般只为字符数据才指定编码

var http = require('http')
var fs = require('fs')

var server = http.createServer()

server.on('request', function (req, res) {
  // / index.html
  var url = req.url

  if (url === '/') {
    // 肯定不这么干
    // res.end('<!DOCTYPE html><html lang="en"><head><meta charset="UTF-8"><title>Document</title></head><body><h1>首页</h1></body>/html>')

    // 我们要发送的还是在文件中的内容
    fs.readFile('./resource/index.html', function (err, data) {
      if (err) {
        res.setHeader('Content-Type', 'text/plain; charset=utf-8')
        res.end('文件读取失败，请稍后重试！')
      } else {
        // data 默认是二进制数据，可以通过 .toString 转为咱们能识别的字符串
        // res.end() 支持两种数据类型，一种是二进制，一种是字符串
        res.setHeader('Content-Type', 'text/html; charset=utf-8')
        res.end(data)
      }
    })
  } else if (url === '/xiaoming') {
    // url：统一资源定位符
    // 一个 url 最终其实是要对应到一个资源的
    fs.readFile('./resource/ab2.jpg', function (err, data) {
      if (err) {
        res.setHeader('Content-Type', 'text/plain; charset=utf-8')
        res.end('文件读取失败，请稍后重试！')
      } else {
        // data 默认是二进制数据，可以通过 .toString 转为咱们能识别的字符串
        // res.end() 支持两种数据类型，一种是二进制，一种是字符串
        // 图片就不需要指定编码了，因为我们常说的编码一般指的是：字符编码
        res.setHeader('Content-Type', 'image/jpeg')
        res.end(data)
      }
    })
  }
})

server.listen(3000, function () {
  console.log('Server is running...')
})

~~~

#### 代码风格问题

为了约定大家的代码风格，所以在社区中诞生了一些比较规范的代码风格规范：

- [JavaScript Standard Style](https://standardjs.com/)
- Airbnb JavaScript Style

分号：当你采用了无分号代码风格的时候，只需要注意以下情况就不会有上面的问题了
           当一行代码是以：
                           （
                              [
                               `  ` 
   `
                        开头的时候，则在前面补上一个分号用以避免一些语法解析错误。
        （`是 EcmaScript 6 中新增的一种字符串包裹方式，叫做：模板字符串它支持换行和非常方便拼接变量）

*所以你会发现在一些第三方的代码中能看到一上来就以一个 ; 开头。*

结论：

   无论你的代码是否有分号，都建议如果一行代码是以 (、[、` 开头的，则最好都在其前面补上一个分号。

   有些人也喜欢玩儿一些花哨的东西，例如可以使用 ! ~ 等。

#### 指定服务器的根目录作为web目录

手动设置：

~~~javascript
var wwwDir = 'D:/Movie/www'
server.on('request', function (req, res) {
  var url = req.url
  // / index.html
  // /a.txt wwwDir + /a.txt
  // /apple/login.html wwwDir + /apple/login.html
  // /img/ab1.jpg wwwDir + /img/ab1.jpg
  if (url === '/') {
    fs.readFile(wwwDir + '/index.html', function (err, data) {
      // if (err) {
      //   res.end('404 Not Found.')
      // } else {

      // }

      if (err) {
        // return 有两个作用：
        //  1. 方法返回值
        //  2. 阻止代码继续往后执行
        return res.end('404 Not Found.')
      }
      res.end(data)
    })
  } else if (url === '/a.txt') {
    fs.readFile(wwwDir + '/a.txt', function (err, data) {
      if (err) {
        return res.end('404 Not Found.')
      }
      res.end(data)
    })
  } else if (url === '/index.html') {
    fs.readFile(wwwDir + '/index.html', function (err, data) {
      if (err) {
        return res.end('404 Not Found.')
      }
      res.end(data)
    })
  } else if (url === '/apple/login.html') {
    fs.readFile(wwwDir + '/apple/login.html', function (err, data) {
      if (err) {
        return res.end('404 Not Found.')
      }
      res.end(data)
    })
  }
})

// 3. 绑定端口号，启动服务
server.listen(3000, function () {
  console.log('running...')
})
~~~

上面方法每次在根目录中添加一个文件就要多写一条if分支，因此改进为一下代码

~~~javascript
var http = require('http')
var fs = require('fs')

var server = http.createServer()

var wwwDir = 'D:/Movie/www'

server.on('request', function (req, res) {
  var url = req.url
  // / index.html
  // /a.txt wwwDir + /a.txt
  // /apple/login.html wwwDir + /apple/login.html
  // /img/ab1.jpg wwwDir + /img/ab1.jpg
  

  var filePath = '/index.html'
  if (url !== '/') {
    filePath = url
  }

  fs.readFile(wwwDir + filePath, function (err, data) {
    if (err) {
      return res.end('404 Not Found.')
    }
    res.end(data)
  })
})

// 3. 绑定端口号，启动服务
server.listen(3000, function () {
  console.log('running...')
})

~~~

#### 像Apache那样显示文件目录

##### 回顾 在浏览器中使用模板引擎

~~~javascript
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>06-在浏览器中使用art-template</title>
</head>
<body>
  <!-- 
    注意：在浏览器中需要引用 lib/template-web.js 文件

    强调：模板引擎不关心你的字符串内容，只关心自己能认识的模板标记语法，例如 {{}}
    {{}} 语法被称之为 mustache 语法，八字胡啊。
   -->
  <script src="node_modules/art-template/lib/template-web.js"></script>
  <script type="text/template" id="tpl">
    <!DOCTYPE html>
    <html lang="en">
    <head>
      <meta charset="UTF-8">
      <title>Document</title>
    </head>
    <body>
      <p>大家好，我叫：{{ name }}</p>
      <p>我今年 {{ age }} 岁了</p>
      <h1>我来自 {{ province }}</h1>
      <p>我喜欢：{{each hobbies}} {{ $value }} {{/each}}</p>
    </body>
    </html>
  </script>
  <script>
    var ret = template('tpl', {
      name: 'Jack',
      age: 18,
      province: '北京市',
      hobbies: [
        '写代码',
        '唱歌',
        '打游戏'
      ]
    })

    console.log(ret)
  </script>
</body>
</html>

~~~

#####  在node中使用模板引擎

~~~javascript
// art-template
// art-template 不仅可以在浏览器使用，也可以在 node 中使用

// 安装：
//    npm install art-template
//    该命令在哪执行就会把包下载到哪里。默认会下载到 node_modules 目录中
//    node_modules 不要改，也不支持改。

// 在 Node 中使用 art-template 模板引擎
// 模板引起最早就是诞生于服务器领域，后来才发展到了前端。
// 
// 1. 安装 npm install art-template
// 2. 在需要使用的文件模块中加载 art-template
//    只需要使用 require 方法加载就可以了：require('art-template')
//    参数中的 art-template 就是你下载的包的名字
//    也就是说你 isntall 的名字是什么，则你 require 中的就是什么
// 3. 查文档，使用模板引擎的 API


var template = require('art-template')
var fs = require('fs')

// 这里不是浏览器
// template('script 标签 id', {对象})

// var tplStr = `
// <!DOCTYPE html>
// <html lang="en">
// <head>
//   <meta charset="UTF-8">
//   <title>Document</title>
// </head>
// <body>a
//   <p>大家好，我叫：{{ name }}</p>
//   <p>我今年 {{ age }} 岁了</p>
//   <h1>我来自 {{ province }}</h1>
//   <p>我喜欢：{{each hobbies}} {{ $value }} {{/each}}</p>
// </body>
// </html>
// `

fs.readFile('./tpl.html', function (err, data) {
  if (err) {
    return console.log('读取文件失败了')
  }
  // 默认读取到的 data 是二进制数据
  // 而模板引擎的 render 方法需要接收的是字符串
  // 所以我们在这里需要把 data 二进制数据转为 字符串 才可以给模板引擎使用
  var ret = template.render(data.toString(), {
    name: 'Jack',
    age: 18,
    province: '北京市',
    hobbies: [
      '写代码',
      '唱歌',
      '打游戏'
    ],
    title: '个人信息'
  })

  console.log(ret)
})

~~~

##### 回到正题 显示目录

~~~JavaScript
var http = require('http')
var fs = require('fs')
var template = require('art-template')

var server = http.createServer()

var wwwDir = 'D:/Movie/www'

server.on('request', function (req, res) {
  var url = req.url
  fs.readFile('./template-apache.html', function (err, data) {
    if (err) {
      return res.end('404 Not Found.')
    }
    // 1. 如何得到 wwwDir 目录列表中的文件名和目录名
    //    fs.readdir
    // 2. 如何将得到的文件名和目录名替换到 template.html 中
    //    2.1 在 template.html 中需要替换的位置预留一个特殊的标记（就像以前使用模板引擎的标记一样）
    //    2.2 根据 files 生成需要的 HTML 内容
    // 只要你做了这两件事儿，那这个问题就解决了
    fs.readdir(wwwDir, function (err, files) {
      if (err) {
        return res.end('Can not find www dir.')
      }

      // 这里只需要使用模板引擎解析替换 data 中的模板字符串就可以了
      // 数据就是 files
      // 然后去你的 template.html 文件中编写你的模板语法就可以了
      var htmlStr = template.render(data.toString(), {
        title: '哈哈',
        files: files
      })

      // 3. 发送解析替换过后的响应数据
      res.end(htmlStr)
    })
  })
})
server.listen(3000, function () {
  console.log('running...')
})

~~~

#### 模板引擎 客户端渲染和服务端渲染对比

服务端渲染：

![Screenshot_2019-11-06-23-44-22-254_tv.danmaku.bil](E:\QQ文件\2577475761\FileRecv\MobileFile\Screenshot_2019-11-06-23-44-22-254_tv.danmaku.bil.png)

客户端渲染：

![Screenshot_2019-11-06-23-40-20-514_tv.danmaku.bil](E:\QQ文件\2577475761\FileRecv\MobileFile\Screenshot_2019-11-06-23-40-20-514_tv.danmaku.bil.png)

- 服务端渲染
  + 说白了就是在服务端使用模板引擎
  + 模板引擎最早诞生于服务端，后来才发展到了前端

- 服务端渲染和客户端渲染的区别
  + 客户端渲染不利于 SEO 搜索引擎优化（SEO爬虫）
  + 服务端渲染是可以被爬虫抓取到的，客户端异步渲染是很难被爬虫抓取到的
  + 所以你会发现真正的网站既不是纯异步也不是纯服务端渲染出来的
  + 而是两者结合来做的
  + 例如京东的商品列表就采用的是服务端渲染，目的了为了 SEO 搜索引擎优化
  + 而它的商品评论列表为了用户体验，而且也不需要 SEO 优化，所以采用是客户端渲染

#### URL模块

~~~JavaScript
var url = require('url')

var obj = url.parse('/pinglun?name=的撒的撒&message=的撒的撒的撒', true)

console.log(obj)
console.log(obj.query)

~~~

*// 使用 url.parse 方法将路径解析为一个方便操作的对象，第二个参数为 true 表示直接将查询字符串转为一个对象（通过 query 属性来访问）*

  var parseObj = url.parse(req.url, true)

  *// 单独获取不包含查询字符串的路径部分（该路径不包含 ? 之后的内容）*

  var pathname = parseObj.pathname

#### CommonJS模块规范

  在Node中的Javascirpt还有一个很重要的概念：模块系统。

        - 模块作用域
        - 使用require方法用来加载模块
        - 使用exports接口对象用来导出模块中的成员

##### 加载require

语法：var 自定义变量名称 = require（‘模块’）
两个作用：执行被加载模块中的代码
                   得到被加载模块中的exports导出接口对象

优先从缓存加载：

~~~javascript
require('./a')

// 优先从缓存加载
// 由于 在 a 中已经加载过 b 了
// 所以这里不会重复加载
// 可以拿到其中的接口对象，但是不会重复执行里面的代码
// 这样做的目的是为了避免重复加载，提高模块加载效率
var fn = require('./b')

console.log(fn)

~~~

标识符分析：

~~~
// 如果是非路径形式的模块标识
// 路径形式的模块：
//  ./ 当前目录，不可省略
//  ../ 上一级目录，不可省略
//  /xxx 几乎不用
//  d:/a/foo.js 几乎不用
//  首位的 / 在这里表示的是当前文件模块所属磁盘根路径
//  .js 后缀名可以省略
// require('./foo.js')

// 核心模块的本质也是文件
// 核心模块文件已经被编译到了二进制文件中了，我们只需要按照名字来加载就可以了
// require('fs')
// require('http')

// 第三方模块
// 凡是第三方模块都必须通过 npm 来下载
// 使用的时候就可以通过 require('包名') 的方式来进行加载才可以使用
// 不可能有任何一个第三方包和核心模块的名字是一样的
// 既不是核心模块、也不是路径形式的模块
//    先找到当前文件所处目录中的 node_modules 目录
//    node_modules/art-template
//    node_modules/art-template/package.json 文件
//    node_modules/art-template/package.json 文件中的 main 属性
//    main 属性中就记录了 art-template 的入口模块
//    然后加载使用这个第三方包
//    实际上最终加载的还是文件

//    如果 package.json 文件不存在或者 main 指定的入口模块是也没有
//    则 node 会自动找该目录下的 index.js
//    也就是说 index.js 会作为一个默认备选项
//    
//    如果以上所有任何一个条件都不成立，则会进入上一级目录中的 node_modules 目录查找
//    如果上一级还没有，则继续往上上一级查找
//    。。。
//    如果直到当前磁盘根目录还找不到，最后报错：
//      can not find module xxx
// var template = require('art-template')
// 
// 注意：我们一个项目有且只有一个 node_modules，放在项目根目录中，这样的话项目中所有的子目录中的代码都可以加载到第三方包
// 不会出现有多个 node_modules
// 模块查找机制
//    优先从缓存加载
//    核心模块
//    路径形式的文件模块
//    第三方模块
//      node_modules/art-template/
//      node_modules/art-template/package.json
//      node_modules/art-template/package.json main
//      index.js 备选项
//      进入上一级目录找 node_modules
//      按照这个规则依次往上找，直到磁盘根目录还找不到，最后报错：Can not find moudle xxx
//    一个项目有且仅有一个 node_modules 而且是存放到项目的根目录

require('a')

~~~



##### 导出exports

1.node中是模块作用域，默认文件中所有的成员只在当前文件模块有效
2.对于希望可以被其他模块访问的成员，我们就需要把这些公开的成员都挂载到exports接口对象中就可以了
    导出多个对象（必须在对象中）。

~~~javascript
exports.a = 123
exports.b = 'hello'
exports.c = function () {
    console.log('ccc')
}
exports.d = {
    foo: 'bar'
}
~~~

导出单个成员（拿到的就是：函数、字符串）

~~~JavaScript
module.exports = 'hello'
~~~

以下情况会覆盖：

~~~javascript
module.exports = 'hello'

//以这个为准，后者会覆盖前者
module.exports = function (x, y) {
  return x+y
}
~~~

也可以导出多个成员：

~~~javascript
module.exports = {
  add: function () {
  return x+y
  },
  str: 'hello'
}
~~~

##### 原来解析

~~~JavaScript
// 在 Node 中，每个模块内部都有一个自己的 module 对象
// 该 module 对象中，有一个成员叫：exports 也是一个对象
// 也就是说如果你需要对外导出成员，只需要把导出的成员挂载到 module.exports 中

// 我们发现，每次导出接口成员的时候都通过 module.exports.xxx = xxx 的方式很麻烦，点儿的太多了
// 所以，Node 为了简化你的操作，专门提供了一个变量：exports 等于 module.exports

// var module = {
//   exports: {
//     foo: 'bar',
//     add: function
//   }
// }

// 也就是说在模块中还有这么一句代码
// var exports = module.exports

// module.exports.foo = 'bar'

// module.exports.add = function (x, y) {
//   return x + y
// }

// 两者一致，那就说明，我可以使用任意一方来导出内部成员
// console.log(exports === module.exports)

// exports.foo = 'bar'
// module.exports.add = function (x, y) {
//   return x + y
// }

// 当一个模块需要导出单个成员的时候
// 直接给 exports 赋值是不管用的

// exports.a = 123

// exports = {}
// exports.foo = 'bar'

// module.exports.b = 456

// 给 exports 赋值会断开和 module.exports 之间的引用
// 同理，给 module.exports 重新赋值也会断开

// 这里导致 exports !== module.exports
// module.exports = {
//   foo: 'bar'
// }

// // 但是这里又重新建立两者的引用关系
// exports = module.exports

// exports.foo = 'hello'

// {foo: bar}
exports.foo = 'bar'


// {foo: bar, a: 123}
module.exports.a = 123

// exports !== module.exports
// 最终 return 的是 module.exports
// 所以无论你 exports 中的成员是什么都没用
exports = {
  a: 456
}

// {foo: 'haha', a: 123}
module.exports.foo = 'haha'

// 没关系，混淆你的
exports.c = 456

// 重新建立了和 module.exports 之间的引用关系了
exports = module.exports

// 由于在上面建立了引用关系，所以这里是生效的
// {foo: 'haha', a: 789}
exports.a = 789

// 前面再牛逼，在这里都全部推翻了，重新赋值
// 最终得到的是 Function
module.exports = function () {
  console.log('hello')
}

// 真正去使用的时候：
//    导出多个成员：exports.xxx = xxx
//    导出多个成员也可以：module.exports = {
//                        }
//    导出单个成员：module.exports


// 谁来 require 我，谁就得到 module.exports
// 默认在代码的最后有一句：
// 一定要记住，最后 return 的是 module.exports
// 不是 exports
// 所以你给 exports 重新赋值不管用，
// return module.exports

~~~

~~~JavaScript
var fooExports = require('./foo')

console.log(fooExports)


// 如果你实在分不清楚 exports 和 module.exports
// 你可以选择忘记 exports
// 而只使用 module.exports 也没问题
// 
// module.exports.xxx = xxx
// moudle.exports = {}

~~~

总结：

1.exports是module.exports的一个引用：等价关系
2.导出单个对象，直接给exports赋值不管用，因为最后return的是module.exports不是exports

##### each和foreach的区别

jQuery 的 each 和 原生的 JavaScript 方法 forEach

+ EcmaScript 5 提供的
  * 不兼容 IE 8
+ jQuery 的 each 由 jQuery 这个第三方库提供
  * jQuery 2 以下的版本是兼容 IE 8 的
  * 它的 each 方法主要用来遍历 jQuery 实例对象（伪数组）
  * 同时它也可以作为低版本浏览器中 forEach 替代品
  * jQuery 的实例对象不能使用 forEach 方法，如果想要使用必须转为数组才可以使用
  * `[].slice.call(jQuery实例对象)`

##### 301 和 302 状态码区别

+ 301 永久重定向，浏览器会记住
+ 302 临时重定向

##### exports 和 module.exports 的区别

+ 每个模块中都有一个 module 对象
+ module 对象中有一个 exports 对象
+ 我们可以把需要导出的成员都挂载到 module.exports 接口对象中
+ 也就是：`moudle.exports.xxx = xxx` 的方式
+ 但是每次都 `moudle.exports.xxx = xxx` 很麻烦，点儿的太多了
+ 所以 Node 为了你方便，同时在每一个模块中都提供了一个成员叫：`exports`
+ `exports === module.exports` 结果为  `true`s
+ 所以对于：`moudle.exports.xxx = xxx` 的方式 完全可以：`expots.xxx = xxx`
+ 当一个模块需要导出单个成员的时候，这个时候必须使用：`module.exports = xxx` 的方式
+ 不要使用 `exports = xxx` 不管用
+ 因为每个模块最终向外 `return` 的是 `module.exports`
+ 而 `exports` 只是 `module.exports` 的一个引用
+ 所以即便你为 `exports = xx` 重新赋值，也不会影响 `module.exports`
+ 但是有一种赋值方式比较特殊：`exports = module.exports` 这个用来重新建立引用关系的
+ 之所以让大家明白这个道理，是希望可以更灵活的去用它

#### npm & package.json

npm全称：node package manager
例如：npm install art-template --save   加了--save导入包之后会在package.json文件中显示导入的包名以方便自己查看导                   入了什么包。
建议每一个项目都有一个package.json文件（包描述文件），都加上--save
package.json可以通过npm init的方式来自动初始化出来。
初始化以及装包示例：

~~~
PS C:\Users\bbk\Desktop\taxt> npm init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help json` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg>` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
package name: (taxt)
version: (1.0.0) 0.0.1
description: 这是一个测试项目
entry point: (index.js) main.js
test command:
git repository:
keywords:
author: bxk
license: (ISC)
About to write to C:\Users\bbk\Desktop\taxt\package.json:

{
  "name": "taxt",
  "version": "0.0.1",
  "description": "这是一个测试项目",
  "main": "main.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "bxk",
  "license": "ISC"
}


Is this OK? (yes)
PS C:\Users\bbk\Desktop\taxt> npm install --save jquery
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN taxt@0.0.1 No repository field.

+ jquery@3.4.1
added 1 package from 1 contributor and audited 1 package in 17.147s
found 0 vulnerabilities

PS C:\Users\bbk\Desktop\taxt>
~~~

json中显示：

~~~
{
  "name": "taxt",
  "version": "0.0.1",
  "description": "这是一个测试项目",
  "main": "main.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "bxk",
  "license": "ISC",
  "dependencies": {
    "jquery": "^3.4.1"
  }
}
~~~

这样做的另一个好处，当package.json文件还在但node_modules专门用来装导入的包文件夹丢失时，可以直接在cmd中输入npm install，它会自动找到package.json文件并把里面的包自动装好。

##### npm网站

npmjs.com

##### npm命令行工具

npm就是一个命令行工具，只要安装了node就已经安装了npm
可以通过在命令行中输入npm --version查看版本号
升级npm：npm install --global npm  

##### npm常用命令

npm init -y 可以跳过向导，快速生成
npm install 包名  ：只下载包 简写：npm i 包名
npm install --save 包名  ：下载并保存依赖项（package.json）文件中的dependencies选项 简写：npm i -S 包名
npm install ：一次性把dependencies选项中的依赖项全部安装 简写：npm i
npm uninstall 包名：只删除，依赖项依然保存 简写：npm un
npm uninstall --save 包名： 删除同时依赖项也删除  简写：npm un -S 包名
npm help：查看使用帮助
npm 命令 --help：查看指定命令使用帮助，如查看uninstall命令简写：输入npm uninstall --help可查看

##### 解决npm被墙问题

npm储存包文件的服务器在国外，有时候会被墙，速度很慢，所以我们需要解决这个问题
http://npm.taobao.org/  淘宝的开发团队把npm在国内做了一个备份
安装淘宝的cnpm：

~~~
npm install --global cnpm  #在任意目录下执行都可以，安装到全局，而非当前目录
~~~

接下来在安装包的时候把之前的npm替换成cnpm
例如：cnpm install jQuery 使用cnpm就会通过淘宝的服务器下载jQuery，速度会快一些。
如果不想装cnpm也可以用以下方式使用淘宝的cnpm：
![1573293375928](C:\Users\bbk\AppData\Roaming\Typora\typora-user-images\1573293375928.png)

#### path路径操作模块

path是个核心模块：专门用来操作路径，详细参考nodejs.org

path.basename：
              获取一个路径的文件名，包含扩展名

path.dirname
             获取一个路径中的目录部分

path.extname
            获取一个路径中的扩展名部分

path.parse
           把一个路径转为对象：
              root   根路径
               dir   目录
               base  包含后缀名的文件名
                ext   后缀名
               name  不包含后缀名的文件名

path.join
         当需要进行路径拼接的时候，推荐使用这个方法

path.isAbsolute：判断一个路径是否是绝对路径

#### node中的其他成员

在每个模块中，除了require、exports等模块相关API之外，还有两个特殊成员

- __dirname可以用来获取当前文件模块所属目录的绝对路径（动态获取）
  - __filename可以用来获取当前文件的绝对路径（动态获取）
    这两种方法都不受执行node命令所属路径影响的

在文件操作中，使用相对路径是不可靠的，因为node中的文件操作的路径被设计为相对于执行node命令所处的路径，为了解决这个问题只需要把相对路径变为绝对路径就可以了。
这里就可以使用__dirname或者__filename来帮助解决这个问题。
在拼接路径的过程中，为了避免手动拼接带来的一些低级错误，所以推荐多使用：path.join()来辅助拼接。
以后在文件操作中使用的相对路径都统一转换为这样的动态绝对路径。（模块中的路径标识和这里的路径没关系不受影响，就是相对于当前文件模块）

##### 文件路径问题

~~~JavaScript
var fs = require('fs')
var path = require('path')


// 模块中的路径标识和文件操作中的相对路径标识不一致
// 模块中的路径标识就是相对于当前文件模块，不受执行 node 命令所处路径影响
require('./b')

// ./a.txt 相对于当前文件路径
// ./a.txt 相对于执行 node 命令所处的终端路径
// 这不是错误，Node 就是这样设计的
// 就是说，文件操作路径中，相对路径设计的就是相对于执行 node 命令所处的路径
// fs.readFile('C:/Users/lpz/Desktop/nodejs/06/code/foo/a.txt', 'utf8', function (err, data) {
//   if (err) {
//     throw err
//   }
//   console.log(data)
// })

// console.log(__dirname + '/a.txt')

// C:\Users\lpz\Desktop\nodejs\06\code
fs.readFile(path.join(__dirname, './a.txt'), 'utf8', function (err, data) {
  if (err) {
    throw err
  }
  console.log(data)
})

~~~



#### Express

原生的http在某些方面表现不足以应对我们的开发需求，所以我们就需要使用框架来加快我们的开发效率，框架的目的就是提高效率，让我们的代码高度统一
node中也有其他开发框架，这里学习express

官网：expressjs.com

入门：

~~~JavaScript
// 0. 安装
// 1. 引包
var express = require('express')

// 2. 创建你服务器应用程序
//    也就是原来的 http.createServer
var app = express()


// 在 Express 中开放资源就是一个 API 的事儿
// 公开指定目录
// 只要这样做了，你就可以直接通过 /public/xx 的方式访问 public 目录中的所有资源了
app.use('/public/', express.static('./public/'))
app.use('/static/', express.static('./static/'))
app.use('/node_modules/', express.static('./node_modules/'))

// 模板引擎，在 Express 也是一个 API 的事儿

// 得到路径
// 一个一个的判断
// 以前的代码很丑

app.get('/about', function(req, res) {
    // 在 Express 中可以直接 req.query 来获取查询字符串参数
    console.log(req.query)
    res.send('你好，我是 Express!')
})

app.get('/pinglun', function(req, res) {
    // req.query
    // 在 Express 中使用模板引擎有更好的方式：res.render('文件名， {模板对象})
    // 可以自己尝试去看 art-template 官方文档：如何让 art-template 结合 Express 来使用
})

// 当服务器收到 get 请求 / 的时候，执行回调处理函数
app.get('/', function(req, res) {
    res.send(`
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Document</title>
  </head>
<body>
  <h1>hello Express！你好</h1>
</body>
</html>
`)
})

// 相当于 server.listen
app.listen(3001, function() {
    console.log('app is running at port 3001.')
})
~~~

#### 修改完代码node自动启动

用第三方命名航工具：nodemon来解决频繁修改代码重启服务器问题
它是一个基于node.js开发的一个第三方命令行工具，我们使用的时候需要独立安装

~~~
npm install --global nodemon
~~~

安装完之后使用时，nodemon app.js来启动就会自动监视文件变化，会自动重启服务器。

#### 基本路由

- 请求方法
- 请求路径
- 请求处理函数

get：

~~~shell 
app.get('/', function (req, res) {
  res.send('hello world!')
})
~~~

post:

~~~shell
app.post('/', function (req, res) {
  res.send('get a post request')
})
~~~

#### 静态服务

~~~javascript
var express = require('express')

// 1. 创建 app
var app = express()

// 当以 /public/ 开头的时候，去 ./public/ 目录中找找对应的资源
// 这种方式更容易辨识，推荐这种方式
// app.use('/public/', express.static('./public/'))

// 必须是 /a/puiblic目录中的资源具体路径
// app.use('/abc/d/', express.static('./public/'))

// 当省略第一个参数的时候，则可以通过 省略 /public 的方式来访问
// 这种方式的好处就是可以省略 /public/
app.use(express.static('./public/'))


app.get('/', function (req, res) {
  res.send('hello world')
})

app.listen(3000, function () {
  console.log('express app is running ...')
})
~~~

#### 在Express中配置使用art-template模板引擎

可以查看GitHub中文档 https://aui.github.io/art-template/ 

1.安装

~~~shell
npm install --save art-template
npm install --save express-art-template
~~~

2.配置

~~~shell
app.engine('art', require('express-art-template'))
~~~

3.使用

~~~shell
app.get('/', function (req, res) {
//Express 默认会去项目中的views目录找index.html
  res.render('index.html', {
    title: 'hello wrold'
  })
})
~~~

如果希望修改默认的views视图渲染存储目录，可以：

~~~JavaScript
 //如果想要修改默认的 views 目录，则可以
app.set('views', 目录路径)
~~~

代码例子：

~~~javascript
var express = require('express')
var bodyParser = require('body-parser')

var app = express()

app.use('/public/', express.static('./public/'))

// 配置使用 art-template 模板引擎
// 第一个参数，表示，当渲染以 .art 结尾的文件的时候，使用 art-template 模板引擎
// express-art-template 是专门用来在 Express 中把 art-template 整合到 Express 中
// 虽然外面这里不需要记载 art-template 但是也必须安装
// 原因就在于 express-art-template 依赖了 art-template
app.engine('html', require('express-art-template'))

// Express 为 Response 相应对象提供了一个方法：render
// render 方法默认是不可以使用，但是如果配置了模板引擎就可以使用了
// res.render('html模板名', {模板数据})
// 第一个参数不能写路径，默认会去项目中的 views 目录查找该模板文件
// 也就是说 Express 有一个约定：开发人员把所有的视图文件都放到 views 目录中

// 如果想要修改默认的 views 目录，则可以
// app.set('views', render函数的默认路径)

// 配置 body-parser 中间件（插件，专门用来解析表单 POST 请求体）
// parse application/x-www-form-urlencoded
app.use(bodyParser.urlencoded({ extended: false }))
// parse application/json
app.use(bodyParser.json())

var comments = [{
    name: '张三',
    message: '今天天气不错！',
    dateTime: '2015-10-16'
  },
  {
    name: '张三2',
    message: '今天天气不错！',
    dateTime: '2015-10-16'
  },
  {
    name: '张三3',
    message: '今天天气不错！',
    dateTime: '2015-10-16'
  },
  {
    name: '张三4',
    message: '今天天气不错！',
    dateTime: '2015-10-16'
  },
  {
    name: '张三5',
    message: '今天天气不错！',
    dateTime: '2015-10-16'
  }
]

app.get('/', function (req, res) {
  res.render('index.html', {
    comments: comments
  })
})

app.get('/post', function (req, res) {
  res.render('post.html')
})

// 当以 POST 请求 /post 的时候，执行指定的处理函数
// 这样的话我们就可以利用不同的请求方法让一个请求路径使用多次
app.post('/post', function (req, res) {
  // 1. 获取表单 POST 请求体数据
  // 2. 处理
  // 3. 发送响应

  // req.query 只能拿 get 请求参数
  // console.log(req.query)

  var comment = req.body
  comment.dateTime = '2017-11-5 10:58:51'
  comments.unshift(comment)

  // res.send
  // res.redirect
  // 这些方法 Express 会自动结束响应
  res.redirect('/')
  // res.statusCode = 302
  // res.setHeader('Location', '/') 
})

// app.get('/pinglun', function (req, res) {
//   var comment = req.query
//   comment.dateTime = '2017-11-5 10:58:51'
//   comments.unshift(comment)
//   res.redirect('/')
//   // res.statusCode = 302
//   // res.setHeader('Location', '/')
// })

app.listen(3000, function () {
  console.log('running...')
})

~~~

#### 在express中获取表单post请求体数据

express内置了一个api，可以直接通过req.query来获取

#### 在express中获取表单post请求体数据

在express中没有内置获取表单post请求体的api，我们需要用一个第三方包：body-parser

安装：npm install --save body-parser

配置：中间件

~~~JavaScript
var express = require('express')
var bodyParser = require('body-parser')

var app = express()

//加入这个配置会多出body这个属性，通过req.body来获取post请求体数据
// parse application/x-www-form-urlencoded
app.use(bodyParser.urlencoded({ extended: false }))

// parse application/json
app.use(bodyParser.json())

~~~

使用：

~~~JavaScript
app.use(function (req, res) {
  res.setHeader('Content-Type', 'text/plain')
  res.write('you posted:\n')
  res.end(JSON.stringify(req.body, null, 2))
})
~~~

#### 常见的需要使用异步函数的地方

~~~
// 注意：凡是需要得到一个函数内部异步操作的结果
    // setTimeout
    // readFile
    // wirteFile
    // readdir
    // ajax
    // 往往异步 API 都伴随有一个回调函数
    // var ret = fn()
    // $.get('dsadas', fucntion () {})
    // var ret = $.get()
~~~

##### 杂乱知识总结

~~~javascript
## 复习

- 文件路径中的 `/` 和模块标识中的 `/`
- Express 中配置使用 art-template 模板引擎
- Express 中配置使用 body-parser
- Express 中配置处理静态资源
- CRUD 案例中单独提取路由模块

## 上午总结

- 回调函数
  + 异步编程
  + 如果需要得到一个函数内部异步操作的结果，这是时候必须通过回调函数来获取
  + 在调用的位置传递一个函数进来
  + 在封装的函数内部调用传递进来的函数
- find、findIndex、forEach
  + 数组的遍历方法，都是对函数作为参数一种运用
    + every
  + some
  + includes
  + map
  + reduce
- package-lock.json 文件的作用
  + 下载速度快了
  + 锁定版本
- JavaScript 模块化
  + Node 中的 CommonJS
  + 浏览器中的
    * AMD require.js
    * CMD sea.js
  + EcmaScript 官方在 EcmaScript 6 中增加了官方支持
  + EcmaScript 6
  + 后面我们会学，编译工具
- MongoDB 数据库
  + MongoDB 的数据存储结构
    * 数据库
    * 集合（表）
    * 文档（表记录）
- MongoDB 官方有一个 mongodb 的包可以用来操作 MongoDB 数据库
  + 这个确实和强大，但是比较原始，麻烦，所以咱们不使用它
- mongoose
  + 真正在公司进行开发，使用的是 mongoose 这个第三方包
  + 它是基于 MongoDB 官方的 mongodb 包进一步做了封装
  + 可以提高开发效率
  + 让你操作 MongoDB 数据库更方便
- 掌握使用 mongoose 对数据集合进行基本的 CRUD
- 把之前的 crud 案例改为了 MongoDB 数据库版本
- 使用 Node 操作 mysql 数据库

//    find
//    findIndex

// find 接收一个方法作为参数，方法内部返回一个条件
// find 会遍历所有的元素，执行你给定的带有条件返回值的函数
// 符合该条件的元素会作为 find 方法的返回值
// 如果遍历结束还没有符合该条件的元素，则返回 undefined

var users = [
  {id: 1, name: '张三'},
  {id: 2, name: '张三'},
  {id: 3, name: '张三'},
  {id: 4, name: '张三'}
]

Array.prototype.myFind = function (conditionFunc) {
  // var conditionFunc = function (item, index) { return item.id === 4 }
  for (var i = 0; i < this.length; i++) {
    if (conditionFunc(this[i], i)) {
      return this[i]
    }
  }
}

var ret = users.myFind(function (item, index) {
  return item.id === 2
})

console.log(ret)
~~~

#### 在express配置使用express-session

参考文档：https://github.com/expressjs/session

安装：

~~~
npm install express-session
~~~

配置：

~~~JavaScript
app.use(session({
  // 配置加密字符串，它会在原有加密基础之上和这个字符串拼起来去加密
  // 目的是为了增加安全性，防止客户端恶意伪造
  secret: 'itcast',
  resave: false,
  saveUninitialized: false // 无论你是否使用 Session ，我都默认直接给你分配一把钥匙
}))
~~~

使用：

~~~javascript
//添加session数据
req.session.foo = 'bar'

//获取session数据
req.session.foo
~~~

提示：默认session数据是内存存储的，服务器一旦重启就会丢失，真正的生产环境会把session进行持久化存储。



#### MongoDB

详细： https://www.runoob.com/mongodb/mongodb-tutorial.html 
步骤：下载--安装--配置环境变量--输入MongoD --version

基本概念：有数据库--集合--文档

可以有多个数据库，一个数据库中可以有多个集合（表），一个集合中可以有多个文档（表记录），文档结构和灵活，没有任何限制。不需要像MySQL一样先创建数据库、表、设计结构，是需要在需要插入数据的时候指定往哪个数据库的哪个集合操作就可以了，MongoDB自动完成创建库创建表这件事。

~~~
{
  qq:{
     users:[
         {name:'张三', age:15},
         {name:'李四', age:15},
         {name:'张三54', age:15},
         {name:'张三1543', age:15}
         ....
     ],
     products:[
     
     ],
     ....
  },
  taobao:{
  
  }
  baidu:{
  
  }
}
~~~



##### 关系型数据库和非关系型数据库

表就是关系
   或者说表与表直接的存在关系。
1.所有的关系型数据库都需要通过sql语言来操作
2.所有的关系型数据库在操作之前都需要设计表结构
3.而且数据表还支持约束

- 唯一的
- 主键
- 默认值
- 非空

4.非关系型数据库非常灵活
5.有的菲关系型数据库就是key-value对儿
6.但是mogongdb是长得最像关系型数据库的非关系型数据库
    数据库-->数据库
    数据表-->集合（数组）
    表记录-->文档对象
7.mogongdb不需要设计表结构
8.也就是说你可以任意的往里面存数据，没有结构性这么一说

#### 启动和关闭数据库

启动：

~~~
# 使用所处盘符根目录下的/data/db作为自己的数据存储目录
# 所以在第一次执行该命令之前先自己手动新建一个/data/db
mongod
~~~

如果想要修改默认的数据库存储目录，可以：

~~~
Mongod --dbpath=数据存储目录路径
~~~

停止：

~~~
直接Ctrl+c
或者直接关闭cmd
~~~

#### 连接和退出数据库

连接：

~~~
# 该命令默认连接本机的MongoDB服务
mongo
~~~

退出：

~~~
# 在连接状态
exit
~~~

#### MongoDB基本命令

1.show.dbs
      查看显示所以数据库
2.db
     查看当前操作的数据库
3.use 数据库名称
     切换到指定的数据库（如果没有会新建）
4.插入数据

#### 在node中操作MongoDB数据库

##### 使用官方MongoDB包来操作

 https://github.com/mongodb/node-mongodb-native 

##### 使用第三方mongoose来操作MongoDB操作

这个包是基于上面官方包做了一次封装
官方文档： http://www.mongoosejs.net/ 

安装：

~~~
npm i mongoose
~~~

使用:

~~~JavaScript
const mongoose = require('mongoose');
mongoose.connect('mongodb://localhost/test');

const Cat = mongoose.model('Cat', { name: String });

const kitty = new Cat({ name: 'Zildjian' });
kitty.save().then(() => console.log('meow'));
~~~

##### 使用例子：设计scheme发布model

##### 以及增删改查

~~~JavaScript
var mongoose = require('mongoose')

var Schema = mongoose.Schema

// 1. 连接数据库
// 指定连接的数据库不需要存在，当你插入第一条数据之后就会自动被创建出来
mongoose.connect('mongodb://localhost/itcast')

// 2. 设计文档结构（表结构）
// 字段名称就是表结构中的属性名称
// 约束的目的是为了保证数据的完整性，不要有脏数据
var userSchema = new Schema({
  username: {
    type: String,
    required: true // 必须有
  },
  password: {
    type: String,
    required: true
  },
  email: {
    type: String
  }
})

// 3. 将文档结构发布为模型
//    mongoose.model 方法就是用来将一个架构发布为 model
//    第一个参数：传入一个大写名词单数字符串用来表示你的数据库名称
//                 mongoose 会自动将大写名词的字符串生成 小写复数 的集合名称
//                 例如这里的 User 最终会变为 users 集合名称
//    第二个参数：架构 Schema
//   
//    返回值：模型构造函数
var User = mongoose.model('User', userSchema)


// 4. 当我们有了模型构造函数之后，就可以使用这个构造函数对 users 集合中的数据为所欲为了（增删改查）
// **********************
// #region /新增数据
// **********************
// var admin = new User({
//   username: 'zs',
//   password: '123456',
//   email: 'admin@admin.com'
// })

// admin.save(function (err, ret) {
//   if (err) {
//     console.log('保存失败')
//   } else {
//     console.log('保存成功')
//     console.log(ret)
//   }
// })
// **********************
// #endregion /新增数据
// **********************




// **********************
// #region /查询数据
// **********************
查询所有
// User.find(function (err, ret) {
//   if (err) {
//     console.log('查询失败')
//   } else {
//     console.log(ret)
//   }
// })

按条件查询
// User.find({
//   username: 'zs'
// }, function (err, ret) {
//   if (err) {
//     console.log('查询失败')
//   } else {
//     console.log(ret)
//   }
// })

按条件查询单个
// User.findOne({
//   username: 'zs'
// }, function (err, ret) {
//   if (err) {
//     console.log('查询失败')
//   } else {
//     console.log(ret)
//   }
// })
// **********************
// #endregion /查询数据
// **********************



// **********************
// #region /删除数据
// **********************
// User.remove({
//   username: 'zs'
// }, function (err, ret) {
//   if (err) {
//     console.log('删除失败')
//   } else {
//     console.log('删除成功')
//     console.log(ret)
//   }
// })
// **********************
// #endregion /删除数据
// **********************


// **********************
// #region /更新数据
// **********************
// User.findByIdAndUpdate('5a001b23d219eb00c8581184', {
//   password: '123'
// }, function (err, ret) {
//   if (err) {
//     console.log('更新失败')
//   } else {
//     console.log('更新成功')
//   }
// })
// **********************
// #endregion /更新数据
// **********************

~~~

#### 使用node操作MySQL数据包

包下载：npm install --save mysql

~~~JavaScript
var mysql = require('mysql');

// 1. 创建连接
var connection = mysql.createConnection({
  host: 'localhost',
  user: 'root',
  password: 'root',
  database: 'users' // 对不起，我一不小心把数据库名字和表名起成一样的，你知道就行
});

// 2. 连接数据库 打开冰箱门
connection.connect();

// 3. 执行数据操作 把大象放到冰箱
connection.query('SELECT * FROM `users`', function (error, results, fields) {
  if (error) throw error;
  console.log('The solution is: ', results);
});

// connection.query('INSERT INTO users VALUES(NULL, "admin", "123456")', function (error, results, fields) {
//   if (error) throw error;
//   console.log('The solution is: ', results);
// });

// 4. 关闭连接 关闭冰箱门
connection.end();

~~~

#### promise

深入学习参考文档： http://es6.ruanyifeng.com/#docs/promise 

##### callback hell

无法保证顺序的代码：

~~~javascript
var fs = require('fs')

fs.readFile('./data/a.txt', 'utf8', function(err, data) {
    if (err) {
        // return console.log('读取失败')
        // 抛出异常
        //    1. 阻止程序的执行
        //    2. 把错误消息打印到控制台
        throw err
    }
    console.log(data)


})
fs.readFile('./data/b.txt', 'utf8', function(err, data) {
    if (err) {
        // return console.log('读取失败')
        // 抛出异常
        //    1. 阻止程序的执行
        //    2. 把错误消息打印到控制台
        throw err
    }
    console.log(data)
})
fs.readFile('./data/c.txt', 'utf8', function(err, data) {
    if (err) {
        // return console.log('读取失败')
        // 抛出异常
        //    1. 阻止程序的执行
        //    2. 把错误消息打印到控制台
        throw err
    }
    console.log(data)
})
~~~

通过回调嵌套的方式来保证顺序：

~~~JavaScript
var fs = require('fs')

fs.readFile('./data/a.txt', 'utf8', function (err, data) {
  if (err) {
    // return console.log('读取失败')
    // 抛出异常
    //    1. 阻止程序的执行
    //    2. 把错误消息打印到控制台
    throw err
  }
  console.log(data)
  fs.readFile('./data/b.txt', 'utf8', function (err, data) {
    if (err) {
      // return console.log('读取失败')
      // 抛出异常
      //    1. 阻止程序的执行
      //    2. 把错误消息打印到控制台
      throw err
    }
    console.log(data)
    fs.readFile('./data/c.txt', 'utf8', function (err, data) {
      if (err) {
        // return console.log('读取失败')
        // 抛出异常
        //    1. 阻止程序的执行
        //    2. 把错误消息打印到控制台
        throw err
      }
      console.log(data)
    })
  })
})

~~~

为了解决以上编码方式带来的问题（回调地狱嵌套），所以在ecmascript6中新增了一个api：promise。
    promise的英文就是承诺、保证的意思

容器概念：

![1573910053196](C:\Users\bbk\AppData\Roaming\Typora\typora-user-images\1573910053196.png)



 promiseAPI代码图示： 

![1573910303191](C:\Users\bbk\AppData\Roaming\Typora\typora-user-images\1573910303191.png)

~~~JavaScript                             
var fs = require('fs')

var p1 = new Promise(function (resolve, reject) {
  fs.readFile('./data/a.txt', 'utf8', function (err, data) {
    if (err) {
      reject(err)
    } else {
      resolve(data)
    }
  })
})

var p2 = new Promise(function (resolve, reject) {
  fs.readFile('./data/b.txt', 'utf8', function (err, data) {
    if (err) {
      reject(err)
    } else {
      resolve(data)
    }
  })
})

var p3 = new Promise(function (resolve, reject) {
  fs.readFile('./data/c.txt', 'utf8', function (err, data) {
    if (err) {
      reject(err)
    } else {
      resolve(data)
    }
  })
})

p1
  .then(function (data) {
    console.log(data)
    // 当 p1 读取成功的时候
    // 当前函数中 return 的结果就可以在后面的 then 中 function 接收到
    // 当你 return 123 后面就接收到 123
    //      return 'hello' 后面就接收到 'hello'
    //      没有 return 后面收到的就是 undefined
    // 上面那些 return 的数据没什么卵用
    // 真正有用的是：我们可以 return 一个 Promise 对象
    // 当 return 一个 Promise 对象的时候，后续的 then 中的 方法的第一个参数会作为 p2 的 resolve
    // 
    return p2
  }, function (err) {
    console.log('读取文件失败了', err)
  })
  .then(function (data) {
    console.log(data)
    return p3
  })
  .then(function (data) {
    console.log(data)
    console.log('end')
  })

~~~

![1573911200137](C:\Users\bbk\AppData\Roaming\Typora\typora-user-images\1573911200137.png)

 封装promiseAPI 

~~~JavaScript
var fs = require('fs')

function pReadFile(filePath) {
  return new Promise(function (resolve, reject) {
    fs.readFile(filePath, 'utf8', function (err, data) {
      if (err) {
        reject(err)
      } else {
        resolve(data)
      }
    })
  })
}

pReadFile('./data/a.txt')
  .then(function (data) {
    console.log(data)
    return pReadFile('./data/b.txt')
  })
  .then(function (data) {
    console.log(data)
    return pReadFile('./data/c.txt')
  })
  .then(function (data) {
    console.log(data)
  })

~~~

##### 运用场景

~~~JavaScript
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>

<body>
  <form action="00-js中的一等公民函数.js" id="user_form">
  </form>
  <script type="text/template" id="tpl">
    <div>
      <label for="">用户名</label>
      <input type="text" value="{{ user.username }}">
    </div>
    <div>
      <label for="">年龄</label>
      <input type="text" value="{{ user.age }}">
    </div>
    <div>
      <label for="">职业</label>
      <select name="" id="">
        {{ each jobs }} {{ if user.job === $value.id }}
        <option value="{{ $value.id }}" selected>{{ $value.name }}</option>
        {{ else }}
        <option value="{{ $value.id }}">{{ $value.name }}</option>
        {{ /if }} {{ /each }}
      </select>
    </div>
  </script>
  <script src="node_modules/art-template/lib/template-web.js"></script>
  <script src="node_modules/jquery/dist/jquery.js"></script>
  <script>
    // 用户表
    //  其中一个接口获取用户数据
    //  职业：2
    // 职业信息表
    //  其中一个接口获取所有的职业信息
    // get('http://127.0.0.1:3000/users/4', function (userData) {
    //   get('http://127.0.0.1:3000/jobs', function (jobsData) {
    //     var htmlStr = template('tpl', {
    //       user: JSON.parse(userData),
    //       jobs: JSON.parse(jobsData)
    //     })
    //     console.log(htmlStr)
    //     document.querySelector('#user_form').innerHTML = htmlStr
    //   })
    // })

    // var data = {}
    // $.get('http://127.0.0.1:3000/users/4')
    //   .then(function (user) {
    //     data.user = user
    //     return $.get('http://127.0.0.1:3000/jobs')
    //   })
    //   .then(function (jobs) {
    //     data.jobs = jobs
    //     var htmlStr = template('tpl', data)
    //     document.querySelector('#user_form').innerHTML = htmlStr
    //   })

    // var data = {}
    // pGet('http://127.0.0.1:3000/users/4')
    //   .then(function (user) {
    //     data.user = user
    //     return pGet('http://127.0.0.1:3000/jobs')
    //   })
    //   .then(function (jobs) {
    //     data.jobs = jobs
    //     var htmlStr = template('tpl', data)
    //     document.querySelector('#user_form').innerHTML = htmlStr
    //   })

    // pGet('http://127.0.0.1:3000/users/4', function (data) {
    //   console.log(data)
    // })

    pGet('http://127.0.0.1:3000/users/4')
      .then(function (data) {
        console.log(data)
      })

    function pGet(url, callback) {
      return new Promise(function (resolve, reject) {
        var oReq = new XMLHttpRequest()
        // 当请求加载成功之后要调用指定的函数
        oReq.onload = function () {
          // 我现在需要得到这里的 oReq.responseText
          callback && callback(JSON.parse(oReq.responseText))
          resolve(JSON.parse(oReq.responseText))
        }
        oReq.onerror = function (err) {
          reject(err)
        }
        oReq.open("get", url, true)
        oReq.send()
      })
    }


    // 这个 get 是 callback 方式的 API
    // 可以使用 Promise 来解决这个问题
    function get(url, callback) {
      var oReq = new XMLHttpRequest()
      // 当请求加载成功之后要调用指定的函数
      oReq.onload = function () {
        // 我现在需要得到这里的 oReq.responseText
        callback(oReq.responseText)
      }
      oReq.open("get", url, true)
      oReq.send()
    }
  </script>
</body>

</html>

~~~

##### mongoose所有的API都支持promise

~~~
User.findOne({
//     username: 'aaa'
//   })
//   .then(function (user) {
//     if (user) {
//       // 用户已存在，不能注册
//       console.log('用户已存在')
//     } else {
//       // 用户不存在，可以注册
//       return new User({
//         username: 'aaa',
//         password: '123',
//         email: 'dsadas'
//       }).save()
//     }
//   })
//   .then(function (ret) {
//   })

对比
User.findOne({ username: 'aaa' }, function (user) {
//   if (user) {
//     console.log('已存在')
//   } else {
//     new User({
//       username: 'aaa',
//       password: '123',
//       email: 'dsadas'
//     }).save(function () {
      
//     })
//   }
// })
~~~

#### art-template模板继承和子模板

.在项目中新建一个views目录
 views目录中让各个页面联合起来，使用art-template：

​    页面中有公共的头部和底部，views中新建header.html和footer.html写需要的头部和底部内容
  其他一些主页、列表页....等等都具有相似的内容所以在views中再新建一个layout.html作为被继承的页面，同时这个layout中就引入一些jQuery、bootstrip包等，这样只要继承了这个页面的页面都具有这些包了，不需要重复引入。

因此cmd中输入：npm i bootstrap jquery

layout被继承页面：

~~~
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <link rel="stylesheet" href="/node_modules/bootstrap/dist/css/bootstrap.css"> {{ block 'head' }}{{ /block }}
</head>

<body>
    {{ include './header.html'}}
    <!-- 留个坑，以后去填 -->
    {{ block 'content'}}
    <h1>默认内容</h1>
    {{ /block }} {{ include './footer.html'}}
    <script src="/node_modules/jquery/dist/jquery.js"></script>
    <script src="/node_modules/bootstrap/dist/js/bootstrap.js"></script>
    {{ block 'script' }}{{ /block }}
</body>

</html>
~~~

index主页继承layout页面语法例子：

~~~
{{extend './layout.html'}} {{ block 'head'}}
<style>
    body {
        background-color: skyblue;
    }
</style>
{{ /block }} {{ block 'content'}}
<div>
    <h1>index 页面填坑内容</h1>
</div>
{{ /block }} {{ block 'script'}}
<script>
    window.alert('index 页面自己的js脚本')
</script>
{{ /block }}
~~~

上面页面之间的联系只是使用例子，现在需要把整理好的页面放到相应位置

#### express中间件 middleware

http://expressjs.com/en/gulde/using-middleware.html

中间件的本质就是一个请求处理方式，我们把用户从请求到响应的整个过程分发到多个中间件中去处理，这样做的目的是提高代码的灵活性，动态可扩展。

- 同一个请求所经过的中间件都是同一个请求对象和响应对象

引用程序级别中间件：
    有两分类：一是万能匹配，不关心任何请求路径和请求方法、二是匹配以'/xxx/'开头的。

路由级别的中间件：
   有这几个分类：get、post、put、delete

~~~JavaScript
var express = require('express')

var app = express()

// 中间件：处理请求的，本质就是个函数

// 在 Express 中，对中间件有几种分类

// 当请求进来，会从第一个中间件开始进行匹配
//    如果匹配，则进来
//       如果请求进入中间件之后，没有调用 next 则代码会停在当前中间件
//       如果调用了 next 则继续向后找到第一个匹配的中间件
//    如果不匹配，则继续判断匹配下一个中间件
//    
// 不关心请求路径和请求方法的中间件
// 也就是说任何请求都会进入这个中间件
// 中间件本身是一个方法，该方法接收三个参数：
//    Request 请求对象
//    Response 响应对象
//    next     下一个中间件
// 当一个请求进入一个中间件之后，如果不调用 next 则会停留在当前中间件
// 所以 next 是一个方法，用来调用下一个中间件的
// 调用 next 方法也是要匹配的（不是调用紧挨着的那个）

// app.use(function (req, res, next) {
//   console.log('1')
//   next()
// })

// app.use(function (req, res, next) {
//   console.log('2')
//   next()
// })

// app.use(function (req, res, next) {
//   console.log('3')
//   res.send('333 end.')
// })

// app.use(function (req, res, next) {
//   console.log(1)
//   next()
// })


// app.use('/b', function (req, res, next) {
//   console.log('b')
// })

// // 以 /xxx 开头的路径中间件
// app.use('/a', function (req, res, next) {
//   console.log('a')
//   next()
// })

// app.use(function (req, res, next) {
//   console.log('2')
//   next()
// })

// app.use('/a', function (req, res, next) {
//   console.log('a 2')
// })

// 除了以上中间件之外，还有一种最常用的
// 严格匹配请求方法和请求路径的中间件
// app.get
// app.post

app.use(function (req, res, next) {
  console.log(1)
  next()
})

app.get('/abc', function (req, res, next) {
  console.log('abc')
  next()
})

app.get('/', function (req, res, next) {
  console.log('/')
  next()
})

app.use(function (req, res, next) {
  console.log('haha')
  next()
})

app.get('/abc', function (req, res, next) {
  console.log('abc 2')
})

app.use(function (req, res, next) {
  console.log(2)
  next()
})

app.get('/a', function (req, res, next) {
  console.log('/a')
})

app.get('/', function (req, res, next) {
  console.log('/ 2')
})

// 如果没有能匹配的中间件，则 Express 会默认输出：Cannot GET 路径

app.listen(3000, function () {
  console.log('app is running at port 3000.')
})



~~~



#### 配置一个全局错误处理中间件

~~~JavaScript
app.get('/', function (req, res, next) {
  fs.readFile('.d/sa./d.sa/.dsa', function (err, data) {
    if (err) {
      // return res.status(500).send('Server Error')
      // 当调用 next 的时候，如果传递了参数，则直接往后找到带有 四个参数的应用程序级别中间件
      // 当发生错误的时候，我们可以调用 next 传递错误对象
      // 然后就会被全局错误处理中间件匹配到并处理之
      next(err)
    }
  })
})
~~~



~~~JavaScript
// 配置错误处理中间件
app.use(function (err, req, res, next) {
  res.status(500).send(err.message)
})
~~~







404错误处理中间件：需要放在加载路由之后，也就是在app.use(router)之后

~~~
app.use(function (req, res, next) {
  res.send('404')
})
~~~

















