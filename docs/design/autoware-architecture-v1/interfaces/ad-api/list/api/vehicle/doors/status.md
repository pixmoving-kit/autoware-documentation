---
title: /api/vehicle/doors/status
status: v1.2.0
method: notification
type:
  name: autoware_adapi_v1_msgs/msg/DoorStatusArray
  msg:
    - name: doors.status
      text: 当前车门状态
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
各车门的状态，例如开启或关闭。
{% endblock %}
