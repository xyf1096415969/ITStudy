# composer.lock文件的作用

## 环境参数

php版本：7.4.13

composer版本：2.3

laravel版本：7.30.6

> 在使用composer后目录中会出现2个文件，`composer.lock`和`composer.json`,现在来说说这两个文件的作用。
> 1、`composer.json`composer.json文件中保存的是我们安装的组件及组件的版本要求。
> 2、`comopser.lock`composer.lock文件中保存的是组件及其依赖的具体版本，在多人协同开发的情况下，这个文件能很好的解决组件不同而产生的问题。在使用`composer install`的时候是不会修改`composer.lock`这个文件,所以会把这个文件也放入版本管理中，其它人在使用时只需要`composer install`就可以了。而使用`composer update`后修改这个文件。

上面这段话是网上的说法，但是我觉得说的还是有点云里雾里的，所以自己动手操作一下，来看看lock文件到底什么作用？

我打算下载symfony/mailer这个第三方库，`symfony/mailer`最新的版本是6.1。以此作为示例。现在我的laravel框架已经下载完成，lock和json文件都是有的。

我们通过require命令下载5.2版本的：

```
composer require "symfony/mailer:5.2"
```

这个时候去看composer.json,多了一行`"symfony/mailer": "5.2"`

![PHP第1篇1](https://cdn.jsdelivr.net/gh/xyf1096415969/blogImgs@main/imgs/PHP第1篇1.png)

composer.lock也会多了一些信息【注意看此时版本号5.2.0】：

![PHP第1篇2](https://cdn.jsdelivr.net/gh/xyf1096415969/blogImgs@main/imgs/PHP第1篇2.png)

在vendor/symfony下面也会增加一个mailer文件夹。

好了，现在还看不出来效果，我稍微改造一下，把composer.json的`"symfony/mailer": "5.2"`改为`"symfony/mailer": "^5.2"`，仔细看多了个`^`符号，至于符号意义看下图：

![PHP第1篇3](https://cdn.jsdelivr.net/gh/xyf1096415969/blogImgs@main/imgs/PHP第1篇3.jpg)

然后执行update：

```
composer update symfony/mailer
```

执行update是通过composer.json文件的，结果会把版本变为5.4.7

![PHP第1篇4](https://cdn.jsdelivr.net/gh/xyf1096415969/blogImgs@main/imgs/PHP第1篇4.png)

我们先来看看两个文件的变化：

composer.json发现没有变化：

![PHP第1篇5](https://cdn.jsdelivr.net/gh/xyf1096415969/blogImgs@main/imgs/PHP第1篇5.png)

composer.lock发现版本号变为5.4.7了

![PHP第1篇6](https://cdn.jsdelivr.net/gh/xyf1096415969/blogImgs@main/imgs/PHP第1篇6.png)

问题一：为什么update之后不会变为最新版本6.1？

答：因为composer.json文件是^符号，主版本号5固定的，update就是匹配符合的最新版本，5.4.7是主版本号5最大的版本了。

然后我们再做个实验，把lock文件删除，json文件不变。把vendor/symfony下面mailer文件夹也删除。【或则直接`composer remove symfony/mailer`直接删，这样删json文件的`"symfony/mailer": "^5.2"`也会没掉，手动补上就行】

我们模拟场景：我现在的本地`symfony/mailer`版本是5.2.0,来了新同事，如果没有lock文件或则lock没有放到git版本库，那么新同事运行`composer install`的时候，是通过composer.json的，这个时候首先创建lock文件，再去下载库，但是他下载的最新版本的mailer库也就是5.4.7的【道理同问题一】，那这样不同人版本不同是非常混乱的。此时新同事的lock文件是这样的：

![PHP第1篇7](https://cdn.jsdelivr.net/gh/xyf1096415969/blogImgs@main/imgs/PHP第1篇7.png)

参考：https://packagist.org/packages/symfony/mailer

https://www.php.cn/tool/composer/432282.html

https://blog.csdn.net/Destiny_shine/article/details/109390424