# Установка и обновление

Установить последнюю версию пакета `turtlebro` можно из репозитория GitHub [https://github.com/voltbro/turtlebro](https://github.com/voltbro/turtlebro)

Пакет необходимо установить в директорию /home/pi/ros\_catkin\_ws/src/turtlebro

Скачать пакет \(если пакет не был установлен\)

```text
cd ~/ros_catkin_ws/src/
git clone https://github.com/voltbro/turtlebro
```

Обновить пакет \(если пакет был установлен через git\)

```text
cd ~/ros_catkin_ws/src/turtlebro
git pull
```

После обновления необходимо произвести сборку пакета `turtlebro` утилитой `catkin_make`

```text
cd ~/ros_catkin_ws
sudo ./src/catkin/bin/catkin_make_isolated --install -DCMAKE_BUILD_TYPE=Release --install-space /opt/ros/melodic --pkg=turtlebro
```

При сборке будут перезаписаны все файлы пакета в директорию `/opt/ros/melodic` и все файлы управления `systemd` сервисами.

Далее необходимо остановить и запустить сервис [turtlebro](../administrirovanie-ros/services.md)

