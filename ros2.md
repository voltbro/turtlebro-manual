# Обновление ROS2

{% hint style="info" %}
Данный документ носит временный статус, создан для тестирование прошивки `ROS2` и платформы `turtlebro`. При разработке новой прошивки `ROS2` ставилась задача создать максимально похожую прошивку с `ROS1`.
{% endhint %}

### Обновление образа microSD-карты Raspberry Pi

Образ SD-карты с `ROS2` создан на основе дистрибутива `ROS2 jazzy`. В качестве базовой ОС используется `Ubuntu Server 24.04 LTS`

Образ ROS2 проверен на микрокомпьютерах:
  * `Raspberry Pi 4 - 2 Гб`
  * `Raspberry Pi 5 - 2 Гб`

При подключении `Raspberry Pi 5` к роботу `TurtleBro1`, `Raspberry Pi 5` может не хватать питания. Поэтому мы не рекомендуем обновлять микрокомпьютер, а использовать `Raspberry Pi 5` только на плате `TurtleBro2`   


Образ SD-карты, можно скачать по ссылке: [latest](https://disk.yandex.ru/d/fwXInv5GtNlwPg) 

Инструкция по [обновлению образа](administrirovanie-ros/raspberrypi.md)

[Просмотреть](https://youtu.be/OGzLALB51Pc?si=Lx_xFs8W3NmIZM82) видеоинструкция по обновлению образа. 

В образе уже установлены дополнительные пользовательские пакеты (аналоги старых пакетов ROS1):

* turtlebro [https://github.com/voltbro/turtlebro2](https://github.com/voltbro/turtlebro2)
* turtlebro_navigation [https://github.com/voltbro/turtlebro2_navigation/](https://github.com/voltbro/turtlebro2_navigation/)
* turtlebro_web [https://github.com/voltbro/turtlebro2_web](https://github.com/voltbro/turtlebro2_web)


### Обновление прошивки системной платы робота

Для поддержки управления роботом через Raspberry необходимо обновить ПО платы Turtleboard. Новая прошивка создана на базе фреймворка `microROS` ([https://micro.ros.org](https://micro.ros.org)), являющегося "идейным" продолжением библиотеки `rosserial`. Все системные топики управления платформой работают на микроконтроллере.

Прошивку для робота TurtleBro1 и `microROS` можно скачать по ссылке: [latest](https://disk.yandex.ru/d/-XHvTQyW293yzw) 

{% hint style="danger" %}
**Внимание** Файлы прошивки для "желтой" и "синей" плат отличаются. Если вы обновляете "синюю" плату (робот TurtleBro1), убедитесь, что имя файла прошивки начинается с "TB1".
{% endhint %}
 

Инструкция по [обновлению МК](platforma-turtleboard/obnovlenie-mikroprogrammy/) (необходим USB-UART переходник или программатор ST-LINK V2)

### Запуск и подключение к роботу

После обновления SD-карты и прошивки - робот готов для работе.

При включении, робот попытается подключиться к Wi-Fi сети согласно [настройкам сети](pervoe-vklyuchenie-i-nastroika-robota/networking.md).

Имя робота после установки нового образа: `turtlebro01`

SSH подключение [согласно страницы](pervoe-vklyuchenie-i-nastroika-robota/ssh.md)  (пользователь `pi`, пароль `brobro`)

Все необходимые сервисы и пакеты запускаются при включения робота.

#### Настройка работа после загрузки

Образ был оптимизирован для платы TurtleBro2 а не TurtleBro1. Для корректного функционирования робота с новым образом, необходимо внести изменения в конфигурационный файл.

Для этого в файле `/home/pi/.ros_params` следует модифицировать переменную окружения `BOARD_VERSION`, установив её значение на `5`.


### Веб-интерфейс

Для быстрой проверки работоспособности доступен старый [Веб-интерфейс](pervoe-vklyuchenie-i-nastroika-robota/web-interfeis.md).

### Подключение к ROS на роботе

После подключения к роботу по SSH, работают все `ros2` команды.

Робот настроен для работы в режиме [Discovery-Server](https://docs.ros.org/en/iron/Tutorials/Advanced/Discovery-Server/Discovery-Server.html). Работа в режиме  [Simple Discovery](https://fast-dds.docs.eprosima.com/en/v2.1.0/fastdds/discovery/simple.html) в условиях работы по Wi-Fi и множества аналогичных устройств в сети показала себя более запутанной и не надежной.

Робот запускает `fastdds server` с настройками `-l 127.0.0.1 -p 11811`

Управление `fastdds` через `systemd`

```
sudo systemctl start fastdds
sudo systemctl stop fastdds
```

Для настройки компьютера для подключению к `ROS2` необходимо провести настройки в режим `super_client`. Для этого необходимо на компьютере:

1. Скачать .xml файл:

```bash
wget https://raw.githubusercontent.com/voltbro/turtlebro2/master/extra/fastdds_supeclient.xml
```

2. Поменять настройки адреса подключения в скаченном файле:

```xml
<udpv4>
	<address>127.0.0.1</address>
	<port>11811</port>
</udpv4>
```

Вместо 127.0.0.1 необходимо указать IP-адрес робота.

3. Установить переменную окружения, указав месторасположения файла:

```bash
export FASTRTPS_DEFAULT_PROFILES_FILE=./fastdds_supeclient.xml
```

После завершения настроек, при выполнении команды `ros2 topic list` вы увидите топики вашего робота.

### Доступные сервисы

Основной сервис необходимый для работы робота `turtlebro`

```bash
sudo systemctl start turtlebro
sudo systemctl stop turtlebro
```

При выполнении запуска сервиса запускается `launch` файл пакета `turtlebro` `turtlebro.xml`

Сервис-агент для работы `microros`

```bash
sudo systemctl start microros
sudo systemctl stop microros
```
