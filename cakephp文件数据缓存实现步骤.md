#### 环境
笔者在`cakephp 2.4.6`中进行的实践，不同版本可能稍有出入。
#### 配置
* 将`./Config/core.php`中`Configure::write('Cache.disable', true);`一行注释。
* 在`./Config/bootstrap.php`中添加如下代码：
```php
// 添加一个cakephp缓存配置
Cache::config('article', [ // article是缓存配置名称
    'engine' => 'File', // 使用文件缓存
    'duration' => '+6 hours', // 缓存有效时间
    'path' => CACHE.'article'.DS, // 缓存文件保存目录 如果该文件夹无法自动创建需要手动创建并保证可写
]);
```

#### 使用
* 缓存读取方法：
```php
Cache::read('cache_key', 'article');
```
其中`cache_key`根据需要具体设置，缓存会保存在缓存配置中指定目录下以该值为名的文件中。`article`就是缓存配置中设置的缓存配置名称。
* 缓存写入方法：
```php
Cache::write('cache_key', $data, 'article');
```
其中`cache_key`根据需要具体设置，缓存会保存在缓存配置中指定目录下以该值为名的文件中。`$data`是要缓存的数据，`article`就是缓存配置中设置的缓存配置名称。