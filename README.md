# Установка suckless dwm на fedora

## Введение

В данной инструкции описывается установка dwm в fedora 33+ в качестве графического окружения. Может использоваться совместно с другими графическими окружениями с удобным переключением в окне ввода пароля.

### Что такое DWM?

DWM(dynamic window manager for X) минималистичный оконный менеджер написанный на C. Вся конфигурация и настройка происходит прямо в исходном коде, что позволяет настраивать оконный менеджер под себя без всяких ограничений.


## Ссылки
* [Fedora Linux](https://getfedora.org/)
* [Suckless Software](https://suckless.org/)

## Подготовка

### Обновление системы
```shell
localhost# dnf clean all
localhost# dnf update
```

### Установка необходимых пакетов
```shell
localhost# dnf install libX11-devel libXft-devel libXinerama-devel libXrandr-devel xorg-x11-xinit-session

# Пакеты необходимые для установки фона рабочего стола в dwm
localhost# dnf install feh xautolock
```

### Создание папки для исходного кода suckless программ
```shell
localhost$ cd /home/fedora
localhost$ mkdir /home/fedora/suckless
```

### Клонируем исходный код из репозиториев suckless
```shell
localhost$ cd /home/fedora/suckless
# Минималистичное меню. Используется в dwm
localhost$ git clone https://git.suckless.org/dmenu 
localhost$ git clone https://git.suckless.org/dwm
# Минималистичный эмулятор терминала от suckless
localhost$ git clone https://git.suckless.org/st

```

## Изменения в файлах /home/fedora/suckless/*/config.mk

### dmenu
```diff
--- a/config.mk   2020-12-10 00:49:19.557134533 +0000
+++ b/config.mk   2020-12-10 00:45:43.045783511 +0000
@@ -5,8 +5,8 @@
 PREFIX = /usr/local
 MANPREFIX = $(PREFIX)/share/man
 
-X11INC = /usr/X11R6/include
-X11LIB = /usr/X11R6/lib
+X11INC = /usr/include/X11
+X11LIB = /usr/lib64/X11
 
 # Xinerama, comment if you don't want it
 XINERAMALIBS  = -lXinerama
```

### dwm
```diff
--- a/config.mk   2020-12-10 00:52:16.391197270 +0000
+++ b/config.mk   2020-12-10 00:45:43.045783511 +0000
@@ -7,8 +7,8 @@
 PREFIX = /usr/local
 MANPREFIX = ${PREFIX}/share/man
 
-X11INC = /usr/X11R6/include
-X11LIB = /usr/X11R6/lib
+X11INC = /usr/include/X11
+X11LIB = /usr/lib64/X11
 
 # Xinerama, comment if you don't want it
 XINERAMALIBS  = -lXinerama
```

### st
```diff
--- a/config.mk   2020-12-10 00:55:15.384669426 +0000
+++ b/config.mk   2020-12-10 00:45:43.045783511 +0000
@@ -7,8 +7,8 @@
 PREFIX = /usr/local
 MANPREFIX = $(PREFIX)/share/man
 
-X11INC = /usr/X11R6/include
-X11LIB = /usr/X11R6/lib
+X11INC = /usr/include/X11
+X11LIB = /usr/lib64/X11
 
 PKG_CONFIG = pkg-config
```

## Сборка программ из исходного кода

### dmenu
```shell
localhost$ cd /home/fedora/suckless/dmenu
localhost$ make clean
localhost$ make
localhost$ sudo make install
localhost$ make clean
```

### dwm
```shell
localhost$ cd /home/fedora/suckless/dwm
localhost$ make clean
localhost$ make
localhost$ sudo make install
localhost$ make clean
```


### st
```shell
localhost$ cd /home/fedora/suckless/st
localhost$ make clean
localhost$ make
localhost$ sudo make install
localhost$ make clean
```

## /usr/share/xsessions/dwm.desktop

### Создаем dwm.desktop
```shell
localhost# cat /usr/share/xsessions/dwm.desktop
[Desktop Entry]
Name=dwm
Exec=/usr/libexec/xinit-compat
```

### Установка прав на dwm.desktop
```shell
localhost# chown root:root /usr/share/xsessions/dwm.desktop
localhost# chmod 0644 /usr/share/xsessions/dwm.desktop
```


## /home/fedora/.xsession

### Создаем файл ~/.xsession
```shell
localhost$ cat /home/fedora/.xsession
#!/bin/sh

/usr/bin/xset b off
/usr/bin/xsetroot -solid darkslategrey
# If feh was installed, this can be uncommented.
#/usr/bin/feh --bg-center 'IMAGE PATH HERE'
# If xautolocal was installed, this can be uncommented.
#/usr/bin/xautolock -locker /usr/local/bin/slock -detectsleep -secure -time 10 &
/usr/local/bin/slstatus &
/usr/local/bin/dwm
```

### И устанавливаем права на /home/fedora/.xsession
```shell
localhost$ chown fedora:fedora /home/fedora/.xsession
localhost$ chmod 0755 /home/fedora/.xsession
```

## Как использовать DWM в качестве оконного менеджера?

Перезагрузите компьютер. В окне ввода пароля, в нижнем левом углу в выпадающем списке выберите "dwm". Введите пароль. Добро пожаловать в DWM!

## Как открыть терминал?
```shell
`#MODKEY` + Shift + Enter
```
Поумолчанию, #MODKEY это Left Alt:

```shell
Left Alt + Shift + Enter
```

## Выход из DWM

If `#MODKEY` was left as the default:
```shell
#MODKEY + Shift + Q
```




