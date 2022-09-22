# nginx配置跨域在苹果6上无效

之前做的一个项目，前后端分离的，laravel8框架，但是在不久前，测试人员发现一个奇怪问题：出现跨域。因为我们之前已经在nginx上配置了：

```
add_header Access-Control-Allow-Origin *;
```

这些跨域的措施，但是就是行不通，而且发现其他手机都是正常的，就是苹果6这个机型报跨域。

所以没办法，只能想个其他方法。我们项目都在一个目录下的：

![第一篇1](https://cdn.jsdelivr.net/gh/xyf1096415969/blogImgs@main/imgs/Nginx/第一篇1.png)

第一个是后端项目，其他两个是前端项目。

**解决思路就是前后端使用同一个域名**。

配置过程如下：

1先在后端项目的入口文件`index.php`的同级目录下面，新增两个软连接

![第一篇2](https://cdn.jsdelivr.net/gh/xyf1096415969/blogImgs@main/imgs/Nginx/第一篇2.png)

如图所示.新增两个软连接admin和wap，这两个软连接分别指向对应前端项目的dist目录

2.然后我们把nginx配置改一下，新增两个`location`配置：

```
server {
   listen 80;
   server_name test.admin.com test-api.admin.com;
   root /home/jssclubetc/jssclubetc-admin/public;

   index index.html index.htm index.php;

   charset utf-8;

   location / {
        try_files $uri $uri/ /index.php?$query_string;
   }

   location /admin {
        try_files $uri $uri/ /admin/index.html?$query_string;
   }

   location /wap {
        try_files $uri $uri/ /wap/index.html?$query_string;
   }

   location = /favicon.ico { access_log off; log_not_found off; }
   location = /robots.txt  { access_log off; log_not_found off; }

   error_page 404 /index.php;

   access_log /mnt/log/nginx/etc.log;
   error_log  /mnt/log/nginx/etc.log;

   location ~ \.php$ {
        fastcgi_pass unix:/var/run/php-fpm.sock;
   }
} 
```

新增了`location /admin{}`和`location /wap{}`这两个。以`location /wap{}`为例简单说明一下：当用户输入`test.admin.com/wap`URL的时候会自动定位到这个location里面，去运行`/wap/index.html?$query_string`这个`wap`就是对应软连接的名字。