---
title: /api/fail_safe/list_mrm_description
status: v1.9.0
method: function call
type:
  name: autoware_adapi_v1_msgs/srv/ListMrmDescription
  res:
    - name: descriptions.behavior
      text: MRM 的行为 ID。
    - name: descriptions.name
      text: MRM 的名称。
    - name: descriptions.description
      text: MRM 的说明。
    - name: status
      text: 响应状态
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
获取 MRM 说明列表。
详情请参阅[故障安全](../../../features/fail-safe.md)。
{% endblock %}
