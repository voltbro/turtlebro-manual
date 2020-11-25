# Работа со светодиодной лентой

На роботе turtlebro светодидная лента выполнена из адресных светодиодов WS2812.   
[https://cdn-shop.adafruit.com/datasheets/WS2812.pdf](https://cdn-shop.adafruit.com/datasheets/WS2812.pdf)

WS2812 - это три светодиода RGB и микросхема-драйвер для управления этими светодиодами, собранные в одном SMD корпусе. Корпус каждого светодиода имеет 4 вывода: два вывода данных \(IN - вход и OUT - выход\) и два вывода питания \(Vcc и GND\).  Выводы данных  предыдущих светодиодов соединены со входами следующих, создавая цепочку светодиодов.

Лента подключена к пину D30 встроенного котроллера Аrduino. Число светодиодов - 24. Для управления светодиодами мы рекомендуем использовать библиотеку FastLed.  
 [https://github.com/FastLED/FastLED](https://github.com/FastLED/FastLED)

Пример управления светодиодной лентой из arduino-скетча:  
[https://randomnerdtutorials.com/guide-for-ws2812b-addressable-rgb-led-strip-with-arduino/](https://randomnerdtutorials.com/guide-for-ws2812b-addressable-rgb-led-strip-with-arduino/)  
[https://github.com/FastLED/FastLED/tree/master/examples](https://github.com/FastLED/FastLED/tree/master/examples)

