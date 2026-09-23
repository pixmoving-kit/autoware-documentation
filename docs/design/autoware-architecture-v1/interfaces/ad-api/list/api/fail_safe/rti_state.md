---
title: /api/fail_safe/rti_state
status: not released
method: notification
type:
  name: autoware_adapi_v1_msgs/msg/RtiState
  msg:
    - name: request
      text: 表示是否请求 RTI 的标志。
    - name: message
      text: 包含 RTI 原因等信息的消息。
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
获取 RTI 状态。
详情请参阅[故障安全](../../../features/fail-safe.md)。
{% endblock %}
