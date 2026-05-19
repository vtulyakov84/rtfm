## 1. Обновление языка интерпретатора Python

При первом зауске `GXDLMSClient` из пакета `gurux_dlms`, возникала ошибка, в связи с использованием неподходящей версии python, использовалась версия `3.9`, в связи с чем, 1-ая рекомендация - обновить версию языка интерпретатора `Python`:

[Инструкция по сборке `Python 3.14.5` из исходных кодов в Alt Linux](../../../programming/python_build_from_src.md)

## 2. Использование виртуального окружения

Написание кода необходимо выполнять в виртуальном окружении `venv`.

Создадим виртуальное окружение для проекта `Gurux Serial`, версии `0.1`, название каталога проекта `gxserial_01`;

```python
# Создаем каталог для проекта
$ mkdir -p ~/develop/github.com/gurux/python/gxserial_01

# Меняем локация на каталог проекта
$ cd ~/develop/github.com/gurux/python/gxserial_01

# Создание виртуального окружения
$ python3.14 -m venv .venv

# Активация виртуального окружения
$ source .venv/bin/activate

# Просмтор списка установленных пакетов в venv
(.venv) [user@host gxserial_01]$ pip3.14 list
Package Version
------- -------
pip     26.1.1

# Установка пакетов, необходимых для реализации проекта
(.venv) [user@host gxserial_01]$ pip3.14 install gurux-dlms
(.venv) [user@host gxserial_01]$ pip3.14 install gurux-common
(.venv) [user@host gxserial_01]$ pip3.14 install gurux-net
(.venv) [user@host gxserial_01]$ pip3.14 install gurux-serial

# Просмотр списка установленных пакетов в venv
(.venv) [user@host gxserial_01]$ pip3.14 list
Package      Version
------------ -------
gurux-common 1.0.13
gurux_dlms   1.0.197
gurux_net    1.0.23
gurux_serial 1.0.21
pip          26.1.1

# Деактивация виртуального окружения
(.venv) [user@host gxserial_01]$ deactivate
[user@host gxserial_01]$ 
```

## 3. Написание тестового кода

В связи с тем, что на версии языка интерпретатора `Python 3.9` возникала ошибка при создании объекта `GXDLMSClient`, именно до создания этого объекта и выполним тестовый код.

Файлова структура проекта `gxserial_01`:
```bash
# Определяем текущую локацию (print work directory)
[user@host gxserial_01]$ pwd
/home/user/develop/github.com/gurux/python/gxserial_01

# Создаем каталог для исходных кодов `src/`
[user@host gxserial_01]$ mkdir src/

[user@host gxserial_01]$ tree . -L 2 -d -a
.
├── src
└── .venv
    ├── bin
    ├── include
    ├── lib
    └── lib64 -> lib

# Выполняем активацию виртуального окружения
[user@host gxserial_01]$ source .venv/bin/activate
(.venv) [user@host gxserial_01]$ 

# Создаем первый файл в составе проекта
(.venv) [user@host gxserial_01]$ nano src/test01.py
```

Содержание файла `src/test01.py`
```python
#!../.venv/bin/python3.14
from gurux_serial.GXSerial import GXSerial
from gurux_dlms.GXDLMSClient import GXDLMSClient

if __name__=='__main__':
    # 1. Создаем объект канальной коммуникации
    media = GXSerial('/dev/ttyS0')

    # 2. Создаем объект коммуникации уровня приложения
    client = GXDLMSClient()

    try:
        media.open()
    except Exception:
        print('Ошибка открытия порта.')
    finally:
        print('Порт открыт.')

    print('Порт закрыт')
    media.close()
```

Запустите файл на выполнение:
```bash
(.venv) [user@host gxserial_01]$ python3.14 src/test_01.py 
```

Ошибка, возникающая при первом запуске:
```bash
# При запуске файла скрипта, возникает следующая ошибка
(.venv) [user@host gxserial_01]$ python3.14 src/test_01.py 
Traceback (most recent call last):

  ...

  File "/home/user/develop/python/gurux/.venv/lib/python3.14/site-packages/gurux_dlms/objects/GXDLMSObjectCollection.py", line 258
    print(repr(xml_str))  # repr n�ytt�� my�s piilomerkit kuten \n, \t jne.
                                  ^
SyntaxError: Non-UTF-8 code starting with '\xe4' on line 258, but no encoding declared; see https://peps.python.org/pep-0263/ for details
```
Откройте файл `/home/user/develop/python/gurux/.venv/lib/python3.14/site-packages/gurux_dlms/objects/GXDLMSObjectCollection.py` в текстовом редакторе и замените 4 непечатаемых символа - `�` в комментарии `# repr n�ytt�� my�s piilomerkit kuten \n, \t jne.` на символ `_` (нижнее подчеркивание).

Повторите запуск скрипта:
```bash
(.venv) [user@host src]$ python3.14 gxserial_01.py 
Порт открыт.
Порт закрыт
(.venv) [user@host src]$ 
```