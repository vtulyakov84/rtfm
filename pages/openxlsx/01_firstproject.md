## C++17 и OpenXLSX: Создание и чтение Excel-файлов с нуля

#### Время публикации:
09.09.2026 19:11

#### Цель:
Освоение интеграции библиотеки `OpenXLSX` в проекты на **C++17** и проверка корректности работы с Excel-файлами из кода на C/C++.

#### Хостовая система:
```bash
$ cat /etc/os-release | grep -E '^NAME=|^VERSION='
NAME="ALT Workstation"
VERSION="10.4"

$ uname -a
Linux host 6.1.173-un-def-alt1 #1 SMP PREEMPT_DYNAMIC Fri May 15 16:00:43 UTC 2026 x86_64 GNU/Linux
```

#### Требования:
* для работы с библиотекой `OpenXLSX` требуется минимальная версия стантарта **C++17**,

* обязательной кодровкой строк, перед передачей на обработку является **UTF-8** [*](https://github.com/troldal/OpenXLSX#1)

* для работы с большими файлами (миллионы ячеек) рекомендуется использовать итераторы по диапазонам вместо циклического вызова cell() для каждой ячейки, что значительно ускоряет работу [*](https://stackoverflow.com/questions/79879621/openxlsx-access-of-cell-values-is-too-slow#1)

* на момент написания статьи, библиотека `OpenXLSX` не поддерживает форматирование ячеек, диаграммы и графики

#### Содержание:

1. Файловая структура каталогов проекта

2. Подключение OpenXLSX к проекту (CMakeLists.txt)

3. Исходный код проекта (src/main.cpp)

4. Сборка и запуск проекта

### 1. Файловая структура каталогов проекта

```bash
# Создаем каталог проекта, и переходим в него
mkdir openxlsx-test01 && cd openxlsx-test01  
```
```bash
# Создаем каталоги проекта
mkdir build external src doc

# Создаем файл сценария сборки проекта
touch CMakeLists.txt
```
```bash
# Итоговая файловая структура проекта
[user@host openxlsx-test01]$ tree . -L 1 --dirsfirst
.
├── build
├── external
├── src
└── CMakeLists.txt

3 directories, 1 file
```

### 2. Подключение OpenXLSX к проекту (CMakeLists.txt)
Подключем исходный код OpenXLSX как поддиректорию нашего проекта. Такой подход, рекомендуемый авторами библиотеки, также решит проблемы с версиями и компиляцией:

```cmake
cmake_minimum_required(VERSION 3.15)
project(openxlsx-test01 VERSION 1.0.0)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# 1. Подключаем исходный код OpenXLSX
add_subdirectory(external/OpenXLSX)

# 2. Создаём исполняемый файл
add_executable(${PROJECT_NAME}
    src/main.cpp
)

# 3. Линкуем библиотеку к нашему приложению
target_link_libraries(${PROJECT_NAME}
    PRIVATE
        OpenXLSX::OpenXLSX
)

# 4. Указываем, где искать заголовочные файлы OpenXLSX
target_include_directories(${PROJECT_NAME}
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/external/OpenXLSX/include
)
```

### 3. Исходный код проекта (src/main.cpp)

Приведенные пример создаёт новый файл `example.xlsx`, записывает данные в ячейки и считывает их обратно .

> Важно: OpenXLSX работает только с UTF-8. Убедитесь, что ваш исходный файл сохранён в UTF-8 .

```cpp
#include <iostream>
#include <OpenXLSX.hpp>

using namespace OpenXLSX;

int main() {
    try {
        // 1. Создаём новый документ
        XLDocument doc;
        doc.create("example.xlsx");

        // 2. Получаем доступ к первому листу
        auto wks = doc.workbook().worksheet("Sheet1");

        // 3. Записываем данные в ячейки
        wks.cell("A1").value() = "Привет, OpenXLSX!";
        wks.cell("B1").value() = 42;
        wks.cell("C1").value() = 3.14159;

        std::cout << "Данные записаны." << std::endl;

        // 4. Сохраняем файл
        doc.save();
        std::cout << "Файл example.xlsx сохранён." << std::endl;

        // 5. Переоткрываем файл для чтения
        doc.open("example.xlsx");
        auto readWks = doc.workbook().worksheet("Sheet1");

        // 6. Читаем данные обратно
        std::string strVal = readWks.cell("A1").value().get<std::string>();
        int intVal = readWks.cell("B1").value().get<int>();
        double doubleVal = readWks.cell("C1").value().get<double>();

        std::cout << "\nПрочитанные данные:" << std::endl;
        std::cout << "A1: " << strVal << std::endl;
        std::cout << "B1: " << intVal << std::endl;
        std::cout << "C1: " << doubleVal << std::endl;

        doc.close();

    } catch (const std::exception& e) {
        std::cerr << "Ошибка: " << e.what() << std::endl;
        return 1;
    }

    return 0;
}
```

### 4. Сборка и запуск проекта

```bash
# 1. Проверяем локацию, что мы в каталоге проекта
$ pwd
/home/user/develop/gcc/openxlsx-test01

# 2. Клонируем OpenXLSX в папку external
$ git clone https://github.com/troldal/OpenXLSX.git external/OpenXLSX

# 3. Переходим в каталог для сборки
$ cd build

# 4. Выполняем инициализацию CMakeLists.txt
$ cmake ..

# 5. Выполняем сборку проекта
$ make

# 6. Запускаем приложение
$ ./openxlsx-test01 
Данные записаны.
Файл example.xlsx сохранён.

Прочитанные данные:
A1: Привет, OpenXLSX!
B1: 42
C1: 3.14159

# 7. Проверяем наличие выходного файла example.xlsx
$ ls -la example.xlsx 
-rw-r--r-- 1 user user 6505 сен  9 19:02 example.xlsx
```