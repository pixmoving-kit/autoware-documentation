---
title: /api/operation_mode/enable_autoware_control
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
启用 Autoware 对车辆的控制。
详情请参阅[操作模式](../../../features/operation_mode.md)。
如果车辆不支持通过软件切换模式，此 API 调用会失败。
{% endblock %}
