---
title: /api/system/heartbeat
status: v1.3.0
method: realtime stream
type:
  name: autoware_adapi_v1_msgs/msg/Heartbeat
  msg:
    - name: stamp
      text: Autoware 内部时间戳，用于检查延迟。
    - name: seq
      text: 用于验证顺序的序列号，达到 65535 后回绕。
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
心跳频率为 10 Hz。
{% endblock %}
