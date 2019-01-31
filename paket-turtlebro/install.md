# Установка и обновлени

Установить последную версию пакета turtlebro возможно из репозитория GitHub [https://github.com/voltbro/turtlebro](https://github.com/voltbro/turtlebro)

Пакет необходимо установить в директорию /home/pi/catcin\_ws/src/turtlebro

Скачать пакет \(если пакет не был установле\)

```text
cd ros_catcin_ws/src/
git clone https://github.com/voltbro/turtlebro
```

Обновить пакет \(если пакет был установлен через git\)

```text
cd ros_catcin_ws/src/turtlebro
git pull 
```

После обновления необходимо произвести сборку пакета утилитой catkin\_make

```text
cd ros_catkin_ws
sudo ./src/catkin/bin/catkin_make_isolated --install -DCMAKE_BUILD_TYPE=Release --install-space /opt/ros/kinetic --pkg=navibro
```

При сборке будут перезаписаны все файлы пакета в директорю /opt/ros/kinetic и все файлы управления systemd сервисами.

Далее необходимо перезапустить сервис [turtlebro](../administrirovanie-ros/services.md)

