# PHP设计模式---装饰模式

转载地址：https://www.cnblogs.com/taijun/p/4048446.html

装饰模式 ：装饰模式是在不必改变原类文件和使用继承的情况下，动态的扩展一个对象的功能。它是通过创建一个包装对象，也就是装饰来包裹真实的对象。

装饰模式是一种 结构型模式

主要角色 ：

1. 抽象构件（Component）角色：给出一个抽象接口，以规范准备接收附加责任的对象。
2. 具体构件（Concrete Component）角色：定义一个将要接收附加责任的类。
3. 装饰（Decorator）角色：持有一个构件（Component）对象的实例，并实现一个与抽象构件接口一致的接口。
4. 具体装饰（Concrete Decorator）角色：负责给构件对象添加上附加的责任。


具体代码如下：

```php
<?php
/*
 * 装饰模式：在不必改变原类文件和使用继承的情况下，动态地扩展一个对象的功能。它是通过创建一个包装对象，也就是装饰来包裹真实的对象。
 *
 * 角色
 * 抽象构件(Component)角色：定义一个对象接口，以规范准备接收附加职责的对象，从而可以给这些对象动态地添加职责。
 * 具体构件(Concrete Component)角色：定义一个将要接收附加职责的类。
 * 装饰(Decorator)角色：持有一个指向Component对象的指针，并定义一个与Component接口一致的接口。
 * 具体装饰(Concrete Decorator)角色：负责给构件对象增加附加的职责。
 */
 
//1抽象构件角色
abstract class drink{
    abstract function showprice();
}
 
 
//具体构件角色
class coffee extends drink{
    public $name = '咖啡';
    public $price = '10';
    public function showprice(){
        echo $this->name.'   ';
        echo $this->price.'元';
    }
}
 
//生成对象。
$coffee = new coffee();
$coffee->showprice();
 
 
 
//上面我已经生成了一个对象，现在已经在线上运行
//但是我想动态的为coffee对象添加功能，而不改变原有的类和继承关系，怎么办？
//装饰模式
 
 
 
//装饰角色
class decretor extends drink{
    public $drink;
    public function __construct(drink $drink){//包装了对象哦
        $this->drink = $drink;
    }
     
    public function showprice(){//实现对象原有功能，
        $this->drink->showprice();
        $this->add();//新增功能
    }
    public function add(){
         
    }
}
 
//具体装饰角色
class suger extends decretor{
    public $price = 1.5;
    public $name = '加糖';
    public function add(){
        echo $this->name.'   '.$this->price;
        echo '  一共'.($this->drink->price+$this->price).'元';
    }
}
 
$suger = new suger($coffee);
$suger->showprice();
?>
```

参考地址：https://baijunyao.com/article/172

https://segmentfault.com/a/1190000004467783