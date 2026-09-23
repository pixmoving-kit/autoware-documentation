---
title: /api/localization/initialization_state
status: v1.0.0
method: notification
type:
  name: autoware_adapi_v1_msgs/msg/LocalizationInitializationState
  msg:
    - name: state
      text: 定位初始化状态值。
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
获取定位的初始化状态。
详情请参阅[定位](../../../features/localization.md)。
{% endblock %}
