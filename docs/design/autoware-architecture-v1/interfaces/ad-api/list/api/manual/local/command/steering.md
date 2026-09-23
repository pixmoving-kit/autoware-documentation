---
title: /api/manual/local/command/steering
status: v1.8.0
method: realtime stream
type:
  name: autoware_adapi_v1_msgs/msg/SteeringCommand
  msg:
    - name: stamp
      text: 发送此消息时的时间戳。
    - name: steering_tire_angle
      text: 目标转向轮转角 [rad]。
    - name: steering_tire_velocity
      text: 目标转向轮角速度 [rad/s]。
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
向此 API 发送转向指令。
使用此 API 前，请按照[手动控制](../../../../../features/manual-control.md)中的说明选择相应模式。
{% endblock %}
