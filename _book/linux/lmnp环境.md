# lmnp环境

## www用户的创建
```
id www   //检查www用户是否存在
groupadd www  //创建www用户组
useradd -g www  -s /sbin/nologin www
id www   //检查下www是否成功
```


## 修改nginx 和 php都用户组
* nginx.conf下
```
user www
```
* /etc/php-fpm.d/www.conf下的

  ```
  user = ****

  group = ****
  ```
修改上面之后，重启nginx和php-fpm，ps -ef|grep … 会发现应用的用户和用户组变了，但是这还能算万事大吉

还需要修改下面的一些目录下的权限

>nobody就是一个普通账户，因为默认登录shell是/sbin/nologin,所以这个用户是无法直接登录系统的，也就是黑客很难通过漏洞连接到你的服务器来做破坏。此外这个用户的权限也给配置的很低。因此有比较高的安全性。一切都只给最低权限。这就是nobody存在的意义.

* nginx的日志目录 /var/log/nginx ，chown -R nobody:nobody /var/log/nginx 否则不能读写access.log 和error.log

* 还有 上传文件用到的一个路径 /var/lib/nginx/tmp/ ， chown -R nobody:nobody /var/lib/nginx 否则上传文件出错，因为保存不了上传的临时文件

* session存储路径也要给权限
在php-fpm的配置文件中查找到session的目录

```
php_value[session.save_handler] = files
php_value[session.save_path] = /var/lib/php/session
```

调整权限

```
chown -R nobody:nobody /var/lib/php/session
```
