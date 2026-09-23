---
title: /api/fail_safe/mrm_request/send
status: v1.9.0
method: function call
type:
  name: autoware_adapi_v1_msgs/srv/SendMrmRequest
  req:
    - name: request.sender
      text: MRM 请求发送方的名称。
    - name: request.strategy
      text: MRM 请求的策略。
  res:
    - name: status
      text: 响应状态
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
发送 MRM 请求。
详情请参阅[故障安全](../../../../features/fail-safe.md)。
{% endblock %}
