# Работа с Arduino

На плате размещен микроконтроллер ATMega 2560 с обвязкой, идентичной плате Arduino Mega. Контроллер поставляется с прошитым бутлоадером Arduino. Таким образом, микроконтроллер полностью готов к запуску скетчей Arduino IDE и работе со стандартными шилдами для Ардуино.

Для работы с МК необходимо [скачать](https://www.arduino.cc/en/Main/Software) и запустить Arduino IDE с сайта[ arduino.cc](https://www.arduino.cc/en/Main/Software). В настройках IDE выбрать плату Arduino Mega 2560.

## Библиотека Arduino ros\_lib

Для работы с Arduino через ROS необходимо установить библиотеку ros\_lib: `<ros.h>`.

Скачать библиотеку необходимо с вашего робота. Для этого можно воспользоваться утилитой копирования файлов между компьютерами SCP:

```
scp pi@turtlebroXX.local:/home/pi/ros_lib_noetic.tar.gz /home/<USERNAME>/
```

Вам необходимо распаковать архив, зайти в распакованную директорию и внутри нее должна находиться папка с названием `ros_lib`. Она должна содержать большое количество файлов, необходимых для компиляции программ содержащих вызовы `<ros.h>`.\
После этого необходимо скопировать папку `ros_lib` в папку `libraries`, которая находится внутри той директории, куда Arduino IDE сохраняет новые скетчи. Там же должны находиться и те библиотеки, которые вы загружали стандартным способом - через менеджер библиотек Arduino IDE  [https://www.arduino.cc/en/guide/libraries](https://www.arduino.cc/en/guide/libraries)

Но если вы используете собственные сообщения или у вас появляются ошибки при сборке скетчей, вам необходимо "пересобрать" библиотеку `ros_lib` самостоятельно с помощью команды (выполнив ее на роботе)

```
rosrun rosserial_arduino make_libraries.py .
```

Вызванная утилита `rosserial_arduino` соберет новую библиотеку на основе настроек ROS вашего робота и положит ее в корневую директорию пользователя `/home/pi/`. Дальше вам надо переписать ros\_lib с робота на ваш компьютер и поместить его в директорию библиотек Arduino в соответствии с инструкцией по установке библиотек для Arduino IDE.

## Взаимодействие с ROS

Arduino Mega подключена к Raspberry через порт Serial1. Со стороны ROS запущен сервис `rosserial` который организует взаимодействие МК и ROS.

Для подключения к ROS со стороны Arduino необходимо инициализировать библиотеку ros\_lib `<ros.h>` отвечающую за коммуникацию между Arduino и ROS, указав параметры Serial1 и скорость 115200, как показано ниже.

```c
#include <ros.h>

class NewHardware : public ArduinoHardware
{
  public:
  NewHardware():ArduinoHardware(&Serial1, 115200){};
};

ros::NodeHandle_<NewHardware>  nh;
```

Примеры можно посмотреть в официальной документации rosserial [http://wiki.ros.org/rosserial\_arduino/Tutorials](http://wiki.ros.org/rosserial\_arduino/Tutorials)

## Дополнительные возможности Arduino

В передней части системной платы робота расположены две кнопки, подключенные к ножкам `D24` и `D23` МК Arduino и два переключателя, подключенные соответственно к `D22` и `D25`. При нажатии кнопки или переключателя на пине Arduino будет сигнал `HIGH` Для чтения значения необходимо использовать Arduino функцию `digitalRead(pin)`

С левой стороны системной платы находятся пины `D44 D45 D46`, которые можно использовать для подключения сервомашинок.&#x20;

{% hint style="danger" %}
**ВНИМАНИЕ: на сервомашинки подается напряжение 5В от отдельного источника питания! Ни в коем случае нельзя соединять пины питания сервомашинок с пинами питания, выведенными на колодку Ардуино.**
{% endhint %}

Перед лидаром расположены 4 светодиода, подключенные к пинам `D26, D27, D28, D29.`

## Общение с Arduino через Serial

{% hint style="info" %}
**Внимание!** Для прямого общения с Arduino необходимо соединить usb-порт Raspberry и microusb порт контроллера кабелем.
{% endhint %}

\
В некоторых случаях необходимо получать данные от встроенного микроконтроллера без применения ROS. Для этого можно применять консольные утилиты типа minicom ([https://linux.die.net/man/1/minicom](https://linux.die.net/man/1/minicom)).\
Для чтения данных от Ардуино можно применить следующую команду:

```
minicom -b 9600 -o -D /dev/serial/by-id/usb-Silicon_Labs_CP2102_USB_to_UART_Bridge_Controller_0001-if00-port0
```

для завершения чтения следует нажать `Ctrl-A` и затем  `X`. Скорость и параметры подключения следует указать такими же, как при инициализации Serial в скетче Arduino.\


Еще одна возможность для получения данных от Arduino это применении библиотеки Serial языка Python[ https://pyserial.readthedocs.io/en/latest/shortintro.html](https://pyserial.readthedocs.io/en/latest/shortintro.html)

Пример простого скрипта для чтения данных от Arduino:

```c
#!/usr/bin/env python3

import serial
if __name__ == '__main__':
    ser = serial.Serial('/dev/serial/by-id/usb-Silicon_Labs_CP2102_USB_to_UART_Bridge_Controller_0001-if00-port0', 9600, timeout=1)
    ser.flush()
    while True:
        if ser.in_waiting > 0:
            line = ser.readline().decode('utf-8').rstrip()
            print(line)
```

## Работа со светодиодной лентой

Под платой расположено 24 RGB светодиода модели `WS2812`. \
[https://cdn-shop.adafruit.com/datasheets/WS2812.pdf](https://cdn-shop.adafruit.com/datasheets/WS2812.pdf)

`WS2812` - это три RGB-светодиода  и микросхема-драйвер для управления этими светодиодами, собранные в одном SMD корпусе. Корпус каждого светодиода имеет 4 вывода: два вывода данных и два вывода питания.  Выводы данных  предыдущих светодиодов соединены со входами следующих, создавая цепочку светодиодов, управляемых через один пин микроконтроллера.

Лента подключена к пину `D30` встроенного контроллера Аrduino. Число светодиодов - 24. Для управления светодиодами мы рекомендуем использовать библиотеку FastLed.\
&#x20;[https://github.com/FastLED/FastLED](https://github.com/FastLED/FastLED)

Пример управления светодиодной лентой из Аrduino-скетча:\
[https://randomnerdtutorials.com/guide-for-ws2812b-addressable-rgb-led-strip-with-arduino/](https://randomnerdtutorials.com/guide-for-ws2812b-addressable-rgb-led-strip-with-arduino/)\
[https://github.com/FastLED/FastLED/tree/master/examples](https://github.com/FastLED/FastLED/tree/master/examples)

Для тестирования работоспособности светодиодной ленты, можно воспользоваться тестовым скетчем

{% embed url="https://github.com/voltbro/ws-sro/tree/main/Turtlebro-tester" %}

## **Удаленная загрузка скетча Arduino**

Если есть необходимость удаленно (без доступа к роботу) обновить прошивку Arduino, то это возможно сделать имея только удаленный доступ.

**Подготовить скетч к загрузке**

1. В Arduino IDE выбрать МК Arduino/Genuino Mega or Mega 2560
2. Скомпилировать программу (кнопка Проверить)
3. В меню выбрать Скетч→Экспорт Бинарного файла
4. В директории где находиться файл скетча, будут созданы два файла с бинарными данными вида (sketch\_mar24a.ino.mega.hex sketch\_mar24a.ino.with\_bootloader.mega.hex)
5. Мы должны использовать файл `sketch_mar24a.ino.mega.hex`

**Загрузить бинарный файл на Arduino:**

1. Скопировать файлы `.hex` на робота, например командой linux scp
2. На роботе выполнить команду "прошивки" платы Arduino&#x20;

```c
avrdude -v -v -p atmega2560 -c wiring -P /dev/serial/by-id/usb-Silicon_Labs_CP2102_USB_to_UART_Bridge_Controller_0001-if00-port0 -b 115200 -D -U flash:w:sketch_mar24a.ino.mega.hex:i
```

Где `sketch_mar24a.ino.mega.hex` имя файла с прошивкой.\
\
Для возможности удаленной прошивки платы, необходимо чтобы Arduino разъем на плате Turtlebro был подключен к RaspberryPi через Micro-USB.
