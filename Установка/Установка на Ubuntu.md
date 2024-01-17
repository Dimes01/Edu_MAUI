> Также с процессом установки можно ознакомиться в статье [.NET MAUI on Linux with Visual Studio Code](https://techcommunity.microsoft.com/t5/educator-developer-blog/net-maui-on-linux-with-visual-studio-code/ba-p/3982195).

Для демонстрации примера будет использоваться виртуальная машина со следующими параметрами: 
* Ubuntu (64-bit) 22.04.1 LTS; 
* 8 ГБ ОЗУ;
* 60 ГБ диск; 
* 3 CPU;

> [!info]
> После всех настроек "Системный монитор" может показывает около 30 ГБ занятого пространства, поэтому можно выделить не 60 ГБ, а меньше.

# Установка .NET SDK

Для проверки текущего состояния .NET можно использовать следующую команду в терминале: 
`dotnet --info`.

Для установки нужно перейти на страницу с загрузочными [скриптами](https://dotnet.microsoft.com/en-us/download/dotnet/scripts), где необходимо скопировать скрипт для Bash. На момент написания инструкции он выглядит следующим образом: `https://dotnet.microsoft.com/download/dotnet/scripts/v1/dotnet-install.sh`. Для использования скрипта необходимо ввести команду: 
`wget https://dotnet.microsoft.com/download/dotnet/scripts/v1/dotnet-install.sh`

После этого в домашнем каталоге должен появиться файл `dotnet-install.sh`. Проверить это можно, введя команду `ls`.

Затем введём следующие команды:
* `chmod +x dotnet-install.sh`
* `./dotnet-install.sh --channel 7.0`

После этого необходимо определить переменные `DOTNET ROOT` и `PATH` внутри файла `.bashrc`. Это можно сделать следующими командами:
* `echo 'export DOTNET_ROOT=$HOME/.dotnet' >> ~/.bashrc`
* `echo 'export PATH=$PATH:$DOTNET_ROOT:$DOTNET_ROOT/tools' >> ~/.bashrc`

После настройки необходимо ввести следующую команду: `source .bashrc`

Теперь, если ввести `dotnet --info`, то должна отобразиться информация об установленном .NET SDK.

# Установка дополнительных рабочих нагрузок

Так как на Linux поддерживается только разработка под Android, то устанавливать нагрузки необходимо командой: `dotnet workload install maui-android`. Использование `dotnet workload install maui` выдаст ошибку.

# Установка OpenJDK 11

> Также установка описана в [microsoft_learn](https://learn.microsoft.com/en-us/java/openjdk/install#install-on-ubuntu)

Для установки OpenJDK необходимо загрузить репозиторий:
```bash
ubuntu_release=`lsb_release -rs`
wget https://packages.microsoft.com/config/ubuntu/${ubuntu_release}/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

После загрузки выполнить следующие команды:
```bash
sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install msopenjdk-11
```

> [!warning]
> При копировании команд с документации Майкрософт необходимо поменять последнюю версию (на момент написания заметки это 21) на `msopenjdk-11`.

# Установка Android Studio

Android Studio нужна для установки SDK для Android. Для этого воспользуемся Ubuntu Software. 
![[Android Studio в Ubuntu Software.png]]

Тип установки необходимо выбрать `Standard`.
![[Тип установки Android Studio.png]]

После этого можно просто нажимать `Next`, пока не начнётся установка. После установки необходимо запустить Android Studio и в приветственном окне нажать на выпадающий список `More Actions`, где выбрать `SDK Manager`.
![[Выбор SDK Manager в Android Studio.png]]

Изначально выбран SDK последней версии, однако я установлю ещё и SDK для Android 11 (API 30), так как для отладки буду использовать реальное устройство на Android 11.
![[Выбор SDK Platforms.png]]

# Установка Visual Studio Code
Также, как и Android Studio, устанавливаем VS Code, а затем устанавливаем расширение `.NET MAUI`.
![[Расширение NET MAUI для VS Code.png]]

# Итог
Если всё получилось, то в Visual Studio Code можно будет создать шаблон приложения .NET MAUI.
![[Шаблон для создания приложения MAUI.png]]

Однако файл проекта ещё надо настроить. Для этого откроем файл с расширением `.csproj` и сделаем следующее.

Первый тег `TargetFrameworks` заменяем на это:
```xml
<TargetFrameworks>net7.0-android</TargetFrameworks>
```

Перед строкой
```xml
<TargetFrameworks Condition="$([MSBuild]::IsOSPlatform('windows'))">...</TargetFrameworks>
```
вставляем:
```xml
<TargetFrameworks Condition="!$([MSBuild]::IsOSPlatform('linux'))">$(TargetFrameworks);net7.0-android;net7.0-ios;net7.0-maccatalyst</TargetFrameworks>
```

В итоге, должно получиться примерно следующее:
```xml
...
<TargetFrameworks>net7.0-android</TargetFrameworks>
<TargetFrameworks Condition="!$([MSBuild]::IsOSPlatform('linux'))">$(TargetFrameworks);net7.0-android;net7.0-ios;net7.0-maccatalyst</TargetFrameworks>
<TargetFrameworks Condition="$([MSBuild]::IsOSPlatform('windows'))">$(TargetFrameworks);net7.0-windows10.0.19041.0</TargetFrameworks>
...
```

# Возможные ошибки
## Android SDK: Not Found
Необходимо указать путь до Android SDK. Решение:
1. Вызовите  палитру команд `Ctrl + Shift + P`;
2. Введите `>Configure Android` или `>Настройка Android` и выберите соответствующий пункт.
3. Выберите `Настройка пути пакета SDK для Android`.
4. Укажите путь, где хранится SDK. По умолчанию это будет `Домашняя папка/Android/Sdk`.