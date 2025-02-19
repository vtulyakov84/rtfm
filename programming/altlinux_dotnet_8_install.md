## .NET 8. Варианты установки в ALT Linux

Содержание:
1. Ручной вариант установки .NET 8
2. Установка .NET 8 используя скрипт

### 1. Ручной вариант установки
Шаг 1.1. Определяем архитектуру используемой ОС:
```bash
[user@vhost-129 ~]$ arch
x86_64
```

Шаг 1.2. Со страницы [Скачать .NET 8.0](https://dotnet.microsoft.com/ru-ru/download/dotnet/8.0) скачиваем двоичный файл `SDK 8.0.406` для архитектуры используемой ОС, в нашем случае `x64`.

Шаг 1.3. Создаём переменную окружения `DOTNET_ROOT` рабочего каталога, за которым следует `.dotnet`.

Также создаём переменную окружения `DOTNET_FILE` среды — это имя файла двоичного выпуска .NET, который требуется установить. Этот файл извлекается в `DOTNET_ROOT` каталог. В `DOTNET_ROOT` переменную среды добавляются `PATH` каталог и его tools подкаталог.

> Примеачние. При выполнении этих команд не забудьте изменить `DOTNET_FILE` значение на имя загруженного двоичного файла .NET 8.

```bash
DOTNET_FILE=dotnet-sdk-9.0.100-linux-x64.tar.gz
export DOTNET_ROOT=$(pwd)/.dotnet

mkdir -p "$DOTNET_ROOT" && tar zxf "$DOTNET_FILE" -C "$DOTNET_ROOT"

export PATH=$PATH:$DOTNET_ROOT:$DOTNET_ROOT/tools
```

> В одной папке можно установить несколько версий .NET.

Шаг 1.4. Устанавливаем .NET в домашний каталог, определяемый переменной `HOME` или `~` путем:
```bash
export DOTNET_ROOT=$HOME/.dotnet
```

### 2. Установка .NET 8 используя скрипт

Шаг 2.1. Скачать скрипт установки используя `wget`:
```bash
wget https://dot.net/v1/dotnet-install.sh -O dotnet-install.sh
```

Шаг 2.2. Предоставляем скрипту разрешение на выполнение в качестве исполняемого файла:
```bash
chmod +x ./dotnet-install.sh
```

Шаг 2.3. Для установки последнего выпуска .NET 8 используем параметр `--version latest`:
```bash
./dotnet-install.sh --version latest
```


> __Примечание.__ Чтобы вместо пакета SDK установить среду выполнения .NET, используйте параметр `--runtime`:
```bash
./dotnet-install.sh --version latest --runtime aspnetcore
```

> Для установки необходимой версии .NET, используйте параметр `--channel`. Следующая команда устанавливает пакет SDK для .NET 9.0.
```bash
./dotnet-install.sh --channel 9.0
```

### Источники
- [Скачать .NET 8.0](https://dotnet.microsoft.com/ru-ru/download/dotnet/8.0)

- [Установка .NET в Linux с помощью скрипта установки или извлечения двоичных файлов](https://learn.microsoft.com/ru-ru/dotnet/core/install/linux-scripted-manual)