# Golang之go build go install

## 区别

go build：用于测试编译包，在项目目录下生成可执行文件（有main包）。

go install：主要用来生成库和工具。一是编译包文件（无main包），将编译后的包文件放到 pkg 目录下（`$GOPATH/pkg`）。二是编译生成可执行文件（有main包），将可执行文件放到 bin 目录（`$GOPATH/bin`）。

## 使用

```
go env
```

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1b21a51df88849a098e3b23fd6803936~tplv-k3u1fbpfcp-zoom-1.image)



我的项目结构如下：

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/291f1fb1171548c7b19f829a542f105d~tplv-k3u1fbpfcp-zoom-1.image)

src/hello/test/test1.go

```go
package test
import "fmt"
func MyTest1()  {   
  fmt.Println("test1")
}
```

src/hello/test/test2.go

```go
package test
import "fmt"
func MyTest2()  {   
  fmt.Println("test2")
}
```

sec/hello/main.go

```go
package main
import "fmt"
func main() {   
  fmt.Println("main")   
}
```

### 使用go build

进入hello目录下执行 go build

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/86466cd6df5c479aa1daca386a5b270c~tplv-k3u1fbpfcp-zoom-1.image)

这个时候会在hello目录下生成一个hello.exe文件

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d3dfab148aef4772a6d0e3ed5af59d96~tplv-k3u1fbpfcp-zoom-1.image)

然后我们进入test目录执行

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d088a85f6c9844a9a5b67c211214695b~tplv-k3u1fbpfcp-zoom-1.image)

不管是go build还是go build test1.go  go build test2.go都不会产生额外文件。

### 使用go install

进入hello目录执行go install

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/669331ac78414d47ac57c3861f3f5168~tplv-k3u1fbpfcp-zoom-1.image)

这个时候产生bin目录以及hello.exe文件【这里有没有发现 之前在hello目录下的hello.exe自动被删了】

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4e37bb4b45bf4810b8ba82a5cae3fc3d~tplv-k3u1fbpfcp-zoom-1.image)

进入test目录

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fac08c180f3948b4bfeb2a999d8d069a~tplv-k3u1fbpfcp-zoom-1.image)

这时候产生了pkg目录，但是不会产生bin目录

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8963efc56f0f49cdb3ed495dde081078~tplv-k3u1fbpfcp-zoom-1.image)

> 注意：go install 的时候包名要从$GOPATH/src下面的目录开始写。 比如在src下面有一个项目project，下面有个tools包，应该写成这样： go install project/tools 按照上面这个原则，就会在$GOPATH/pkg下面生成a文件了



参考资料：

https://www.jianshu.com/p/3db831d9b553

https://blog.csdn.net/zyz770834013/article/details/78656985

https://www.jianshu.com/p/d9ee6e88a751

https://seekload.net/2019/01/29/go-package.html