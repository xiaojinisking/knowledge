# phpstrom 编辑器 帮助工具

[barryvdh/laravel-ide-helper](https://packagist.org/packages/barryvdh/laravel-ide-helper)


## 安装
---
1:使用 Composer 安装该扩展包只安装到dev

```
composer require --dev barryvdh/laravel-ide-helper
```

2:安装完成后，在 config/app.php 添加以下内容到 providers 数组。

```
Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider::class,
```

## 使用
---
1：生成Facades的文档

```
php artisan clear-compiled
php artisan ide-helper:generate
```


2:生成model的文档

先安装扩展
```
composer require --dev doctrine/dbal
```

执行命令
```
php artisan ide-helper:models [Post]
```


3：PhpStorm Meta for Container instances

```
php artisan ide-helper:meta
```

## 最终产物
根目录生成下面三个文件，需要加入到.gitignore 文件中
```
_ide_helper.php
_ide_helper_models.php
.phpstorm.meta.php
```
