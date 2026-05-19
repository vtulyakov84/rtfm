# Сборка Python 3.14.5 из исходных кодов

Оригинальный текст: [https://github.com/DataSithAi/Python314-alt-linux-install/blob/main/python314-alt-linux-guide.md](https://github.com/DataSithAi/Python314-alt-linux-install/blob/main/python314-alt-linux-guide.md)

```bash
# Целевая ОС
...]$ uname -a
Linux host 6.1.115-un-def-alt1 _1 SMP PREEMPT_DYNAMIC Thu Nov 14 11:32:13 UTC 2024 x86_64 GNU/Linux

# Установка инструментов сборки
...]$ sudo apt-get install -y gcc make zlib-devel libffi-devel libssl-devel \
    bzlib-devel libreadline-devel libsqlite3-devel wget curl \
    libncurses-devel tk-devel libgdbm-devel liblzma-devel

# Смотрим действующую версия python
...]$ python3 --version
Python 3.9.20

# Меняем локацию на временны каталог
...]$ cd /tmp/

# Копируем с оффициального сайта архив исходных кодов Python 3.14.5
tmp]$ wget https://www.python.org/ftp/python/3.14.5/Python-3.14.5.tgz

# Распаковываем скопированный архи
tmp]$ tar -xf Python-3.14.5.tgz 

# Переходим в каталог
tmp]$ cd Python-3.14.5/

---

# Выполняем конфигурацию сборки
Python-3.14.5]$ ./configure --enable-optimization --prefix=/usr/local

---

# Выполняем сборку
Python-3.14.5]$ make -j$(nproc)

---

# Выполняем установку
# `altinstall`` - устанавливает Python как python3.14 (безопасно)
# `install`     - перезаписывает системный python3 (может сломать систему!)
Python-3.14.5]$ sudo make altinstall

--

# Проверка версии Python
Python-3.14.5]$ python3.14 --version
Python 3.14.5

# Проверка версии pip
Python-3.14.5]$ pip3.14 --version
pip 26.1.1 from /usr/local/lib/python3.14/site-packages/pip (python 3.14)

# Расположение исполняемого файла
Python-3.14.5]$ which python3.14
/usr/local/bin/python3.14

# Список установленных пакетов
Python-3.14.5]$ python3.14 -m pip list
Package Version
------- -------
pip     26.1.1


# Меняем локация на домашний каталог
Python-3.14.4]$ cd ~

# Удаляем свборочные/установочные файлы
~]$ sudo rm -rf /tmp/Python-3.14.5/

# Скопированный с Интернета архив `Python-3.14.5.tgz` перемещаем в домашний каталог
~]$ mv /tmp/Python-3.14.5.tgz ~

# ... или удаляем
~]$ rm -f /tmp/Python-3.14.5.tgz
```