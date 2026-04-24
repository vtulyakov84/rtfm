Задача собрать образ для разворачивания контейнера с apache и php8 на базе образа alt:p10 (`FROM alt:p10`)

### Шаг 1. Определение базового образа
В связи с тем, что работа контейнера предполагает условия работы с объектами критической информационной структуры, в среде исполнения ОС AltLinux, базовым образом будем использовать alt:p10, и компоненты apache2 и php8.2, входящие в состав стандартных репозиториев alt.

```bash
# Проверить наличие образа alt:p10 в локальном хранилище Docker
$ docker images
```

В случае отсутствия образа в репозитории он будет скачан из репозитория Docker Hub при использовании `docker run <image-name>`.

В случае, если на компьютере отсутствует доступ к сети Интернет, можно выполнить перенос используемого образа с друго компьютера:

```bash
# Сохранить требуемый образ в локальном хранилище Docker
$ docker pull alt:p10

# Сохранить образ из локального хранилища в tar-файл
# docker save -o <output_file.tar> <image_name>
$ docker save -o alt-p10.tar alt:p10

# Загрузить образ из tar-файла в локальный репозиторий Docker
$ docker load -i alt-p10.tar
```

### Шаг 2. Установка необходимых компонент
```bash
# 1. Выполняем запуск контейнера на базе alt:p10 с подключене к командной строке /bin/bash,
#    и пробросом порта
docker run -it -p 8080:80 alt:p10 /bin/bash

# 2. Выполняем обновление списка репозиториев (обязательно, без этого не работает `apt-get install...`, `apt-cache search...`)
apt-get update

```bash
# 3. Для проверки наличия копонент apache 2, выполняем,
apt-get install -y \
        apache2-full \
        apache2-mod_php8.2 \
        php8.2 \
        php8.2-gd \
        php8.2-mbstring \
        php8.2-devel \
        php8.2-libs \
        php8.2-pdo \
        php8.2-pdo_pgsql \
        php8.2-pgsql

# 4. Запускаем web-сервис
/usr/sbin/httpd2 -k start 
```

### Шаг 3. Оформление Dockerfile
```Dockerfile
FROM alt:p10

# Установка необходимых пакетов
RUN apt-get update && \
    apt-get install -y \
        apache2-full \
        apache2-mod_php8.2 \
        php8.2 \
        php8.2-gd \
        php8.2-mbstring \
        php8.2-devel \
        php8.2-libs \
        php8.2-pdo \
        php8.2-pdo_pgsql \
        php8.2-pgsql && \
    apt-get clean

# Копирование конфигураций Apache из проекта
COPY ./apache-config /etc/httpd2/conf/extra/
COPY ./apache2.conf /etc/httpd2/conf/httpd2.conf

# Копирование конфигураций PHP из проекта
COPY ./php.ini /etc/php8.2/apache2-php8.2/php.ini

# Копирование веб-страниц
COPY ./www /var/www/html/

# Открываем порт
EXPOSE 80

# Запуск Apache в foreground режиме
CMD ["/usr/sbin/httpd2", "-D", "FOREGROUND"]
```