---
title: /api/manual/local/command/turn_indicators
status: v1.8.0
method: notification
type:
  name: autoware_adapi_v1_msgs/msg/TurnIndicatorsCommand
  msg:
    - name: stamp
      text: 发送此消息时的时间戳。
    - name: command
      text: 目标转向灯状态。
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
发送本地操作模式下使用的转向灯指令。
使用此 API 前，请按照[手动控制](../../../../../features/manual-control.md)中的说明选择相应模式。
{% endblock %}
