## Golang. Портабальная установка в Linux
_(golang portable, portable golang)_

Случаи, когда необходима портабальная установка golang (файловое копирование с настройкой окружения разработки);
1. Компьютер не имеет доступ к сети Интернет;
2. Отсутствует возможность использовать `sudo`, из-за ограниений учётной записи.

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