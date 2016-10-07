# 用JS代码操作 mongodb

我们主要基于一个JS库协助，** [mongoose](http://mongoosejs.com)** 。它可以作为一个 npm 包来安装。解释一下，一个js库就是一组js接口的集合。库英文为library。用图来解释mongoose的作用。
![](https://github.com/happypeter/digicity-express-api/blob/master/doc/img/002-mongoose.png?raw=true)

注：Demo是小案例


## 一、先写一个最简单的 ** express ** 小程序

1.index.js 代码如下：
```
  var express = require ('express');
  var app = express(); //保证app的路由功能

 // 下面这个就是路由代码，触发条件是前台（也就是浏览器）发送了相应的请求（request）
 // 如果前台发送的是 POST /posts
  app.post('/posts',function(req,res){
    console.log('hello world!')
  })

  app.listen(3000,function(){
    console.log('running on port 3000...plz visit http://localhost:3000')
  })

```

前台发送的请求可以使用curl来模拟
相应的curl测试命令是：

```
$ curl --request POST localhost:3000/posts
```
如果可以在运行 node index.js 的位置看到 hello 表示我们这一步胜利完成。
![](https://raw.githubusercontent.com/happypeter/digicity-express-api/master/doc/img/003-curl.png)
## 二、安装mongoose
```
$ npm install --save mongoose
```
作为一个npm 包进行安装
** [link](https://www.npmjs.com/package/mongoose)**
### 1.在js 代码中导入mongoose
```
var mongoose = require('mongoose')

```
### 2.进行数据库的链接
```
mongoose.connect('mongodb://localhost:27017/mongoose-save');
// mongoose-save 是数据库的名称， 数据库如果没有创建，则自动创建。
//mogoose.connect 接口用来连接我们系统上安装的 mongodb 数据库。

```
#### 如何定位数据库的位置
-  一种逻辑上可行的方案，就是用数据存储的文件夹的位置（比如我们前面采用的data/db文件夹），但是实际上 Mongodb 有其他方法
- mongodb 的软件，运行起来类似一个网站，用链接来访问。（mongodb://localhost:27017）
但是，链接之后，要更上数据库的名字。我们每次链接，都是链接到一个数据库。比如我们这里，就是mongoose-save，一般与项目名同名）
#### 如何验证连接成功

```
db.on('error', console.error.bind(console, 'connection error:'));
db.once('open', function () {
  console.log('success!')
});

```
看到输出`success`字样表示连接成功！
### 3.定义数据的概要 Schema
数据天然的具有一定的结构，比如，人员名单中，自然的会涉及姓名、年龄、籍贯等信息，在mongodb这里，一个人员的信息会被作为一条数据记录来保存。所有信息的类型会对应成字段名，由于是跟计算机打交道，每个字段还要涉及它的数据类型（num，string ……）。
那么Schema 就是用来规定一个记录的各个字段的，字段名+数据类型的。
```
var Schema = mongoose.Schema;
var PostSchema= new Schema(
    {
      title:String,
      content:String,
    }
  )
```
上面的代码，规定出了我们的记录能够保存哪些数据。

### 4.创建数据模型 model
数据库的结构是，一个数据库包含多个集合，一个集合会包含多条数据记录。 那么现在，我们数据要在哪个数据库中存？这个问题已经通过前面的`mongoose.connect(xxx)`的语句指定了。
但是数据要保存到哪个集合还没有指定。所以我们的model 创建语句如下：

```
var Post = mongoose.model('post',PostSchema);

```
上面`.model()`的第一个参数，`post`就为我们指定了集合的名字，会对数据库中的posts这个集合，第二个参数是数据schema,就是前面我们定义的。
到这里，所有数据存储的基础设施全部就绪。

### 5.实例化model得到数据对象
现在我们要把实际要存储的数据，放到一个model 的实例（对象）之中了。

```
var post = new Post({title:'myTitle',content:'myContent'})

```


### 6.对象之上呼叫save()
```
post.save(function(){
  console.log('saved')
  })

```
save()是异步函数，需要使用回调函数
就可以把数据保存到数据库之中了。
