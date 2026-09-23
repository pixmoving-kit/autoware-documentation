---
title: /api/vehicle/kinematics
status: v1.1.0
method: realtime stream
type:
  name: autoware_adapi_v1_msgs/msg/VehicleKinematics
  msg:
    - name: geographic_pose
      text: 车辆的经度和纬度。如果地图使用局部坐标，则此信息不可用。
    - name: pose
      text: base_link 的位姿及其协方差。
    - name: twist
      text: 车辆当前的速度及其协方差。
    - name: accel
      text: 车辆当前的加速度及其协方差。
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
发布车辆运动学信息。
{% endblock %}
