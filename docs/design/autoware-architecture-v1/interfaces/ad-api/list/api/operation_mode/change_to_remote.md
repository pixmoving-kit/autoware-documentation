---
title: /api/operation_mode/change_to_remote
status: v1.0.0
method: function call
type:
  name: autoware_adapi_v1_msgs/srv/ChangeOperationMode
  res:
    - name: status
      text: 响应状态
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
将操作模式切换为远程模式。
详情请参阅[操作模式](../../../features/operation_mode.md)。
{% endblock %}
