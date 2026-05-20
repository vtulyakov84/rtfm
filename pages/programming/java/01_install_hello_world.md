# Настройка окружения ОС Alt Linux для разработки на Java

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

Файловая структура проекта `HelloWorld`:
```bash
$ tree ../
../
└── helloworld
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

Компиляция и запуск:

```bash
# Компиляция в байт-код виртуальной машины java
$ javac src/HelloWorld.java

# На выходе компилятора получаем файл `HelloWorld.class`
$ ls -l src/
итого 8
-rw-r--r-- 1 user user 426 мая 20 17:28 HelloWorld.class
-rw-r--r-- 1 user user 111 мая 20 17:25 HelloWorld.java

# Меняем локацию на каталог с кодом
$ cd src/

# Запускам скомпилированное приложение
$ java HelloWorld
Hello world!
```