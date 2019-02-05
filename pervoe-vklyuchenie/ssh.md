# Подключение по SSH

По умолчанию на RaspberryPi запущен SSH сервис.

Для подключения вам необходимо [Настроить сеть](networking.md)

Каждый робот имеет уникальное имя вида [turtlebroNN.local](http://turtlebronn.local/) где NN это номер.

При "правильной" настройки сети и вашего роутера, вы сможете сразу подключиться к Raspberry по его имени

```text
ssh pi@turtlebro20.local
```

Пароль `brobro`

Если подключения не происходит, то вам необходимо найти IP адрес. Это возможно сделать подключившись к роутеру и найдя имя компьютера в списке подключения.

Удобно привязать IP адресс робота к MAC адресу.

Для работы по имени вида .local для windows необходимо установить программу [https://support.apple.com/kb/Dl999?locale=ru\_RU](https://support.apple.com/kb/Dl999?locale=ru_RU)

Администраторам системы необходимо настроить поддержку Multicast-DNS.

Для доступа по SSH из **Windows** можно использовать программу [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html).
