# Golang的结构体初始化与指针

## Go结构体初始化

### 方式一：通过 var 声明结构体

假如现在有个这样的结构体：

```
type Test struct {
	name string
} 
```

通过var初始化：

```
var test Test  //初始化
test.name = "小徐" //对属性赋值
```

### 方式二：使用 new

```
test:=new(Test)
```

使用 new 函数给一个新的结构体变量分配内存，它返回指向已分配内存的**指针**：var t *T = new(T)。

Go语言让我们可以像访问普通结构体一样使用`.`来访问结构体指针的成员。

```
test.name = "小徐"
```

### 方式三：取结构体的地址实例化

```
test := &Test{}
```

在Go语言中，对结构体进行`&`取地址操作时，视为对该类型进行一次 new 的实例化操作,底层仍会调用new函数，此时返回的test也是**指针**

他也可以访问成员：

```
test.name = "小徐"
```

## Go结构体地址与普通变量地址

```
package main

import "fmt"

type Test struct {
	name string
}


func main() {
	a := 90
	test := &Test{} //这里test是指针
	test.name = "小徐"
	var p *int
	p = &a
	fmt.Println(p) //普通变量地址
	fmt.Println(test)
	fmt.Println(*test)
	fmt.Println("---------------")
	var t Test
	var p2 *Test
	p2 = &t
	fmt.Println(t)
	fmt.Println(&t) //取结构体地址
	fmt.Println(p2)
	fmt.Println(*p2)
	fmt.Println("---------------")
} 
```

结果：

```
0xc00000a0c0
&{小徐}
{小徐}
---------------
{}
&{}
&{}
{}
--------------- 
```

可以看到结构体地址都是：&{...}这种格式的。普通变量地址是0xc00000a0c0

参考文章：https://www.cnblogs.com/liyutian/p/10050320.html