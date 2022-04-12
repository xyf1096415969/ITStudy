# Goland导入beego框架源码报红

最近在学习beego框架，使用Goland编辑器，发现框架的源码包导入了，可是都是报红色，Ctrl+左键也不能点进去看源码。如图：

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/08ac88acdf0b4d4e82ef045e1235950f~tplv-k3u1fbpfcp-zoom-1.image)

刚开始以为Go语言的环境安装问题，可是通过bee run命令启动项目，是可以启动的，如图：

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b73e42ff4d2d4083b5c2e11ef80317fe~tplv-k3u1fbpfcp-zoom-1.image)

通过浏览器访问http://127.0.0.1:8088/ 也是成功的：

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9a5ab17f7a7e4a74af0136adbce4feb8~tplv-k3u1fbpfcp-zoom-1.image)

这么一来  估计是编辑器的问题了，搜索了一下，发现一篇文章，大致说：Golang ide对go mod不感冒（至于go mod知识 可以看看[这篇文章](https://blog.csdn.net/qq_43442524/article/details/104906475)），会显示包没有引到

解决方法是加个代理，如图：

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fdd360c41fb0436abffdbfc2cf9214e8~tplv-k3u1fbpfcp-zoom-1.image)

成功之后，就可以正常引入：

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1d56a8d741494fc4898bbff2053835e1~tplv-k3u1fbpfcp-zoom-1.image)



参考文章：https://blog.csdn.net/u013900530/article/details/103178029

​         https://blog.csdn.net/qq_43442524/article/details/104906475