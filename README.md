### Запуск t265 камеры на дроне ###
Необходимо добавить данные строки в файл "clover4.xacro".  
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
*** Пример вставки находиться в папке "drone" в одноименном файле.  
Для установки кемеры необходимо поменять параметр "t265" с false на true. Либо в запускаемом файле (spawn_drone.launch)[drone/spawn_drone.launch] выставить флаг для установки камеры.
### Название топиков ###
* /camera/**fisheye1**/* - изображение с первой камеры
* /camera/**fisheye2**/* - изображение со второй камеры
* /camera/**gyro**/sample - показатели гироскова, не используются
* /camera/**odom**/sample - одомерия камеры, передаем дрону для координации в пространстве
