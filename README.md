php_crontab 
=============
基于php pcntl的定时任务管理器，支持秒级别的定时任务

特性
---------------
+ 通过配置文件管理所有定时任务
+ 支持秒级的定时任务粒度
+ 使用symfony/process进行进程管理
+ 使用React/event-loop执行事件循环

执行
---------------
启动crond
```shell
php bin/crond.php
```
在后台启动crond
```shell
nohup php bin/crond.php > /dev/null 2>&1 &
```

发送USR1信号，安全关闭crond
主进程会等待所有的子进程任务结束，才会正式退出
```shell
kill -USR1 `cat logs/crond.pid`
```


配置
---------------
任务配置文件config/task.php
```php
/**
 * task配置文件
 * 例子：
 * 'process_a' => [
 *      'daemon' => '* * * * * *',//秒 分 时 日 月 周
 *      'filename' => '/usr/local/php/bin/php', //执行程序
 *      'params' => [],//执行程序参数
 *      'single' => true,//如果进程在运行，则不执行，只保持一个进程
 *      'standard_ouput' => '', //标准输出
 *      'error_output' => '', // 错误输出
 *  ]
 */

return [
    'process_a' => [
        'daemon' => '*/3 * * * * *',
        'filename' => '/usr/local/php-5.6.30/bin/php',
        'params' => ['/www/tests/pcntl/examples/a.php'],
        'single' => true,
        'standard_ouput' => '/www/tests/pcntl/examples/a.log',
        'error_output' => '/www/tests/pcntl/examples/a.log',
    ]
];
```