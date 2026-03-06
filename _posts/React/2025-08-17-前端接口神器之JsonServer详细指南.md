---
title: 前端接口神器之JsonServer详细指南
date: 2025-08-17
description: 前端接口神器之 Json Server 详细指南
tags: [React]
categories: [React]
---
# 前端接口神器之 Json Server 详细指南

## <font style="color:rgb(35, 38, 59);">简介</font>

**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">json-server</font>**<font style="color:rgb(35, 38, 59);"> 是一款小巧的接口模拟工具，一分钟内就能搭建一套 Restful 风格的 api，尤其适合前端接口测试使用。</font><font style="color:rgb(35, 38, 59);">🔥🔥🔥</font><font style="color:rgb(35, 38, 59);">\ </font><font style="color:rgb(35, 38, 59);">只需指定一个 </font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">json</font>**<font style="color:rgb(35, 38, 59);"> 文件作为 </font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">api</font>**<font style="color:rgb(35, 38, 59);"> 的数据源即可，使用起来非常方便，基本上有手就行。</font><font style="color:rgb(35, 38, 59);">👍</font>

### <font style="color:rgb(35, 38, 59);">开源地址</font>

<font style="color:rgb(35, 38, 59);">主页地址：</font><https://www.npmjs.com/package/json-server><font style="color:rgb(35, 38, 59);">\ </font><font style="color:rgb(35, 38, 59);">Github项目地址：</font><https://github.com/typicode/json-server>

## <font style="color:rgb(35, 38, 59);">入门</font>

### <font style="color:rgb(35, 38, 59);">环境依赖</font>

* <font style="color:rgb(35, 38, 59);">安装 Node.js 环境即可</font>

### <font style="color:rgb(35, 38, 59);">操作步骤</font>

1. <font style="color:rgb(35, 38, 59);">安装 JSON 服务器</font>

<code><font style="color:rgb(35, 38, 59);">npm install -g json-server </font></code>

2. <font style="color:rgb(35, 38, 59);">创建一个</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">db.json</font>**<font style="color:rgb(35, 38, 59);">包含一些数据的文件</font>

```json
{
  "posts": [
    { "id": 1, "title": "json-server", "author": "typicode" }
  ],
  "comments": [
    { "id": 1, "body": "some comment", "postId": 1 }
  ],
  "profile": { "name": "typicode" }
}
```

3. <font style="color:rgb(35, 38, 59);">启动 </font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">json-server</font>**<font style="color:rgb(35, 38, 59);"> 接口服务器</font>

<code><font style="color:rgb(35, 38, 59);">json-server --watch db.json </font></code>

4. <font style="color:rgb(35, 38, 59);">浏览器访问 </font><http://localhost:3000/posts/1><font style="color:rgb(35, 38, 59);">，你会得到</font>

<code><font style="color:rgb(35, 38, 59);">{ </font><font style="color:rgb(255, 0, 0);">"id"</font><font style="color:rgb(35, 38, 59);">: </font><font style="color:rgb(136, 0, 0);">1</font><font style="color:rgb(35, 38, 59);">, </font><font style="color:rgb(255, 0, 0);">"title"</font><font style="color:rgb(35, 38, 59);">: </font><font style="color:rgb(163, 21, 21);">"json-server"</font><font style="color:rgb(35, 38, 59);">, </font><font style="color:rgb(255, 0, 0);">"author"</font><font style="color:rgb(35, 38, 59);">: </font><font style="color:rgb(163, 21, 21);">"typicode"</font><font style="color:rgb(35, 38, 59);"> } </font></code>

### <font style="color:rgb(35, 38, 59);">补充</font>

* <font style="color:rgb(35, 38, 59);">如果您发出 POST、PUT、PATCH 或 DELETE 请求，更改将自动安全地保存到</font><font style="color:rgb(35, 38, 59);"> </font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">db.json</font>**<font style="color:rgb(35, 38, 59);"> </font><font style="color:rgb(35, 38, 59);">文件中。</font>
* <font style="color:rgb(35, 38, 59);">注意 Id 值是不可变的。</font>

## <font style="color:rgb(35, 38, 59);">路由</font>

<font style="color:rgb(35, 38, 59);">根据之前的</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">db.json</font>**<font style="color:rgb(35, 38, 59);">文件，这里是所有的默认路由。</font>

### <font style="color:rgb(35, 38, 59);">路由形式一</font>

```bash
GET    /posts
GET    /posts/1
POST   /posts
PUT    /posts/1
PATCH  /posts/1
DELETE /posts/1
```

### <font style="color:rgb(35, 38, 59);">路由形式二</font>

```bash
GET    /profile
POST   /profile
PUT    /profile
PATCH  /profile
```

### <font style="color:rgb(35, 38, 59);">筛选</font>

<font style="color:rgb(35, 38, 59);">使用</font><font style="color:rgb(35, 38, 59);"> </font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">.</font>**<font style="color:rgb(35, 38, 59);"> </font><font style="color:rgb(35, 38, 59);">访问筛选</font>

```bash
GET /posts?title=json-server&author=typicode
GET /posts?id=1&id=2
GET /comments?author.name=typicode
```

### <font style="color:rgb(35, 38, 59);">分页</font>

<font style="color:rgb(35, 38, 59);">使用</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">\_page</font>**<font style="color:rgb(35, 38, 59);">和可选地</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">\_limit</font>**<font style="color:rgb(35, 38, 59);">对返回的数据进行分页。</font>

<font style="color:rgb(35, 38, 59);">在</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">Link</font>**<font style="color:rgb(35, 38, 59);">标题，你会得到</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">first</font>**<font style="color:rgb(35, 38, 59);">，</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">prev</font>**<font style="color:rgb(35, 38, 59);">，</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">next</font>**<font style="color:rgb(35, 38, 59);">和</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">last</font>**<font style="color:rgb(35, 38, 59);">链接。</font>

```bash
GET /posts?_page=7
GET /posts?_page=7&_limit=20
```

*<font style="color:rgb(35, 38, 59);">默认返回10项</font>*

### <font style="color:rgb(35, 38, 59);">排序</font>

<font style="color:rgb(35, 38, 59);">添加</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">\_sort</font>**<font style="color:rgb(35, 38, 59);">和</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">\_order</font>**<font style="color:rgb(35, 38, 59);">（默认升序）</font>

```bash
GET /posts?_sort=views&_order=asc
GET /posts/1/comments?_sort=votes&_order=asc
```

<font style="color:rgb(35, 38, 59);">对于多个字段，请使用以下格式：</font>

```bash
GET /posts?_sort=user,views&_order=desc,asc
```

### <font style="color:rgb(35, 38, 59);">切片(分页)</font>

<font style="color:rgb(35, 38, 59);">添加</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">\_start</font>**<font style="color:rgb(35, 38, 59);">和</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">\_end</font>**<font style="color:rgb(35, 38, 59);">或</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">\_limit</font>**

```bash
GET /posts?_start=20&_end=30
GET /posts/1/comments?_start=20&_end=30
GET /posts/1/comments?_start=20&_limit=10
```

*<font style="color:rgb(35, 38, 59);">与</font>*[Array.slice](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)*<font style="color:rgb(35, 38, 59);">完全一样</font>*[工作](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)*<font style="color:rgb(35, 38, 59);">（即</font>****<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">\_start</font>****<font style="color:rgb(35, 38, 59);">开始</font>****<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">\_end</font>****<font style="color:rgb(35, 38, 59);">结束）</font>*

### <font style="color:rgb(35, 38, 59);">特殊符号</font>

<font style="color:rgb(35, 38, 59);">添加</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">\_gte</font>**<font style="color:rgb(35, 38, 59);">或</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">\_lte</font>**<font style="color:rgb(35, 38, 59);">获取范围</font>

```bash
GET /posts?views_gte=10&views_lte=20
```

<font style="color:rgb(35, 38, 59);">添加</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">\_ne</font>**<font style="color:rgb(35, 38, 59);">以排除值</font>

```bash
GET /posts?id_ne=1
```

<font style="color:rgb(35, 38, 59);">添加</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">\_like</font>**<font style="color:rgb(35, 38, 59);">到过滤器（支持正则表达式）</font>

```bash
GET /posts?title_like=server 
```

### <font style="color:rgb(35, 38, 59);">全文搜索</font>

<font style="color:rgb(35, 38, 59);">添加</font><font style="color:rgb(35, 38, 59);"> </font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">q</font>**

```bash
GET /posts?q=internet 
```

### <font style="color:rgb(35, 38, 59);">关系</font>

<font style="color:rgb(35, 38, 59);">要包含子资源，请添加</font><font style="color:rgb(35, 38, 59);"> </font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">\_embed</font>**

```bash
GET /posts?_embed=comments
GET /posts/1?_embed=comments
```

<font style="color:rgb(35, 38, 59);">要包含父资源，请添加</font><font style="color:rgb(35, 38, 59);"> </font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">\_expand</font>**

```bash
GET /comments?_expand=post
GET /comments/1?_expand=post
```

<font style="color:rgb(35, 38, 59);">获取或创建嵌套资源（默认为一级）</font>

```bash
GET  /posts/1/comments
POST /posts/1/comments
```

### <font style="color:rgb(35, 38, 59);">数据库</font>

```bash
GET /db 
```

### <font style="color:rgb(35, 38, 59);">主页</font>

<font style="color:rgb(35, 38, 59);">返回默认索引文件或服务</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">./public</font>**<font style="color:rgb(35, 38, 59);">目录</font>

```sql
GET / 
```

## <font style="color:rgb(35, 38, 59);">附加功能</font>

### <font style="color:rgb(35, 38, 59);">静态文件服务器</font>

<font style="color:rgb(35, 38, 59);">您可以使用 JSON Server 为您的 HTML、JS 和 CSS 提供服务，只需创建一个</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">./public</font>**<font style="color:rgb(35, 38, 59);">目录或用于</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">--static</font>**<font style="color:rgb(35, 38, 59);">设置不同的静态文件目录。</font>

```bash
mkdir public
echo 'hello world' > public/index.html
json-server db.json
```

```basic
json-server db.json --static ./some-other-dir
```

### <font style="color:rgb(35, 38, 59);">替换端口</font>

<font style="color:rgb(35, 38, 59);">您可以使用以下</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">--port</font>**<font style="color:rgb(35, 38, 59);">标志在其他端口上启动</font><font style="color:rgb(35, 38, 59);"> </font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">JSON Server</font>**<font style="color:rgb(35, 38, 59);"> </font><font style="color:rgb(35, 38, 59);">：</font>

```bash
$ json-server --watch db.json --port 3004
```

### <font style="color:rgb(35, 38, 59);">支持跨域</font>

<font style="color:rgb(35, 38, 59);">您可以使用 CORS 和 JSONP 从任何地方访问您模拟的 API 接口。</font>

### <font style="color:rgb(35, 38, 59);">远程模式</font>

<font style="color:rgb(35, 38, 59);">您可以加载远程模式。</font>

```bash
$ json-server http://example.com/file.json
$ json-server http://jsonplaceholder.typicode.com/db
```

### <font style="color:rgb(35, 38, 59);">生成随机数据</font>

<font style="color:rgb(35, 38, 59);">使用 JS 而不是 JSON 文件，您可以通过编程方式创建数据。</font>

```javascript
// index.js
module.exports = () => {
  const data = { users: [] }
  // 创建 1000 个用户信息
  for (let i = 0; i < 1000; i++) {
    data.users.push({ id: i, name: `user${i}` })
  }
  return data
}
```

```bash
$ json-server index.js 
```

<font style="color:rgb(35, 38, 59);"></font>

**<font style="color:rgb(35, 38, 59);">提示</font>**<font style="color:rgb(35, 38, 59);">：使用</font>[Faker](https://github.com/Marak/faker.js)<font style="color:rgb(35, 38, 59);">、</font>[Casual](https://github.com/boo1ean/casual)<font style="color:rgb(35, 38, 59);">、</font>[Chance](https://github.com/victorquinn/chancejs)<font style="color:rgb(35, 38, 59);">或</font>[JSON Schema Faker 等模块](https://github.com/json-schema-faker/json-schema-faker)<font style="color:rgb(35, 38, 59);">。</font>

### <font style="color:rgb(35, 38, 59);">添加自定义路由</font>

<font style="color:rgb(35, 38, 59);">创建一个</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">routes.json</font>**<font style="color:rgb(35, 38, 59);">文件。注意每条路线都以</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">/</font>**<font style="color:rgb(35, 38, 59);">.</font>

```json
{
  "/api/*": "/$1",
  "/:resource/:id/show": "/:resource/:id",
  "/posts/:category": "/posts?category=:category",
  "/articles\\?id=:id": "/posts/:id"
}
```

<font style="color:rgb(35, 38, 59);">使用</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">--routes</font>**<font style="color:rgb(35, 38, 59);">选项启动 JSON 服务器。</font>

```bash
json-server db.json --routes routes.json 
```

<font style="color:rgb(35, 38, 59);">现在您可以使用其他路线访问资源。</font>

```bash
/api/posts # → /posts
/api/posts/1  # → /posts/1
/posts/1/show # → /posts/1
/posts/javascript # → /posts?category=javascript
/articles?id=1 # → /posts/1
```

### <font style="color:rgb(35, 38, 59);">添加中间件</font>

<font style="color:rgb(35, 38, 59);">您可以使用以下</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">--middlewares</font>**<font style="color:rgb(35, 38, 59);">选项从 CLI 添加中间件：</font>

```javascript
// hello.js
module.exports = (req, res, next) => {
  res.header('X-Hello', 'World')
  next()
}
```

```bash
json-server db.json --middlewares ./hello.js
json-server db.json --middlewares ./first.js ./second.js
```

### <font style="color:rgb(35, 38, 59);">命令行使用</font>

```javascript
json-server [options] <source>

Options:
  --config, -c       Path to config file           [default: "json-server.json"]
  --port, -p         Set port                                    [default: 3000]
  --host, -H         Set host                             [default: "localhost"]
  --watch, -w        Watch file(s)                                     [boolean]
  --routes, -r       Path to routes file
  --middlewares, -m  Paths to middleware files                           [array]
  --static, -s       Set static files directory
  --read-only, --ro  Allow only GET requests                           [boolean]
  --no-cors, --nc    Disable Cross-Origin Resource Sharing             [boolean]
  --no-gzip, --ng    Disable GZIP Content-Encoding                     [boolean]
  --snapshots, -S    Set snapshots directory                      [default: "."]
  --delay, -d        Add delay to responses (ms)
  --id, -i           Set database id property (e.g. _id)         [default: "id"]
  --foreignKeySuffix, --fks  Set foreign key suffix, (e.g. _id as in post_id)
                                                                 [default: "Id"]
  --quiet, -q        Suppress log messages from output                 [boolean]
  --help, -h         Show help                                         [boolean]
  --version, -v      Show version number                               [boolean]

Examples:
  json-server db.json
  json-server file.js
  json-server http://example.com/db.json

https://github.com/typicode/json-server
```

<font style="color:rgb(35, 38, 59);">您还可以在</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">json-server.json</font>**<font style="color:rgb(35, 38, 59);">配置文件中设置选项。</font>

```json
{
  "port": 3000
}
```

### <font style="color:rgb(35, 38, 59);">模块</font>

<font style="color:rgb(35, 38, 59);">如果您需要添加身份验证、验证或</font>**<font style="color:rgb(35, 38, 59);">任何行为</font>**<font style="color:rgb(35, 38, 59);">，您可以将项目作为模块与其他 Express 中间件结合使用。</font>

#### <font style="color:rgb(35, 38, 59);">简单的例子</font>

```bash
$ npm install json-server --save-dev
```

```javascript
// server.js
const jsonServer = require('json-server')
const server = jsonServer.create()
const router = jsonServer.router('db.json')
const middlewares = jsonServer.defaults()

server.use(middlewares)
server.use(router)
server.listen(3000, () => {
  console.log('JSON Server is running')
})
```

```bash
$ node server.js 
```

<font style="color:rgb(35, 38, 59);">您提供给</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">jsonServer.router</font>**<font style="color:rgb(35, 38, 59);">函数的路径是相对于您启动节点进程的目录的。如果从另一个目录运行上述代码，最好使用绝对路径：</font>

```javascript
const path = require('path')
const router = jsonServer.router(path.join(__dirname, 'db.json'))
```

<font style="color:rgb(35, 38, 59);">对于内存数据库，只需将对象传递给</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">jsonServer.router()</font>**<font style="color:rgb(35, 38, 59);">.</font>

<font style="color:rgb(35, 38, 59);">另请注意，</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">jsonServer.router()</font>**<font style="color:rgb(35, 38, 59);">它可用于现有的 Express 项目。</font>

#### <font style="color:rgb(35, 38, 59);">自定义路由示例</font>

<font style="color:rgb(35, 38, 59);">假设您想要一个回显查询参数的路由和另一个在创建的每个资源上设置时间戳的路由。</font>

```javascript
const jsonServer = require('json-server')
const server = jsonServer.create()
const router = jsonServer.router('db.json')
const middlewares = jsonServer.defaults()

// 设置默认中间件（记录器、静态、cors 和无缓存）
server.use(middlewares)

// 写在自定义路由之前
server.get('/echo', (req, res) => {
  res.jsonp(req.query)
})

// 要处理 POST、PUT 和 PATCH，您需要使用 body-parser
// 您可以使用 JSON Server
server.use(jsonServer.bodyParser)
server.use((req, res, next) => {
  if (req.method === 'POST') {
    req.body.createdAt = Date.now()
  }
  // 继续到 JSON Server 路由器
  next()
})

// 使用默认路由器
server.use(router)
server.listen(3000, () => {
  console.log('JSON Server is running')
})
```

#### <font style="color:rgb(35, 38, 59);">访问控制示例</font>

```javascript
const jsonServer = require('json-server')
const server = jsonServer.create()
const router = jsonServer.router('db.json')
const middlewares = jsonServer.defaults()

server.use(middlewares)
server.use((req, res, next) => {
 if (isAuthorized(req)) { // 在此处添加您的授权逻辑
   next() // 继续到 JSON Server 路由器
 } else {
   res.sendStatus(401)
 }
})
server.use(router)
server.listen(3000, () => {
  console.log('JSON Server is running')
})
```

#### <font style="color:rgb(35, 38, 59);">自定义输出示例</font>

<font style="color:rgb(35, 38, 59);">要修改响应，请覆盖</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">router.render</font>**<font style="color:rgb(35, 38, 59);">方法：</font>

```javascript
// 在这个例子中，返回的资源将被包装在一个 body 属性
router.render = (req, res) => {
  res.jsonp({
    body: res.locals.data
  })
}
```

<font style="color:rgb(35, 38, 59);">您可以为响应设置自己的状态代码：</font>

```javascript
// 在这个例子中，我们模拟了一个服务器端错误响应
router.render = (req, res) => {
  res.status(500).jsonp({
    error: "error message here"
  })
}
```

#### <font style="color:rgb(35, 38, 59);">重写器示例</font>

<font style="color:rgb(35, 38, 59);">要添加重写规则，请使用</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">jsonServer.rewriter()</font>**<font style="color:rgb(35, 38, 59);">：</font>

```javascript
// 写在 server.use(router) 之前
server.use(jsonServer.rewriter({
  '/api/*': '/$1',
  '/blog/:resource/:id/show': '/:resource/:id'
}))
```

#### <font style="color:rgb(35, 38, 59);">在另一个端点上挂载 JSON 服务器示例</font>

<font style="color:rgb(35, 38, 59);">或者，您也可以将路由器安装在</font>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">/api</font>**<font style="color:rgb(35, 38, 59);">.</font>

```javascript
server.use('/api', router)
```

#### <font style="color:rgb(35, 38, 59);">API</font>

<code>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">jsonServer.create()</font>**</code>

<font style="color:rgb(35, 38, 59);">返回一个 Express 服务器。</font>

<code>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">jsonServer.defaults([options])</font>**</code>

<font style="color:rgb(35, 38, 59);">返回 JSON 服务器使用的中间件。</font>

* <font style="color:rgb(35, 38, 59);">选项</font>
  * **<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">static</font>**<font style="color:rgb(35, 38, 59);"> </font><font style="color:rgb(35, 38, 59);">静态文件的路径</font>
  * **<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">logger</font>**<font style="color:rgb(35, 38, 59);"> </font><font style="color:rgb(35, 38, 59);">启用记录器中间件（默认值：true）</font>
  * **<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">bodyParser</font>**<font style="color:rgb(35, 38, 59);"> </font><font style="color:rgb(35, 38, 59);">启用 body-parser 中间件（默认值：true）</font>
  * **<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">noCors</font>**<font style="color:rgb(35, 38, 59);"> </font><font style="color:rgb(35, 38, 59);">禁用 CORS（默认值：false）</font>
  * **<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">readOnly</font>**<font style="color:rgb(35, 38, 59);"> </font><font style="color:rgb(35, 38, 59);">只接受 GET 请求（默认值：false）</font>

<code>**<font style="color:rgb(216, 59, 100);background-color:rgb(249, 242, 244);">jsonServer.router([path|object])</font>**</code>

<font style="color:rgb(35, 38, 59);">返回 JSON 服务器路由器。</font>


> 更新: 2024-07-11 13:31:48  
> 原文: <https://www.yuque.com/hutaoao/blog/mp3i7l>