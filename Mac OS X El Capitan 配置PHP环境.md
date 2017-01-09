要配置的PHP环境是Apache,PHP和MySQL的软件组合，由Apache提供服务，MySQL提供数据库支持，PHP作为开发语言，在此基础上开发人员可以进行PHP脚本的编写与运行。

在Mac OS X中，Apache是自带的，只需要对其进行配置，让它能够支持PHP的运行就可以了。具体步骤如下：
* 打开命令行终端，输入如下命令：
```
sudo su -
```
这样就可以以root用户的角色来执行之后的命令，以此来保证每条命令的执行都不会有权限问题。
* 确保Apache是在运行中，可以执行如下命令：
```
apachectl start
```
可以打开浏览器，访问[localhost](http://localhost/ 'localhost')来判断Apache是否已经正常启动。
* 配置Apache来让其能够支持PHP的运行
1 执行如下命令：
```
cd /etc/apache2/
vim httpd.conf
```
找到Apache以模块方式加载PHP的配置，如下：
```
#LoadModule php5_module libexec/apache2/libphp5.so
```
把前面的#去掉，如下：
```
LoadModule php5_module libexec/apache2/libphp5.so
```
因为Apache默认的文档目录在`/Library/WebServer/Documents`，所以需要根据实际情况来决定是否修改该配置。如果修改的话在`httpd.conf`中找到`DocumentRoot`，将值改为需要的值，同时需要修改的是`DocumentRoot`对应的`Directory`，以我本地为例：
```
DocumentRoot "/Users/betterzfz/sites"
<Directory "/Users/betterzfz/sites">
    #
    # Possible values for the Options directive are "None", "All",
    # or any combination of:
    #   Indexes Includes FollowSymLinks SymLinksifOwnerMatch ExecCGI MultiViews
    #
    # Note that "MultiViews" must be named *explicitly* --- "Options All"
    # doesn't give it to you.
    #
    # The Options directive is both complicated and important.  Please see
    # http://httpd.apache.org/docs/2.4/mod/core.html#options
    # for more information.
    #
    Options FollowSymLinks Multiviews
    MultiviewsMatch Any

    #
    # AllowOverride controls what directives may be placed in .htaccess files.
    # It can be "All", "None", or any combination of the keywords:
    #   AllowOverride FileInfo AuthConfig Limit
    #
    AllowOverride None

    #
    # Controls who can get stuff from this server.
    #
    Require all granted
</Directory>
```
2 重启Apache
```
apachectl restart
```
3 在项目目录中创建文件`index.php`，内容如下：
```php
<?php
    phpinfo();
```
4 打开浏览器访问[http://localhost/index.php](http://localhost/index.php 'localhost')，如能看到PHP的信息则表示已经配置成功。
5可以继续配置Apache让访问[http://localhost/](http://localhost/ 'localhost')时就能默认访问目录下的`index.php`，配置如下：
```
<IfModule dir_module>
    DirectoryIndex index.php index.html
</IfModule>
```
记住需要重启Apache，否则新的配置不会生效。

安装MySQL，到MySQL官网[下载](http://dev.mysql.com/downloads/mysql/)，按照Mac OS下DMG安装包的安装流程安装MySQL。安装完成后，用户root会得到一个为空的默认密码。正常情况下MySQL会被安装到`/usr/local/mysql`目录下，为了方便使用MySQL命令，可以将`/usr/local/mysql/bin`加到PATH：
```
export PATH=/usr/local/mysql/bin:$PATH
```
这样就可以在任意目录下使用MySQL命令了。

MySQL安装好后就可以用PHP对其进行连接了，修改`index.php`为如下内容：
```php
<?php
    $mysqli = new mysqli('localhost', 'root', '');
    if ($mysqli->connect_error) {
            die('connect error('.$mysqli->connect_errno.')'.$mysqli->connect_error);
    }
    echo 'success... '.$mysqli->host_info;
    $mysqli->close();
```
刷新[http://localhost/index.php](http://localhost/index.php 'localhost')后将得到下图显示的内容：
![PHP连接MySQL.png](http://upload-images.jianshu.io/upload_images/2050891-6488593bc55d24b5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果连接没有成功则需要根据提示进行相应的调整了。