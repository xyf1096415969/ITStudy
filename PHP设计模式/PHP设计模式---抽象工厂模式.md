# PHP设计模式---抽象工厂模式

抽象工厂模式的用意为：给客户端提供一个接口，可以创建多个产品族中的产品对象 ，而且使用抽象工厂模式还要满足以下条件：

1. 系统中有多个产品族，而系统一次只可能消费其中一族产品。
2. 同属于同一个产品族的产品可以使用。

**产品族：位于不同产品等级结构中，功能相关联的产品组成的家族。下面例子的 汽车和空调就是两个产品树， 奔驰C200+格力某型号空调就是一个产品族， 同理， 奥迪A4+海尔某型号空调也是一个产品族。**

##### 产品类

```php
// 汽车（抽象产品接口）
interface AutoProduct
{
    public function dirve();
}


//奥迪A4（具体产品类）
class AudiA4Product implements AutoProduct
{
    //获取汽车名称
    public function dirve()
    {
        echo "开奥迪A4"."<br>";
    }
}

//奔驰C200（具体产品类）
class BenzC200Product implements AutoProduct
{
    //获取汽车名称
    public function dirve()
    {
        echo "开奔驰C200"."<br>";
    }
}
//空调（抽象产品接口）
interface AirCondition
{
    public function blow();
}

//格力空调某型号（具体产品类）
class GreeAirCondition implements AirCondition
{
    public function blow()
    {
        echo "吹格力空调某型号"."<br>";
    }
}

//海尔空调某型号（具体产品类）
class HaierAirCondition implements AirCondition
{
    public function blow()
    {
        echo "吹海尔空调某型号"."<br>";
    }
}
```

##### 抽象工厂类

```php
//工厂接口
interface Factory
{
    public function getAuto();
    public function getAirCondition();
}
```

具体工厂

```php
//工厂A = 奥迪A4 + 海尔空调某型号
class AFactory implements Factory
{
    //汽车
    public function getAuto()
    {
        return new AudiA4Product();
    }

    //空调
    public function getAirCondition()
    {
        return new HaierAirCondition();
    }
}
//工厂B = 奔驰C200 + 格力空调某型号
class BFactory implements Factory
{
    //汽车
    public function getAuto()
    {
        return new BenzC200Product();
    }

    //空调
    public function getAirCondition()
    {
        return new GreeAirCondition();
    }
}
```

客户端

```php
//客户端测试代码
$factoryA = new AFactory();
$factoryB = new BFactory();

//A工厂制作车
$auto_carA = $factoryA->getAuto();
$auto_airA = $factoryA->getAirCondition();

//B工厂制作车
$auto_carB = $factoryB->getAuto();
$auto_airB = $factoryB->getAirCondition();

//开奥迪车+吹海尔空调
$auto_carA->dirve();
$auto_airA->blow(); //热的时候可以吹吹空调

//开奔驰车+吹格力空调;
$auto_carB->dirve();
$auto_airB->blow(); //热的时候可以吹吹空调
```

参考：https://www.cnblogs.com/zxqblogrecord/p/9994550.html

https://segmentfault.com/a/1190000016659904