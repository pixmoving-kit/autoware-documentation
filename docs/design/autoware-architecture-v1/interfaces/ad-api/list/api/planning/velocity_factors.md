---
title: /api/planning/velocity_factors
status: not released
method: realtime stream
type:
  name: autoware_adapi_v1_msgs/msg/VelocityFactorArray
  msg:
    - name: factors.pose
      text: 与速度因素相关的 base_link 位姿。
    - name: factors.distance
      text: 从 base_link 到上述位姿的距离。
    - name: factors.status
      text: 速度因素的状态。
    - name: factors.behavior
      text: 速度因素的行为类型。
    - name: factors.sequence
      text: 速度因素的序列类型。
    - name: factors.detail
      text: 速度因素的附加信息。
    - name: factors.cooperation
      text: 模块支持时提供的协作状态。
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
获取速度因素，并按距离升序排列。
详情请参阅[规划因素](../../../features/planning-factors.md)。
{% endblock %}
