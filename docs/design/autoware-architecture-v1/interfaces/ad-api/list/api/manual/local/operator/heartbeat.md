---
title: /api/manual/local/operator/heartbeat
status: v1.8.0
method: realtime stream
type:
  name: autoware_adapi_v1_msgs/msg/ManualOperatorHeartbeat
  msg:
    - name: stamp
      text: 发送此消息时的时间戳。
    - name: ready
      text: 操作员是否能够继续驾驶。
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
应用程序需要判断操作员是否能够驾驶，并通过此 API 发送判断结果。
详情请参阅[手动控制](../../../../../features/manual-control.md)。
{% endblock %}
