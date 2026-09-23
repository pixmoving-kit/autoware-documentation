---
title: /api/localization/initialize
status: v1.0.0
method: function call
type:
  name: autoware_adapi_v1_msgs/srv/InitializeLocalization
  req:
    - name: pose
      text: 用作初始估计的全局位姿。如果省略，将使用 GNSS 位姿。
  res:
    - name: status
      text: 响应状态
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
请求初始化定位。
详情请参阅[定位](../../../features/localization.md)。
{% endblock %}
