### PHP composer

#### Установка с командной строки
Для быстрой установки Composer в текущем каталоге, выполните в терминале последовательно следующие команды:
```bash
# 1.a Загружается установщик в текущий каталог
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"

# Выполняется проверка SHA-384 установщика
php -r "if (hash_file('sha384', 'composer-setup.php') === 'c8b085408188070d5f52bcfe4ecfbee5f727afa458b2573b8eaaf77b3419b0bf2768dc67c86944da1544f06fa544fd47') { echo 'Installer verified'.PHP_EOL; } else { echo 'Installer corrupt'.PHP_EOL; unlink('composer-setup.php'); exit(1); }"

# Запуск установщика
php composer-setup.php

# Удаление установщика
php -r "unlink('composer-setup.php');"
```

...если возникла ошибка при скачивании файла на шаге 1.а, скачиваем с github.com:
```bash
get https://github.com/composer/composer/releases/latest/download/composer.phar
```

Для вызова composer из любого каталога, его необходимо поместить в переменную окружения `PATH` или переместить в каталог `/usr/local/bin`:
```bash
sudo mv composer.phar /usr/local/bin/composer
```

Далее, находясь в каталоге проект, например `/var/www/html`, выполняем инициализацию проекта:
```bash
composer init
```

При следующей структуре каталога:
```bash
[user@host energy-rms-web.local]$ pwd
/var/www/energy-rms-web.local
[user@host energy-rms-web.local]$ tree .
.
├── composer.json
├── logs
├── public
│   ├── index.php
│   └── templates
│       └── index.html
├── src
│   └── framework
│       └── core
│           ├── PostgreSQL.php
│           └── Template.php
└── vendor
    ├── autoload.php
    └── composer
        ├── autoload_classmap.php
        ├── autoload_namespaces.php
        ├── autoload_psr4.php
        ├── autoload_real.php
        ├── autoload_static.php
        ├── ClassLoader.php
        └── LICENSE

8 directories, 13 files
[user@host energy-rms-web.local]$ 
```

... для использования класса `Template` в файле `public/index.php`, в файл `composer.json` необходимо добавить строку `"Framework\\Core\\": "src/framework/core/"`:
```json
{
    "name": "vtulyakov84/energy-rms-web",
    "type": "project",
    "autoload": {
        "psr-4": {
            "Vtulyakov84\\EnergyRmsWeb\\": "src/",
            "Framework\\Core\\": "src/framework/core/"
        }
    },
    "authors": [
        {
            "name": "vtulyakov84",
            "email": "vtulyakov84@yandex.ru"
        }
    ],
    "require": {}
}
```

После внесения изменений в файл `composer.json`, выполняем:

```bash
[user@host energy-rms-web.local]$ composer dump-autoload
```

Таким образом, в `public\index.php` используем класс `Template` следующим образом:

```php
// public/index.php
<?php
    require_once __DIR__ . '../../vendor/autoload.php';

    use Framework\Core\Template;

    $tpl = new Template();
?>
```