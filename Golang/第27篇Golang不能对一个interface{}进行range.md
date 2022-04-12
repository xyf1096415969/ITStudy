# Golang不能对一个interface{}进行range

直接看一个demo：

```
func main() {
	sli := []interface{}{[]int{1, 2, 3, 4}}
	for _, v := range sli { //sli是interface{}类型切片
		for _, val := range v { //v是interface{}类型 cannot range over v (type interface {})
			fmt.Println(val)
		}
	}
}
```
```
D:\workspace\go\src\test>go run main.go
# command-line-arguments
.\main.go:50:17: cannot range over v (type interface {}) 
```

这段代码在for _, val := range v 的时候编译不通过，大概意思是：range不能作用于interface{]类型的v。

刚开始的时候我一直以为v是一个切片（`[]int{1, 2, 3, 4}`），所以一直认为对切片进行range是没有问题的。甚至我还去打印了v的类型：

```
func main() {
	sli := []interface{}{[]int{1, 2, 3, 4}}
	for _, v := range sli { //sli是interface{}类型切片
		ty := reflect.ValueOf(v)
		fmt.Println(ty.Kind()) //slice
	}
}
```

从结果来看v是切片，那为什么对v进行range会编译错误呢？

我们再看看第一段代码，我们定义了一个切片sli，它是interface{}类型，它也就一个值：`[]int{1, 2, 3, 4}`，也就是说sli里面的值类型都是interface{}类型，sli自己本身就是**interface{}的切片**，所以第一个`range sli`是没有问题的，但是v他是**interface{}类型【注意：interface{}类型切片与interface{}类型的区别】**，**在编译阶段**range并不能知道它是[]int类型，至于第二个例子代码打印是切片类型，因为去掉了v的range，编译通过了，再打印自然是实际的数据类型。如果加上还是编译不通过：

```
sli := []interface{}{[]int{1, 2, 3, 4}}
	for _, v := range sli {
		ty := reflect.ValueOf(v)
		fmt.Println(ty.Kind())
		for _, val := range v {
			fmt.Println(val)
		}
	}
```
```
D:\workspace\go\src\test>go run main.go
# command-line-arguments
.\main.go:51:17: cannot range over v (type interface {})
```