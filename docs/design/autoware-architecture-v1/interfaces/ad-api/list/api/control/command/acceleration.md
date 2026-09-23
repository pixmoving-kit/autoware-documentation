---
title: /api/control/command/acceleration
status: v1.9.0
method: realtime stream
type:
  name: autoware_adapi_v1_msgs/msg/AccelerationCommand
  msg:
    - name: stamp
      text: 发送此消息时的时间戳。
    - name: acceleration
      text: 目标加速度 [m/s^2]。
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
Autoware 发送给车辆的目标加速度。
{% endblock %}
