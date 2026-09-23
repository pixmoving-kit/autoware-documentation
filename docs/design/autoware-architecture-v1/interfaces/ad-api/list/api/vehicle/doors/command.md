---
title: /api/vehicle/doors/command
status: v1.2.0
method: function call
type:
  name: autoware_adapi_v1_msgs/srv/SetDoorCommand
  req:
    - name: doors.index
      text: 目标车门的索引。
    - name: doors.command
      text: 目标车门的指令。
  res:
    - name: status
      text: 响应状态
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
设置车门指令。仅当车辆支持通过软件控制车门时，此 API 才可用。
如果无法安全地开启或关闭车门，此 API 调用会失败。
{% endblock %}
