---
title: /api/control/command/velocity
status: v1.9.0
method: realtime stream
type:
  name: autoware_adapi_v1_msgs/msg/VelocityCommand
  msg:
    - name: stamp
      text: 发送此消息时的时间戳。
    - name: velocity
      text: 目标速度 [m/s]。
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
Autoware 发送给车辆的目标速度。
{% endblock %}
