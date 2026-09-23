---
title: /api/manual/local/control_mode/status
status: v1.8.0
method: notification
type:
  name: autoware_adapi_v1_msgs/msg/ManualControlModeStatus
  msg:
    - name: stamp
      text: 发送此消息时的时间戳。
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
获取当前的手动操作模式。
{% endblock %}
