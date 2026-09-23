---
title: /api/routing/route
status: v1.0.0
method: notification
type:
  name: autoware_adapi_v1_msgs/msg/Route
  msg:
    - name: header
      text: 用于位姿变换的消息头
    - name: data
      text: 以 Lanelet 格式表示的路线
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
获取包含 Lanelet 格式途经路段的路线。如果未设置路线，则返回空值。
{% endblock %}
