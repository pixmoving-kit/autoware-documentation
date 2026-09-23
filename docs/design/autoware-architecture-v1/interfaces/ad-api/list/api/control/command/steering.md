---
title: /api/control/command/steering
status: v1.9.0
method: realtime stream
type:
  name: autoware_adapi_v1_msgs/msg/SteeringCommand
  msg:
    - name: stamp
      text: 发送此消息时的时间戳。
    - name: steering_tire_angle
      text: 目标转向轮转角 [rad]。
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
Autoware 发送给车辆的目标转向指令。
{% endblock %}
