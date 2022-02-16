# Работа с Lidar

Основной датчик робота, лазерный лидар [Slamtec RpLidar A1](https://www.slamtec.com/en/Lidar/A1)

Изменения режимов работы лидара возможна в файле rplidar.launch

Лидары старой модели (с нижней платой под лидаром) могут работать в режимах

```
Standard: max_distance: 12.0 m, Point number: 2.0K
Express: max_distance: 12.0 m, Point number: 4.0K
Boost: max_distance: 12.0 m, Point number: 8.0K
```

Новые лидары могу работать в режимах

```
Standard: max_distance: 12.0 m, Point number: 2.3K
Express: max_distance: 12.0 m, Point number: 4.0K
Boost: max_distance: 12.0 m, Point number: 7.9K
Sensitivity: max_distance: 12.0 m, Point number: 7.9K
Stability: max_distance: 12.0 m, Point number: 5.0K
```

По умолчания лидары включаются в режиме `Boost`, в котором лидар дает 720 точек в топик `/scan`

Выбор режима работы лидара зависит от условий отражений объектов и размера тестового полигона. В некоторых случаях требует изменение режима работы для более подходящих условий.

