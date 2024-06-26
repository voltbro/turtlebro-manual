# Работа с камерой (v0.10)

{% hint style="info" %}
Актуально для версии 0.10 и более новых
{% endhint %}

При включении робота автоматически включается камера `video0` и начинают публиковаться данные в топики камеры:

```
/front_camera/camera_info # Информация о камере
/front_camera/image_raw  # Данные в формате sensor_msgs/Image (.jpeg)
/front_camera/image_raw/compressed sensor_msgs/CompressedImage 
```

Данные c камеры принимаются в формате `mjpeg` и без дополнительной обработки публикуются в топик `/front_camera/image_raw/compressed`. Для заполнения топика `/front_camera/image_raw` происходит перекодирование jpeg->raw с использованием пакета `image_transport`.

Если вы не используете данные `/front_camera/image_raw` рекомендуется отключить лишнее преобразование (аргумент `republish_raw`).

{% hint style="info" %}
Изменение настроек параметров камеры возможно в файле: `/home/pi/catkin_ws/src/turtlebro/launch/camera_ros.launch`
{% endhint %}

## Визуализация данных&#x20;

#### Веб-интерфейс

Для просмотра видео через топики ROS, можно использовать [веб-интерфейс](../pervoe-vklyuchenie-i-nastroika-robota/web-interfeis.md) робота. При загрузке веб-интерфейса происходит поиск всех доступных видеоданных (топики с `sensor_msgs/CompressedImage`). Если такие топики найдены, то все они будет добавлены в веб-интерфейс для просмотра. При добавлении новых топиков, веб-интерфейс робота необходимо перезагрузить.

#### Rviz

Вы можете использовать rviz для отображения видеопотока камеры. При удаленной работе рекомендуем выбирать для просмотра сжатые видеоданные (топик `/front_camera/image_raw/compressed`)

#### rqt\_image\_view или image\_view

Для просмотра видео можно использовать специальные Linux программы. Например [http://wiki.ros.org/rqt\_image\_view](http://wiki.ros.org/rqt\_image\_view) или [http://wiki.ros.org/image\_view](http://wiki.ros.org/image\_view)

## Пакет uvc\_camera

Пакет публикует сжатые данные `sensor_msgs/CompressedImage` в топик `front_camera/image_raw/compressed`

Официальная документация пакета [http://wiki.ros.org/uvc\_camera](http://wiki.ros.org/uvc\_camera)

## Работа с камерой в OpenCV

Для работы с OpenCV данные из камеры ROS необходимо конвертировать из топиков в объект OpenCV

#### Использование RAW топика

Для создания объекта с OpenCV используется библиотеке CvBridge и метод `imgmsg_to_cv2`, где `image_msg` это сообщение из топика.

```
from cv_bridge import CvBridge
from sensor_msgs.msg import CompressedImage, Image

cvBridge = CvBridge()
cv_image = cvBridge.imgmsg_to_cv2(image_msg, "bgr8")
```

#### Использование Сompressed топика

```
from cv_bridge import CvBridge
from sensor_msgs.msg import CompressedImage, Image

cvBridge = CvBridge()
cv_image = cvBridge.compressed_imgmsg_to_cv2(image_msg, desired_encoding='passthrough')
```

Важно понимать, что в случае использования compressed происходит дополнительное преобразование jpeg->raw. Но при этом происходит экономия сетевых ресурсов.&#x20;

#### Прямой захват видео

Также вы можете отключить ноду ROS по захвату изображения с камеры и сделать захват данных из своего OpenCV приложения самостоятельно.

`cap = cv2.VideoCapture(0)`

После этого удобно опубликовать данные в топик самостоятельно для просмотра через веб-интерфейс и тп.

Далее производить с видео все необходимые манипуляции, и после этого, при необходимости, публиковать видео в топики.

Пример программы на pytnon, которая, используя `opencv` , следит за цветным мячиком и управляет роботом: [ball\_tracking.py](https://github.com/voltbro/turtlebro\_examples/blob/master/src/ball\_tracking.py)
