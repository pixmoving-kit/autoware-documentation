---
title: /api/manual/local/control_mode/list
status: v1.8.0
method: function call
type:
  name: autoware_adapi_v1_msgs/srv/ListManualControlMode
  res:
    - name: status
      text: 响应状态
    - name: modes
      text: 可用模式列表。
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
按照[手动控制](../../../../../features/manual-control.md)中的说明，列出可用的手动控制模式。
可用模式列表不包含已禁用的模式。
{% endblock %}
