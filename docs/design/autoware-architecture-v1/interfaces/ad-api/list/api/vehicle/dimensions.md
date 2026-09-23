---
title: /api/vehicle/dimensions
status: v1.1.0
method: function call
type:
  name: autoware_adapi_v1_msgs/srv/GetVehicleDimensions
  res:
    - name: status
      text: 响应状态
    - name: dimensions
      text: 车辆尺寸
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
获取车辆尺寸。各数值的定义见[此处](../../../../components/vehicle-dimensions.md)。
{% endblock %}
