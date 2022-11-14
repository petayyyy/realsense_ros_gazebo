### Запуск t265 камеры на дроне ###
Необходимо добавить данные строки в файл [clover4.xacro](drone/clover4.xacro).  
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
Для установки камеры необходимо поменять параметр "t265" с false на true. Либо в запускаемом файле [spawn_drone.launch](drone/spawn_drone.launch) выставить флаг для установки камеры.
```bash
<!-- Use t265 camera -->
<arg name="t265" default="true"/>
<arg name="cmd" default="$(find xacro)/xacro $(find clover_description)/urdf/clover/clover4.xacro main_camera:=$(arg main_camera) rangefinder:=$(arg rangefinder) led:=$(arg led) gps:=$(arg gps) maintain_camera_rate:=$(arg maintain_camera_rate) use_clover_physics:=$(arg use_clover_physics) t265:=$(arg t265)"/>
```
## !!!!!!!!!!!!Оба файла, которые вы редактируете это файлы запуска симуляции и редактировать их нужно в вашем [симуляторе]([drone/spawn_drone.launch](https://github.com/CopterExpress/clover/tree/master/clover_description)), а не в этой директории.
### Название топиков ###
* /camera/**fisheye1**/* - изображение с первой камеры
* /camera/**fisheye2**/* - изображение со второй камеры
* /camera/**gyro**/sample - показатели гироскова, не используются
* /camera/**odom**/sample - одомерия камеры, передаем дрону для координации в пространстве
