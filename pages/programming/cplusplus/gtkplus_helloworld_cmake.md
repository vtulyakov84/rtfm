### Файловая структура проекта
```bash
$ tree
.
└── test01_gtk3_helloworld
    ├── CMakeLists.txt
    └── src
        └── main.cpp
```

### Исходный код
```c
#include <gtk/gtk.h>

// Обработчик закрытия окна
static void on_window_destroy(GtkWidget *widget, gpointer data) {
    gtk_main_quit();
}

// Обработчик нажатия кнопки
static void on_button_clicked(GtkWidget *widget, gpointer data) {
    GtkWidget *label = GTK_WIDGET(data);
    gtk_label_set_text(GTK_LABEL(label), "Hello, World from GTK+!");
}

int main(int argc, char *argv[]) {
    GtkWidget *window;
    GtkWidget *vbox;
    GtkWidget *button;
    GtkWidget *label;
    
    // Инициализация GTK
    gtk_init(&argc, &argv);
    
    // Создание главного окна
    window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "GTK+ Hello World");
    gtk_window_set_default_size(GTK_WINDOW(window), 400, 300);
    gtk_container_set_border_width(GTK_CONTAINER(window), 10);
    
    // Подключение сигнала закрытия окна
    g_signal_connect(window, "destroy", G_CALLBACK(on_window_destroy), NULL);
    
    // Создание вертикального контейнера
    vbox = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(window), vbox);
    
    // Создание метки
    label = gtk_label_new("Click the button below");
    gtk_box_pack_start(GTK_BOX(vbox), label, TRUE, TRUE, 0);
    
    // Создание кнопки
    button = gtk_button_new_with_label("Click Me!");
    gtk_box_pack_start(GTK_BOX(vbox), button, FALSE, FALSE, 0);
    
    // Подключение сигнала нажатия кнопки
    g_signal_connect(button, "clicked", G_CALLBACK(on_button_clicked), label);
    
    // Отображение всех виджетов
    gtk_widget_show_all(window);
    
    // Запуск главного цикла GTK
    gtk_main();
    
    return 0;
}
```

### Содержание файла CMakeFile.txt
```c
cmake_minimum_required(VERSION 3.10)
project(hello_gtk VERSION 1.0.0)

# Устанавливаем стандарт C++
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Находим GTK+ библиотеку
find_package(PkgConfig REQUIRED)
pkg_check_modules(GTK REQUIRED gtk+-3.0)

# Создаем исполняемый файл
add_executable(${PROJECT_NAME}
            src/main.cpp)

# Подключаем GTK+ к цели
target_link_libraries(${PROJECT_NAME}
            PRIVATE
                ${GTK_LIBRARIES})

target_include_directories(${PROJECT_NAME}
            PRIVATE
                ${GTK_INCLUDE_DIRS})

target_compile_options(${PROJECT_NAME}
            PRIVATE
                ${GTK_CFLAGS_OTHER})

# Для отображения путей при сборке (опционально)
message(STATUS "GTK+ include dirs: ${GTK_INCLUDE_DIRS}")
message(STATUS "GTK+ libraries: ${GTK_LIBRARIES}")
```