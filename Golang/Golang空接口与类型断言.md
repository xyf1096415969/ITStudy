# Golang空接口与类型断言

## 空接口

### 定义

空接口是特殊形式的接口类型，普通的接口都有方法，而空接口没有定义任何方法口，也因此，我们可以说所有类型都至少实现了空接口。

```go
type test interface {
}
```

每一个接口都包含两个属性，一个是值，一个是类型。

```go
var i interface{}
fmt.Printf("类型：%T----值：%v\n", i, i) //类型：<nil>----值：<nil>
```

可见对于空接口来说，这两者都是 nil

### 使用场景

**第一**，通常我们会直接使用 `interface{}` 作为类型声明一个实例，而这个实例可以承载任意类型的值。

```go
func main() {
	var i interface{}

	i = 100
	fmt.Println(i) //100

	i = "yif"
	fmt.Println(i) //yif

	i = 3.14
	fmt.Println(i) //3.14

	i = false
	fmt.Println(i) //false
}
```

**第二**，如果想让你的函数可以接收任意类型的值 ，也可以使用空接口。如下代码都正常打印：

```go
func main() {
	i := 100
	s := "yif"
	f := 3.14

	test(i)
	test(s)
	test(f)
}

func test(i interface{}) {
	fmt.Println(i)
}
```

上面写法有点麻烦，可以使用可变参数的函数。如下：

```go
func main() {
	i := 100
	s := "yif"
	f := 3.14

	test(i, s, f)
}

func test(res ...interface{}) {
	fmt.Println(res) //res是切片
	for k, v := range res {
		fmt.Println(k, v)
	}
}
```

结果：

```
D:\workspace\go\src\test>go run main.go
[100 yif 3.14]
0 100
1 yif
2 3.14 
```

**第三**，你也定义一个可以接收任意类型的 array、slice、map、strcut，例如这边定义一个切片

```go
func main() {
	sli := make([]interface{}, 4)
	sli[0] = 100
	sli[1] = "yif"
	sli[2] = []int{1, 2, 3}
	sli[3] = [...]int{5, 6, 7}
	fmt.Println(sli)
	for k, v := range sli {
		fmt.Println(k, v)
	}
}
```

结果：

```
D:\workspace\go\src\test>go run main.go
[100 yif [1 2 3] [5 6 7]]
0 100
1 yif
2 [1 2 3]
3 [5 6 7] 
```

## 空接口几个要注意的坑

**第一，**空接口可以承载任意值，但不代表任意类型就可以承接空接口类型的值

空接口类型可以保存任何值，也可以从空接口中取出原值。

但要是你把一个空接口类型的对象，再赋值给一个固定类型（比如 int, string等类型）的对象赋值，是会报错的。

```go
var i interface{} = 100
var t int = i // cannot use i (type interface {}) as type int in assignment: need type assertion
```

但是你使用短变量声明，是可以的：

```go
var i interface{} = 100
t := i
fmt.Println(t) //100
```

因为编译器会根据等号右边的值来推导变量的类型完成初始化。

**第二：**当空接口承载数组和切片后，该对象无法再进行切片

```go
sli := []int{1, 2, 3, 4}
var i interface{}
i = sli
fmt.Println(i[1:2]) //cannot slice i (type interface {})
```

## 类型断言

类型断言（Type Assertion）是一个使用在接口值上的操作，用于检查接口类型变量所持有的值是否实现了期望的接口或者具体的类型

**类型断言，仅能对静态类型为空接口（interface{}）的对象进行断言，否则会抛出错误**

### Go语言中类型断言的两种语法

在Go语言中类型断言的第一种语法格式如下：

```
t := i.(T)
```

这个表达式可以断言一个接口对象（i）里不是 nil，并且接口对象（i）存储的值的类型是 T，如果断言成功，就会返回值给 t，如果断言失败，就会触发 panic。

```go
func main() {
	var i interface{} = 100
	t := i.(int)
	fmt.Println(t) //100
	
	fmt.Println("------------------------------------")

	s := i.(string)
	fmt.Println(s)
}
```

结果【执行第二次断言的时候失败了，并且触发了 panic】：

```
D:\workspace\go\src\test>go run main.go
100
------------------------------------
panic: interface conversion: interface {} is int, not string

goroutine 1 [running]:
main.main()
        D:/workspace/go/src/test/main.go:32 +0x10e
exit status 2 
```

如果要断言的接口值是 nil，那我们来看看也是不是也如预期一样会触发panic

```go
var i interface{}
var _ = i.(interface{})
```

结果：

```
D:\workspace\go\src\test>go run main.go
panic: interface conversion: interface is nil, not interface {}

goroutine 1 [running]:
main.main()
        D:/workspace/go/src/test/main.go:27 +0x34
exit status 2 
```

在Go语言中类型断言的另一种语法格式如下：

```
t, ok:= i.(T)
```

和上面一样，这个表达式也是可以断言一个接口对象（i）里不是 nil，并且接口对象（i）存储的值的类型是 T，如果断言成功，就会返回其类型给 t，并且此时 ok 的值 为 true，表示断言成功。

如果接口值的类型，并不是我们所断言的 T，就会断言失败，但和第一种表达式不同的事，这个不会触发 panic，而是将 ok 的值设为 false ，表示断言失败，此时t 为 T 的零值。

```go
func main() {
    var i interface{} = 10
    t1, ok := i.(int)
    fmt.Printf("%d-%t\n", t1, ok)

    fmt.Println("=====分隔线1=====")

    t2, ok := i.(string)
    fmt.Printf("%s-%t\n", t2, ok)

    fmt.Println("=====分隔线2=====")

    var k interface{} // nil
    t3, ok := k.(interface{})
    fmt.Println(t3, "-", ok)

    fmt.Println("=====分隔线3=====")
    k = 10
    t4, ok := k.(interface{})
    fmt.Printf("%d-%t\n", t4, ok)

    t5, ok := k.(int)
    fmt.Printf("%d-%t\n", t5, ok)
}
```

结果【运行后输出如下，可以发现在执行第二次断言的时候，虽然失败了，但并没有触发了 panic】：

```
D:\workspace\go\src\test>go run main.go
10-true
=====分隔线1=====
-false
=====分隔线2=====
<nil> - false
=====分隔线3=====
10-true
10-true 
```

上面这段输出，你要注意的是第二个断言的输出在`-false` 之前并不是有没有输出任何 t2 的值，而是由于断言失败，所以 t2 得到的是 string 的零值也是 `""` ，它是零长度的，所以你看不到其输出。

### 类型断言配合 switch 使用

如果需要区分多种类型，可以使用 type switch 断言。

```go
func main() {
	test(100)
	test("yif")
	test(3.14)
	
	var i interface{} //nil
	test(i)
	
	test(nil)
}

func test(i interface{}) {
	switch r := i.(type) {
	case int:
		fmt.Println(r, "是int型")
	case string:
		fmt.Println(r, "是字符串")
	case nil:
		fmt.Println(r, "是nil")
	default:
		fmt.Println("没有结果！")
	}
}
```

结果：

```
D:\workspace\go\src\test>go run main.go
100 是int型
yif 是字符串
没有结果！
<nil> 是nil
<nil> 是nil 
```