---
title: /api/routing/state
status: v1.0.0
method: notification
type:
  name: autoware_adapi_v1_msgs/msg/RouteState
  msg:
    - name: state
      text: 路线状态值。
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
获取路线状态。
详情请参阅[路线规划](../../../features/routing.md)。
{% endblock %}
