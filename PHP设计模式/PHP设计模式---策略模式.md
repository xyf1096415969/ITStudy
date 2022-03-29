# PHP设计模式---策略模式

策略模式是对象的行为模式，用意是对一组算法的封装。动态的选择需要的算法并使用。

策略模式指的是程序中涉及决策控制的一种模式。策略模式功能非常强大，因为这个设计模式本身的核心思想就是面向对象编程的多形性思想。

策略模式的三个角色：

1．抽象策略角色

2．具体策略角色

3．环境角色（对抽象策略角色的引用）

实现步骤：

1．定义抽象角色类（定义好各个实现的共同抽象方法）

2．定义具体策略类（具体实现父类的共同方法）

3．定义环境角色类（私有化申明抽象角色变量，重载构造方法，执行抽象方法）

我们以电商算价格为简单例子来说明一下：算价格有满减，打折等等不同算法

## 我们先声明策略的行为接口

```php
<?php
  /*
   * 声明策略的接口，约定策略包含的行为。
   */
  interface Strategy
  {
      function getTitle();
      function getPrice();
  }
```

## 然后我们声明具体产品类

```php
/**
 * 满减类
 */
class ManJianStrategy implements Strategy
 {
    public function getTitle()
    {
        echo '满减算法';
    }
	
    public function getPrice($money)
    {
        return $money-10;
    }
 }
```



```php
/**
 * 打折类
 */
class DaZheStrategy implements Strategy
 {
    public function getTitle()
    {
        echo '打折算法';
    }
	
	public function getPrice($money)
    {
        return $money*0.5;
    }
 }
```

## 定义一个策略工厂类构造不同产品类

```php
class StrategyFactory
{
    private $strategy_mode;

    /**
     * 初始时，传入具体的策略对象
     * @param $mode
     */
    public function __construct($mode)
    {
        $this->strategy_mode = $mode;
    }
	
    /**
     * 执行输入标题算法
     */
	public function getTitle()
    {
        $this->strategy_mode->getTitle();
    }

    /**
     * 执行算钱算法
     * @param $money
     */
    public function getPrice($money)
    {
        $this->strategy_mode->getPrice($money);
    }
}
```

## 客户端测试

```php
<?php

// 满减
$mode1 = new StrategyFactory(new ManJianStrategy());
$mode1->getTitle();
$mode1->getPrice(100);


// 打折
$mode2= new StrategyFactory(new DaZheStrategy());
$mode2->getTitle();
$mode2->getPrice(100);
```

如果以后要扩展算钱，比如要原价，那么就写一个原价类就行了