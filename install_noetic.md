# Обновление платформы

### Переход на версию 0.9 

В прошивке начиная с версии 0.9 используеться ROS Noetic и python3. Для обновления на актуальную версию необходимо.

1. Обновить [прошивку системной платы](platforma-turtleboard/obnovlenie-mikroprogrammy.md) на прошивку версии 2.0 или новее.
2. Подготовить [SD карту](administrirovanie-ros/raspberrypi.md) для RaspberryPi с версией ПО 0.9 или новее

В файле .ros\_params проверить значение переменной окружения [ROVER\_WHEEL\_PARAM](paket-turtlebro/params.md#nastroika-parametrov-v-faile-ros_params)

Начиная с версии 0.9 пакеты ROS устанавливываються в двух директориях. Системные в `ros_catkin_ws` для системных пакетов. И в директорию `catkin_ws` для пользовательских.



