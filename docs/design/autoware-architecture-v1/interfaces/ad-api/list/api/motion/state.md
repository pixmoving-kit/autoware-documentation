---
title: /api/motion/state
status: not released
method: notification
type:
  name: autoware_adapi_v1_msgs/msg/MotionState
  msg:
    - name: state
      text: 运动状态值。
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
获取运动状态。
详情请参阅[运动状态](../../../features/motion.md)。
{% endblock %}
