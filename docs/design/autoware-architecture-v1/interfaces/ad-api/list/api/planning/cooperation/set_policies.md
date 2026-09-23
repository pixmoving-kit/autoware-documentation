---
title: /api/planning/cooperation/set_policies
status: not released
method: function call
type:
  name: autoware_adapi_v1_msgs/srv/SetCooperationPolicies
  req:
    - name: policies.behavior
      text: 目标行为的类型。
    - name: policies.sequence
      text: 目标序列的类型。
    - name: policies.policy
      text: 协作策略的类型。
  res:
    - name: status
      text: 响应状态
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
设置默认决策，在操作员尚未作出决定时使用。
详情请参阅[协作](../../../../features/cooperation.md)。
{% endblock %}
