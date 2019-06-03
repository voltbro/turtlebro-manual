# Подключение к Сети

Мы рекомендуем использовать единую сеть для всех устройств входящих в учебный класс. Желательно наличие интернета в этой сети.

Все роботы по умолчанию настроены для работы в WiFi и ethernet в режиме клиента с получением настроек по DHCP.

## Настройка подключения к WiFi

#### Подключение к точке WiFi по умолчанию

По умолчанию при старте raspberry попытается подключиться к WiFi точке доступа с параметрами

```text
SSID: TurtleBro
Pass: turtlew001
```

или

```text
SSID: TurtleBro5G
Pass: turtlew001
```



**Настройка через Ethernet кабель**

Самый простой способ, но требует наличия Ethernet кабеля, подключенного к роутеру.

Подключите работающий Raspberry к Ethernet кабелю.

Определите IP адрес устройства через web-интерфейс роутера.

Зайдите на ssh на устройство `ssh pi@IP`или `pi@turtlebroNNN.local`[ Подключение по SSH](ssh.md)

Откройте в редакторе файл

```text
sudo mcedit /etc/wpa_supplicant/wpa_supplicant.conf
```

В конец файла добавьте настройки WiFi сети

```text
network={
    ssid="WIFI_NETWORK_NAME"
    psk="wifipassword"
}
```

При указании непосредственно паролья от wi-fi сети, обязательно наличие кавычек.

Перезагрузите raspberry.

**Настройка через SD карту**

Если при загрузке Raspberry найдет файл в разделе `/boot/wpa_supplicant.conf` то этот файл будет перемещен в `/etc/wpa_supplicant/wpa_supplicant.conf` 

Подключите SD карту к персональному компьютеру

Создайте файл `/boot/wpa_supplicant.conf`с содержанием

```text
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=US

network={
    ssid="WIFI_NETWORK_NAME"
    psk="wifipassword"
}
```

Установите SD карту в Raspberry и дождитесь завершения загрузки.

**Внимание**, файл перезапишет все ваши текущие настройки WiFi в директории `/etc/wpa_supplicant`

## Генерация passphrase

Если вы не хотите показывать ваш пароль для пользователей, вы можете сгенерировать специальный ключ `passphrase` и указать его в файле `wpa_supplicant.conf`. Пользователи не смогут "подсмотреть" пароль от wifi и подлкючать к сети свои устройства.

Для генерации `passphrase` необходимо запустить программу `wpa_passphrase` далее указать имя WiFi сети и пароль. Далее вывод который покажет прогамма необходимо записать в файл `wpa_supplicant.conf` 

## Дополнительные команды WiFi

```text
sudo iwlist wlan0 scan | grep ESSID #сканирование сети
sudo iwlist wlan0 freq  #доступные каналы wifi
```

Более подробно о настройке WiFi через командную строчку: [https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md)

