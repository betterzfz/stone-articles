homebrew 是 mac os 下常用的包管理工具，在使用`brew`命令时可能会遇到如下问题：

![error_info.png](http://upload-images.jianshu.io/upload_images/2050891-3440ac9984c5580d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

该问题可能是由于`/usr/local`的权限造成的，可能跟 os x 升级到 ei capitan 有关系。可以通过下面的一组命令进行解决：
```
chown -R $(whoami):admin /usr/local
cd $(brew --prefix)
git fetch origin
```
执行过程如下图：

![change_access.png](http://upload-images.jianshu.io/upload_images/2050891-2b7e3c4825bbf6c3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![git_fetch_origin.png](http://upload-images.jianshu.io/upload_images/2050891-ba273d3b74af91ce.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

以上过程除了修改了`/usr/local`的权限，还同步了远程分支。再次执行`brew update`如图：


![brew_update.png](http://upload-images.jianshu.io/upload_images/2050891-4a185a59848436ff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)