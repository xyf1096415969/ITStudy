
# Golang通过指针进行数据交换
```
package main

import (
	"fmt"
)

func main() {
	x, y := 1, 2
	fmt.Printf("x交换前的地址=%p\n",&x)
	fmt.Printf("y交换前的地址=%p\n",&y)
	swap(&x, &y)
	fmt.Println(x, y)
	fmt.Printf("x交换后的地址=%p\n",&x)
	fmt.Printf("y交换后的地址=%p\n",&y)
}

func swap(a, b *int) {
	fmt.Printf("a变量本身的地址=%p\n", &a)
	fmt.Printf("b变量本身的地址=%p\n", &b)
	fmt.Printf("a交换前的x赋予的地址=%p\n", a)
	fmt.Printf("b交换前的y赋予的地址=%p\n", b)
	fmt.Printf("a交换前的值=%d\n", *a)
	fmt.Printf("b交换前的值=%d\n", *b)
	*b, *a = *a, *b
	fmt.Printf("a变量本身的地址=%p\n", &a)
	fmt.Printf("b变量本身的地址=%p\n", &b)
	fmt.Printf("a交换后的x赋予的地址=%p\n", a)
	fmt.Printf("b交换后的y赋予的地址=%p\n", b)
	fmt.Printf("a交换后的值=%d\n", *a)
	fmt.Printf("b交换后的值=%d\n", *b)
}  
```

结果如下：
```go
x交换前的地址=0xc0000a0068
y交换前的地址=0xc0000a0080
a变量本身的地址=0xc0000ca020
b变量本身的地址=0xc0000ca028
a交换前的x赋予的地址=0xc0000a0068
b交换前的y赋予的地址=0xc0000a0080
a交换前的值=1
b交换前的值=2
a变量本身的地址=0xc0000ca020
b变量本身的地址=0xc0000ca028
a交换后的x赋予的地址=0xc0000a0068
b交换后的y赋予的地址=0xc0000a0080
a交换后的值=2
b交换后的值=1
2 1
x交换后的地址=0xc0000a0068
y交换后的地址=0xc0000a0080 
```

从倒数第三行知道，值交换成功了

下面我把代码改一下把

*b, *a = *a, *b 

改为

```
b, a = a, b
```

结果如下：

```go
x交换前的地址=0xc00000a0c0
y交换前的地址=0xc00000a0c8
a变量本身的地址=0xc000006030
b变量本身的地址=0xc000006038
a交换前的x赋予的地址=0xc00000a0c0
b交换前的y赋予的地址=0xc00000a0c8
a交换前的值=1
b交换前的值=2
a变量本身的地址=0xc000006030
b变量本身的地址=0xc000006038
a交换后的x赋予的地址=0xc00000a0c8
b交换后的y赋予的地址=0xc00000a0c0
a交换后的值=2
b交换后的值=1
1 2
x交换后的地址=0xc00000a0c0
y交换后的地址=0xc00000a0c8 
```

发现x与y的值没有成功交换

为什么两则有差异，我们知道a，b分别是x，y的地址（也就是&x，&y的值）

## 内存地址粗略草图

![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/4/19/17190ff7ecd0db38~tplv-t2oaga2asx-image.image?raw=true)

## 我们先看b,a=a,b这种情况

从第二种结果打印我们知道，a,b自己本身也有地址，区别在于他们的地址**指向的值**是x，y的地址，所以b,a=a,b这种相当于把**内存这一列**的值进行对调（也就是0xc00000a0c0和0xc00000a0c8对调一下），但是ab本身地址没有改变，由于内存的地址值变了，所以ab的值所指向的值也对调了（也就是*a，*b的值对调了），从图上来说，ab与xy没有关系

## 我们再看*b,*a=*a,*b这种情况

我们先看一个知识点：*a = *b这句话的含义：

> `*`操作符作为右值时，意义是取指针的值，作为左值时，也就是放在赋值操作符的左边时，表示 a 指针指向的变量。其实归纳起来，`*`操作符的根本意义就是操作指针指向的变量。当操作在右值时，就是取指向变量的值，当操作在左值时，就是将值设置给指向的变量。

简单来说就是：取b指针的值（这里值为2）, 赋给a指针指向的变量，a指针指向的变量就是x呀，所以相当于把2赋给x 同理*b = *a也是这个道理



参考：

https://baijiahao.baidu.com/s?id=1598627292221915297&wfr=spider&for=pc
