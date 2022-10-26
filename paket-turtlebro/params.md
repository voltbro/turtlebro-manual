# Параметры и настройка через launch

## Загрузка робота

После включения робота, происходит загрузка системы и автоматический запуск всех необходимых для работы робота нод. Управление нодами, которые будут загружены, возможно через лаунч-файл `/etc/ros/turtlebro.d/turtlebro.launch`

**По умолчанию запускаются следующие ноды:**

`/arduino_serial_node`\
`/republish_raw`\
`/robot_state_publisher`\
`/rosapi`\
`/rosbridge_websocket`\
`/rosout`\
`/rplidarNode`\
`/simple_odom`\
`/stm_serial_node`\
`/uvc_camera_node`\
`/web_telemetry_node`\
`/web_video_server`\
`/webserver`

**Работают топики:**

`/bat`\
`/client_count`\
`/cmd_vel`\
`/connected_clients`\
`/diagnostics`\
`/front_camera/camera_info`\
`/front_camera/image_raw`\
`/front_camera/image_raw/compressed`\
`/imu`\
`/joint_states`\
`/odom`\
`/odom_pose2d`\
`/raw_odom`\
`/republish_raw/compressed/parameter_descriptions /republish_raw/compressed/parameter_updates`\
`/rosout`\
`/rosout_agg`\
`/scan`\
`/tf`\
`/tf_static`\
`/web_tele`

**Запущены сервисы:**

`/arduino_serial_node/get_loggers /arduino_serial_node/set_logger_level /board_info /power/off /power/reset /republish_raw/compressed/set_parameters /republish_raw/get_loggers /republish_raw/set_logger_level /reset /robot_state_publisher/get_loggers /robot_state_publisher/set_logger_level /rosapi/action_servers /rosapi/delete_param /rosapi/get_loggers /rosapi/get_param /rosapi/get_param_names /rosapi/get_time /rosapi/has_param /rosapi/message_details /rosapi/node_details /rosapi/nodes /rosapi/publishers /rosapi/search_param /rosapi/service_host /rosapi/service_node /rosapi/service_providers /rosapi/service_request_details /rosapi/service_response_details /rosapi/service_type /rosapi/services /rosapi/services_for_type /rosapi/set_logger_level /rosapi/set_param /rosapi/subscribers /rosapi/topic_type /rosapi/topics /rosapi/topics_and_raw_types /rosapi/topics_for_type /rosbridge_websocket/get_loggers /rosbridge_websocket/set_logger_level /rosout/get_loggers /rosout/set_logger_level /rplidarNode/get_loggers /rplidarNode/set_logger_level /set_camera_info /set_pid /simple_odom/get_loggers /simple_odom/set_logger_level /start_motor /stm_serial_node/get_loggers /stm_serial_node/set_logger_level /stop_motor /uvc_camera_node/get_loggers /uvc_camera_node/set_logger_level /web_telemetry_node/get_loggers /web_telemetry_node/set_logger_level /web_video_server/get_loggers /web_video_server/set_logger_level /webserver/get_loggers /webserver/set_logger_level`

## Настройка параметров запуска

### Настройка параметров в файле .ros\_params <a href="#.ros-params" id=".ros-params"></a>

Файл `/home/pi/.ros_params` содержит параметры окружения для старта ROS:

```
export ROS_HOSTNAME=$machine_ip
export ROS_IP=$machine_ip
export ROS_MASTER_URI=http://$machine_ip:11311
export ROVER_MODEL=turtlebro
export ROVER_WHEEL_PARAM=12280
```

Переменная окружения ROVER\_WHEEL\_PARAM определяет параметр **wheel\_param** для определения типа моторов. Для старых моторов применяется значение: 22500; для новых моторов: 12280. Если одометрия робота из топика не совпадает с реальным перемещением робота, необходимо провести калибровку параметра.

### Параметры одометрии

Для того, чтобы одометрия точно отображала реальное перемещение робота используются два параметра:&#x20;

* **stm\_serial\_node/wheel\_distance** double&#x20;
  * тип данных - _числовой 64-битный тип_
  * размерность - _метры_
  * обозначает - _расстояние между колесами_
* **stm\_serial\_node/wheel\_param** uint32\_t&#x20;
  * тип данных - _числовой 32-битовый без знака_
  * размерность - _безразмерный_
  * обозначает- _число тиков энкодера на метр_. Является расчётным коэффициентом.

Для расчёта параметра **wheel\_param** используется следующая формула:

```
wheel_param = ticks*red_ratio/circle

ticks - количество тиков энкодера на один оборот мотора без учёта редуктора, штук
red_ratio - коэффициент передачи редуктора, безразмерный
circle - длина окружности колеса, метров

Пример рассчета для текущих моторов:
red_ratio=56
circle=pi * D(диаметр колеса)=3,14 * 0,065=0,2041
ticks=ticks_per_wheel_rotation(кол-во тиков на 1 оборот колеса)/red_ratio=2506/56=44,75

wheel_param = 44,75 * 56/0,2041 = ~12278
```

#### Установка параметров&#x20;

Установка параметра **wheel\_distance** возможна в launch-файле `rosserial.launch` в пакете turtlebro:&#x20;

```
sudo nano ~/catkin_ws/src/turtlebro/launch/rosserial.launch

==============

<param name="wheel_distance" type="double" value="0.185"/>
```

Также в лаунч-файле `rosserial.launch` есть возможность задачи параметра **wheel\_param**:

```
<param name="wheel_param" value="$(optenv ROVER_WHEEL_PARAM 22500)"/>
```

но как можно заметить, значение данного параметра берется из переменной окружения операционной системы, а конкретнее задается в файле `.ros_params`:

[[#nastroika-parametrov-v-faile-.ros\_params](params.md#nastroika-parametrov-v-faile-.ros\_params "mention") ](params.md#nastroika-parametrov-v-faile-.ros\_params)

После изменения параметров необходимо выполнить на роботе команду синхронизации данных:

```
sudo sync
```

и путем выключения/включения тумблера питания перезагрузить всего робота.

В случае, когда пользователь не установил никаких параметров, происходит загрузка платы TurtleBoard с параметрами по умолчанию, которые обеспечивают работу идущих в комплекте с роботом моторов и колес. Процедура обновления параметров нужна только в том случае, когда плата TurtleBoard настраивается для работы с нестандартным шасси.

Загруженные параметры не сохраняются в постоянную память контроллера, поэтому при установке неправильных параметров необходимо произвести перезапуск устройства, чтобы загрузить значения по умолчанию.

### Настройка ПИД параметров моторов

Для настройки коэффициентов ПИД регулятора можно использовать сервис ros `/set_pid`

```
rosservice call /set_pid "Ki: 0.0 Kp: 0.0 Kd: 0.0"
```

## Файл turtlebro.launch

Главный файл конфигурации -- подключает другие .launch файлы и глобальные настройки.

```
<arg name="run_rosserial" default="true"/>
<arg name="run_rplidar" default="true"/>
<arg name="run_turtlebro_web" default="true"/>
<arg name="run_camera_ros" default="true"/>
<arg name="run_simple_odom" default="true"/>
```

`run_rosserial` -- запуск rosserial.launch для соединения с Arduino и STM МК\
`run_rplidar` -- запустить получение данные с RPLidar\
`run_turtlebro_web` -- запуск веб-интерфейса робота\
`run_camera_ros` -- включить камеру через пакет `uvc_camera`\
`run_simple_odom` -- подключить паблишер приведенной одометрии. Топик `/odom_pose2d`

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

Файл для запуска издателя с данными полученными из фронтальной камеры. Подробнее о [работе с камерой](video-new.md#paket-uvc\_camera)
