[教程](https://blog.csdn.net/qq_27295403/article/details/82967600)


## generator生成器，便捷生成一连串的文件
app/config/repository.php

默认配置
```
'generator'  => [
        'basePath'      => app()->path(),
        'rootNamespace' => 'App\\',
        'stubsOverridePath' => app()->path(),
        'paths'         => [
            'models'       => 'Models',
            'repositories' => 'Repositories\\Eloquent',
            'interfaces'   => 'Contracts\\Repositories',
            'transformers' => 'Transformers',
            'presenters'   => 'Presenters',
            'validators'   => 'Validators',
            'controllers'  => 'Http/Controllers',
            'provider'     => 'RepositoryServiceProvider',
            'criteria'     => 'Criteria'
        ]
    ]
```


可调整为

```
generator'  => [
        'basePath'      => app()->path(),
        'rootNamespace' => 'App\\',
        'stubsOverridePath' => app()->path(),
        'paths'         => [
            'models'       => 'Models',
            'repositories' => 'Repositories\\Eloquent',
            'interfaces'   => 'Contracts\\Repositories',
            'transformers' => 'Transformers',
            'presenters'   => 'Presenters',
            'validators'   => 'Validators',
            'controllers'  => 'Http/Controllers',
            'provider'     => 'RepositoryServiceProvider',
            'criteria'     => 'Criteria'
        ]
    ]
```

如果上面某学paths进行注释那么命令器将不自动生成对应的目录


### 相关命令
* 生成一切模型

```
php artisan make:entity Post
```
* controller 控制器
* Validator 验证器
* Model 模型
* Repository 仓库
* Presenter 呈现器，非必须
* Transform 转换器，非必须
