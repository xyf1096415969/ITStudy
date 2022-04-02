#  Goalng的import"a/b" ，b是目录名还是包名？

使用import关键字，导入要使用的标准库包或第三方依赖包

```go
import "a/b/c"
import "fmt"

c.Func1()
fmt.Println("Hello, World")
```

很多Golang初学者看到上面代码，都会想当然的将import后面的"c"、"fmt"当成包名，将其与c.Func1()和 fmt.Println()中的c和fmt认作为同一个语法元素：包名

我们来验证一下：

项目结构如下【看src/test项目】：

![img](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/462122b547174175860ddb7d3bf86ce4~tplv-k3u1fbpfcp-watermark.image)

主要看hello.go与main.go:

hello.go

```go
package myPkg

func MyTest() (a int, b string) {
	a = 10006
	b = "hello go"
	return
} 
```

main.go

```go
package main

import (
	"fmt"
	"test/myPkg" //myPkg代表目录
)

func main() {
	aa, bb := myPkg.MyTest() //myPkg代表包名
	fmt.Println(aa, bb)
} 
```

此时运行：

```
D:\workspace\go\src\test>go run main.go 
10006 hello go
```

发现正常运行，我们现在很难看出import"test/myPkg"和myPkg.MyTest()这两个myPkg代表的是包名还是目录名。

我们假设import路径中的最后一个元素是包名，而非路径名。

我们改下代码：

hello.go

```go
package myPkg2 //包名修改了  其他没有变

func MyTest() (a int, b string) {
	a = 10006
	b = "hello go"
	return
} 
```

main.go

```go
package main

import (
	"fmt"
	"test/myPkg2"
)

func main() {
	aa, bb := myPkg2.MyTest()
	fmt.Println(aa, bb)
} 
```

运行的时候发现编译错误，无法找到test/myPkg2包

我们的假设错了，我们把它改为路径：

main.go

```go
package main

import (
	"fmt"
	"test/myPkg" //改为目录
)

func main() {
	aa, bb := myPkg2.MyTest() //myPkg2代表包名
	fmt.Println(aa, bb)
} 
```

运行正常，**所以import"test/myPkg"的myPkg代表的是目录，myPkg.MyTest()的myPkg代表包名**

参考：https://blog.csdn.net/jacktangj/article/details/79079820

https://blog.csdn.net/xixihahalelehehe/article/details/105283888