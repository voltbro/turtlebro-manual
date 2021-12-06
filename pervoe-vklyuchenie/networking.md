# Подключение робота к Сети

Мы рекомендуем использовать единую сеть для всех устройств входящих в учебный класс. Желательно наличие интернета в этой сети.

Все роботы по умолчанию настроены для работы в WiFi и ethernet в режиме клиента с получением настроек по DHCP.

## Настройка подключения к WiFi

#### Подключение к точке WiFi по умолчанию

По умолчанию при старте raspberry попытается подключиться к WiFi точке доступа с параметрами

```
SSID: TurtleBro
Pass: turtlew001
```

или

```
SSID: TurtleBro5G
Pass: turtlew001
```

## **Настройка подключения к новой** WiFi **через Ethernet кабель**

Это самый простой способ, но требует наличия Ethernet кабеля, подключенного к роутеру.

Подключите работающий Raspberry к Ethernet кабелю.

Определите IP адрес устройства через web-интерфейс роутера.

Зайдите на ssh на устройство `ssh pi@IP`или `pi@turtlebroNNN.local`[ Подключение по SSH](ssh.md)

Откройте в редакторе `mcedit` файл `wpa_supplicant.conf`

```
sudo mcedit /etc/wpa_supplicant/wpa_supplicant.conf
```

В конец файла добавьте настройки WiFi вашей сети в виде:

```
network={
    ssid="WIFI_NETWORK_NAME"
    psk="wifipassword"
}
```

При указании пароля от wi-fi сети, обязательно наличие кавычек.

Сохраните файл и перезагрузите raspberry.

## **Настройка подключения к новой WiFi через SD карту**

На SD карте, содержащей готовый образ системы для запуска на роботе, есть два раздела разного размера. Обычно они называются `system` и `boot`, но иногда система может назвать их по-другому при подключении к компьютеру.&#x20;

Раздел `system` содержит стандартный набор директорий файловой системы Linux и занимает основной объем SD карты (подробнее - [https://ru.wikipedia.org/wiki/FHS](https://ru.wikipedia.org/wiki/FHS)). \


Раздел `boot`небольшой и содержит настройки запуска Raspberry (подробнее - [https://www.raspberrypi.org/documentation/configuration/boot\_folder.md](https://www.raspberrypi.org/documentation/configuration/boot\_folder.md)) \
Если на этапе загрузки Raspberry найдет файл `wpa_supplicant.conf` в разделе `boot` то этот файл будет перемещен в `/etc/wpa_supplicant/wpa_supplicant.conf` и таким образом станет конфиграционным файлом подключения к Wi-Fi сетям.

Чтобы сконфигурировать Raspberry в этом режиме подключите SD карту Raspberry к вашему компьютеру.

Создайте на SD карте файл `/boot/wpa_supplicant.conf`с содержанием:

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=US

network={
    ssid="WIFI_NETWORK_NAME"
    psk="wifipassword"
}
```

Использование кавычек `"` в файле с настройками обязательно.

Сохраните файл.

Установите SD карту в Raspberry и дождитесь завершения загрузки.&#x20;

{% hint style="danger" %}
**Внимание**, файл перезапишет все ваши текущие настройки WiFi в директории `/etc/wpa_supplicant`
{% endhint %}

## Генерация passphrase

Если вы не хотите показывать ваш пароль для пользователей, вы можете сгенерировать специальный ключ `passphrase` и указать его в файле `wpa_supplicant.conf`. Пользователи не смогут "подсмотреть" в файле пароль от wifi и подключать к вашей сети свои устройства.

Для генерации `passphrase` необходимо запустить программу `wpa_passphrase` далее указать имя WiFi сети и пароль. Далее вывод который покажет программа необходимо записать в файл `wpa_supplicant.conf` в параметр `passphrase = ""`

## Дополнительные команды WiFi

```
sudo iwlist wlan0 scan | grep ESSID #сканирование сети
sudo iwlist wlan0 freq  #доступные каналы wifi
```

Более подробно о настройке WiFi через командную строку: [https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md)
