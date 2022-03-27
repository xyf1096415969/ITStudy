# Golang get下载得包不在$GOPATH$/src目录下

通过前一篇文章[通过代理下载Beego框架](https://juejin.im/post/6854573210483032077) 我们下载好了框架源码包，可是有没有发现这个源码包他并没有出现在$GOPATH/src目录下面，而是在$GOPATH/pkg目录下面：

![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/7/19/17364da8ef85fd01~tplv-t2oaga2asx-image.image)

我们上一篇文章我们是开启了模块功能GO111MODULE=on

> 官方在 v1.11 中加入了 Go Module 作为官方包管理形式，就这样 dep 无奈的结束了使命。 最初的 Go Module 提案的名称叫做 vgo，下面为了介绍简称为 gomod。不过在 v1.11 和 v1.12 的 Go 版本中 gomod 是不能直接使用的。 可以通过 go env 命令返回值的 GOMOD 字段是否为空来判断是否已经开启了 gomod，如果没有开启，可以通过设置环境变量 export GO111MODULE=on 开启 

我的go版本是1.14.1

### GO111MODULE 有三个值：off, on和auto（默认值）

- `GO111MODULE=off`，go命令行将不会支持module功能，寻找依赖包的方式将会沿用旧版本那种通过vendor目录或者GOPATH模式来查找。

- `GO111MODULE=on`，**go命令行会使用modules，而一点也不会去GOPATH目录下查找**。

- ```
  GO111MODULE=auto
  ```

  ，默认值，go命令行将会根据当前目录来决定是否启用module功能。这种情况下可以分为两种情形：

  - 当前目录在GOPATH/src之外且该目录包含go.mod文件
  - 当前文件在包含go.mod文件的目录下面

### 依赖包存储位置

- 使用go get获取的包放在`$GOPATH/src/`目录下
- 使用go mod下载的依赖包放在`$GOPATH/pkg/mod/`目录下，所有项目共享

在你需要导入第三方包的地方,打开终端输入`go mod init 你的命名`
然后在该文件夹会出现`go.mod`文件

![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/7/19/17364e1e004f21ba~tplv-t2oaga2asx-image.image)

然后使用`go get 你想要导入的包地址`

``

![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/7/19/17364e67d425f224~tplv-t2oaga2asx-image.image)

这个时候go.mod文件就会多一条你刚才导入得包：

![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/7/19/17364e7853d68f28~tplv-t2oaga2asx-image.image)



参考文章：https://blog.csdn.net/qq_43442524/article/details/104906475

​         https://www.cnblogs.com/HKUI/p/11876988.html