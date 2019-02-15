# Параметры и настройка через launch

## Параметры \(rosparams\)

Устрановка параметров возможна через команду `rosparam set` или через `.launch` файлы

* **stm\_serial\_node/pid** float\[3\] for Kp, Ki, Kd  -- ПИД регулятор для управления колесами робота
* **stm\_serial\_node/wheel\_distance** double; meters -- расстояние между колесами
* **stm\_serial\_node/wheel\_param** uint32\_t; number of ticks per meter -- расчетный коэффициент
* **stm\_serial\_node/motor\_inversion** uint32\_t; 0/1 -- направления вращения колеса отнасительно направления робота
* формула для расчета **wheel\_param**:

  **wheel\_param** = ticks\*red\_ratio/circle  
  где

  * **ticks** - количество тиков энкодера на один оборот мотора, штук
  * **red\_ratio** - коэффициент передачи редуктора, безразмерный
  * **circle** - длина окружности колеса, метров

После изменения параметров необходимо перезапустить контроллер TurtleBoard нажав на кнопку `restart` с правой стороны робота. Прошивка TurtleBoard читает параметры только при загрузке.

В случае, когда пользователь не установил никаких параметров, происходит загрузка TurtleBoard с параметрами по умолчанию, которые обеспечивают работу идущих в комплекте с роботом моторов и колес. Процедура обновления параметров нужна только в том случае, когда плата TurtleBoard настраивается для работы с нестандартным шасси.   
  
Загруженные параметры не сохраняются в постоянную память контроллера, поэтому при установке неправильных параметров необходимо произвести переконфигурацию устройства, чтобы загрузить значения по умолчанию.

### Настройка ПИД параметров моторов

Для настройки коэффициентов ПИД ругулятора можно использовать сервис ros `/set_pid`

```text
rosservice call /set_pid "Ki: 0.0 Kp: 0.0 Kd: 0.0"
```

## Файл turtlebro.launch

Главный файл конфигурации -- подключает другие .launch файлы и глобальные настройки.

```text
<arg name="run_rosserial" default="true"/>
<arg name="run_rplidar" default="true"/>
<arg name="run_uvc_camera" default="false"/>
<arg name="run_slam_gmapping" default="false"/>
```

`run_rosserial` -- запуск rosserial.launch для соединения с Arduino и STM МК  
`run_rplidar` -- запустить получение данные с RPLidar

`run_uvc_camera` -- включить камеру через пакет uvc\_camera

`run_slam_gmapping` -- включить работу робота в режиме SLAM

## Файл rosserial.launch

Файл запуска нод `stm_serial_node` и `arduino_serial_node` необходимых для работы с Arduino и STM

#### МК STM TurtleBoard

Подключается через устройство, скорость **460800** бод/с`/dev/serial/by-id/usb-Silicon_Labs_CP2102_USB_to_UART_Bridge_Controller_0012-if00-port0`

#### Arduino

Подключается через устройство, скорость **115200** бод/с`/dev/serial/by-id/usb-Silicon_Labs_CP2102_USB_to_UART_Bridge_Controller_0010-if00-port0`

## Файл rplidar.launch

Файл для запуска ноды обработки данных с лидара. Для работы необходим пакет `rplidar` от производителя лидара. Параметры файла настроены для работы с лидаром RPLidar A1 на скорости 115200 бод/с через устройство `/dev/serial/by-id/usb-Silicon_Labs_CP2102_USB_to_UART_Bridge_Controller_0011-if00-port0`

Данные с лидара вычисляются относительно фрейма `base_scan`

## Файл uvc\_camera.launch

Файл для запуска издателя с данными полученными из фронтальной камеры. Подробнее о [работе с камерой](video.md#paket-uvc_camera)

