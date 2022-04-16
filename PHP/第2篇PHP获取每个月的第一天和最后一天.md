

# PHP获取每个月的第一天和最后一天

![第2篇1](https://cdn.jsdelivr.net/gh/xyf1096415969/blogImgs@main/imgs/PHP/第2篇1.png)

我们每次需要获取这个月第一天和最后一天的时间戳代码一般如下：

```php
strtotime(date('Y-m-01')); //获取从1号0点开始的时间戳。
strtotime('+1 month -1 day', strtotime(date('Y-m-01'))); //获取这个月最后一天23点59分的时间戳。
```

但是这样子会计算多次。

如果我们使用Relative Formats来计算的话，会变的简单得多。

```php
strtotime('first day of today'); //获取从1号0点开始的时间戳。

strtotime('last day of 23:59'); //获取这个月最后一天23点59分的时间戳。
```

上面代码在5.5版本对的，5.6之后就不对了

可以使用下面的[依然是有bug的，转换日期格式的话是没有问题的，时间戳是不对的]：

```php
$first = "first day of this month"; //本月第一天
$last = "last day of this month"; //本月最后一天
echo "'$first': ".date("Y-m-d", strtotime($first)).PHP_EOL;
echo "'$last': ".date("Y-m-d", strtotime($last)).PHP_EOL;
echo "Today: ".date("Y-m-d").PHP_EOL;
```

结果：

```php
'first day of this month': 2021-08-01
'last day of this month': 2021-08-31
Today: 2021-08-12
```

https://www.php.net/manual/en/datetime.formats.relative.php