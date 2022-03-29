# PHP设计模式---观察者模式

1：观察者模式(Observer)，当一个对象状态发生变化时，依赖它的对象全部会收到通知，并自动更新。
2：场景:一个事件发生后，要执行一连串更新操作。传统的编程方式，就是在事件的代码之后直接加入处理的逻辑。当更新的逻辑增多之后，代码会变得难以维护。这种方式是耦合的，侵入式的，增加新的逻辑需要修改事件的主体代码。
3：观察者模式实现了低耦合，非侵入式的通知与更新机制。

我们以具体场景为例：比如用户注册成功后，给用户送积分，发优惠券等等操作。

我们定义一个观察者接口，定义观察者具体需要执行的方法，当然方法名和方法个数可以自定

```php
interface Observer
{
    public function update();
}
```

再定义一个事件类：定义添加观察者和广播通知的方法

```php
/**
 * 事件产生类
 * Class EventGenerator
 */
abstract class EventGenerator
{
    private $ObServers = [];

    //增加观察者
    public function add(ObServer $ObServer)
    {
        $this->ObServers[] = $ObServer;
    }

    //事件通知
    public function notify()
    {
        foreach ($this->ObServers as $ObServer) {
            $ObServer->update();
        }
    }

}
```

定义观察者具体类

```php
/**
 * 观察者【送积分】
 */
class Point implements ObServer
{
    public function update()
    {
        echo "送你积分\n";
    }
}

/**
 * 观察者【发优惠券】
 */
class Coupon implements ObServer
{
    public function update()
    {
        echo "发优惠券了\n";
    }
}
```

定义具体事件类

```php
/**
 * 用户注册事件
 * Class Event
 */
class UserRegisteredEvent extends EventGenerator
{
    /**
     * 触发事件
     */
    public function trigger()
    {
        //通知观察者
        $this->notify();
    }
}
```

测试

```php
//创建一个事件
$event = new UserRegisteredEvent();
//为事件增加观察者
$event->add(new Point());
$event->add(new Coupon());
//执行事件 通知观察者
$event->trigger();
```

参考：https://www.cnblogs.com/chrdai/p/11184221.html

https://www.cnblogs.com/onephp/p/6108344.html