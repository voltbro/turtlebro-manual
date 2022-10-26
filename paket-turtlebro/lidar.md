# Работа с лидаром

Основной датчик робота - лазерный лидар [Slamtec RpLidar A1](https://www.slamtec.com/en/Lidar/A1)

Изменения режимов работы лидара возможна в лаунч-файле `rplidar.launch`:

```
/home/pi/catkin_ws/src/turtlebro/launch/rplidar.launch
```

Лидары старой модели (с нижней платой под лидаром) могут работать в слудеющийх режимах:

```
Standard: max_distance: 12.0 m, Point number: 2.0K
Express: max_distance: 12.0 m, Point number: 4.0K
Boost: max_distance: 12.0 m, Point number: 8.0K
```

Новые лидары могу работать в следующих режимах:

```
Standard: max_distance: 12.0 m, Point number: 2.3K
Express: max_distance: 12.0 m, Point number: 4.0K
Boost: max_distance: 12.0 m, Point number: 7.9K
Sensitivity: max_distance: 12.0 m, Point number: 7.9K
Stability: max_distance: 12.0 m, Point number: 5.0K
```

По умолчанию, лидар включается в режиме `Boost`, в котором он выводит 720 точек в топик `/scan`

Выбор режима работы лидара зависит от условий отражений объектов и размера тестового полигона. В некоторых случаях требует изменение режима работы для более подходящих условий.

