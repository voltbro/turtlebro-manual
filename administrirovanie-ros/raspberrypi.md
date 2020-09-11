# RaspberryPi

Компьютеры Raspberry, идущие в комплекте с роботами, поставляются с предустановленными ОС  
`Raspbian Buster Lite` \([https://www.raspberrypi.org/downloads/raspbian/](https://www.raspberrypi.org/downloads/raspbian/)\) ,`ROS Melodic`   и всеми необходимыми системными пакетами. 

Обновление образа ОС возможно через скачивание и полную перезапись SD карты. Для работы необходима карта размером **16Gb.** 

Образ можно использовать для следующих моделей Raspberry: 

* Raspberry 3 Model B
* Raspberry 3 Model B+
* Raspberry 4

### Скачать образ

Текущая версия прошивки: **turtlebro\_v0.5.img.gz** Архив доступных образов: [https://yadi.sk/d/U0G80JoXs9eqcA](https://yadi.sk/d/U0G80JoXs9eqcA)

### Загрузка образа ОС на SD-карту

Проще всего загрузить образ на SD карту с помощью программы Etcher  [https://www.balena.io/etcher/](https://www.balena.io/etcher/) Программа обладает поддержкой всех основных операционных систем.

![](../.gitbook/assets/etcher.png)

По умолчанию, имя робота установленно `turtlebro01` Рекомендуется сразу изменить его на имя согласно номера платы `turtlebroNN`. Для этого необходимо отредактировать файлы `/etc/hosts` и `/etc/hostname` и переименовать `turtlebro01->tutlebroNN`. Удобнее всего это сделать на дестктопе с ОС Линукс подключив SD карту. Или уже на включенном роботе, а потом перезагрузить его.

Версию образа можно посмотреть в файле /boot/version на SD Карте





