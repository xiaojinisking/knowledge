[来源](https://learnku.com/articles/25947#fcb0fe)
# horizon 管理异步队列

## 用途
开发中，我们也经常需要使用异步队列，来加快我们的响应速度。比如发送短信，发送验证码等。但是队列执行结果的成功或者失败只能通过日志来查看。这里，我们使用 horizonl 来管理异步队列，完成登陆和刷新 token 时，将 token 存入 last_token 的因为放在异步完成。

>Horizon 提供了一个漂亮的仪表盘，并且可以通过代码配置你的 Laravel Redis 队列，同时它允许你轻易的监控你的队列系统中诸如任务吞吐量，运行时间和失败任务等关键指标。

[手册](https://laravel.com/docs/5.8/horizon)
## 安装
>由于 Horizon 中使用了异步处理信号，所以安装扩展包需要 PHP 在 7.1 以上。其次，你应该确保 queue 配置文件中设置了 redis 队列驱动。

你应该使用 Composer 来安为你的 Laravel 项目安装 Horizon：

```
composer require laravel/horizon
```

安装完成后，使用 vendor:publish 发布 Artisan 命令：

```
php artisan vendor:publish --provider="Laravel\Horizon\HorizonServiceProvider"
```
### 配置
发布 Horizon 相关文件后，他的主要配置文件会放在 config/horizon.php。你可以在这个文件中配置队列相关选项，并且每个配置项都有详细的使用说明，请详细阅读此文件。

#### 均衡配置
Horizon 提供了三种均衡策略：simple，auto， 和 false。默认的是 simple , 会将收到的任务均分给队列进程：
```
'balance' => 'simple',
```

auto 策略会根据当前的工作量调整每个队列的工作进程任务数量。例如：如果 notifications 队列有 1000 个待执行任务，但是你的 render 队列是空的，Horizon 会分配更多的工作进程给 notifications 队列，直到 notifications 队列中所有任务执行完成。当配置项 balance 配置为 false 时，Horizon 会使用 Laravel 默认执行行为，它将按照配置中列出的顺序处理队列任务。

### 仪表盘权限验证
Horizon 仪表盘路由是 /horizon。 默认情况下，你只能在 local 环境下访问仪表盘。我们可以使用 Horizon::auth 方法给仪表盘定义更具体的访问策略。auth 方法接收一个回调函数作为参数，该回调函数应当返回 true 或 false，以确认用户是否具有仪表盘的访问权限。通常情况下，你应当在 AppServiceProvider 的 boot 方法中调用 Horizon::auth：

```
Horizon::auth(function ($request){
    if(env('APP_ENV','local')){
        return true;
    }else{
        $get_ip = $request->getClientIp();
        $can_ip=env('HORIZON_IP','127.0.0.1');
        return $get_ip == $can_ip;
    }
});
```


### 运行 Horizon
```
php artisan horizon               //启动horizon
php artisan horizon:pause         //暂停任务队列
php artisan horizon:continue      //继续任务队列
php artisan horizon:terminate     //优雅的终止 Horizon 主进程,会把正在执行的任务处理完毕后退出：
```

### 部署 Horizon
如果你将 Horizon 部署到线上服务器时，则需要配置一个进程监控器来检测 php artisan horizon 命令，在它意外退出时自动重启。上线新代码时则需要该进程监控器终止 Horizon 进程并以修改后的代码重启 Horizon。


#### 方法1：Supervisor 配置
```
[program:horizon]
process_name=%(program_name)s
command=php /home/forge/app.com/artisan horizon
autostart=true
autorestart=true
user=forge
redirect_stderr=true
stdout_logfile=/home/forge/app.com/horizon.log
```
#### 方法2：使用systemd运行 Horizon   [博客](https://learnku.com/articles/10442/running-horizon-with-systemd)
我们创建的 service 文件会放在 /etc/systemd/system/ 目录下
```
vim /etc/systemd/system/horizon.service
```
以下是写好的 service 文件，⚠️注意修改工作目录

```
[Unit]
# 描述
Description=Laravel Horizon
# 表明本服务要在 mysql 和 redis 之后启动，Laravel 依赖 mysql，Horizon 依赖 redis
After=mysqld.service redis-server.service

[Service]
# !!!这里修改为laravel项目根目录
WorkingDirectory=/project/laravel/root
# 这里可以指定运行的用户
User=www
Group=www
# 启动命令，php 建议使用绝对路径
ExecStart=/usr/bin/php artisan horizon
# 停止命令，使用 horizon 提供的优雅停止方法
ExecStop=/usr/bin/php artisan horizon:terminate
# 可以控制服务在什么情况下重新启动，这里设置为异常退出时重新启动
Restart=on-failure
# 重新启动的前等待的时间
RestartSec=30s
# 指定正确退出的代码，一些没有处理 TERM 信号的程序退出代码会是 143 ，Horizon 的退出代码是 0
SuccessExitStatus=0

[Install]
# 指定在 多用户 模式下启动，就是一般的命令行模式啦，也包括图形界面模式
WantedBy=multi-user.target
```
然后重新加载配置文件
```
systemctl daemon-reload
```
现在来启动服务：
```
systemctl start horizon.service
```

正常启动之后是不会有输出的，我们可以通过 status 命令来查看状态：
```
systemctl status horizon.service
```

如果更新了代码，我们可以使用 restart 命令来重启 Horizon 进程，使用新的代码运行：
```
systemctl restart horizon.service
```

使用 enable 命令可以指定服务开机启动：
```
systemctl enable horizon.service
```

这样下次重启的时候 Horizon 就自己启动啦～

如果不再需要自动启动，那就用 disable 命令：
```
systemctl disable horizon.service
```
当然 disable 命令并不会马上停止正在运行的服务，它只是让服务在下次开机的时候不自动启动，我们可以使用 stop 命令来直接停止正在运行服务：

当然 disable 命令并不会马上停止正在运行的服务，它只是让服务在下次开机的时候不自动启动，我们可以使用 stop 命令来直接停止正在运行服务：
```
systemctl stop horizon.service
```
最后还有个查看 systemd 的日志的命令 journalctl
我们可以这样查看一个服务输出的日志：

```
journalctl -u horizon.service
```

加上参数 -f 可以看到实时输出的日志。
```
journalctl -u horizon.service -f
```
