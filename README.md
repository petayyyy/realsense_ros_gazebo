# !Данная установка камеры проверенна и корректно работает на 18.04 ubuntu в версии симулятора [0.3](https://github.com/CopterExpress/clover_vm/releases/tag/v0.3).  
### Запуск t265 камеры на дроне ###
Необходимо добавить данные строки в файл [clover4.xacro]([drone/clover4.xacro](https://github.com/CopterExpress/clover/blob/master/clover_description/urdf/clover/clover4.xacro)).  
```bash
<!-- t265 flag -->
<xacro:arg name="t265" default="false"/>
<!-- Create t265 plugin -->
<xacro:if value="$(arg t265)">
  <xacro:include filename="$(find realsense_ros_gazebo)/xacro/tracker.xacro"/>
  <xacro:realsense_T265 sensor_name="camera" parent_link="base_link" rate="30.0">
    <origin rpy="0.0 1.57 0.0" xyz="-0.07 0 -0.04"/>
  </xacro:realsense_T265>
</xacro:if>
```
Для установки камеры необходимо поменять параметр "t265" с false на true. Либо в запускаемом файле [spawn_drone.launch](https://github.com/CopterExpress/clover/blob/master/clover_description/launch/spawn_drone.launch) выставить флаг для установки камеры. Для этого необходимо довавить и заменить строки:
```bash
<!-- Use t265 camera -->
<arg name="t265" default="true"/>
<arg name="cmd" default="$(find xacro)/xacro $(find clover_description)/urdf/clover/clover4.xacro main_camera:=$(arg main_camera) rangefinder:=$(arg rangefinder) led:=$(arg led) gps:=$(arg gps) maintain_camera_rate:=$(arg maintain_camera_rate) use_clover_physics:=$(arg use_clover_physics) t265:=$(arg t265)"/>
```
Примеры редактирования данных файлов вы можете посмотреть в [директории](drone/).
## !!!!!!!!!!!!Оба файла, которые вы редактируете это файлы запуска симуляции и редактировать их нужно в вашем [симуляторе]([drone/spawn_drone.launch](https://github.com/CopterExpress/clover/tree/master/clover_description)), а не в этой директории.
### Название топиков ###
* /camera/**fisheye1**/* - изображение с первой камеры
* /camera/**fisheye2**/* - изображение со второй камеры
* /camera/**gyro**/sample - показатели гироскова, не используются
* /camera/**odom**/sample - одомерия камеры, передаем дрону для координации в пространстве

## Кастомное расположение камеры
Для изменения положения необходимо изменять ее расположени за счет изменения данных праметров:
```bash
<origin rpy="0.0 1.57 0.0" xyz="-0.07 0 -0.04"/>
```
Где "rpy" углы поворота камеры yaw, pitch, roll в радианах. При значениях по дефолту стоит направленной вниз.  
![drone-roll](https://user-images.githubusercontent.com/47917455/201782371-0ab02661-18cb-4a53-91b0-09c758f334c4.gif)
И где "xyz" расположение камеры относительно центра полетного контроллера (относительно "base_link"). При значениях по дефолту камера располагается на нижней деке сзади. Как строются оси можно посмотреть на картинках ниже.
![image](https://user-images.githubusercontent.com/47917455/201780537-d0264b4c-0663-4596-9c05-cd7bb48d3bc8.png)

![image](https://user-images.githubusercontent.com/47917455/201781249-8f758745-4e76-4020-87bd-f628db2fad0d.png)
