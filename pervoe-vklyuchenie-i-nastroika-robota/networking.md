# Подключение робота к Сети

Мы рекомендуем использовать единую сеть для всех устройств входящих в учебный класс. Желательно наличие интернета в этой сети.

Все роботы по умолчанию настроены для работы в WiFi и Ethernet в режиме клиента с получением настроек по DHCP.

## Настройка подключения к Wi-Fi

Видео по [настройке подключения робота TurtleBro к новым Wi-Fi сетям](https://youtu.be/7Y\_IsCfOdNw)

#### Подключение к точке WiFi по умолчанию

По умолчанию при старте Raspberry Pi попытается подключиться к Wi-Fi точке доступа с параметрами:

```
SSID: TurtleBro
Pass: turtlew001
```

или

```
SSID: TurtleBro5G
Pass: turtlew001
```

Поэтому мы рекомендуем использовать именно такие настройки Wi-Fi роутера для работы с роботом TurtleBro

## **Настройка подключения к новой** Wi-Fi **через Ethernet кабель**

Это самый простой способ, но требует наличия Ethernet кабеля, подключенного к роутеру.

* Подключите Ethernet кабель к соответствующему разъему Raspberry Pi на включенном роботе
* Зайдите в веб-интерфейс роутера и определите IP адрес робота
* Зайдите на робота используя протокол SSH ([Подключение по SSH](ssh.md)) используя одну из команд:

```
ssh pi@IP-адрес
```

или

```
pi@turtlebroNN.local
```

* Откройте в текстовом редакторе на роботе файл `wpa_supplicant.conf`

```
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
```

* В конец файла добавьте настройки Wi-Fi вашей сети в виде:

```
network={
    ssid="WIFI_NETWORK_NAME"
    psk="wifipassword"
}
```

При указании пароля от Wi-Fi  сети, обязательно наличие кавычек.

* Сохраните файл (CTRL + S) и перезагрузите Raspberry Pi.

## **Настройка подключения к новой Wi-Fi через SD карту**

На SD карте, содержащей готовый образ системы для запуска на роботе, есть два раздела разного размера. Обычно они называются `system` и `boot`, но иногда система может назвать их по-другому при подключении к компьютеру.&#x20;

Раздел `system` содержит стандартный набор директорий файловой системы Linux и занимает основной объем SD карты (подробнее - [https://ru.wikipedia.org/wiki/FHS](https://ru.wikipedia.org/wiki/FHS)). \


Раздел `boot`небольшой и содержит настройки запуска Raspberry (подробнее - [https://www.raspberrypi.org/documentation/configuration/boot\_folder.md](https://www.raspberrypi.org/documentation/configuration/boot\_folder.md))&#x20;

![](<../.gitbook/assets/Screenshot from 2022-02-08 16-22-24.png>)

Если на этапе загрузки Raspberry найдет файл `wpa_supplicant.conf` в **разделе** `boot` то этот файл будет перемещен в **раздел** `system`, а именно в `/etc/wpa_supplicant/wpa_supplicant.conf` и таким образом станет конфигурационным файлом подключения к Wi-Fi сетям.

Чтобы сконфигурировать Raspberry Pi в этом режиме необходимо:

* Подключите microSD карту Raspberry к вашему компьютеру с помощью карт-ридера
* Откройте **раздел** `boot` и создайте файл `wpa_supplicant.conf`:

```
touch wpa_supplicant.conf
```

* Откройте текстовый редактор и добавьте в этот файл следующее содержание:

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=US

network={
    ssid="WIFI_NETWORK_NAME"
    psk="wifipassword"
}
```

Использование кавычек `"` в файле с настройками обязательно!

* Сохраните файл (CTRL + S)
* Установите microSD карту в Raspberry и дождитесь завершения загрузки.

{% hint style="danger" %}
**Внимание**, файл перезапишет все ваши текущие настройки Wi-Fi в файле `/etc/wpa_supplicant/wpa_supplicant.conf`
{% endhint %}

## Генерация passphrase

Если вы не хотите показывать ваш пароль для пользователей, вы можете сгенерировать специальный ключ `passphrase` и указать его в файле `wpa_supplicant.conf`. Пользователи не смогут "подсмотреть" в файле пароль от Wi-Fi и подключать к вашей сети свои устройства.

Для генерации `passphrase` необходимо запустить программу `wpa_passphrase` далее указать имя Wi-Fi сети и пароль. Далее вывод который покажет программа необходимо записать в файл `wpa_supplicant.conf` в параметр `passphrase = ""`

## Дополнительные команды Wi-Fi

```
sudo iwlist wlan0 scan | grep ESSID #сканирование сети
sudo iwlist wlan0 freq  #доступные каналы Wi-Fi
```

Более подробно о настройке Wi-Fi через командную строку: [https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md)
