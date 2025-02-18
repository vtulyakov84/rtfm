## Golang. Портабальная установка в Linux
_(golang portable, portable golang)_

Случаи, когда необходима портабальная установка golang (файловое копирование с настройкой окружения разработки);
1. Компьютер не имеет доступ к сети Интернет;
2. Отсутствует возможность использовать `sudo`, из-за ограниений учётной записи пользователя.

Со страницы загрузки [golang](https://go.dev/dl/) загружаем последнюю версию языка программирования [go1.24.0.linux-amd64.tar.gz](https://go.dev/dl/go1.24.0.linux-amd64.tar.gz).

Скаченный архив разархивируем в домашний каталог пользователя:
`mkdir -p ~/develop/compilers/go`.

Структура каталогов должна выглядеть следующим образом:
```
[user@vhost-129 compilers]$ tree -L 3 ..
..
├── compilers
│   └── go
│       ├── api
│       ├── bin
│       ├── codereview.cfg
│       ├── CONTRIBUTING.md
│       ├── doc
│       ├── go.env
│       ├── lib
│       ├── LICENSE
│       ├── misc
│       ├── PATENTS
│       ├── pkg
│       ├── README.md
│       ├── SECURITY.md
│       ├── src
│       ├── test
│       └── VERSION

```

Далее необходимо настроить переменную окружения `PATH`, для выполнения команды `go` находясь в любом каталоге.

Читаем переменную окружения PATH:

```
[user@vhost-129 compilers]$ printenv `PATH`
/home/user/bin:/usr/local/bin:/usr/bin:/bin:/usr/games
```

Добавляем к переменно окружения `PATH`, путь к каталогу `go`:
```
[user@vhost-129 compilers]$ export PATH=$PATH:~/develop/compilers/go/bin
```

Снова считываем переменную окружения `PATH`:
```
[user@vhost-129 compilers]$ printenv PATH
/home/user/bin:/usr/local/bin:/usr/bin:/bin:/usr/games:/home/user/develop/compilers/go/bin
```

Выполняем проверку версии `golang`:
```
[user@vhost-129 compilers]$ go version
go version go1.24.0 linux/amd64
```

Для программ на golang будем использовать каталог `~/develop/golang/HelloWorld`:
```
[user@vhost-129 develop]$ mkdir ~/develop/golang/HelloWorld
[user@vhost-129 develop]$ cd ~/develop/golang/HelloWorld/
[user@vhost-129 HelloWorld]$ 
```

Текст программы `HelloWorld` следующий:
```
[user@vhost-129 HelloWorld]$ cat main.go
package main

import (
	"fmt"
)

func main() {
	fmt.Println("Hello world!")
}

```

Запуск программы в режиме интерпритации пакета:

```
[user@vhost-129 HelloWorld]$ go run main.go
Hello world!
[user@vhost-129 HelloWorld]$ 
```

Вариант запуска программы через компиляцию пакета:
```
[user@vhost-129 HelloWorld]$ go build main.go
[user@vhost-129 HelloWorld]$ ls -l
итого 2168
-rwxr-xr-x 1 user user 2208801 фев 18 22:28 main
-rw-r--r-- 1 user user      78 фев 18 22:26 main.go
-rw-r--r-- 1 user user      77 фев 18 22:25 main.go~
[user@vhost-129 HelloWorld]$ ./main 
Hello world!
[user@vhost-129 HelloWorld]$ 
```

### Инициализция проекта

В каталоге с программой выполняем `go mod init main`:
```
user@vhost-129 HelloWorld]$ ls -l
итого 2168
-rwxr-xr-x 1 user user 2208801 фев 18 22:28 main
-rw-r--r-- 1 user user      78 фев 18 22:26 main.go
-rw-r--r-- 1 user user      77 фев 18 22:25 main.go~
[user@vhost-129 HelloWorld]$ go mod init main
go: creating new go.mod: module main
go: to add module requirements and sums:
	go mod tidy
[user@vhost-129 HelloWorld]$ ls -l
итого 2172
-rw-r--r-- 1 user user      23 фев 18 22:36 go.mod
-rw-r--r-- 1 user user      78 фев 18 22:26 main.go
[user@vhost-129 HelloWorld]$ cat go.mod 
module main

go 1.24.0
[user@vhost-129 HelloWorld]$
```

Теперь для запуска пакета в режиме интепритации можно выполнить следующую синтаксическую нотацию языка:
```
[user@vhost-129 HelloWorld]$ go run .
Hello world!
```

Для компиляции пакета выполняем `go build .`:
```
[user@vhost-129 HelloWorld]$ ls -l
итого 8
-rw-r--r-- 1 user user 23 фев 18 22:36 go.mod
-rw-r--r-- 1 user user 78 фев 18 22:26 main.go
[user@vhost-129 HelloWorld]$ go build .
[user@vhost-129 HelloWorld]$ ls -l
итого 2168
-rw-r--r-- 1 user user      23 фев 18 22:36 go.mod
-rwxr-xr-x 1 user user 2208801 фев 18 22:40 main
-rw-r--r-- 1 user user      78 фев 18 22:26 main.go
[user@vhost-129 HelloWorld]$ ./main
Hello world!
[user@vhost-129 HelloWorld]$ 
```