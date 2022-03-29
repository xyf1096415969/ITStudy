# PHP设计模式---注册模式

注册模式，解决全局共享和交换对象。已经创建好的对象，挂在到某个全局可以使用的数组上，在需要使用的时候，直接从该数组上获取即可。将对象注册到全局的树上。任何地方直接去访问。

## 定义注册类

```php
<?php
class Register
{
    protected static $objects;

    function set($alias, $object)//将对象注册到全局的树上
    {
        self::$objects[$alias] = $object;//将对象放到树上
    }

    static function get($name)
    {
        return self::$objects[$name];//获取某个注册到树上的对象
    }

    function _unset($alias)
　　{
        unset(self::$objects[$alias]);//移除某个注册到树上的对象。
    }
}
```

## 定义要注册到注册类的类

```php
<?php
class Single
{
    function test()
　　{
       echo "hello world!";
    }
}
```

## 测试

```php
<?php
 $single = new Single();
 Register::set('single',$single);
 $single = Register::get('single');
 var_dump($single->test());
```

参考：https://www.cnblogs.com/zhangzhijian/p/12305529.html

https://segmentfault.com/a/1190000007495855