---
title: /api/manual/remote/control_mode/select
status: v1.8.0
method: function call
type:
  name: autoware_adapi_v1_msgs/srv/SelectManualControlMode
  req:
    - name: mode
      text: 要使用的手动控制模式。
  res:
    - name: status
      text: 响应状态
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
按照[手动控制](../../../../../features/manual-control.md)中的说明，选择手动控制模式。
当[操作模式](../../../../../features/operation_mode.md)为远程模式时，此 API 调用会失败。
{% endblock %}
