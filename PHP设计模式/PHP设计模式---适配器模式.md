# PHP设计模式---适配器模式

1.简介

适配器模式（Adapter Pattern）是作为两个不兼容的接口之间的桥梁。这种类型的设计模式属于结构型模式，它结合了两个独立接口的功能。

**意图**：将一个类的接口转换成客户希望的另外一个接口。适配器模式使得原本由于接口不兼容而不能一起工作的那些类可以一起工作。

**主要解决**：主要解决在软件系统中，常常要将一些"现存的对象"放到新的环境中，而新环境要求的接口是现对象不能满足的。

**优点**： 1、可以让任何两个没有关联的类一起运行。 2、提高了类的复用。 3、增加了类的透明度。 4、灵活性好。

**缺点**： 1、过多地使用适配器，会让系统非常零乱，不易整体进行把握。比如，明明看到调用的是 A 接口，其实内部被适配成了 B 接口的实现，一个系统如果太多出现这种情况，无异于一场灾难。因此如果不是很有必要，可以不使用适配器，而是直接对系统进行重构。

**使用场景**：有动机地修改一个正常运行的系统的接口，这时应该考虑使用适配器模式。

**注意事项：**适配器不是在详细设计时添加的，而是解决正在服役的项目的问题。

2.角色

**源(Adaptee)角色**：需要进行适配的接口

**目标(Target)角色**：定义客户端使用的与特定领域相关的接口，这也就是我们所期待得到的
**适配器(Adapter)角色**：对Adaptee的接口与Target接口进行适配；适配器是本模式的核心，适配器把源接口转换成目标接口，此角色为具体类。

适配器有两种：使用继承的适配器模式；使用组件的适配器模式。适配器模式在不修改现有代码的基础上，保留了架构。使用继承的适配器和使用组件的适配器各有利弊，继承的类冗余度/空间复杂度偏高，组件的调用栈/时间复杂度偏高，应该结合实际情况选择。

本文例子是组件的适配器模式。

## 定义源角色类【猫类】

```php
<?php
class Cat {
  public function eatFish() {
    echo "cat eat fish.\n";
  }
}
```

## 定义目标角色

```php
<?php
interface Animal {
  function eatFish();
  function eatMoss();
}
?>
```

## 定义适配器角色

```php
<?php
class CatAdapter implements Animal {
  public function __construct() {
    $this->cat = new Cat();
  }
  public function eatFish() {
    $this->cat->eatFish();
  }
  //源角色没有eatMoss方法，在此适配
  public function eatMoss() {
    echo "cat don't eat moss.\n";
  }
}
```

## 测试

```php
<?php
 $catAdapter = new CatAdapter();
 $catAdapter->eatFish();
 $catAdapter->eatMoss();
```

参考：https://segmentfault.com/a/1190000005806009

https://www.cnblogs.com/myvic/p/8046526.html