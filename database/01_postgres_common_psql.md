# PostgreSQL. Общий порядок работы, psql

> Целевая ОС: Alt Linux

```bash
# Определение установленных компонент postgresql
$ sudo rpm -qa | grep postgresql
```

```bash
#
# Установка postgrgesql, если он не установлен
#

# 1/2. Обновляем репозитории пакетов
$ sudo apt-get update -y

# 2/2. Выполняем устанавку основных компонент
$ sudo apt-get install postgresql16 postgresql16-server postgresql-common -y
```

```bash
# Входим в интерактивную оболочку от имени postgres
$ sudo psql -U postgres
psql (16.13)
Введите "help", чтобы получить справку.

postgres=# 

# Подключение к удаленному серверу
$ psql -h localhost -p 5432 -U username -d db_name
```

### Основные команды
```bash
# Показать список всех баз данных
postgres=# \l
postgres=# \list

# Подключиться к указанной базе данных
postgres=# \c db_name

# Отобразить список таблиц в текуще базе данных
postgres=# \dt

# Отобразить струтктура (схему) таблицы
postgres=# \d table_name

# Показать список всех пользователей (ролей)
postgres=# \du

# Отобразить информацию о текущем подключении
postgres=# \conninfo

# Выйти из оболочки psql
postgres=# \q
```

### Создание и управление базами данных 
```bash
# Создать базу данных
postgres=# CREATE DATABSE db_name;

# Создать базу данных с назначением владельца
postgres=# CREATE DATABASE db_name OWNER username;

# Сменить владельца базы данных
postgres=# ALTER DATABASE db_name OWNER TO username;

# Удаление базы данных
postgres=# DROP DATABSE db_name;
```

### Создание пользователей и управление ролями

```bash
# Создание пользователя (роли) с правом входа
postgres=# CREATE USER username WITH PASSWORD 'userpassword';

# Создание роли (без входа), используется как группа
postgres=# CREATE ROLE role_name;

# Назначить права на базу данных
postgres=# GRANT CONNECT ON DATABSE db_name TO username;

# Назначить все права на базу данных
postgres=# GRANT ALL PRIVILEGES ON DATABASE db_name TO username;

# Предоставление доступа к объектам схемы
postgres=# GRANT USAGE ON SCHEMA public TO username;

# Предоставить все права на все таблицы в схеме
postgres=# GRANT SELECT, INSERT ON ALL TABLES IN SCHEMA public TO username;
```

> * Если вы дали права на базу данных (`GRANT ALL ON DATABSE`), пользователь сможет подключиться к базе данных, но не сможет просмотреть или изменить таблицы. Права на таблицы необходимо определять дополнительно.
> * Чтобы пользователь смог полностью управлять таблицами (удалить, изменить структуру), он должен быть ее **владельцем**.

### Вероятные сценарии

#### Сыоздать пользователя, который является владельцем базы данных
```bash
# 1. Создать пользователя
postgres=# CREATE USER owner_user WITH PASSWORD '123';

# 2. Создать БД с назначением владельца
postgres=# CREATE DATABASE mydb OWNER owner_user;
```
#### Определение всех прав на все таблицы для пользователя
```bash
# Даем доступ к схемам
postgres=# GRANT USAGE ON SCHEMA public TO user_name;

# Даем все операции с данными на все таблицы
postgres=# GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO user_name;
```

#### Назначение прав для позже создаваемых таблиц
```bash
# Все новые таблицы, которые создаст пользователь admin, будут доступны для чтения пользователю john
postgres=# ALTER DEFAULT PRIVILEGES FOR USER admin IN SCHEMA public GRANT SELECT ON TABLES TO username;
```