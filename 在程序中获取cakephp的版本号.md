使用cakephp开发过程中会出现需要获取当前的版本号的情况，但是通常情况下程序员没有服务器权限，这个时候可以通过程序去获取，代码如下：
```php
echo Configure::version();
```
可能得到如下结果：

![cakephp版本号.png](http://upload-images.jianshu.io/upload_images/2050891-d975a668553b7aff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)