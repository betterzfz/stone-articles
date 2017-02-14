#### 说明
* 本文属笔者拙见，不一定完全正确。
* 部分问题针对的是特定的php开发框架。
* 本文提到的问题都是在开发中会多次出现或有很多类似的情况。

#### 具体问题
* 指定页面编码方式：
```php
header("Content-Type:text/html;charset=utf8");
``` 
应该是：
```php
header('Content-Type: text/html; charset=utf-8');
```
* 接收请求数据应该做安全检查和处理，而不需要增加变量，可以直接使用。如果非要增加变量也没有必要加上无意义的遍历，如：
```php
$s_data = array();
$flag_g = $flag_gc = false; # 记录事务        
foreach ($_POST as $k => $v) {
    $s_data[$k] = $v;
}
```
可以改为：
```php
$s_data = $_POST;
$flag_g = $flag_gc = false; # 记录事务
```
需要说明的是上面的做法依然不科学，因为没有必要将`$_POST`赋值给`$s_data`，直接使用`$_POST`即可。当然如果要与数据库交互，在使用`$_POST`的时候要先对数据进行过滤和转义，如果仅仅是根据`$_POST`数据进行数据库查询，可以使用下面类似的方法处理：
```php
/**
 * 执行查询sql语句之前的数据过滤
 * @param $data 需要过滤的数据 数组
 * @return 过滤的结果 数组
 * @author stone
 */
public function filter_data_before_query($data){
    if (!empty($data) && is_array($data)) {
        foreach ($data as $key => $value) {
            if (!is_array($value) && !is_object($value)) {
                $data[$key] = htmlspecialchars(trim($data[$key]));
                if (!get_magic_quotes_gpc()) {
                    $data[$key] = addslashes($data[$key]);
                }
                if (is_int($value)) {
                    $data[$key] = intval($data[$key]);
                }
            }
        }
        return $data;
    } else {
        return ['code' => -1, 'message' => '提供的参数需要为数组'];
    }
}
```
* 数据库字段的备注应尽量和页面上使用的文本保持一致。

* 在数据查询中应该尽量在条件中先过滤掉尽可能多的数据。

* 在`html`页面中进行注释的时候，大部分程序员会忽略嵌入`html`中的`php`代码，如：
```php
<!--<li>
    <a href="#systemSetting" class="nav-header" data-toggle="collapse">
        <i class="glyphicon glyphicon-cog"></i>
        爆款管理
           <span class="pull-right glyphicon glyphicon-chevron-down"></span>
    </a>
    <ul id="systemSetting" class="nav nav-list secondmenu">
        <li class="<?php if($current == 'pastblast'){echo 'active';}?>"><a href="/blast/pastblast"><i class="glyphicon glyphicon-th-list"></i> 爆款总览</a></li>
        <li class="<?php if($current == 'newblast'){echo 'active';}?>"><a href="/blast/newblast"><i class="glyphicon glyphicon-plus"></i> 新建爆款</a></li>
    </ul>
</li>-->
```
如果真的要注释，那么应该这样做：
```php
<!--<li>
    <a href="#systemSetting" class="nav-header" data-toggle="collapse">
        <i class="glyphicon glyphicon-cog"></i>
        爆款管理
           <span class="pull-right glyphicon glyphicon-chevron-down"></span>
    </a>
    <ul id="systemSetting" class="nav nav-list secondmenu">
        <li class="<?php //if($current == 'pastblast'){echo 'active';}?>"><a href="/blast/pastblast"><i class="glyphicon glyphicon-th-list"></i> 爆款总览</a></li>
        <li class="<?php //if($current == 'newblast'){echo 'active';}?>"><a href="/blast/newblast"><i class="glyphicon glyphicon-plus"></i> 新建爆款</a></li>
    </ul>
</li>-->
```
当一段代码不再使用的时候，应该及时去掉，如果在可以预见的将来会再次用到这段代码那也应该连`php`代码一起注释掉。

* 的`edit_goods()`方法中，在`567`行：
```php
$s_data['modified']    = date("Y-m-d H:i:s");
```
这行代码其实是没有必要的，当使用`cakephp`的`ORM`提供的方法进行数据的添加和修改时，默认会使用`created`和`modified`来作为记录的创建时间和修改时间，如果数据表中也确实是使用者两个英文单词作为创建时间和修改时间的话，则ORM会在数据添加或修改时自动把当前时间作为`created`或`modified`的值。

* 当使用`cakephp`作为开发框架的时候，`created`和`modified`是数据库中保存记录的创建时间和修改时间的默认字段名，目前存在许多数据表使用了各式各样记录的创建时间和修改时间的字段名，这并不耽误使用和功能的实现，但在使用`cakephp`作为开发框架的情况下，`created`和`modified`始终是更好的选择。
* 没有使用模板引擎的项目中，在模板文件里面进行逻辑判断时很多开发这会使用`echo`或`print`进行文本输出，如：
```php
<h1 <?php if ($i > 0) { echo 'class="good"'; } ?>>hello</h1>
```
这么做在功能上不会有问题，但是跳出`php`解析模式会更有效率，更好的做法如下：
```php
<h1 <?php if ($i > 0): ?>class="good"<?php endif; ?>>hello</h1>
```
* 很多开发者喜欢使用`php`短标记，如`<?= 'hello' ?>`，虽然在`php5.4`不需要配置即可使用该特性，但是在未来的版本中`php`可能移除该特性，所以`<?php echo 'hello'; ?>`这种始终会被支持的写法是更好的选择。