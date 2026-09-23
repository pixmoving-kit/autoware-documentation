---
title: /api/planning/cooperation/set_commands
status: not released
method: function call
type:
  name: autoware_adapi_v1_msgs/srv/SetCooperationCommands
  req:
    - name: commands.uuid
      text: 协作状态中的 ID。
    - name: commands.cooperator
      text: 操作员的决策。
  res:
    - name: status
      text: 响应状态
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
设置操作员对协作请求的决策。
详情请参阅[协作](../../../../features/cooperation.md)。
{% endblock %}
