可以通过函数php_sapi_name()获取web服务器与php间的接口类型，有如下代码：
```php
<?php
    echo php_sapi_name();
```
若通过浏览器访问可得：

![浏览器访问.png](http://upload-images.jianshu.io/upload_images/2050891-323e7ee43106dd8d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

若通过命令`php.exe D:\www\test\t1.php`访问可得：


![命令行访问.png](http://upload-images.jianshu.io/upload_images/2050891-49e5d1ab26df3fd8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

函数php_sapi_name()可能返回的值包括但不限于aolserver, apache, apache2filter, apache2handler, caudium, cgi, cgi-fcgi, cli, cli-server, continuity, embed, fpm-fcgi, isapi, litespeed, milter, nsapi, phttpd, pi3web, roxen, thttpd, tux, webjames等。