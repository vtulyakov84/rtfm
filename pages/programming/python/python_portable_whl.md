# Python. Портабальная установка whl-файлов

### Файловая структура репозитория
```bash
[user@host github.com]$ tree smartmeter/ -L 2 -d
smartmeter_w/                # Базовый каталог репозитория
├── doc                      # Документы по артефактам репозитория
├── gurux                    # Исходные коды используемые gurux
│   └── python               # Исходные коды на python
│        ├── .gurux_venv     # Виртуальное окружение с компонентами gurux
│        ├── exam1           # Проект 1
│             ...            #
│        └── examN           # Проекта N
└── packages                 # Каталог хранения пакетов библиотек
    └── whl                  # Python-библиотеки
```

### 1. Сохрание gurux-пакетов в whl-файлы

```bash
# Скачаивание whl-файлов
.\packages\whl> pip download gurux-serial gurux-common gurux-net gurux-dlms -d .

# Выполняем создание виртуального окружения
> python3 -m venv .gurux_venv

# Активируем виртуальное окружение
> .\.gurux_venv\Scripts\Activate.ps1
(.gurux_venv) $ 

# Выполняем обновление менеджера пакетов
(.gurux_venv) $ pip install --upgrade pip

# Выполняем установку whl-файлов в виртуальное окружение
(.gurux_venv) $ pip install --no-index --find-links=../../packages/whl/ gurux-serial gurux-common gurux-net gurux-dlms

# Просмотреть список пакетов в виртуальном окружении
(.gurux_venv) $ pip3 list
Package      Version
------------ -------
gurux-common 1.0.13
gurux_dlms   1.0.197
gurux_net    1.0.23
gurux_serial 1.0.21
pip          26.0.1
setuptools   58.1.0

# Деактивация виртуального окружения
(.gurux_venv) $ deactivate
```