## Go + GTK в Linux: Hello World

#### Пошаговое руководство по созданию простого GUI-приложения "Hello World" на Go с использованием GTK в Linux.

### Шаг 1: Установка GTK и Go

Для начала установите необходимые библиотеки:

```bash
# Установка GTK3 и компилятора
sudo apt update
sudo apt install libgtk-3-dev gcc pkg-config
```

> Внимание! В Alt Linux пакет `libgtk-3-dev` имеет название `libgtk+3-devel` - Development files and tools for GTK+ applications.


```bash
# Проверка установки Go (если не установлен)
go version
```

### Шаг 2: Установка Go-биндингов для GTK
Для Go существует несколько библиотек для работы с GTK. Наиболее популярные:

<table>
    <tr>
        <td>Библиотека</td>
        <td>GTK версия</td>
        <td>Особенности</td>
    </tr>
    <tr>
        <td>gotk3</td>
        <td>GTK3</td>
        <td>Стабильная, хорошая документирована</td>
    </tr>
    <tr>
        <td>go-gtk</td>
        <td>GTK2/GTK3</td>
        <td>Простая, менее активная</td>
    </tr>
    <tr>
        <td>gotk4</td>
        <td>GTK4</td>
        <td>Современная, более сложная</td>
    </tr>
</table>

Установим gotk3, как самый распространённый вариант:

```bash
go get github.com/gotk3/gotk3/gtk
```

### Шаг 3: Создание "Hello World" приложения

Создайте файл main.go:
```go
go
package main

import (
    "log"
    "os"

    "github.com/gotk3/gotk3/gtk"
)

func main() {
    // Инициализация GTK
    gtk.Init(&os.Args)

    // Создание главного окна
    win, err := gtk.WindowNew(gtk.WINDOW_TOPLEVEL)
    if err != nil {
        log.Fatal("Ошибка создания окна:", err)
    }

    // Настройка окна
    win.SetTitle("Hello World на Go + GTK")
    win.SetDefaultSize(400, 300)
    
    // Обработка закрытия окна
    win.Connect("destroy", func() {
        gtk.MainQuit()
    })

    // Создание текстовой метки
    label, err := gtk.LabelNew("Привет, Мир! 👋")
    if err != nil {
        log.Fatal("Ошибка создания метки:", err)
    }

    // Создание кнопки
    button, err := gtk.ButtonNewWithLabel("Нажми меня!")
    if err != nil {
        log.Fatal("Ошибка создания кнопки:", err)
    }

    // Обработка нажатия кнопки
    button.Connect("clicked", func() {
        label.SetLabel("Кнопка нажата! 🎉")
    })

    // Контейнер для вертикального размещения элементов
    box, err := gtk.BoxNew(gtk.ORIENTATION_VERTICAL, 10)
    if err != nil {
        log.Fatal("Ошибка создания контейнера:", err)
    }
    box.SetMarginStart(20)
    box.SetMarginEnd(20)
    box.SetMarginTop(20)
    box.SetMarginBottom(20)

    // Добавление элементов в контейнер
    box.PackStart(label, false, false, 0)
    box.PackStart(button, false, false, 0)

    // Добавление контейнера в окно
    win.Add(box)

    // Отображение всех элементов
    win.ShowAll()

    // Запуск главного цикла обработки событий
    gtk.Main()
}
```


### Шаг 4: Запуск программы

```bash
# Запуск напрямую
go run main.go

# Или компиляция в исполняемый файл
go build -o hello-gtk main.go
./hello-gtk
```

**Объяснение кода**
`gtk.Init()` — обязательная инициализация GTK перед любыми операциями

`gtk.WindowNew()` — создание главного окна приложения

`Connect("destroy", ...)` — подключение обработчика события закрытия окна

`gtk.BoxNew()` — создание контейнера для организации элементов по вертикали или горизонтали

`PackStart()` — добавление виджетов в контейнер

`gtk.Main()` — запуск основного цикла обработки событий (без этого окно не будет реагировать на действия пользователя)