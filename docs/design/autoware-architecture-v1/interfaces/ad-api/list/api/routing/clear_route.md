---
title: /api/routing/clear_route
status: v1.0.0
method: function call
type:
  name: autoware_adapi_v1_msgs/srv/ClearRoute
  res:
    - name: status
      text: 响应状态
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
清除路线。车辆正在使用该路线时，此 API 调用会失败。
{% endblock %}
