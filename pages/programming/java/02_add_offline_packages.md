# Использование jar-пакетов при сборке приложений Java

## Шаг 1. Определение необходимых пакетов

Поиск необходимых пакетов выполняем в репозитории `maven` используя запрос в поисковой системе яндекса `ya.ru`, например, для `gurux` запрос выглядит следующим образом:
"**gurux jar site:repo1.maven.org**"

В резальтате поиска, находим место расположения необходимых библиотек - "https://repo1.maven.org/maven2/org/gurux/", из состава которых выбираем самые свежие jar-пакеты.
По состоянию на 22.05.2026 17:53, доступны следующие пакеты, по сопутствующим путям, выполняем их скачивание.

Копировать в локальный каталог будет по три файла каждого пакета:

1. `package-name`**.jar** - *зависимость, необходимая для использования пакета в разрабатываемом пакете.*

2. `package-name`**-source.jar** - *исходный код пакета.*

3. `package-name`**-javadoc.jar** - *сгенерированная HTML-документация по реализации пакета.*

```bash
# Копируем пакет gurux.common, version: 1.0.19
$ wget https://repo1.maven.org/maven2/org/gurux/gurux.common/1.0.19/gurux.common-1.0.19.jar
$ wget https://repo1.maven.org/maven2/org/gurux/gurux.common/1.0.19/gurux.common-1.0.19-sources.jar
$ wget https://repo1.maven.org/maven2/org/gurux/gurux.common/1.0.19/gurux.common-1.0.19-javadoc.jar

# Копируем пакет gurux.dlms, version: 4.0.93
$ wget https://repo1.maven.org/maven2/org/gurux/gurux.dlms/4.0.93/gurux.dlms-4.0.93.jar
$ wget https://repo1.maven.org/maven2/org/gurux/gurux.dlms/4.0.93/gurux.dlms-4.0.93-sources.jar
$ wget https://repo1.maven.org/maven2/org/gurux/gurux.dlms/4.0.93/gurux.dlms-4.0.93-javadoc.jar

# Копируем пакет gurux.net
$ wget https://repo1.maven.org/maven2/org/gurux/gurux.net/1.0.32/gurux.net-1.0.32.jar
$ wget https://repo1.maven.org/maven2/org/gurux/gurux.net/1.0.32/gurux.net-1.0.32-sources.jar
$ wget https://repo1.maven.org/maven2/org/gurux/gurux.net/1.0.32/gurux.net-1.0.32-javadoc.jar

# Копируем пакет gurux.serial
$ wget https://repo1.maven.org/maven2/org/gurux/gurux.serial/1.0.31/gurux.serial-1.0.31.jar
$ wget https://repo1.maven.org/maven2/org/gurux/gurux.serial/1.0.31/gurux.serial-1.0.31-sources.jar
$ wget https://repo1.maven.org/maven2/org/gurux/gurux.serial/1.0.31/gurux.serial-1.0.31-javadoc.jar

# Копируем пакет gurux.mqtt
$ wget https://repo1.maven.org/maven2/org/gurux/gurux.mqtt/1.0.4/gurux.mqtt-1.0.4.jar
$ wget https://repo1.maven.org/maven2/org/gurux/gurux.mqtt/1.0.4/gurux.mqtt-1.0.4-sources.jar
$ wget https://repo1.maven.org/maven2/org/gurux/gurux.mqtt/1.0.4/gurux.mqtt-1.0.4-javadoc.jar

# Копируем пакет gurux.sms
$ wget https://repo1.maven.org/maven2/org/gurux/gurux.sms/1.0.23/gurux.sms-1.0.23.jar
$ wget https://repo1.maven.org/maven2/org/gurux/gurux.sms/1.0.23/gurux.sms-1.0.23-sources.jar
$ wget https://repo1.maven.org/maven2/org/gurux/gurux.sms/1.0.23/gurux.sms-1.0.23-javadoc.jar

# Копируем пакет gurux.terminal
$ wget https://repo1.maven.org/maven2/org/gurux/gurux.terminal/1.0.28/gurux.terminal-1.0.28.jar
$ wget https://repo1.maven.org/maven2/org/gurux/gurux.terminal/1.0.28/gurux.terminal-1.0.28-sources.jar
$ wget https://repo1.maven.org/maven2/org/gurux/gurux.terminal/1.0.28/gurux.terminal-1.0.28-javadoc.jar
```

## Шаг 2. Установка Maven

```bash
# Выполняем установку Maven
$ sudo apt-get install maven -y

# Выполняем настройку переменных окружения
$ export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-17.0.18.0.8-alt2.x86_64/
$ export PATH=$PATH:$JAVA_HOME/bin

# Просмотр версии Maven
$ mvn -version
Apache Maven 3.6.3 (Red Hat 3.6.3-alt3_9jpp11)
Maven home: /usr/share/maven
Java version: 17.0.18, vendor: Red Hat, Inc., runtime: /usr/lib/jvm/java-17-openjdk-17.0.18.0.8-alt2.x86_64
Default locale: ru_RU, platform encoding: UTF-8
OS name: "linux", version: "6.1.115-un-def-alt1", arch: "amd64", family: "unix"
```

Для того, чтобы переменные окружения не пришлось пересоздавать после перезагрузки ОС Alt Linux, добавим строки в конец файла `~/.bashrc`:
```bash
$ echo 'export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-17.0.18.0.8-alt2.x86_64/' >> ~/.bashrc

$ echo 'export PATH=$PATH:$JAVA_HOME/bin' >> ~/.bashrc

$ source ~/.bashrc
```