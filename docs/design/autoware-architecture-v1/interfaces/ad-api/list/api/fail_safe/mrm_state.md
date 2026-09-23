---
title: /api/fail_safe/mrm_state
status: v1.1.0
method: notification
type:
  name: autoware_adapi_v1_msgs/msg/MrmState
  msg:
    - name: state
      text: MRM 操作的状态。
    - name: behavior
      text: 当前选择的 MRM 行为。
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
获取 MRM 状态。
详情请参阅[故障安全](../../../features/fail-safe.md)。
{% endblock %}
