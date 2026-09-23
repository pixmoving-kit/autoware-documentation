---
title: /api/routing/change_route_points
status: v1.5.0
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
与 {{ link_ad_api('/api/routing/set_route_points') }} 相同，但用于在行驶过程中更改路线。
此 API 仅在路线状态为 SET 时接受路线。
在其他状态下，请先设置路线，或等待路线更改完成。
{% endblock %}
