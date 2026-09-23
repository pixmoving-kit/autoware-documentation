---
title: /api/system/diagnostics/struct
status: v1.3.0
method: notification
type:
  name: autoware_adapi_v1_msgs/msg/DiagGraphStruct
  msg:
    - name: stamp
      text: 发送此消息时的时间戳。
    - name: id
      text: 用于检查结构与状态对应关系的 ID。
    - name: nodes
      text: 诊断图中节点的静态数据。
    - name: links
      text: 诊断图中连接的静态数据。
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
这是诊断信息的静态部分。
如果发布了使用新 ID 的静态数据，应忽略使用旧 ID 的动态数据。
详情请参阅[诊断](../../../../features/diagnostics.md)。
{% endblock %}
