---
title: /api/vehicle/doors/layout
status: v1.2.0
method: function call
type:
  name: autoware_adapi_v1_msgs/srv/GetDoorLayout
  res:
    - name: status
      text: 响应状态
    - name: doors.roles
      text: 车门在车辆所提供服务中的用途。
    - name: doors.description
      text: 用于界面显示的车门说明。
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
获取车门布局，返回包含各车门用途和说明的数组。
{% endblock %}
