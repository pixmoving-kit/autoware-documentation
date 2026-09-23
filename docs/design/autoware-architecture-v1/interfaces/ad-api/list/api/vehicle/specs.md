---
title: /api/vehicle/specs
status: not released
method: function call
type:
  name: autoware_adapi_v1_msgs/srv/GetVehicleSpecs
  res:
    - name: status
      text: 响应状态
    - name: specs
      text: 车辆规格
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
获取车辆规格。
{% endblock %}
