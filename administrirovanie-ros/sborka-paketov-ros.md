# Сборка пакетов ROS

Пакеты на роботе установлены из исходных кодов, а не загружены при помощи пакетного менеджера `apt` Таким образом, для обновления старых и установки новых пакетов надо научится собирать их из исходных кодов.

#### Сборка дистрибутива

Все команды выполняются в директории "воркспейса" ROS `/home/pi/ros_catkin_ws`

Пересобрать все пакеты ROS

```text
sudo ./src/catkin/bin/catkin_make_isolated --install -DCMAKE_BUILD_TYPE=Release --install-space /opt/ros/melodic
```

Пересобрать один конкретный пакет можно командой: 

```text
sudo ./src/catkin/bin/catkin_make_isolated --install -DCMAKE_BUILD_TYPE=Release --install-space /opt/ros/melodic  --pkg=pkg_name
```

где  `pkg_name` это имя того пакета, который надо собрать отдельно.

#### Установка новых пакетов ROS

Все новые пакеты необходимо также устанавливать из дистрибутива пакетов ROS. 

Установка пакета `new_pack` из дистрибутива пакетов ROS

```text
rosinstall_generator new_pack --rosdistro melodic --deps --wet-only --tar > new_pack.rosinstall
wstool merge -t src new_pack.rosinstall
wstool update -t src
rosdep install -y --from-paths src --ignore-src --rosdistro melodic -r --os=debian:buster
sudo ./src/catkin/bin/catkin_make_isolated --install -DCMAKE_BUILD_TYPE=Release --install-space /opt/ros/melodic
```

Если пакет не имеет зависимостей, то его можно собрать без пересборки всего ROS.

```text
sudo ./src/catkin/bin/catkin_make_isolated --install -DCMAKE_BUILD_TYPE=Release --install-space /opt/ros/melodic --pkg=new_pack
```

#### Установка пакета из исходных кодов

Скачать и распаковать пакет в директорию `ros_catkin_ws/src`

Изучить документацию, установить системные зависимости и необходимые ROS пакеты.

Собрать и установить пакет

```text
sudo ./src/catkin/bin/catkin_make_isolated --install -DCMAKE_BUILD_TYPE=Release --install-space /opt/ros/melodic --pkg=new_pack
```



