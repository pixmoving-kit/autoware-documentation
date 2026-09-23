---
title: /api/motion/accept_start
status: not released
method: function call
type:
  name: autoware_adapi_v1_msgs/srv/AcceptStart
  res:
    - name: status
      text: 响应状态
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
允许车辆起步。当[运动状态](../../../features/motion.md)为 STARTING 时，可以使用此 API。
{% endblock %}
