# Golang基于指针对象的方法

```go
func (p Point) square() {
    p.x *= p.x    //这不会改变p的值，p是值拷贝的。调用的时候构造的一个临时变量
    p.y *= p.y
}

func (p *Point) square() {
    p.x *= p.x   //这会改变p的值，只是传递一个指针
    p.y *= p.y
}
```

而他们的调用方法没什么差异。 都可以使用p.square()，对于指针对象方法，编译器会自动把它变成（(&p).square()，直接在代码中这样写也可以）。