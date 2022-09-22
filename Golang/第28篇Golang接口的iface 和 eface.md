# Golang接口的iface 和 eface

本文通过一个小例子引出：

```
i := 100
a := interface{}(i)
fmt.Printf("%T---%v\n", a,a) //int---100
```

刚开始一直以为类型打印结果会是`interface`,没想到居然是`int`

我们知道一个interface的结构包含两部分：类型和值：

![第28篇interface结构](https://cdn.jsdelivr.net/gh/xyf1096415969/blogImgs@main/imgs/Golang/第28篇interface结构.png)

根据接口是否包含方法，可以将接口分为 `iface` 和 `eface`。

## eface

**eface**，表示不带有方法的接口。结构图如下：

![第28篇eface结构图](https://cdn.jsdelivr.net/gh/xyf1096415969/blogImgs@main/imgs/Golang/第28篇eface结构图.png)

当定义一个空接口的时候，var empty interface{}

此时的类型和值都是`nil`，如下图：

![第28篇eface结构图empty](https://cdn.jsdelivr.net/gh/xyf1096415969/blogImgs@main/imgs/Golang/第28篇eface结构图empty.png)

如果这个时候进行赋值：`empty = 100`

那么类型就会变为`int`，值变为`100`【所以文章开头的例子为什么打印int现在明白了吧】

![第28篇eface结构图赋值](https://cdn.jsdelivr.net/gh/xyf1096415969/blogImgs@main/imgs/Golang/第28篇eface结构图赋值.jpg)



## iface

**iface**，表示带有方法的接口。结构图如下：

![第28篇iface结构图](https://cdn.jsdelivr.net/gh/xyf1096415969/blogImgs@main/imgs/Golang/第28篇iface结构图.png)

Go语言中，每个变量都有唯一个**静态类型**，这个类型是编译阶段就可以确定的。有的变量可能除了静态类型之外，还会有**动态混合类型**。

我们看个网上的例子吧：

```
//带函数的interface
var r io.Reader 

tty, err := os.OpenFile("/dev/tty", os.O_RDWR, 0)
if err != nil {
    return nil, err
}

r = tty
```

运行，`var r io.Reader`，结构图如下图：

![第28篇iface结构图empty](https://cdn.jsdelivr.net/gh/xyf1096415969/blogImgs@main/imgs/Golang/第28篇iface结构图empty.png)



最后一句`r = tty`，结构图变为如下图：

![第28篇iface结构图赋值](https://cdn.jsdelivr.net/gh/xyf1096415969/blogImgs@main/imgs/Golang/第28篇iface结构图赋值.png)

但是记住：虽然有**动态混合类型**，但是对外”表现”依然是静态类型。

参考文章【图片也出自这，感谢大神】：https://i6448038.github.io/2020/02/15/golang-reflection/