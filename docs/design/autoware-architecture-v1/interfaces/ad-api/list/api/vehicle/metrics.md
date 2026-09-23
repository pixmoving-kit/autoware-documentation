---
title: /api/vehicle/metrics
status: v1.9.0
method: reliable stream
type:
  name: autoware_adapi_v1_msgs/msg/VehicleMetrics
  msg:
    - name: energy
      text: 车辆剩余燃油量或电量，以比例表示，最大值为 1.0。
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
发布车辆指标。
{% endblock %}
