# Подключение к Сети

Мы рекомендуем использовать единую сеть для всех устройств входящих в учебный класс. Желательно наличие интернета в этой сети.

Все роботы по умолчанию настроены для работы в WiFi и ethernet в режиме клиента, с получением настроек по DHCP.

### Настройка подключения к WiFi

**Настройка через Ethernet кабель**

Самый простой способ, но требует наличие Ethernet кабеля и подключенного к роутеру.

Подключите работающий Raspberry к Ethernet кабелю.

Определите IP адрес устройства

Зайдите на ssh на устройство `ssh pi@IP`или `pi@turtlebroNNN.local`пароль `brobro`

Откройте в редакторе файл

```text
sudo mcedit /etc/wpa_supplicant/wpa_supplicant.conf
```

В конец файла добавить настройки вида

```text
network={
    ssid="WIFI_NETWORK_NAME"
    psk="wifipassword"
}
```

Обязательно наличие кавычек, если используется ключ wifi а не passphrase

Перезагрузить компьютер

**Настройка через SD карту**

При загрузке, если ОС найдет файл в разделе `/boot/wpa_supplicant.conf`то этот файл будет скопирован в `/etc/wpa_supplicant/wpa_supplicant.conf`

Подключить SD карту к компьютеру

Создать файл `/boot/wpa_supplicant.conf`с содержанием

```text
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=US

network={
    ssid="WIFI_NETWORK_NAME"
    psk="wifipassword"
}
```

Установить SD карту в Raspberry и подождать загрузки.

**Внимание**, файл перезапишет все ваши текущие настройки сети в директории `/etc/`

