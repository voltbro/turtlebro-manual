# Доступные топики

На плате запущены **Издатели** и **Подписчики** взаимодействующие с ROS через библиотеку `rosserial` В raspberry данные приходят через USB-UART преобразователь и USB Switch интегрированными в плату. Поэтому для работы достаточно одного подключения платы к USB разъему raspberry.

### Топик /bat

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

### Топик /cmd\_vel

Топик управления перемещения роботом. Тип сообщения `geometry_msgs/Twist` При публикации данных в топик робот начинает движение. Робот выполняет последнюю полученную команды то тех пор пока не получит новые данные. Поэтому например для остановки робота, необходимо передать "нулевые" значения скорости.

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

### Топик /imu

Данные \(Inertial measurement unit\) инерционного датчика \(Гироскоп, Акселерометр, Компас\). Установленного в центре платы робота. Тип сообщения `sensor_msgs/Imu`

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

 Данные о ковариации не заполняються.

### Топик /odom

Данные одометрии \(положение робота рассчитаное на основании вращения колес\). В текущей версии по умолчанию повороты робота рассчитываются по данным IMU датчика, а перемещение на основе данных энкодеров на моторах. Тип сообщения `nav_msgs/Odometry`

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

### Топик /scan

Данные полученные с Лидара \(облако точек\). Данные идут через Serial интерфейс лидара, USB Uart преобразователь, через USB switch. Тип сообщения `sensor_msgs/LaserScan` .

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
