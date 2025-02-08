## AltLinux. Сборка объектов ядра

> _Долгое время разбирался как собрать объект ядра именно под AltLinux, поскольку под Debian всё собиралось без проблем по книге источник-1 "Linux. Модули ядра. Пособие по программированию (авторы Peter Jay Salzman, Michael Burian, Ori Pomerantz, Bob Mottram,
Jim Huang)"_

> _NB! Необходимо попробовать установку на чистую хост-систему AltLinux_

Источник-2: [Как собрать внешний модуль ядра под Альт](https://www.altlinux.org/Kernel/build_module)

## Установка заголовочных файлов
Установка заголовочных файлов для сборки объекта ядра выполнялась согласно Источник-2.

Для сборки внешнего модуля ядра, например, под флейвор `std-def` необходим пакет `kernel-headers-modules-std-def` и базовая сборочная среда (устанавливаемая пакетом `rpm-build`):

```
# apt-get update
# apt-get dist-upgrade
# apt-get install rpm-build kernel-headers-modules-std-def
```

Приедет `kernel-headers-modules-std-def` _для последнего ядра в репозитории_, если у вас уже более старое ядро, то его нужно тоже обновить.

Альтернативный вариант -- сразу установить и новое ядро и хедеры от него:

```
 # apt-get update
 # apt-get dist-upgrade
 # apt-get install rpm-build
 # update-kernel --headers
 # ребут
 ```
 В дальнейшем `update-kernel` будет ставить хедеры автоматически.

Дальше собираете модуль по инструкциям апстрима.

## Пишем модуль объекта ядра

> Модуль ядра пишем согласно источника-1.

Создаём каталог проекта:

```
$ mkdir -p ~/develop/kernel/hello1 && cd ~/develop/kernel/hello1
```

Создаём файл hello-1.c следующего содержания:

```
/*
* hello-1.c – простейший модуль ядра.
*/
#include <linux/kernel.h> /* необходим для pr_info() */
#include <linux/module.h> /* необходим для всех модулей */
int init_module(void)
{
pr_info("Hello world 1.\n");
/* Если вернётся не 0, значит, init_module провалилась; модули загрузить не
получится. */
return 0;
}
void cleanup_module(void)
{
pr_info("Goodbye world 1.\n");
}
MODULE_LICENSE("GPL");
```

Создаём Makefile:

```
obj-m += hello-1.o
PWD := $(CURDIR)
all:
make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules
clean:
make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
```

Выполняем сборку модуля ядра:
```
make
```

На выходе в каталоге появится файл hello-1.ko.

Просмотреть информацию о собраном модуле:
```
modinfo hello-1.ko
```

Загрузка модуля в адресное пространство ядра ОС:
```
sudo insmod hello-1.ko
```

Выгрузить модль из адресного пространства ОС:
```
rmmod hello-1
```

Просмотр записей в журнале ОС:
```
sudo journalctl --since "1 hour ago" | grep kernel
```

## Чтиво

[ALT Linux Wiki. Как собрать внешний модуль ядра под Альт.](https://www.altlinux.org/Kernel/build_module)

[Linux. Модули ядра. Пособие по программированию.](https://docs.yandex.ru/docs/view?tm=1738973298&tld=ru&lang=ru&name=The-Linux-Kernel-Module-Programming-Guide-ru.pdf&text=Программирования%20ядра%20Linux&url=https%3A%2F%2Fruvds.com%2Fwp-content%2Fuploads%2F2022%2F10%2FThe-Linux-Kernel-Module-Programming-Guide-ru.pdf&lr=37&mime=pdf&l10n=ru&sign=fffc91d2fefb4f3728dabc30ede89ccd&keyno=0&nosw=1&serpParams=tm%3D1738973298%26tld%3Dru%26lang%3Dru%26name%3DThe-Linux-Kernel-Module-Programming-Guide-ru.pdf%26text%3D%25D0%259F%25D1%2580%25D0%25BE%25D0%25B3%25D1%2580%25D0%25B0%25D0%25BC%25D0%25BC%25D0%25B8%25D1%2580%25D0%25BE%25D0%25B2%25D0%25B0%25D0%25BD%25D0%25B8%25D1%258F%2B%25D1%258F%25D0%25B4%25D1%2580%25D0%25B0%2BLinux%26url%3Dhttps%253A%2F%2Fruvds.com%2Fwp-content%2Fuploads%2F2022%2F10%2FThe-Linux-Kernel-Module-Programming-Guide-ru.pdf%26lr%3D37%26mime%3Dpdf%26l10n%3Dru%26sign%3Dfffc91d2fefb4f3728dabc30ede89ccd%26keyno%3D0%26nosw%3D1)

[ALT Linux Wiki. OutOfTreeKernelModule.](https://www.altlinux.org/OutOfTreeKernelModule)

[ALT Linux Wiki. Сборка модулей ядра.](https://www.altlinux.org/Сборка_модулей_ядра#Сборка_нового_модуля)