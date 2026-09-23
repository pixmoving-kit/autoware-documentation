---
title: /api/perception/objects
status: not released
method: realtime stream
type:
  name: autoware_adapi_v1_msgs/msg/DynamicObjectArray
  msg:
    - name: objects.id
      text: 各目标的 UUID
    - name: objects.existence_probability
      text: 目标存在的概率
    - name: objects.classification
      text: 识别到的目标类型及其置信度
    - name: objects.kinematics
      text: 包含目标的位姿、速度、加速度和 predicted_paths
    - name: objects.shape
      text: 通过尺寸和多边形描述目标形状
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
获取识别到的目标数组，包含类别标签、形状、当前位置和预测路径
详情请参阅[感知](../../../features/perception.md)。
{% endblock %}
