# Описание

Пакет должен быть установлен на плате Raspberry, подключенной непосредственно к системной плате `TurtleBoard`

В пакете собраны все необходимые зависимости и launch файлы, необходимые для работы с периферией `TurtleBoard`

* Библиотека `rosserial`, настроенная для работы с пользовательским МК Arduino и системным микроконтроллером STM32
* Работа с Лидарами RPLidar/YDLidar
* Базовая модель робота в формате **URDF,** а также настройки robot\_state\_publisher для работы с /tf топиками

Проверить текущую версию пакета можно командой `rosversion turtlebro`



