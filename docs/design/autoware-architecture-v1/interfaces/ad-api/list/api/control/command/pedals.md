---
title: /api/control/command/pedals
status: v1.9.0
method: realtime stream
type:
  name: autoware_adapi_v1_msgs/msg/PedalsCommand
  msg:
    - name: stamp
      text: 发送此消息时的时间戳。
    - name: throttle
      text: 目标油门踏板比例。
    - name: brake
      text: 目标制动踏板比例。
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
Autoware 发送给车辆的目标踏板指令。
踏板值表示踏下程度的比例，完全踏下时为 1.0。
如果车辆不是通过踏板指令控制的，则不支持此 API。
{% endblock %}
