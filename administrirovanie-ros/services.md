# Сервисы TurtleBro

При загрузке ОС происходит запуск сервиса `turtlebro` `(файл /lib/systemd/system/turtlebro.service`\), который запускает `roscore` и запускает `launch` файл пакета `turtlebro` из `/etc/ros/kinetic/turtlebro.d/turtlebro.launch` . 

Оба файла перезаписываются при сборке пакета, поэтому их редактирование имеет смысл только в экспериментальных целях. Все изменения необходимо производить в файлах пакета `turtlebro`, а потом производить его "сборку".

Запустить сборку пакета turtlebro на роботе вы можете коммандой 

```text
sudo ./src/catkin/bin/catkin_make_isolated --install -DCMAKE_BUILD_TYPE=Release --install-space /opt/ros/kinetic --pkg=turtlebro 
```

Из директории `~/ros_catkin_ws` или запустить скрипт `./build.sh` в этой директории.

Сборкой пакета называют выполнение ряда операций над пакетом \(компиляция бинарный файлов, копирование  необходимых файлов в системные папки\).

## Команды управления сервисом turtlebro

Остановить сервис

```bash
sudo systemctl stop turtlebro
```

Запустить сервис

```bash
sudo systemctl start turtlebro
```

Убрать сервис из автозагрузки

```bash
sudo systemctl disable turtlebro
```

Поставить сервис в автозагрузку

```bash
sudo systemctl enable turtlebro
```

