#  Git. Работа с удалённым репозиторием

> git должен быть установлен на компьютере, и доступен из командной строки ОС Linux или Windows из любого каталога.

## 1. Подготовка локальной системы git для работы

Определяем пользовател системы и адрес электронной почты пользователя:
```
PS D:\library\development\git\rtfm> git config --global user.name "vtulyakov84"
PS D:\library\development\git\rtfm> git config --global user.email "vtulyakov84@yandex.ru"
```

## 2. Создаём локальный репозиторий
Создаем каталог проекта (_проект может быть любым, многофайловая справочная система, проект разрабатываемой компьютерной программы, куча файлов для подготовки журнальной стать или пр._) и переходим в него.

Просматриваем статус репозитория проекта:
```
PS D:\library\development\git\esp32s3_ConverterSerialToBluetooth> git status
fatal: not a git repository (or any of the parent directories): .git
```

Получаем сообщение, что каталог `project_name` не является репозиторием `git` ("`fatal: not a git repository...`").

Выполняем инициализацию репозитория:
```
PS D:\library\development\git\esp32s3_ConverterSerialToBluetooth> git init
Initialized empty Git repository in D:/library/development/git/esp32s3_ConverterSerialToBluetooth/.git/
```

Просматриваем статус репозитория проекта:
```
PS D:\library\development\git\esp32s3_ConverterSerialToBluetooth> git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .gitignore
        .vscode/
        README.md
        include/
        lib/
        platformio.ini
        src/
        test/

nothing added to commit but untracked files present (use "git add" to track)
```

Получаем сообщение о файлах, которые не отслеживаются системой git `Untracked files: ...`

Добавляем файлы проекта в локальный репозиторий:
```
PS D:\library\development\git\esp32s3_ConverterSerialToBluetooth> git add .
warning: in the working copy of '.gitignore', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of '.vscode/extensions.json', LF will be replaced by CRLF the next time Git touches it
```

Снова просматриваем состояние локального репозитория:
```
PS D:\library\development\git\esp32s3_ConverterSerialToBluetooth> git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   .gitignore
        new file:   .vscode/extensions.json
        new file:   README.md
        new file:   include/README
        new file:   lib/README
        new file:   platformio.ini
        new file:   src/main.cpp
        new file:   test/README
```

Получаем сообщение, что файлы готовы к коммиту, выполняем коммит с сообщением о комите:
```
PS D:\library\development\git\esp32s3_ConverterSerialToBluetooth> git commit -m="first commit"
[master (root-commit) d8af5f7] =first commit
 8 files changed, 145 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 .vscode/extensions.json
 create mode 100644 README.md
 create mode 100644 include/README
 create mode 100644 lib/README
 create mode 100644 platformio.ini
 create mode 100644 src/main.cpp
 create mode 100644 test/README
```

Теперь локальный репозиторий готовк к копированию (push) в удаленный репозиторий.

## 3. Создаём удаленный репозиторий на github.com.

Выполняем авторизацию или регистрацию на сайте github.com, и используя web-интерфейс github.com создаём репозиторий одноимённый имени проекта локального репозитория. 

> При создании репозитория получаем подсказку от github.com, какие команды выполнять дальше.

### …or create a new repository on the command line
```
echo "# esp32s3_ConverterSerialToBluetooth" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/vtulyakov84/esp32s3_ConverterSerialToBluetooth.git
git push -u origin main
```

### …or push an existing repository from the command line
```
git remote add origin https://github.com/vtulyakov84/esp32s3_ConverterSerialToBluetooth.git
git branch -M main
```