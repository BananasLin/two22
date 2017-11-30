# two22

# Two
# mongodb鉴权11111
```
问题：mongodb中，什么用户能够使用mongodb，哪些用户能对哪些数据库进行操作，能看到某个数据库的哪些内容？
引出：鉴权
```
- mongodb的鉴权
- 设置单台mongodb服务器鉴权
#
## mongodb的鉴权
MongoDB 缺省是没有设置鉴权的，业界大部分使用 MongoDB 的项目也没有设置访问权限。这就意味着只要知道 MongoDB 服务器的端口，任何能访问到这台服务器的人都可以查询和操作 MongoDB 数据库的内容。在一些项目当中，这种使用方式会被看成是一种安全漏洞。
## 设置单台mongodb服务器鉴权
一旦设置了鉴权，mongodb客户端必须用正确的用户名和密码登录，才能在指定的数据库中操作。
- mongodb用户和权限
- 如何创建用户管理员
- 如何创建数据库用户
### mongodb用户和权限
- 每个数据库都有自己的用户，创建用户的命令是`db.createUser()`
- 每个用户包含三个要素：用户名、密码、角色
```
{
    user: "dbuser",
    pwd : "dbpass",
    roles: ["readWrite", "clusterAdmin"]
}
```
- 注意，admin数据库含有独特的角色
- 进行鉴权检查来开启mongodb，在运行mongodb的命令后追加`--auth`
```
mongod --dbpath ./db1 --port 20000 --auth
```
### 如何创建用户管理员
- 用户管理员的角色名：`userAdminAnyDatabase`，只能在admin数据库中创建，例子如下
```
> use admin
switched to db admin
> db.createUser({user:"root",pwd:"root123",roles:["userAdminAnyDatabase"]})
Successfully added user: { "user" : "root", "roles" : [ "userAdminAnyDatabase" ] }
```
- 登录
```
> db.auth("root","root123")
1
```
### 如何创建数据库用户
- 确定数据库：用use命令切换到目标数据库，同样使用`db.createUser()`
- 普通数据库角色：read和readWrite。
```
> use test
switched to db test
> db.createUser({user:"testuser",pwd:"testpass",roles:["readWrite"]})
Successfully added user: { "user" : "testuser", "roles" : [ "readWrite" ] }
> db.auth("testuser","testpass")
1
```
