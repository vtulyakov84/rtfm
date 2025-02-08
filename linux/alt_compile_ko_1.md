## AltLinux. Сборка объекта ядра
_(altlinux make kernel object file)_

### Обновляем компоненты ОС
```
[user@vhost-129 ~]$ sudo apt-get update -y
...
Чтение списков пакетов... Завершено
Построение дерева зависимостей... Завершено
[user@vhost-129 ~]$ sudo apt-get dist-upgrade -y

...
Завершено.
[user@vhost-129 ~]$ uname -r
6.1.115-un-def-alt1
[user@vhost-129 ~]$ 
```

### Выполняем настройку среды разработки

Для сборки внешнего модуля ядра, например, под флейвор `std-def` устаналвиваем последний пакет заголовочных фалов `kernel-headers-modules-std-def` из репозитория и базовую сборочную среду `rpm-build:`

```
[user@vhost-129 ~]$ sudo apt-get install rpm-build kernel-headers-modules-std-def -y
...
Завершено.
[user@vhost-129 ~]$ 
```

Обновляем ядро ОС:

```
[user@vhost-129 ~]$ sudo update-kernel --headers -y
Running kernel: kernel-image-un-def-6.1.115-alt1
Checking for available un-def kernel packages...
...
[00:00:00] Config file: /etc/initrd.mk
[00:00:02] Generating module dependencies on host ...
...
Done.
[user@vhost-129 ~]$ 
```

Просмотрим версию ядра после обновления:

```
$ uname -r
6.1.115-un-def-alt1
```

Перезагружаем хостовую систему:
```
$ sudo reboot
```

## Пишем внешний модуль ядра

Создаём рабочий каталог и переходим в него:
```
[user@vhost-129 hello_printk]$ mkdir -p ~/develop/kernel/hello_printk && cd ~/develop/kernel/hello_printk
[user@vhost-129 hello_printk]$ 
```

Создаём файл исходного кода модуля ядра:
```
$ vim hello_printk.c
```

...следующего содержания:

```
#include <linux/init.h>
#include <linux/module.h>

MODULE_LICENSE("GPL");
MODULE_AUTHOR("Viktor Tulyakov <vtulyakov84@yandex.ru>");

static int __init hello_init(void) {
        printk("Hello, world! 1.\n");
        return 0;
}

static void __exit hello_exit(void) { 
        printk("Goodby, world! 1.\n");
} 

module_init(hello_init);
module_exit(hello_exit);
```

Создаём Makefile:

```
$ vim Makefile
```

...следующего содержания:

```
obj-m += hello_printk.o
PWD := $(CURDIR)
all:
    make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules
clean:
    make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean

```

Выполняем сборку модуля ядра, выполнив команду `make`:
```
[user@vhost-129 hello_printk]$ make
make -C /lib/modules/6.1.127-un-def-alt1/build M=/home/user/develop/kernel/hello_printk modules
make[1]: вход в каталог «/usr/src/linux-6.1.127-un-def-alt1»
  CC [M]  /home/user/develop/kernel/hello_printk/hello_printk.o
  MODPOST /home/user/develop/kernel/hello_printk/Module.symvers
  CC [M]  /home/user/develop/kernel/hello_printk/hello_printk.mod.o
  LD [M]  /home/user/develop/kernel/hello_printk/hello_printk.ko
  BTF [M] /home/user/develop/kernel/hello_printk/hello_printk.ko
Skipping BTF generation for /home/user/develop/kernel/hello_printk/hello_printk.ko due to unavailability of vmlinux
make[1]: выход из каталога «/usr/src/linux-6.1.127-un-def-alt1»
[user@vhost-129 hello_printk]$ 
```

Просматриваем в каталоге созданные файлы:
```
[user@vhost-129 hello_printk]$ ls -uF -l --sort=extension
итого 204
-rw-r--r-- 1 user user    177 фев  8 14:26 Makefile
-rw-r--r-- 1 user user    332 фев  8 14:26 hello_printk.c
-rw-r--r-- 1 user user    892 фев  8 14:27 hello_printk.mod.c
-rw-r--r-- 1 user user 105048 фев  8 14:27 hello_printk.ko
-rw-r--r-- 1 user user     54 фев  8 14:27 hello_printk.mod
-rw-r--r-- 1 user user  64240 фев  8 14:27 hello_printk.mod.o
-rw-r--r-- 1 user user  12760 фев  8 14:27 hello_printk.o
-rw-r--r-- 1 user user     55 фев  8 14:27 modules.order
-rw-r--r-- 1 user user      0 фев  8 14:27 Module.symvers
```

Просматриваем информацию о собранном внешнем модуле ярда:
```
[user@vhost-129 hello_printk]$ sudo modinfo ./hello_printk.ko
filename:       /home/user/develop/kernel/hello_printk/./hello_printk.ko
author:         Viktor Tulyakov <vtulyakov84@yandex.ru>
license:        GPL
srcversion:     9A0F9C4427253AC901D0E9C
depends:        
retpoline:      Y
name:           hello_printk
vermagic:       6.1.127-un-def-alt1 SMP preempt mod_unload modversions 
[user@vhost-129 hello_printk]$ 
```

Выполняем загрузку `hello_printk.ko` в адресное пространство ядра ОС AltLinux:
```
[user@vhost-129 hello_printk]$ sudo insmod ./hello_printk.ko 
[user@vhost-129 hello_printk]$ 
```

Проверяем наличие нашего модуля, среди модулей в адресном пространстве ядра:
```
[user@vhost-129 hello_printk]$ sudo lsmod | grep hello_printk
hello_printk           16384  0
[user@vhost-129 hello_printk]$ 
```

Выгружаем наш модуль из адресного пространства ядра:
```
[user@vhost-129 hello_printk]$ sudo rmmod hello_printk 
[user@vhost-129 hello_printk]$ 
```

Выполняем снова проверку наличия нашего модуля в адресном пространстве ядра:
```
[user@vhost-129 hello_printk]$ sudo lsmod | grep hello_printk
[user@vhost-129 hello_printk]$ 
```

Проверяем результаты работы нашего модуля:
```
[user@vhost-129 hello_printk]$ sudo journalctl --since "7 minute ago" | grep kernel
...
hello_printk ; USER=root ; COMMAND=/sbin/modinfo ./hello_printk.ko
...
hello_printk ; USER=root ; COMMAND=/sbin/insmod ./hello_printk.ko
фев 08 14:30:49 vhost-129 kernel: Hello, world!
...
hello_printk ; USER=root ; COMMAND=/sbin/rmmod hello_printk
фев 08 14:33:19 vhost-129 kernel: Goodby, world!
...
[user@vhost-129 hello_printk]$ 
```
В системном журнале отображены все наши действия, которые мы выполняли с нашим модулем.


## Чтиво по теме
[ALT Linux Wiki. Kernel/build module](https://www.altlinux.org/Kernel/build_module)

[Расширения ядра Linux: Драйверы и модули (автор: Олег Цирюлик, 2023)]()
[Linux. Модули ядра. Пособие по программированию (авторы: Peter Jay Salzman, Michael Burian, Ori Pomerantz, Bob Mottram, Jim Huang)]()