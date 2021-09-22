# Параметры и настройка через launch

## Загрузка робота

После включения робота, происходит загрузка системы и автоматический запуск всех необходимых для работы робота нод. Управление нодами, которые будут загружены, возможно через .launch файл `/etc/ros/noetic/turtlebro.d/turtlebro.launch`

**По умолчанию запускаются ноды.**

`/arduino_serial_node   
/robot_state_publisher   
/rosout  
/simple_odom   
/rplidarNode   
/stm_serial_node  
/webserver  
/web_telemetry_node`

**Работают топики**

`/bat /client_count /cmd_vel /connected_clients /diagnostics /imu /joint_states /odom /odom_pose2d /raw_odom /rosout /rosout_agg /scan /tf /tf_static /web_tele`

**Запущены сервисы**

`/arduino_serial_node/get_loggers /arduino_serial_node/set_logger_level /board_info /power/off /power/reset /reset /robot_state_publisher/get_loggers /robot_state_publisher/set_logger_level /rosapi/action_servers /rosapi/delete_param /rosapi/get_loggers /rosapi/get_param /rosapi/get_param_names /rosapi/get_time /rosapi/has_param /rosapi/message_details /rosapi/node_details /rosapi/nodes /rosapi/publishers /rosapi/search_param /rosapi/service_host /rosapi/service_node /rosapi/service_providers /rosapi/service_request_details /rosapi/service_response_details /rosapi/service_type /rosapi/services /rosapi/services_for_type /rosapi/set_logger_level /rosapi/set_param /rosapi/subscribers /rosapi/topic_type /rosapi/topics /rosapi/topics_and_raw_types /rosapi/topics_for_type /rosbridge_websocket/get_loggers /rosbridge_websocket/set_logger_level /rosout/get_loggers /rosout/set_logger_level /rplidarNode/get_loggers /rplidarNode/set_logger_level /set_pid /simple_odom/get_loggers /simple_odom/set_logger_level /start_motor /stm_serial_node/get_loggers /stm_serial_node/set_logger_level /stop_motor /web_telemetry_node/get_loggers /web_telemetry_node/set_logger_level /webserver/get_loggers /webserver/set_logger_level`

## Параметры \(rosparams\)

Установка параметров возможна через команду `rosparam set` или через `.launch` файлы

* **stm\_serial\_node/wheel\_distance** double; meters -- расстояние между колесами
* **stm\_serial\_node/wheel\_param** uint32\_t; number of ticks per meter -- расчетный коэффициент
* формула для расчета **wheel\_param**:

  **wheel\_param** = ticks\*red\_ratio/circle  
  где

  * **ticks** - количество тиков энкодера на один оборот мотора, штук
  * **red\_ratio** - коэффициент передачи редуктора, безразмерный
  * **circle** - длина окружности колеса, метров

После изменения параметров необходимо перезапустить контроллер TurtleBoard нажав на кнопку `restart` с правой стороны робота. Прошивка TurtleBoard читает параметры только при загрузке.

В случае, когда пользователь не установил никаких параметров, происходит загрузка TurtleBoard с параметрами по умолчанию, которые обеспечивают работу идущих в комплекте с роботом моторов и колес. Процедура обновления параметров нужна только в том случае, когда плата TurtleBoard настраивается для работы с нестандартным шасси.   
  
Загруженные параметры не сохраняются в постоянную память контроллера, поэтому при установке неправильных параметров необходимо произвести перезапуск устройства, чтобы загрузить значения по умолчанию.

### Настрока параметров в файле .ros\_params

Файл `/home/pi/.ros_params` содержит парамметры окружения для старта ROS

```text
export ROS_HOSTNAME=$machine_ip
export ROS_IP=$machine_ip
export ROS_MASTER_URI=http://$machine_ip:11311
export ROVER_MODEL=turtlebro
#export ROVER_WHEEL_PARAM=12280
```

Переменная окружения ROVER\_WHEEL\_PARAM определяет параметр **wheel\_param** для определения типа моторов. Для старых моторов применяеться значение по умолчан 22500 для новых моторов 12280. Если одометрия робота не совпадает, необходимо провести калибровку параметра.

### Настройка ПИД параметров моторов

Для настройки коэффициентов ПИД регулятора можно использовать сервис ros `/set_pid`

```text
rosservice call /set_pid "Ki: 0.0 Kp: 0.0 Kd: 0.0"
```

## Файл turtlebro.launch

Главный файл конфигурации -- подключает другие .launch файлы и глобальные настройки.

```text
<arg name="run_rosserial" default="true"/>
<arg name="run_rplidar" default="true"/>
<arg name="run_turtlebro_web" default="true"/>
<arg name="run_camera_ros" default="false"/>
<arg name="run_simple_odom" default="true"/>
```

`run_rosserial` -- запуск rosserial.launch для соединения с Arduino и STM МК  
`run_rplidar` -- запустить получение данные с RPLidar

`run_camera_ros` -- включить камеру через пакет `uvc_camera`

`run_simple_odom` -- подключить паблишер приведенной одометрии `/odom_pose2d`

## Файл rosserial.launch

Файл запуска нод `stm_serial_node` и `arduino_serial_node` необходимых для работы с Arduino и STM

#### МК STM TurtleBoard

Подключается через устройство, скорость **460800** бод/с`/dev/serial/by-id/usb-Silicon_Labs_CP2102_USB_to_UART_Bridge_Controller_0012-if00-port0`

#### Arduino

Подключается через устройство, скорость **115200** бод/с`/dev/serial/by-id/usb-Silicon_Labs_CP2102_USB_to_UART_Bridge_Controller_0010-if00-port0`

## Файл rplidar.launch

Файл для запуска ноды обработки данных с лидара. Для работы необходим пакет `rplidar` от производителя лидара. Параметры файла настроены для работы с лидаром RPLidar A1 на скорости 115200 бод/с через устройство `/dev/serial/by-id/usb-Silicon_Labs_CP2102_USB_to_UART_Bridge_Controller_0011-if00-port0`

Данные с лидара вычисляются относительно фрейма `base_scan`

## Файл camera\_ros.launch

Файл для запуска издателя с данными полученными из фронтальной камеры. Подробнее о [работе с камерой](video.md#paket-uvc_camera)

