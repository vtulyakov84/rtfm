#  Git. Работа с удалённым репозиторием

Рассматривается минимум работы с репозиториями на основе двух веток локальной `master`-ветки и удалённой `origin`-ветки. `форки` и `бренчи` не рассматриваются.

Содержание
1. [Настройка локального репозитория git](#part1)
2. [Создаём локальный репозиторий](#part2)
3. [Создаём удаленный репозиторий на github.com](#part3)
4. [Манипуляции при внесении изменений в проект](#part4)

5. [Приложение 1. Коды git к файлам в VSCode](#attach1)

> git должен быть установлен на компьютере, и доступен из командной строки ОС Linux или Windows из любого каталога.

## <a id="part1">1. Настройка локального репозитория git</a>

Определяем пользовател системы и адрес электронной почты пользователя:
```
PS D:\library\development\git\rtfm> git config --global user.name "vtulyakov84"
PS D:\library\development\git\rtfm> git config --global user.email "vtulyakov84@yandex.ru"
```

## <a id="part2">2. Создаём локальный репозиторий</a>

Создаем каталог проекта (_проект может быть любым, многофайловая справочная система, проект разрабатываемой компьютерной программы, куча файлов для подготовки журнальной стать или пр._) и переходим в него.

> Обязательно? в корне каталога создайте файл `README.md` с кратким описание проекта в обыкновенной текстовой форме.

Просматриваем статус репозитория проекта:
```
PS D:\library\development\git\project_name> git status
fatal: not a git repository (or any of the parent directories): .git
```

Получаем сообщение, что каталог `project_name` не является репозиторием `git` ("`fatal: not a git repository...`").

Выполняем инициализацию репозитория:
```
PS D:\library\development\git\project_name> git init
Initialized empty Git repository in D:/library/development/git/project_name/.git/
```

Определяем имя мастер ветки:
```
PS D:\library\development\git\project_name> git branch -M master
```

Просматриваем статус репозитория проекта:
```
PS D:\library\development\git\project_name> git status
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
PS D:\library\development\git\project_name> git add .
warning: in the working copy of '.gitignore', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of '.vscode/extensions.json', LF will be replaced by CRLF the next time Git touches it
```

Снова просматриваем состояние локального репозитория:
```
PS D:\library\development\git\project_name> git status
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
PS D:\library\development\git\project_name> git commit -m="first commit"
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

## <a id="part3">3. Создаём удаленный репозиторий на github.com</a>

Выполняем авторизацию или регистрацию на сайте github.com, и используя web-интерфейс github.com создаём репозиторий одноимённый имени проекта локального репозитория. 

> При создании репозитория получаем подсказку от github.com, какие команды выполнять дальше.

### …or create a new repository on the command line
```
echo "# project_name" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/vtulyakov84/project_name.git
git push -u origin main
```

### …or push an existing repository from the command line
```
git remote add origin https://github.com/vtulyakov84/project_name.git
git branch -M main
```

Для копирование локального репозитория в глобальный выполняем создание ветки
```
git branch -M main
git remote add origin https://github.com/vtulyakov84/project_name.git
git push -u origin main
```

Выполняем:
```
git remote add origin https://github.com/vtulyakov84/project_name.git
```

Выполняем `push` локального репозитория в удаленный (_при установленной системе git запрос авторизации пользователя появится при первом копировании локального репозитория_):
```
git push -u origin main
```

## <a id="part4">4. Манипуляции при внесении изменений в проект</a>

Добавляем изменения во всех файлах проекта в локальный репозиторий:
```
PS D:\library\development\git\project_name> git add .
```

Выполняем фиксацию изменений в локальном репозитории `commit`:
```
PS D:\library\development\git\project_name> git commit -m="commit comment text"
```

Пушим локальный репозиторий в удаленный репозиторий  на github.com:
```
PS D:\library\development\git\project_name> git push -u origin master
```

## <a id="attach1"> Приложение 1. Коды git к файлам в VSCode </a>
При работе с файлами проекта в IDE VSCode, в дереве каталога напротив каждого файла возможно появление одного следующих символов:

<kbd>A</kbd> - Added (This is a new file that has been added to the repository)

<kbd>M</kbd> - Modified (An existing file has been changed)

<kbd>D</kbd> - Deleted (a file has been deleted)

<kbd>U</kbd> - Untracked (The file is new or has been changed but has not been added to the repository yet)

<kbd>C</kbd> - Conflict (There is a conflict in the file)

<kbd>R</kbd> - Renamed (The file has been renamed)

<kbd>S</kbd> - Submodule (In repository exists another subrepository)

<kbd>T</kbd> - Typechange (The file changed from symlink to regular file, or visa versa)