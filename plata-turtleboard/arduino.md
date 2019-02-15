# Работа с Arduino

На плате размешен МК AtMega 2560, по схеме идентичной плате Arduino Mega \(включая пины расширения для дополнительных плат\).

Для работы с МК необходимо скачать и запустить Arduino IDE с сайта arduino.cc. В настройках IDE выбрать плату Arduino Mega 2560.

## Взаимодействие с ROS

Arduino Mega подключена к Raspberry через порт Serial1. Со стороны ROS запущен сервис `rosserial` который организует взаимодействие МК и ROS.

Для подключения к ROS, со стороны Arduino необходимо инициализировать ros\_lib указав параметры работы через Serial1 и скорость 115200 как показано ниже.

```c
#include <ros.h>

class NewHardware : public ArduinoHardware
{
  public:
  NewHardware():ArduinoHardware(&Serial, 115200){};
};

ros::NodeHandle_<NewHardware>  nh;
```

Остальные примеры можно взять из официальной документации [http://wiki.ros.org/rosserial\_arduino/Tutorials](http://wiki.ros.org/rosserial_arduino/Tutorials)

## Библиотека Arduino ros\_lib

Для работы с Arduino через ROS необходимо установить библиотеку ros\_lib.

Скачать библиотеку: [https://yadi.sk/d/BcI1126boKkf-A](https://yadi.sk/d/BcI1126boKkf-A)

Инструкция по установке библиотек для Arduino IDE [https://www.arduino.cc/en/guide/libraries](https://www.arduino.cc/en/guide/libraries#)

Если вы используете кастомные сообщения, то вам необходимо "собрать" библиотеку `ros_lib` самостоятельно командой

```text
rosrun rosserial_arduino make_libraries.py .
```

И далее переписать в директорию библиотек Arduino.

## Дополнительные возможности Arduino

В передней части системной платы робота расположены две кнопки, подключенные к контактам `D24` и `D23` МК Arduino. И два переключателя, подключенных к `D22` и `D25`. При нажатии кнопки или переключателя на пине Arduino будет сингал `HIGH` Для чтения значения необходимо использовать Arduino функцию `digitalRead(pin)`

С левой стороны системной платы, находятся пины `D44 D45 D46`, которые возможно использовать для подключения серво машинок. Питание для сервоприводов 5v, с отдельной цепи питания.

Перед лидаром расположены 4 светодиода, подключенные к пинам D26, D27,D28,D29

## Работа со светодиодной лентой

Под платой расположено 24 RGB светодиода. Для работы со светодиодной лентой, рекомендуем использовать библиотеку fastled [https://github.com/FastLED/FastLED](https://github.com/FastLED/FastLED) Управляющий пин для ленты D30. Используеться контроллер Ws2812

