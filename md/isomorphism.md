# 使用NODE作为中间件替代PHP层的实现方案

## 流程对比图:

![Alt text](../image/coupling.png)
###### 基于PHP的实现方式
------------
![Alt text](../image/separation.png)
###### 基于nodeServer的实现方式
-------------
## node层代码实现
### 目前提供四块功能：server(日志、响应码、安全策略等)， route， HTML静态模板与JSON API， token缓存。

### 1. server
#### 由express framework 提供，该框架是node 层核心框架通过增加middleware完善功能，主要middleware有
* helmet：安全策略
* compression：request处理
* wrapRender：提供默认数据
* logger：日志服务
* bodyParser 预处理body
* cookieParser 预处理cookie

`app.use(shareRoot(config.basename))
 app.use(helmet())
 app.use(compression())
 // app.use(favicon(path.join(__dirname, '../static/favicon.ico')))

 // view engine setup
 app.set('views', path.join(__dirname, '../routes'))
 app.set('view engine', 'ejs');

 app.use(
 	wrapRender({
 		defaults: config
 	})
 )

 app.use(logger('dev'))
 app.use(bodyParser.json({
 	limit: '10MB'
 }))
 app.use(
 	bodyParser.urlencoded({
 		extended: false
 	})
 )`

### 2. route
#### 由express提供，属于middleware的一种。按业务类型分三种
* 静态资源：直接返回对应目录下文件
* 需要动态组装的HTML：由模板引擎进行组装
* JSON：页面需要异步获取的信息等

*todo： 根据具体业务制定route层级*

### 3.HTML 模板与模板cache
#### demo中使用ejs,为了达到同构效果，按照当前我们前端模式，使用vue.
*todo： 生成HTML的接口的实现，模板cache的实现（区分dev与production模式）*

### 4.token
#### 使用Memcached框架。