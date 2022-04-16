# Golang里阻塞死锁情况(一)

学通道channel，发现一个简单的demo：

```php
package main

import "fmt"

func main() {
    chanInt := make(chan int)
    go func() {
        chanInt <- 100 //发送者先运行，那就阻塞在这，直到接受者接受数据
    }()

    res := <-chanInt  //接收者先运行，那就阻塞在这，直到发送者发数据
    fmt.Println(res)
}
```

输出结果是100，这个没有问题。但是之前在学goroutine的时候有看到过一个例子：

```php
package main

import "fmt"

func hello() {
    fmt.Println("Hello Goroutine!")
}
func main() {
    go hello() // 启动另外一个goroutine去执行hello函数
    fmt.Println("main goroutine done!")
}
```

这个例子输出的只有：main goroutine done! 并没有Hello Goroutine!
看过解释：**在程序启动时，Go程序就会为main()函数创建一个默认的goroutine。当main()函数返回的时候该goroutine就结束了，所有在main()函数中启动的goroutine会一同结束。**

那么这个解释放到第一个例子为什么不适用了？

我的理解是：运行到res := <-chanInt这句会阻塞，下面代码就不会去执行，直到协程写入通道后，就马上读取，继续执行打印语句。

然后就是关于阻塞的情况，比如我把第一个例子改一下：

```php
package main

import (
    "fmt"
    "time"
)

func main() {
    chanInt := make(chan int)
    go func() {
        chanInt <- 100
    }()
    time.Sleep(10 * time.Second)
    res := <-chanInt
    fmt.Println(res)
}
```

多了time.Sleep(10 * time.Second)等待10秒钟，10秒后输出100,这个没有问题。

然后再看一个例子：

```php
func main() {
    chanInt := make(chan int)
    chanInt <- 100
    res := <-chanInt
    fmt.Println(res)
}
//我们说过 channel 是用来给不同 goroutine 通信的，所以是不能在同一个协程又发送又接收，这根本就达不到隧道通信的效果
```

这个例子就会死锁，阻塞在chanInt <- 100这句，发现没有接收者所以死锁了，这边不理解的是：睡眠10秒的时候，在这10秒里面为什么不会造成死锁？而最后一个例子一运行马上报死锁？

我的理解是：最后一个例子，因为main goroutine 阻塞在 chanInt <- 100 收取操作上了，而且现在没有其他的 goroutine，所以 main goroutine 永远不可能解除阻塞。如果有另外的 goroutine 在运行，它就有可能向 channel 发送数据，让 main 解除阻塞。而至于10秒那个例子，是因为另起了协程不会阻塞【无缓冲通道在无数据发送时，接收端会阻塞，**直到有新数据发送过来为止**】，还有就是通道必须在**多个**`goroutine`中使用

补充：

```php
package main

import (
	"fmt"
	"time"
)

func main() {
	chanInt := make(chan int)
	go func() {
		time.Sleep(10 * time.Second)
		//chanInt <- 100
		fmt.Println("123")
	}()

	res := <-chanInt
	fmt.Println(res)
} 
```

这个例子里面，我在协程里面睡眠10秒，运行代码的时候发现，等了10秒打印123之后，然后报错deadlock（具体看下图）：

![第16篇1](https://cdn.jsdelivr.net/gh/xyf1096415969/blogImgs@main/imgs/Golang/第16篇1.png)

也就是说死锁的发生要在新开goroutine运行完成之后才会知道有没有goroutine往通道放数据。如果我把fmt.Println("123")改为chanInt <- 100代码就正常运行



参考文档：https://golang.coding3min.com/4.concurrent/4-3-channel/