# Обновление платформы

### Обновление на версию 0.9&#x20;

В прошивке начиная с версии 0.9 используеться ROS Noetic и python3. Для обновления на актуальную версию необходимо.

1. Обновить [прошивку системной платы](platforma-turtleboard/obnovlenie-mikroprogrammy/obnovlenie-mikroprogrammy.md) на прошивку версии 2.0 или новее.
2. Подготовить [SD карту](administrirovanie-ros/raspberrypi.md) для RaspberryPi с версией ПО 0.9 или новее

В файле .ros\_params проверить значение переменной окружения [ROVER\_WHEEL\_PARAM](paket-turtlebro/params.md#nastroika-parametrov-v-faile-ros\_params) для задания коэффициента вращения моторов.

Начиная с версии 0.9 пакеты ROS устанавливаются в двух разных директориях окружений. Системные пакеты установленны в директорию `ros_catkin_ws`. В директорию `catkin_ws` установленны пользовательские пакеты. Подробнее и использовании разных [рабочих окружений](paket-turtlebro/install.md)

