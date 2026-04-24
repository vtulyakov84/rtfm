## Работа с образом базы данных `registry.altlinux.org/c10f/postgresql`

Обзор конфигурации контейнера:

* Запуск контейнера `registry.altlinux.org/c10f/postgresql`

* Определение каталога для хранения данных PostgreSQL

* Определение файлов конфигурации `postgresql.conf`, `pg_hba.conf`

### 1. Файловая структура проекта
```bash
[user@host postgres]$ tree .
postgres
├── config
│   ├── pg_hba.conf
│   └── postgresql.conf
├── scripts
│   └── initdb.sql
│
└── Dockerfile
```

`./config/pg_hba.conf`     - конфигурация аутентификации и доступа к базе данных
`./config/postgresql.conf` - конфигурация основных настроек сервера
`./scripts/`               - каталог скриптов инициализации базы данных  
`./Dockerfile`             - сценарий создание контейнера

### 2. Запуск контейнера

```bash
# При отсутствии образа в локальном хранилище хоста
# он будет скачан с указанного репозитория
[user@host postgres]$ docker run -it registry.altlinux.org/c10f/postgresql bash
bash-4.4$ 
```

```bash
# Определяем:
# - текущего пользователя
bash-4.4$ whoami
postgres

# - используемую версию PostgreSQL
bash-4.4$ postgres --version
postgres (PostgreSQL) 16.11

# - домашний каталог
bash-4.4$ pwd
/var/lib/pgsql

# - содержимое домашнего каталога
bash-4.4$ ls -la
total 24
drwx------ 4 postgres postgres 4096 Dec  2 10:52 .
drwxr-xr-x 1 root     root     4096 Dec  2 10:52 ..
-rw-r--r-- 1 postgres postgres   70 Dec  2 10:52 .bash_profile
drwx------ 2 postgres postgres 4096 Nov 13 05:21 backups
drwx------ 2 postgres postgres 4096 Nov 13 05:21 data

# - содержание файла `.bash_profile`
bash-4.4$ cat .bash_profile 
PGLIB=/usr/share/pgsql
PGDATA=/var/lib/pgsql/data
export PGLIB PGDATA

# - переменные программного окружения
bash-4.4$ printenv
HOSTNAME=7d151d3da7c0
PWD=/var/lib/pgsql
HOME=/var/lib/pgsql
TERM=xterm
SHLVL=1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
_=/usr/bin/printenv
```

> Файлы конфигураций из `./config/postgresql.conf` и ``./config/pg_hba.conf`` в сценарии сборка образа Docker будут скопированы в каталог контейнера `/var/lib/pgsql/data`.

#### 2.2. Сбор информации о работе postgres

```bash
# Отобразить справку о вариантах работы с командной `postgres`
bash-4.4$ postgres --help
```

### 3. Содержание конфигурационных файлов `postgresql.conf` и `pg_hba.conf`.

#### 3.1. Файл конфигурации работы сервера `./postgresql.conf`:
```ini
listen_addresses = '*'
port = 5432
max_connections = 100
shared_buffers = 128MB
dynamic_shared_memory_type = posix
log_timezone = 'UTC'
datestyle = 'iso, mdy'
timezone = 'UTC'
lc_messages = 'C'
lc_monetary = 'C'
lc_numeric = 'C'
lc_time = 'C'
default_text_search_config = 'pg_catalog.english'
```

#### 3.2. Файл конфигурации аутентификации сервера `pg_hba.conf`:
```
# TYPE  DATABASE        USER            ADDRESS                 METHOD
local   all             all                                     trust
host    all             all             127.0.0.1/32            trust
host    all             all             0.0.0.0/0               md5
host    all             all             ::1/128                 trust
```

> ... в доработке, поменять место хранение файлов базы данных из каталога `/var/lib/pgsql/data` в хостовый каталог `/var/lib/postgresql/data?`. Пробрасывать каталог будем при запуске контейнера `docker run -v source:dest ...` или в секции `volume` файла `docker-compose.yml`.

### 4. Сборка своего образа на базе `registry.altlinux.org/c10f/postgresql`

**Внимание!!!**, перед сборкой образа проверьте структуру каталога, именования фалов и каталогов проекта и содежание конфигурационных файлов.

#### Содержание Dockerfile
```Dockerfile
FROM registry.altlinux.org/c10f/postgresql:latest

ENV POSTGRES_USER=postgres
ENV POSTGRES_PASSWORD=postgres
ENV PGDATA=/var/lib/pgsql/data

RUN chown -R postgres:postgres /var/lib/pgsql

RUN initdb -D ${PGDATA} --locale=C --encoding=UTF8

COPY config/pg_hba.conf ${PGDATA}/pg_hba.conf
COPY config/postgresql.conf ${PGDATA}/postgresql.conf

# Меняем права на конфиги
USER root
RUN chown postgres:postgres ${PGDATA}/pg_hba.conf ${PGDATA}/postgresql.conf
USER postgres

# Запуск и настройка пароля
RUN pg_ctl -D ${PGDATA} start && \
    psql -U postgres -c "ALTER USER postgres WITH PASSWORD '${POSTGRES_PASSWORD}';" && \
    pg_ctl -D ${PGDATA} stop
    
EXPOSE 5432

HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
  CMD pg_isready -U ${POSTGRES_USER} || exit 1

# CMD ["sh", "-c", "postgres -D ${PGDATA}"]

# Запускаем с явным указанием конфигов
CMD ["sh", "-c", "postgres -D ${PGDATA} -c config_file=${PGDATA}/postgresql.conf -c hba_file=${PGDATA}/pg_hba.conf"]
```

>Добавить:
* Команды сборки образа
* Команды управления контейнером
* Команды проверки подклчения к БД SHOW, psql -U postgres (из контейнера)
* Подключение из DBeaver (, не забыть пробросить порты -p 5432:5432) (с наружи контейнера и с другого компьютера)
* Инициализация структуры базы данных energy_rms