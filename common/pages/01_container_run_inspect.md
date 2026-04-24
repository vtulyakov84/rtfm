# Запуск и инспектирование контейнеров Docker

## 1. Запуск контейнера 
 ### Базовый запуск контейнера
 ```bash
 # Запустить контейнер из образа
docker run nginx

# Запустить в фоновом режиме (detached)
docker run -d nginx

# Дать контейнеру имя
docker run -d --name my_nginx nginx

# С пробросом портов (host:container)
docker run -d -p 8080:80 --name my_nginx nginx

# С монтированием томов
docker run -d -v /host/path:/container/path nginx
 ```

 ### Полезные флаги запуска
 ```bash
 # Интерактивный режим с псевдо-TTY
docker run -it ubuntu bash

# Установка переменных окружения
docker run -e MY_VAR=value -d nginx

# Ограничение ресурсов
docker run --memory="512m" --cpus="1" -d nginx

# Автоматический перезапуск
docker run --restart always -d nginx
 ```

 ## 2. Инспектирование контейнеров
 
 ### Базовые команды для просмотра
 ```bash
 # Список запущенных контейнеров
docker ps

# Список всех контейнеров (включая остановленные)
docker ps -a

# Просмотр логов
docker logs my_nginx
docker logs -f my_nginx  # следить в реальном времени
docker logs --tail 50 my_nginx  # последние 50 строк
 ```

### Детальная информация
```bash
# Полная информация о контейнере (JSON)
docker inspect my_nginx

# Выборочная информация из inspect
docker inspect --format='{{.State.Status}}' my_nginx
docker inspect --format='{{.NetworkSettings.IPAddress}}' my_nginx
docker inspect --format='{{.Config.Image}}' my_nginx

# Просмотр процессов внутри контейнера
docker top my_nginx

# Просмотр использования ресурсов
docker stats my_nginx
docker stats --no-stream  # разово, без постоянного обновления
``` 

### Инспектирование файловой системы

```bash
# Просмотр изменений в файловой системе
docker diff my_nginx

# Выполнение команды внутри контейнера
docker exec my_nginx ls -la
docker exec -it my_nginx /bin/bash  # интерактивная сессия

# Копирование файлов из контейнера
docker cp my_nginx:/etc/nginx/nginx.conf ./nginx.conf
```

## 3. Практические примеры

```bash
# 1. Запускаем контейнер с Nginx
docker run -d --name web -p 8080:80 nginx:alpine

# 2. Проверяем, что запустился
docker ps | grep web

# 3. Смотрим логи
docker logs web

# 4. Получаем детальную информацию
docker inspect web --format='Name: {{.Name}}, IP: {{.NetworkSettings.IPAddress}}'

# 5. Заходим внутрь контейнера
docker exec -it web sh

# 6. Проверяем использование ресурсов
docker stats web --no-stream

# 7. Останавливаем и удаляем
docker stop web
docker rm web
```

## 4. Полезные команды для диагностики
```bash
# Просмотр событий контейнера
docker events --filter container=my_nginx

# Проверка портов контейнера
docker port my_nginx

# Просмотр метаданных (лайфстайл)
docker inspect -f '{{.State}}' my_nginx

# Мониторинг в реальном времени
watch -n 2 'docker ps | grep my_nginx'

# Проверка, запущен ли контейнер
docker ps --filter name=my_nginx --format "table {{.Names}}\t{{.Status}}"
```

## Основные флаги для `docker inspect`
`{{.State.Running}}`	- Запущен ли контейнер
`{{.State.Pid}}`	- PID процесса контейнера
`{{.Config.Env}}`	- Переменные окружения
`{{.HostConfig.PortBindings}}`	- Проброс портов
`{{.Mounts}}`	- Смонтированные тома