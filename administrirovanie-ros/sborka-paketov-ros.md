# Сборка пакетов ROS

## Рабочее окружение&#x20;

На роботе создано два рабочих окружения  для ROS пакетов. Директория `catkin_ws` для пользовательский пакетов и `ros_catkin_ws` для системных пакетов.&#x20;

#### Пользовательское окружение в директории `catkin_ws`

Рекомендуется все новые и пользовательские пакеты устанавливать а директорию `catkin_ws/src`

Например установка пакета из git-репозитория:

```
cd ~/catkin_ws/src
git clone repo_name
cd ..
catkin_make --pkg repo_name
```

При работе в директории `catkin_ws` ROS использует исходные файлы сразу из этой директории. Поэтому, например, при изменении лаунч-файлов, дополнительно собирать пакеты не нужно. Такой подход упрощает тестирование и разработку.

### Обновление системных пакетов

Системные пакеты ROS  установлены из исходных кодов, а не загружены при помощи пакетного менеджера `apt`.Таким образом, для обновления старых системных пакетов и установки новых необходимо собирать пакеты из исходных кодов.

#### Сборка дистрибутива ROS

Все команды выполняются в директории основного окружения ROS `/home/pi/ros_catkin_ws`

Пересобрать все системные пакеты ROS

```
sudo ./src/catkin/bin/catkin_make_isolated --install -DCMAKE_BUILD_TYPE=Release --install-space /opt/ros/noetic -DPYTHON_EXECUTABLE=/usr/bin/python3
```

Все системные пакеты после сборки будут остановленны в директорию `/opt/ros/noetic`

Собрать один конкретный пакет можно командой:&#x20;

```
sudo ./src/catkin/bin/catkin_make_isolated --install -DCMAKE_BUILD_TYPE=Release --install-space /opt/ros/noetic -DPYTHON_EXECUTABLE=/usr/bin/python3 --pkg=pkg_name
```

где  `pkg_name` это имя того пакета, который надо собрать отдельно.

#### Установка новых системных пакетов из дистрибутива ROS

Все новые пакеты необходимо также устанавливать из дистрибутива пакетов ROS.&#x20;

Установка пакета `new_pack` из дистрибутива пакетов ROS:

```
rosinstall_generator new_pack --rosdistro noetic --deps --tar > new_pack.rosinstall
vcs import src < new_pack.rosinstall
rosdep install --from-paths ./src --ignore-packages-from-source --rosdistro noetic -y
sudo ./src/catkin/bin/catkin_make_isolated --install -DCMAKE_BUILD_TYPE=Release --install-space /opt/ros/noetic -DPYTHON_EXECUTABLE=/usr/bin/python3 --pkg=new_pack
```

Если пакет не имеет зависимостей, то его можно собрать без сборки всего ROS.

```
sudo ./src/catkin/bin/catkin_make_isolated --install -DCMAKE_BUILD_TYPE=Release --install-space /opt/ros/noetic -DPYTHON_EXECUTABLE=/usr/bin/python3 --pkg=new_pack
```



