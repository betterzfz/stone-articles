大部分人会使用数组`array_unique()`来去除数组中的重复值，这么干是可以达到目的，不过比较低效，更为高效的方法是使用函数`array_flip()`。例证：
```php
$arr_origin = ['China', 'America', 'Russia', 'England', 'France'];
$arr_test = [];
for ($i = 0;$i < 100;$i++ ) {
        $arr_test[] = $arr_origin[rand(0, 4)];
}

$time_start_one = microtime(true);
for ($i = 0;$i < 10000;$i++) {
        array_unique($arr_test);
}
$time_end_one = microtime(true);
echo 'array_unique()所用时间为：'.($time_end_one - $time_start_one);

echo '<br />';

$time_start_two = microtime(true);
for ($i = 0;$i < 10000;$i++) {
        array_flip(array_flip($arr_test));
}
$time_end_two = microtime(true);
echo 'array_flip()所用时间为：'.($time_end_two - $time_start_two);
```
运行以上代码三次得到结果为：

![第一次.png](http://upload-images.jianshu.io/upload_images/2050891-a6bf9c4b60d4ecf9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![第二次.png](http://upload-images.jianshu.io/upload_images/2050891-d14bc81127d96c45.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![第三次.png](http://upload-images.jianshu.io/upload_images/2050891-ca0d734f56cf2bf3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

有兴趣的同学可以多运行几次，结果基本是稳定的，毫无疑问我们可以得出结论：`array_flip()`要比`array_unique()`更高效。

为什么`array_flip()`要比`array_unique()`更高效呢？笔者没有去研究源代码，个人觉得跟这两个函数的运行机制有关系。`array_flip()`是将数组的键和值相互交换，在php中数组的键是不能重复的，如果重复则后面的元素会覆盖前面的元素，这样就把原来值重复的项只保留了最后一个，再次调用`array_flip()`函数将键和值再次交换得到去除重复值的数组。`array_unique()`去除数组重复值应该是用到了遍历，所以它的效率会比`array_flip()`差很多。