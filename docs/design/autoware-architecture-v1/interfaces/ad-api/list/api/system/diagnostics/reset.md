---
title: /api/system/diagnostics/reset
status: v1.9.0
method: function call
type:
  name: autoware_adapi_v1_msgs/srv/ResetDiagGraph
  res:
    - name: status
      text: 响应状态
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
重置诊断图的锁存状态。
详情请参阅[诊断](../../../../features/diagnostics.md)。
{% endblock %}
