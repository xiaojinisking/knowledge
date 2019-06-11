# 调试工具Laravel-debugbar

工具非适合于API的开发，但是适合于带有view的开发
API的调试可以使用工具Laravel Telescope

## 安装
---

composer 安装包
```
composer require barryvdh/laravel-debugbar --dev
```

laravel5.5以上版本已经使用了包自动发现了，而不需要加入到ServiceProvider内

若不需要自动发现可以加入到config/app.php

```
Barryvdh\Debugbar\ServiceProvider::class,
```

如果想要使用日志门面,添加门面到app.php内
```
'Debugbar' => Barryvdh\Debugbar\Facade::class,
```
若不使用门面可以使用,
```
App::make('debugbar');
```



发布配置文件
```
php artisan vendor:publish --provider="Barryvdh\Debugbar\ServiceProvider"
```
