# PHP设计模式---单例模式

单例模式确保某个类只有一个实例，而且自行实例化并向整个系统提供这个实例。

单例模式是一种常见的设计模式，在计算机系统中，线程池、缓存、日志对象、对话框、打印机、数据库操作、显卡的驱动程序常被设计成单例。

单例模式有以下3个特点：

1．只能有一个实例。

2．必须自行创建这个实例。

3．必须给其他对象提供这一实例。

那么为什么要使用PHP单例模式？

PHP一个主要应用场合就是应用程序与数据库打交道的场景，在一个应用中会存在大量的数据库操作，针对数据库句柄连接数据库的行为，使用单例模式可以避免大量的new操作。因为每一次new操作都会消耗系统和内存的资源。

单例模式分3种：懒汉式单例、饿汉式单例、登记式单例。

懒汉式单例:

```
<?php
//懒汉式单例类.在第一次调用的时候实例化自己 
class Singleton
{    //创建静态私有的变量保存该类对象
    static private $instance;

    //防止使用new直接创建对象
    private function __construct(){}

    //防止使用clone克隆对象
    private function __clone(){}

    static public function getInstance()
    {
        //判断$instance是否是Singleton的对象，不是则创建
        if (!self::$instance instanceof self) {
            self::$instance = new self();
        }
        return self::$instance;
    }

    public function test()
    {
        echo "我是一个单例模式";
    }
}

$sing = Singleton::getInstance();
$sing->test();
$sing2 = new Singleton(); //Fatal error: Uncaught Error: Call to private Singleton::__construct() from invalid context in
$sing3 = clone $sing; //Fatal error: Uncaught Error: Call to private Singleton::__clone() from context
```

**PHP中不支持饿汉式的单例模式**
因为PHP不支持在类定义时给类的成员变量赋予非基本类型的值。如表达式，new操作等等

```php
<?php
//饿汉式单例类.在类初始化时，已经自行实例化
class Test{
//在初始化类的属性时它的值 必须要是常量类型，不能使用非常量，否则就会报错
private static $instance = new self(); //Fatal error: Constant expression contains invalid operations in /usercode/file.php on line 3


private function __construct()
{
}
    
public static function getInstance()
{
    return self::$instance;
}
}
$test = Test::getInstance()
?>
```

参考：https://www.cnblogs.com/hxun/p/11840839.html