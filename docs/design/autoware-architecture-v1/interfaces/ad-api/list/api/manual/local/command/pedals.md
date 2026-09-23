---
title: /api/manual/local/command/pedals
status: v1.8.0
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
发送本地操作模式下使用的踏板指令。踏板值表示踏下程度的比例，完全踏下时为 1.0。
使用此 API 前，请按照[手动控制](../../../../../features/manual-control.md)中的说明选择相应模式。
{% endblock %}
