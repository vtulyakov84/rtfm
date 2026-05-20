# Настройка окружения ОС Alt Linux для разработки на Java

## Содержание

1. Установка openjdk-17, openjdk-17-devel

2. Файловая структура проекта `HelloWorld`

3. Компиляция и запуск `HelloWorld.java` с командной строки

4. Компиляция и запуск `HelloWorld.java` с использованием системы сборки `make`

---

### 1. Установка openjdk-17, openjdk-17-devel
```bash
# 1. Установка openjdk и openjdk-devel
$ sudo apt-get install java-17-openjdk java-17-openjdk-devel -y

# 2. Обзор установленных версий java и javac
$ java --version
openjdk 17.0.18 2026-01-20
OpenJDK Runtime Environment (Red_Hat-17.0.18.0.8-alt2) (build 17.0.18+8)
OpenJDK 64-Bit Server VM (Red_Hat-17.0.18.0.8-alt2) (build 17.0.18+8, mixed mode, sharing)

$ javac --version
javac 17.0.18
```

### 2. Файловая структура проекта `HelloWorld`

```bash
$ tree .
 helloworld
 ├── doc                     # Каталог сопроводительной документации по коду
 └── src                     # Каталог исходных кодов
      └── HelloWorld.java     # Исходный код проекта

3 directories, 1 file

# Меняем локацию на каталог проекта HelloWorld
$ cd helloworld

# Пишем исходный код проекта (см. следующая секция кода ниже)
$ nano src/HelloWorld.java
```

Содержание файла `./helloworld/src/HelloWorld.java`
```java
public class HelloWorld {
        public static void main(String[] argv) {
                System.out.println("Hello world!");
        }
}
```

### 3. Компиляция и запуск `HelloWorld.java` с командной строки

```bash
# Меняем локацию на каталог с кодом
$ cd src/

# Компиляция в байт-код виртуальной машины java
$ javac HelloWorld.java

# На выходе компилятора получаем файл `HelloWorld.class`
$ ls -l
итого 8
-rw-r--r-- 1 user user 426 мая 20 17:28 HelloWorld.class
-rw-r--r-- 1 user user 111 мая 20 17:25 HelloWorld.java


# Запускам скомпилированное приложение
$ java HelloWorld
Hello world!
```

### 4. Компиляция и запуск `HelloWorld.java` с использованием системы сборки `make`

Преимуществом использования системы сборки `make` является, компиляция многофайловых проектов, когда к процессу компиляции необходимо подключать внешние зависимости в виде `jar-`файлов, или в случае проекта с большим количеством файлов, выполнять сборку только тех файлов, которые были изменены.

Для использования системы сборки `make` для нашего однофайлового проекта, в корне необходимо создать файл `Makefile`:

```bash
# Расположение Makefile в файловой структуре проекта
$ tree .
 helloworld
 ├── Makefile
 ├── doc                     # Каталог сопроводительной документации по коду
 └── src                     # Каталог исходных кодов
      └── HelloWorld.java     # Исходный код проекта
```

```bash
# Убедимся, что локация определена в каталоге проекта `helloworld`:
$ pwd
/home/user/develop/gurux/java/helloworld

# Создадим файл `Makefile`
$ touch Makefile

# Проверим, файловую структуру проекта
$ tree .
.
├── doc
├── Makefile
└── src
    ├── HelloWorld.class
    └── HelloWorld.java

2 directories, 3 files

# Далее будем редактировать Makefile
# ...
```

Определим формальные требования к системе сборки проекта `HelloWorld`:

1. Компиляция проекта

2. Запуск проекта

3. Очистка проекта от бинарных файлов

4. Документирование кода

5. Полная пересборка проекта

> **Внимание!** При создании  Makefile отступы необходимо создавать используя клавишу <Tab>, иначе при выполнении правил будет выводиться ошибка `*** пропущен разделитель.  Останов.`, с указанием номера строки. 

```makefile
# Makefile для проекта HelloWorld

JAVAC   = javac
JAVA    = java

# Директории проекта 
SRC_DIR = src
BIN_DIR = bin
DOC_DIR = doc

# Исходный файл проекта
SOURCE  = $(SRC_DIR)/HelloWorld.java

# Компиляция
compile:
    mkdir -p $(BIN_DIR)
    $(JAVAC) -d $(BIN_DIR) $(SOURCE)

# Запуск
run: compile
    $(JAVA) -cp $(BIN_DIR) HelloWorld

# Очистка
clean:
    rm -rf $(BIN_DIR)

# Документация
javadoc:
    mkdir -p $(DOC_DIR)/api
    javadoc -d $(DOC_DIR)/api $(SRC_DIR)/*.java

# Полная пересборка
rebuild: clean compile run

.PHONY compile run clean javadoc rebuild
```

Внимание!, ниже приведены сценарии выполнения правил, описанных в `Makefile`, при необходимости изучения изменения файловой структуры проекта, после выполнения каждого правила выполняйте команду `$ tree .`, находясь в каталоге проекта `./helloworld/`, это позволит визуально понять какие файлы создаются в результате выполнения того или иного правила.

```bash
# Из каталога helloworld
cd helloworld

#
# Возможные сценарии выполнения правил управления сборкой проекта 
#

# Компиляция
make compile

# Запуск
make run

# Очистка
make clean

# Создание документации
make javadoc

# Полная пересборка
make rebuild
```