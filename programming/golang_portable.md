## Golang. Портабальная установка в Linux
_(golang portable, portable golang)_

### Содержание статьи
1. Портабальная установка Golang
2. Настройка переменной окружения PATH
3. Тестовая программа `HelloWorld`
4. Инициализация проекта `HelloWorld`
5. Настройка переменных окружения `Golang`
6. Инициализация переменных окружения при запуске `Terminal` (~/.bashrc)

Случаи, когда необходима портабальная установка golang (файловое копирование с настройкой программного окружения);
1. Компьютер не имеет доступ к сети Интернет;
2. Отсутствует возможность использовать `sudo`, из-за ограниений учётной записи пользователя.

### 1. Портабальная установка Golang

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

### 2. Настройка переменной окружения PATH

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

### 3. Тестовая программа `HelloWorld` 

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

### 4. Инициализция проекта `HelloWorld`

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

### 5. Настройка переменных окружения Golang
"Одной из важных состовляющих экосистемы GoLang, позволяющих среде слаженно функционировать как единое целое, является набор __переменных окружения__." (Олег Цирюлик "Linux и Go. Эффективное низкоуровневое программирование", 2023 г., стр. 39).
```
[user@vhost-129 HelloWorld]$ go env
AR='ar'
CC='gcc'
CGO_CFLAGS='-O2 -g'
CGO_CPPFLAGS=''
CGO_CXXFLAGS='-O2 -g'
CGO_ENABLED='1'
CGO_FFLAGS='-O2 -g'
CGO_LDFLAGS='-O2 -g'
CXX='g++'
GCCGO='gccgo'
GO111MODULE=''
GOAMD64='v1'
GOARCH='amd64'
GOAUTH='netrc'
GOBIN=''
GOCACHE='/home/user/.cache/go-build'
GOCACHEPROG=''
GODEBUG=''
GOENV='/home/user/.config/go/env'
GOEXE=''
GOEXPERIMENT=''
GOFIPS140='off'
GOFLAGS=''
GOGCCFLAGS='-fPIC -m64 -pthread -Wl,--no-gc-sections -fmessage-length=0 -ffile-prefix-map=/tmp/.private/user/go-build1193713493=/tmp/go-build -gno-record-gcc-switches'
GOHOSTARCH='amd64'
GOHOSTOS='linux'
GOINSECURE=''
GOMOD='/home/user/develop/golang/HelloWorld/go.mod'
GOMODCACHE='/home/user/go/pkg/mod'
GONOPROXY=''
GONOSUMDB=''
GOOS='linux'
GOPATH='/home/user/go'
GOPRIVATE=''
GOPROXY='https://proxy.golang.org,direct'
GOROOT='/home/user/develop/compilers/go'
GOSUMDB='sum.golang.org'
GOTELEMETRY='local'
GOTELEMETRYDIR='/home/user/.config/go/telemetry'
GOTMPDIR=''
GOTOOLCHAIN='auto'
GOTOOLDIR='/home/user/develop/compilers/go/pkg/tool/linux_amd64'
GOVCS=''
GOVERSION='go1.24.0'
GOWORK=''
PKG_CONFIG='pkg-config'
```

Полный список переменных окружения среды разработки Golang можем просмотреть используя интерактивную справочну систему:
```
user@vhost-129 HelloWorld]$ go help environment
The go command and the tools it invokes consult environment variables
for configuration. If an environment variable is unset or empty, the go
command uses a sensible default setting. To see the effective setting of
the variable <NAME>, run 'go env <NAME>'. To change the default setting,
run 'go env -w <NAME>=<VALUE>'. Defaults changed using 'go env -w'
are recorded in a Go environment configuration file stored in the
per-user configuration directory, as reported by os.UserConfigDir.
The location of the configuration file can be changed by setting
the environment variable GOENV, and 'go env GOENV' prints the
effective location, but 'go env -w' cannot change the default location.
See 'go help env' for details.

General-purpose environment variables:
...
```

#### Переменная окружения GOPATH
Переменная окружения `GOPATH` определяет корень струкутуры файлового поддерева рабочих проектов пользователя. В нашем случае переменная окружения `GOPATH` содержит значение `/home/user/go`, не спотря на то, что проект `HelloWorld` мы расположили в каталоге `/home/develop/golang` (_надо будет изменить размещение проектов под значение переменной окружения GOPATH..._); 
Переменная окружения `GOPATH` может переустанавливаться под каждый сменяемый проект в разработке.

#### Переменная окружения GOROOT
Переменная окружения `GOROOT` указывает на стабильное местоположени, опредленное инстялляцией инструментырия.

### 6. Инициализация переменных окружения при запуске `Terminal` (~/.bashrc)
Выше инициализированная нами переменная окружения `PATH` восстановиться в свое первоначальное значение при закрытии и повторном запуске терминальной программы или при открытии нового окна терминальной программы.

Для того, что бы переменная окружения `PATH` содержала значение необходимое для работы с компилятором `golang` в конец файла `~/.bashrc` необходимо добавить:
```
# ~/.bashrc
# The individual per-interactive-shell startup file.

# Source global definitions.
if [ -r /etc/bashrc ]; then
        . /etc/bashrc
fi

# Define user specific aliases and functions.
export PATH=$PATH:/home/user/develop/compilers/go/bin:/home/user/compilers/go/bin
```

_Статья окончена! Благодарю за внимание!_