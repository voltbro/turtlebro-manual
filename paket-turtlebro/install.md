# Установка и обновление пакетов

Установить последнюю версию пакета `turtlebro` можно из репозитория GitHub [https://github.com/voltbro/turtlebro](https://github.com/voltbro/turtlebro)

Пакет необходимо установить в директорию /home/pi/ros\_catkin\_ws/src/turtlebro

Скачать пакет \(если пакет не был установлен\)

```text
cd ~/catkin_ws/src/
git clone https://github.com/voltbro/turtlebro
```

Обновить пакет \(если пакет был установлен через git\)

```text
cd ~/catkin_ws/src/turtlebro
git pull
```

После обновления необходимо произвести сборку пакета `turtlebro` утилитой `catkin_make`

```text
cd ~/catkin_ws
catkin_make --pkg turtlebro
```

При сборке будут перезаписаны все файлы пакета в директорию `/opt/ros/noetic` и все файлы управления `systemd` сервисами.

Далее необходимо остановить и запустить сервис [turtlebro](../administrirovanie-ros/services.md)

