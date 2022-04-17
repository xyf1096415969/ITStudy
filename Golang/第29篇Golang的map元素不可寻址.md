# Golang的map元素不可寻址

 map 元素是无法取址的，这个我们看下面例子就知道了：

```go
func main() {
	m := map[int]string{
		1: "A",
		2: "B",
	}
	fmt.Printf("%p\n",&m[1]) //cannot take the address of m[1]
}
```

从上面例子可以看出，map的value值是不可寻址的。

但是我们再看下面的例子：

```go
func main() {
	m := map[int]string{
		1: "A",
		2: "B",
	}
	m[1] = "C"
    fmt.Println(m) //map[1:C 2:B]
}
```

我们对map第一个元素进行修改，发现是可以成功的，那这就有个疑惑：既然不能寻址，为何又可以修改呢？

因为map底层是一个hash表，它的key和value键值是存储在bucket桶里，存储的是value值所以不能取址。为什么可以修改？因为存储键值对存储在bucket桶里，可以根据hash值的低8位确定key的位置，如果查找到了就可以修改值了。

我们接着看第三个例子：

```go
func main() {
	m := map[int]Person{
		1: Person{"Andy", 10},
		2: Person{"Tiny", 20},
		3: Person{"Jack", 30},
	}
	m[1].name = "TOM" // cannot assign to struct field m[1].name in map
	fmt.Println(m)
}

type Person struct {
	name string
	age  int
}
```

当map的value是结构体的时候，我通过`m[1].name = "TOM"`对第一个结构体修改name，发现这句报错：`cannot assign to struct field m[1].name in map`，不能对他进行修改。为什么这里又不能修改元素值了？

因为这个时候的`m[1]`是一个结构体`Person{"Andy", 10}`，他是不可寻址的，你要对不可寻址的结构体进行`.name`操作自然会报错。他与第二个例子还是有区别的，不要搞混。

如果要对结构体修改值，可以通过新建变量，修改后再覆盖：

```go
func main() {
	m := map[int]Person{
		1: Person{"Andy", 10},
		2: Person{"Tiny", 20},
		3: Person{"Jack", 30},
	}
	p := m[1]
	p.name = "TOM"
	m[1] = p
	fmt.Println(m[1]) //{TOM 10}
}

type Person struct {
	name string
	age  int
}
```

或则可以通过**指针**：

```go
func main() {
	m := map[int]*Person{
		1: &Person{"Andy", 10},
		2: &Person{"Tiny", 20},
		3: &Person{"Jack", 30},
	}
	m[1].name = "TOM"
	fmt.Println(m[1].name) //TOM
}

type Person struct {
	name string
	age  int
}
```