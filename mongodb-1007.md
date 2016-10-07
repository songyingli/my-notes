# Mongodb数据库基本操作

## 一、启动
 - 创建文件夹
 ```
 $ mkdir -p data/db
 ```

 "-p":参数的作用是同时创建父文件夹和子文件夹，多级文件夹

  执行上面命令后是在创建的data文件夹中再创建db文件夹

- 启动数据库
 ```
 $ mongod --dbpath=./data/db
 ```
 --dbpath后的值表示数据库文件的存储路径,而且后面的路径必须事先创建好，必须已经存在，否则服务开启失败

## 二、mongodb启动操作界面

 - 用户图形接口 **GUI**

 - 命令行接口 **CLI**

 对于 mongodb 我们使用 mongo shell 这个命令行来操作，启动mongo shell 的形式是：
 ```
 $ mongo
 ```

## 三、创建一个数据库

### 1.开启Mongo shell命令行

  ```
  $ mongo
  ```

### 2.创建一个数据库

```
$ use digicity-express-api
```

注：如果此数据库存在，则切换到此数据库下,如果此数据库还不存在也可以切过来

数据库是mongodb中的顶级存储单位，之下一级是 **集合**
### 3.创建一个集合

 ```
 $ db.createCollection('posts')
 ```
### 4.插入数据记录
一个集合（例如：posts），里面可以插入多个数据记录
```
$ db.posts.insert({title:'myTitle',content:'myContent'})
```
### 5.查看集合中的数据记录
```
$ db.posts.find()

```
