通过curl对接口发起post请求的时候很少会遇到请求数据是二维数组的情况，一般情况下只需要按照正常的方式发送请求就可以了，可能的代码如下：
```php
$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, $url);
curl_setopt($ch, CURLOPT_HEADER, FALSE);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, $data);

$result = curl_exec($ch);

if ($result) {
    curl_close($ch);

    $res_data = json_decode($result, true);

    echo '<pre>';
    print_r($res_data);
    echo '</pre>';

} else {
    $error = curl_errno($ch);

    echo 'curl出错，错误码('.$error.')';

    curl_close($ch);
}
```
但是当请求的数据是`$data`是二维数组的时候，php就会提示Array to string conversion，这个时候需要使用函数`http_build_query()`来处理`$data`，调整后的代码为：
```php
$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, $url);
curl_setopt($ch, CURLOPT_HEADER, FALSE);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($data));

$result = curl_exec($ch);

if ($result) {
    curl_close($ch);

    $res_data = json_decode($result, true);

    echo '<pre>';
    print_r($res_data);
    echo '</pre>';

} else {
    $error = curl_errno($ch);

    echo 'curl出错，错误码('.$error.')';

    curl_close($ch);
}
```
这样提示信息就不会再出现了。