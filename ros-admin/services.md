# Сервисы TurtleBro

При загрузке ОС происходит запуск сервиса `turtlebro` `(файл /lib/systemd/system/turtlebro.service`\), который запускает `roscore` и запускает `launch` файл пакета `turtlebro` из `/etc/ros/kinetic/turtlebro.d/turtlebro.launch` . Оба файла перезаписываются при компиляции пакета, поэтому их редактирование имеет смысл только в эксперементальных целях. Изменения необходимо вностить в файлы пакета `turtlebro`

### Управление Сервисом

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

