# Доступные топики

На плате запущены **Издатели** и **Подписчики**, взаимодействующие с ROS через библиотеку `rosserial`. STM32 и Arduino не имеют собственного интерфейса USB, поэтому, для того, чтобы они могли общаться с Raspberry, на плате TurtleBoard установлены UART-USB преобразователи и USB-hub, которые позволяют обеспечить коммуникацию с микрокомпьютером с помощью всего лишь одного провода USB.

Контроллер платы turtlebro STM32 публикует в ROS Raspberry следующие топики, поддерживающие стандартные для ROS типы сообщений:

## Топик /bat

Содержит данные о состоянии подключенной батарейки к плате. Тип сообщения `sensor_msgs/BatteryState`

```text
std_msgs/Header header
  uint32 seq
  time stamp
  string frame_id
float32 voltage
float32 current
float32 charge
float32 capacity
float32 design_capacity
float32 percentage
uint8 power_supply_status
uint8 power_supply_health
uint8 power_supply_technology
bool present
float32[] cell_voltage
string location
string serial_number
```

## Топик /cmd\_vel

Топик управления перемещения роботом. Тип сообщения `geometry_msgs/Twist` При публикации данных в топик робот начинает движение. Робот выполняет последнюю полученную команду до тех пор, пока не получит новые данные. Поэтому, например, для остановки робота необходимо передать "нулевые" значения скорости.

```text
geometry_msgs/Vector3 linear
  float64 x
  float64 y
  float64 z
geometry_msgs/Vector3 angular
  float64 x
  float64 y
  float64 z
```

## Топик /imu

Данные инерционного датчика  \(Inertial measurement unit\), включающего в себя гироскоп, акселерометр и компас. Тип сообщения `sensor_msgs/Imu`

```text
std_msgs/Header header
  uint32 seq
  time stamp
  string frame_id
geometry_msgs/Quaternion orientation
  float64 x
  float64 y
  float64 z
  float64 w
float64[9] orientation_covariance
geometry_msgs/Vector3 angular_velocity
  float64 x
  float64 y
  float64 z
float64[9] angular_velocity_covariance
geometry_msgs/Vector3 linear_acceleration
  float64 x
  float64 y
  float64 z
float64[9] linear_acceleration_covariance
```

Данные о ковариации не заполняются. Данные публикуются с частотой 20 герц. В связи с тем, что rviz по умолчанию некорректно отображает данные IMU, необходимо поставить соответствующий плагин для корректного отображения данных с IMU. Установка плагина осуществляется командой:

`sudo apt install ros-melodic-rviz-imu-plugin`

При добавлении визуализации данных в rviz необходимо выбрать тип сообщения rviz\_imu\_plugin-&gt;Imu

## Топик /odom

Данные одометрии \(положения робота, рассчитанного на основании вращения колес\). В текущей версии повороты робота рассчитываются по данным IMU датчика, а перемещение на основе данных энкодеров на моторах. Тип сообщения `nav_msgs/Odometry`

```text
std_msgs/Header header
  uint32 seq
  time stamp
  string frame_id
string child_frame_id
geometry_msgs/PoseWithCovariance pose
  geometry_msgs/Pose pose
    geometry_msgs/Point position
      float64 x
      float64 y
      float64 z
    geometry_msgs/Quaternion orientation
      float64 x
      float64 y
      float64 z
      float64 w
  float64[36] covariance
geometry_msgs/TwistWithCovariance twist
  geometry_msgs/Twist twist
    geometry_msgs/Vector3 linear
      float64 x
      float64 y
      float64 z
    geometry_msgs/Vector3 angular
      float64 x
      float64 y
      float64 z
  float64[36] covariance
```

## Топик /odom\_pose2d

Упрощенные данные одометрии в 2d пространстве

```text
float64 x
float64 y
float64 theta
```



## Топик /scan

Данные, полученные с лидара \(облако точек\). Данные идут через Serial интерфейс лидара, USB-UART преобразователь и через USB hub. Тип сообщения `sensor_msgs/LaserScan` .

```text
std_msgs/Header header
  uint32 seq
  time stamp
  string frame_id
float32 angle_min
float32 angle_max
float32 angle_increment
float32 time_increment
float32 scan_time
float32 range_min
float32 range_max
float32[] ranges
float32[] intensities
```

## Топик /raw\_odom

Данные полученные энкодеров колес. Время, счетчик "тиков" и угол датчика IMU

```text
turtlebro/RawOdom

uint64 timestamp
int32 left_ticks
int32 right_ticks
float32 theta
```

