---
title: /api/interface/version
status: v1.0.0
method: function call
type:
  name: autoware_adapi_version_msgs/srv/InterfaceVersion
  res:
    - name: major
      text: 主版本号
    - name: minor
      text: 次版本号
    - name: patch
      text: 修订版本号
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
获取接口版本。版本号遵循语义化版本规范。
{% endblock %}
