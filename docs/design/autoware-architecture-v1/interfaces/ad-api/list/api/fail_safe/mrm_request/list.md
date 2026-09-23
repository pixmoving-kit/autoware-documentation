---
title: /api/fail_safe/mrm_request/list
status: v1.9.0
method: notification
type:
  name: autoware_adapi_v1_msgs/msg/MrmRequestList
  msg:
    - name: requests.sender
      text: MRM 请求发送方的名称。
    - name: requests.strategy
      text: MRM 请求的策略。
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
列出所有发送方的 MRM 请求。
详情请参阅[故障安全](../../../../features/fail-safe.md)。
{% endblock %}
