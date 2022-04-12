# Golang之数组与切片

## 数组

### 数组简介



> 数组是内置(build-in)类型,是一组同类型数据的集合，它是值类型，通过从0开始的下标索引访问元素值。在初始化后长度是固定的，无法修改其长度。当作为方法的参数传入时将复制一份数组而不是引用同一指针。数组的长度也是其类型的一部分，通过内置函数len(array)获取其长度。

注意：和C中的数组相比，又是有一些不同的：

- Go中的数组是值类型，换句话说，如果你将一个数组赋值给另外一个数组，那么，实际上就是将整个数组拷贝一份
- 如果Go中的数组作为函数的参数，那么实际传递的参数是一份数组的拷贝，而不是数组的指针。这个和C要区分开。因此，在Go中如果将数组作为函数的参数传递的话，那效率就肯定没有传递指针高了。
- 数组的长度也是Type的一部分，这样就说明[10]int和[20]int是不一样的

### 数组定义

```go
/*几种数组定义写法*/

//定义数组arr
var arr [3]int
arr = [3]int{1, 2, 3}

//定义数组arr1	
arr1 := [2]int{3, 6}

//定义数组arr2	
var arr2 [2]string = [2]string{"hello", "world"}

//在数组的定义中，如果在数组长度的位置出现“...”省略号，则表示数组的长度是根据初始化值的个数来计算
arr3 := [...]int{11,22}

//打印数组	
fmt.Println(arr)
fmt.Println(arr1)
fmt.Println(arr2)
fmt.Printfln(arr3)
```

结果：

```
[1 2 3]
[3 6]
[hello world]
[1 4] 
```

### 数组遍历

```go
arr := [3]int{1, 2, 3}
//for遍历
for i := 0; i < len(arr); i++ {
	fmt.Println(arr[i])
}

fmt.Println("分隔符。。。。。。。。。")
//配合range遍历
for k, v := range arr {
	fmt.Println(k, v)
}
```

结果：

```
1
2
3
分隔符。。。。。。。。。
0 1
1 2
2 3 
```

## 切片

### 切片简介

> 数组的长度不可改变，在特定场景中这样的集合就不太适用，Go中提供了一种灵活，功能强悍的内置类型Slices切片(“动态数组"),与数组相比切片的长度是不固定的，可以追加元素，在追加时可能使切片的容量增大。切片中有两个概念：一是len长度，二是cap容量，长度是指已经被赋过值的最大下标+1，可通过内置函数len()获得。容量是指切片目前可容纳的最多元素个数，可通过内置函数cap()获得。切片是引用类型，因此在当传递切片时将引用同一指针，修改值将会影响其他的对象

### 切片初始化

1.从数组或切片生成新的切片

切片默认指向一段连续内存区域，可以是数组，也可以是切片本身

> slice [开始位置 : 结束位置]

语法说明如下：

- slice：表示目标切片对象；
- 开始位置：对应目标切片对象的索引；
- 结束位置：对应目标切片的结束索引，终止索引标识的项不包括在切片内。

```go
arr := [3]int{1, 2, 3}
fmt.Println(arr[1:2]) //下标1开始到下标2结束，但是不包括下标2的值
```

结果：

```
[2]
```

2.直接声明切片

```go
// 声明字符串切片
var strList []string

// 声明整型切片
var numList []int

// 声明一个空切片
var numListEmpty = []int{}

// 输出3个切片
fmt.Println(strList, numList, numListEmpty)

//不能使用下面语法定义切片
//test := []int

// 输出3个切片大小
fmt.Println(len(strList), len(numList), len(numListEmpty))

// 切片判定空的结果
fmt.Println(strList == nil)
fmt.Println(numList == nil)
fmt.Println(numListEmpty == nil)
```

结果：

```
[] [] []
0 0 0
true
true
false 
```

3.使用make()函数构造切片

如果需要动态地创建一个切片，可以使用 make() 内建函数，格式如下：

> make( []Type, size, cap )

```go
a := make([]int, 2)
b := make([]int, 2, 10)
fmt.Println(a, b)
fmt.Println(len(a), len(b))
```

结果：

```
[0 0] [0 0]
2 2 
```

其中 a 和 b 均是预分配 2 个元素的切片，只是 b 的内部存储空间已经分配了 10 个，但实际使用了 2 个元素。
容量不会影响当前的元素个数，因此 a 和 b 取 len 都是 2。

**注意**：使用 make() 函数生成的切片一定发生了内存分配操作，但给定开始与结束位置（包括切片复位）的切片只是将新的切片结构指向已经分配好的内存区域，设定开始与结束位置，不会发生内存分配操作。

### 切片赋值与引用

切片是引用类型，所以当引用改变其中元素的值时，其它的所有引用都会改变该值

```go
arr := [3]int{1,2,3} //定义一个数组
var aSlience []int //定义一个切片
aSlience = arr[1:2]
fmt.Println("数组改变前---------")
fmt.Println(arr)
fmt.Println(aSlience)

fmt.Println("数组改变后---------")
arr[1] = 1000 //数组第二个元素改变一下
fmt.Println(arr)
fmt.Println(aSlience)
```

结果：

```
数组改变前---------
[1 2 3]
[2]
数组改变后---------
[1 1000 3]
[1000] 
```

> 注意：切片和数组在声明时的区别：声明数组时，方括号内写明了数组的长度或使用...自动计算长度，而声明切片时，方括号内没有任何字符。 

参考文章：

https://blog.csdn.net/belalds/article/details/80076739

http://c.biancheng.net/view/27.html

http://c.biancheng.net/view/26.html