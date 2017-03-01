##### /phpcms/modules/member/index.php phpcms注入漏洞修复 
定位到/phpcms/modules/member/index.php 610 行 
源码：
```php
$password = isset($_POST['password']) && trim($_POST['password']) ? trim($_POST['password']) : showmessage(L('password_empty'), HTTP_REFERER);
```
修改为:
```php
$password = isset($_POST['password']) && trim($_POST['password']) ? addslashes(urldecode(trim($_POST['password']))) : showmessage(L('password_empty'), HTTP_REFERER);
```
##### /api/phpsso.php phpcms注入漏洞修复 
定位到/api/phpsso.php 118行 
源码:
```php
$phpssouid = $arr['uid'];
```
修改为:
```php
$phpssouid = intval($arr['uid']);
```
##### /phpcms/modules/content/down.php前台注入导致任意文件读取漏洞修复
定位到/phpcms/modules/content/down.php 17行
源码:
```php
parse_str($a_k);
```
修改为:
```php
$a_k = safe_replace($a_k); parse_str($a_k);
```
定位到/phpcms/modules/content/down.php 89行
源码:
```php
parse_str($a_k);
```
修改为:
```php
$a_k = safe_replace($a_k); parse_str($a_k);
```
定位到/phpcms/modules/content/down.php 120行
源码:
```php
file_down($fileurl, $filename);
```
修改为:
```php
$fileurl = str_replace(array('<','>'), '', $fileurl); file_down($fileurl, $filename);
```
##### /phpcms/modules/pay/respond.php phpcmsv9宽字节注入
定位到/phpcms/modules/pay/respond.php 16行
源码:
```php
$payment = $this->get_by_code($_GET['code']);
```
修改为:
```php
$payment = $this->get_by_code(mysql_real_escape_string($_GET['code']));
```
##### phpsso_server/phpcms/modules/phpsso/index.php phpcms注入漏洞
定位到phpsso_server/phpcms/modules/phpsso/index.php 424行
源码:
```php
$applist = getcache('applist', 'admin');
```
再其下面添加代码:
```php
foreach ($applist as $key => $value) {
    unset($applist[$key]['authkey']);
}
```
##### api/get_menu.php phpcms authkey泄漏漏洞
定位到api/get_menu.php 28行
源码：
```php
$cachefile = str_replace(array('/', '//'), '', $cachefile);
```
修改为
```php
$cachefile = str_replace(array('/', '//', '\\'), '', $cachefile);
```
##### phpcms/modules/poster/poster.php 文件中，未对输入参数$_GET['group']进行严格过滤，导致注入漏洞
定位到/phpcms/modules/poster/poster.php 219行
源码:
```php
if ($_GET['group']) {
```
上面加一行为:
```php
$_GET['group'] = addslashes(urldecode(trim($_GET['group'])));
if ($_GET['group']) {
```