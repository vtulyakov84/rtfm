### PHP composer

#### Установка с командной строки
Для быстрой установки Composer в текущем каталоге, выполните в терминале последовательно следующие команды:
```bash
# Загружается установщик в текущий каталог
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"

# Выполняется проверка SHA-384 установщика
php -r "if (hash_file('sha384', 'composer-setup.php') === 'c8b085408188070d5f52bcfe4ecfbee5f727afa458b2573b8eaaf77b3419b0bf2768dc67c86944da1544f06fa544fd47') { echo 'Installer verified'.PHP_EOL; } else { echo 'Installer corrupt'.PHP_EOL; unlink('composer-setup.php'); exit(1); }"

# Запуск установщика
php composer-setup.php

# Удаление установщика
php -r "unlink('composer-setup.php');"
```

Для вызова composer из любого каталога, его необходимо поместить в переменную окружения `PATH` или переместить в каталог `/usr/local/bin`:
```bash
sudo mv composer.phar /usr/local/bin/composer
```