---
title: /api/vehicle/status
status: v1.4.0
method: realtime stream
type:
  name: autoware_adapi_v1_msgs/msg/VehicleStatus
  msg:
    - name: gear
      text: 挡位状态。
    - name: turn_indicators
      text: 转向灯状态，仅左侧或右侧之一会启用。
    - name: hazard_lights
      text: 危险警告灯状态。
    - name: steering_tire_angle
      text: 车辆当前的车轮转角，单位为弧度。
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
发布车辆状态信息。
{% endblock %}
