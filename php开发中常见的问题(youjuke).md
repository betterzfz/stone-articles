#### 免责声明
* 本篇属笔者拙见，不一定完全正确。
* 具体代码及对应行数在笔者查看之后可能发生变化。

#### 具体问题
* `\bkadmin\Controller\SaleController.php`的`add_goods()`方法中，在`118`行：
```php
header("Content-Type:text/html;charset=utf8");
``` 
应该是：
```php
header('Content-Type: text/html; charset=utf-8');
```
* `\bkadmin\Controller\SaleController.php`的`add_goods()`方法中，在`120`行：
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
* `qbs_goods.unit`的备注是`计价单位`，但是页面上该字段显示的是`计量单位`。

* 在数据查询中应该尽量在条件中先过滤掉尽可能多的数据。