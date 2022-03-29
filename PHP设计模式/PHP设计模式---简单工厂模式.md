# PHP设计模式---简单工厂模式

简单工厂 又称静态工厂方法模式。使用的频率可以说是非常之高，它的官方解释为：定义一个用于创建对象的接口，让子类决定实例化哪一个类。工厂模式使一个类的实例化延迟到其子类。

这个模式本身很简单而且使用在业务较简单的情况下。一般用于小项目或者具体产品扩展较少的情况（这样工厂类才不用经常更改）。

简单工厂的作用是实例化对象，而不需要客户了解这个对象属于哪个具体的子类。**简单工厂实例化的类具有相同的接口或者基类，在子类比较固定并不需要扩展时，可以使用简单工厂，一定程度上可以很好的降低耦合度**

```php
<?php
/**
 *简单工厂模式（静态工厂方法模式）
 */
/**
 * Interface people 人类
 */
interface  people
{
    public function  say();
}
/**
 * Class man 继承people的男人类
 */
class man implements people
{
    // 具体实现people的say方法
    public function say()
    {
        echo '我是男人<br>';
    }
}
/**
 * Class women 继承people的女人类
 */
class women implements people
{
    // 具体实现people的say方法
    public function say()
    {
        echo '我是女人<br>';
    }
}
/**
 * Class SimpleFactoty 工厂类
 */
class SimpleFactoty
{
    static function  createObj($type) {         
        switch ($type) {  
          case 'M':  
             return new man();  
          case 'F':  
             return new women();          //....  
   }  
}
/**
 * 具体调用
 */
$man = SimpleFactoty::createObj('M');$man->say();
$woman = SimpleFactoty::createObj('F');$woman->say();
```
