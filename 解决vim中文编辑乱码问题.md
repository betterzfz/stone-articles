在使用vim进行文档编辑时会遇到中文乱码的情况，比如：

![有乱码.png](http://upload-images.jianshu.io/upload_images/2050891-4b5584773100c7b2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在文档中输入`你好`，出现了乱码。

解决办法是打开或新建文件`~/.vimrc`，输入如下内容：
```php
set fileencodings=utf-8,ucs-bom,gb18030,gbk,gb2312,cp936
set termencoding=utf-8
set encoding=utf-8
```
保存后重新编辑文档就可以正常输入中文了，如图：

![无乱码.png](http://upload-images.jianshu.io/upload_images/2050891-975d672be9819e96.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)