### 前言
最近在做php脚本执行任务时，需要调用python的脚本来完成消息推送！但是这里有一个比较大的坑！就是php执行的用户默认是服务器用户（www-data或apache或nginx），如果执行python脚本的话就会报`ModuleNotFoundError: No module named '模块名'`，原因是网络服务器用户没有python的环境，那怎么解决呢？
### 解决方法：
可以参考我的代码：
* 把`user`这个用户换成你可以正常运行python脚本的用户；
* ` 2>&1 | tee -a /var/www/html/test.log` 这行代码是把标准错误转换成标准输出，方便查看脚本运行的错误信息以方便问题定位。
```php
<?php
$name = $_POST['name'];
shell_exec("sudo su - user -c 'python3 /var/www/html/test.py $name' . 2>&1 | tee -a /var/www/html/test.log");
exit;
?>
```
