---
title: 前端接口神器之JsonServer详细指南
date: 2025-08-17
description: 前端接口神器之 Json Server 详细指南
tags: [React]
categories: [Web前端, React]
---
# 前端接口神器之 Json Server 详细指南

## 简介

**json-server** 是一款小巧的接口模拟工具，一分钟内就能搭建一套 Restful 风格的 api，尤其适合前端接口测试使用。🔥🔥🔥\ 只需指定一个 **json** 文件作为 **api** 的数据源即可，使用起来非常方便，基本上有手就行。👍

### 开源地址

主页地址：<https://www.npmjs.com/package/json-server>\ Github项目地址：<https://github.com/typicode/json-server>

## 入门

### 环境依赖

* 安装 Node.js 环境即可

### 操作步骤

1. 安装 JSON 服务器

<code>npm install -g json-server </code>

2. 创建一个**db.json**包含一些数据的文件

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

3. 启动 **json-server** 接口服务器

<code>json-server --watch db.json </code>

4. 浏览器访问 `http://localhost:3000/posts/1`，你会得到

<code>{ "id": 1, "title": "json-server", "author": "typicode" } </code>

### 补充

* 如果您发出 POST、PUT、PATCH 或 DELETE 请求，更改将自动安全地保存到 **db.json** 文件中。
* 注意 Id 值是不可变的。

## 路由

根据之前的**db.json**文件，这里是所有的默认路由。

### 路由形式一

```bash
GET    /posts
GET    /posts/1
POST   /posts
PUT    /posts/1
PATCH  /posts/1
DELETE /posts/1
```

### 路由形式二

```bash
GET    /profile
POST   /profile
PUT    /profile
PATCH  /profile
```

### 筛选

使用 **.** 访问筛选

```bash
GET /posts?title=json-server&author=typicode
GET /posts?id=1&id=2
GET /comments?author.name=typicode
```

### 分页

使用**\_page**和可选地**\_limit**对返回的数据进行分页。

在**Link**标题，你会得到**first**，**prev**，**next**和**last**链接。

```bash
GET /posts?_page=7
GET /posts?_page=7&_limit=20
```

*默认返回10项*

### 排序

添加**\_sort**和**\_order**（默认升序）

```bash
GET /posts?_sort=views&_order=asc
GET /posts/1/comments?_sort=votes&_order=asc
```

对于多个字段，请使用以下格式：

```bash
GET /posts?_sort=user,views&_order=desc,asc
```

### 切片(分页)

添加**\_start**和**\_end**或**\_limit**

```bash
GET /posts?_start=20&_end=30
GET /posts/1/comments?_start=20&_end=30
GET /posts/1/comments?_start=20&_limit=10
```

*与*[Array.slice](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)*完全一样*[工作](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)*（即****\_start****开始****\_end****结束）*

### 特殊符号

添加**\_gte**或**\_lte**获取范围

```bash
GET /posts?views_gte=10&views_lte=20
```

添加**\_ne**以排除值

```bash
GET /posts?id_ne=1
```

添加**\_like**到过滤器（支持正则表达式）

```bash
GET /posts?title_like=server 
```

### 全文搜索

添加 **q**

```bash
GET /posts?q=internet 
```

### 关系

要包含子资源，请添加 **\_embed**

```bash
GET /posts?_embed=comments
GET /posts/1?_embed=comments
```

要包含父资源，请添加 **\_expand**

```bash
GET /comments?_expand=post
GET /comments/1?_expand=post
```

获取或创建嵌套资源（默认为一级）

```bash
GET  /posts/1/comments
POST /posts/1/comments
```

### 数据库

```bash
GET /db 
```

### 主页

返回默认索引文件或服务**./public**目录

```sql
GET / 
```

## 附加功能

### 静态文件服务器

您可以使用 JSON Server 为您的 HTML、JS 和 CSS 提供服务，只需创建一个**./public**目录或用于**--static**设置不同的静态文件目录。

```bash
mkdir public
echo 'hello world' > public/index.html
json-server db.json
```

```basic
json-server db.json --static ./some-other-dir
```

### 替换端口

您可以使用以下**--port**标志在其他端口上启动 **JSON Server** ：

```bash
$ json-server --watch db.json --port 3004
```

### 支持跨域

您可以使用 CORS 和 JSONP 从任何地方访问您模拟的 API 接口。

### 远程模式

您可以加载远程模式。

```bash
$ json-server http://example.com/file.json
$ json-server http://jsonplaceholder.typicode.com/db
```

### 生成随机数据

使用 JS 而不是 JSON 文件，您可以通过编程方式创建数据。

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



**提示**：使用[Faker](https://github.com/Marak/faker.js)、[Casual](https://github.com/boo1ean/casual)、[Chance](https://github.com/victorquinn/chancejs)或[JSON Schema Faker 等模块](https://github.com/json-schema-faker/json-schema-faker)。

### 添加自定义路由

创建一个**routes.json**文件。注意每条路线都以**/**.

```json
{
  "/api/*": "/$1",
  "/:resource/:id/show": "/:resource/:id",
  "/posts/:category": "/posts?category=:category",
  "/articles\\?id=:id": "/posts/:id"
}
```

使用**--routes**选项启动 JSON 服务器。

```bash
json-server db.json --routes routes.json 
```

现在您可以使用其他路线访问资源。

```bash
/api/posts # → /posts
/api/posts/1  # → /posts/1
/posts/1/show # → /posts/1
/posts/javascript # → /posts?category=javascript
/articles?id=1 # → /posts/1
```

### 添加中间件

您可以使用以下**--middlewares**选项从 CLI 添加中间件：

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

### 命令行使用

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

您还可以在**json-server.json**配置文件中设置选项。

```json
{
  "port": 3000
}
```

### 模块

如果您需要添加身份验证、验证或**任何行为**，您可以将项目作为模块与其他 Express 中间件结合使用。

#### 简单的例子

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

您提供给**jsonServer.router**函数的路径是相对于您启动节点进程的目录的。如果从另一个目录运行上述代码，最好使用绝对路径：

```javascript
const path = require('path')
const router = jsonServer.router(path.join(__dirname, 'db.json'))
```

对于内存数据库，只需将对象传递给**jsonServer.router()**.

另请注意，**jsonServer.router()**它可用于现有的 Express 项目。

#### 自定义路由示例

假设您想要一个回显查询参数的路由和另一个在创建的每个资源上设置时间戳的路由。

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

#### 访问控制示例

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

#### 自定义输出示例

要修改响应，请覆盖**router.render**方法：

```javascript
// 在这个例子中，返回的资源将被包装在一个 body 属性
router.render = (req, res) => {
  res.jsonp({
    body: res.locals.data
  })
}
```

您可以为响应设置自己的状态代码：

```javascript
// 在这个例子中，我们模拟了一个服务器端错误响应
router.render = (req, res) => {
  res.status(500).jsonp({
    error: "error message here"
  })
}
```

#### 重写器示例

要添加重写规则，请使用**jsonServer.rewriter()**：

```javascript
// 写在 server.use(router) 之前
server.use(jsonServer.rewriter({
  '/api/*': '/$1',
  '/blog/:resource/:id/show': '/:resource/:id'
}))
```

#### 在另一个端点上挂载 JSON 服务器示例

或者，您也可以将路由器安装在**/api**.

```javascript
server.use('/api', router)
```

#### API

<code>**jsonServer.create()**</code>

返回一个 Express 服务器。

<code>**jsonServer.defaults([options])**</code>

返回 JSON 服务器使用的中间件。

* 选项
  * **static** 静态文件的路径
  * **logger** 启用记录器中间件（默认值：true）
  * **bodyParser** 启用 body-parser 中间件（默认值：true）
  * **noCors** 禁用 CORS（默认值：false）
  * **readOnly** 只接受 GET 请求（默认值：false）

<code>**jsonServer.router([path|object])**</code>

返回 JSON 服务器路由器。


