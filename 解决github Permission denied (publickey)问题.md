与github交互时可能会报如下错误：

![fatal_info.png](http://upload-images.jianshu.io/upload_images/2050891-8e95d4cacce3836e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
原因是SSH keys没有设置或者过期了，SSH keys 可以在没有密码的情况下信任当前工作的计算机。
解决办法是生成并设置SSH keys，具体步骤如下：
* 通过命令`cd ~/.ssh`切换到当前计算机当前用户的.ssh目录下：
 
![cd ~/.ssh.png](http://upload-images.jianshu.io/upload_images/2050891-bf5af8048b92fbc3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* 通过命令`ssh-keygen`生成SSH key：

![ssh-keygen.png](http://upload-images.jianshu.io/upload_images/2050891-9eb011012ab2e75d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

windows下建议通过git bash进行否则可能会报错：

![ObjectNotFound.png](http://upload-images.jianshu.io/upload_images/2050891-5c1358c67aeb91ed.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* 根据命令`ssh-keygen`提示输入key要存储的位置以及密码：

![save-key.png](http://upload-images.jianshu.io/upload_images/2050891-9d45a88f33d6afc8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 生成key后在指定要存储的地方找到`id_rsa.pub`：

![id_rsa.pub.png](http://upload-images.jianshu.io/upload_images/2050891-e3794ac4ea279761.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* 在github的Settings中新建一个SSH key，将`id_rsa.pub`中的内容复制到Key中：

![new-key.png](http://upload-images.jianshu.io/upload_images/2050891-daa70cb27b789168.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 配置git的用户名和邮箱：

![git-config.png](http://upload-images.jianshu.io/upload_images/2050891-57a89022bed93969.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* 通过命令`ssh-add`解决每次操作都需要输入key的密码的问题：

![ssh-add.png](http://upload-images.jianshu.io/upload_images/2050891-b604b90361000206.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
以上操作均在windows 10 上进行，linux和mac os下的解决方案类似，不赘述了。
问题解决完毕，可以愉快地玩耍了！
