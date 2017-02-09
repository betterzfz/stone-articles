用 cakephp 开发中会遇到`save()`保存不成功又没有报错的问题，这时`save()`会返回`false`，但没有提示信息，很难排查问题，这时可通过`invalidFields()`定位数据保存失败的原因。php 示例代码如下：
```php
$save_model_res = $this->Model->save($datas);
echo '<pre>';
var_dump($save_model_res);
echo '</pre>';
echo '<pre>';
print_r($this->Model->invalidFields());
echo '</pre>';
exit;
```
这个方法主要是针对有字段验证的`model`在保存数据时字段验证没有通过的问题，如果在特定条件下不需要进行字段验证了可以通过`validate`参数去设置。php 示例代码如下：
```php
$save_model_res = $this->Model->save($datas, ['validate' => false]);
echo '<pre>';
var_dump($save_model_res);
echo '</pre>';
exit;
```