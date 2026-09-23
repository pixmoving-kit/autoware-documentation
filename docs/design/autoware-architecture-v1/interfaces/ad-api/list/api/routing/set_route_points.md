---
title: /api/routing/set_route_points
status: v1.0.0
method: function call
type:
  name: autoware_adapi_v1_msgs/srv/SetRoutePoints
  req:
    - name: header
      text: 用于位姿变换的消息头
    - name: goal
      text: 目标位姿
    - name: waypoints
      text: 途经点位姿
  res:
    - name: status
      text: 响应状态
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
使用途经点位姿设置路线。如果未指定起点位姿，则使用当前位姿。
此 API 仅在路线状态为 UNSET 时接受路线。在其他状态下，请先清除路线。
{% endblock %}
