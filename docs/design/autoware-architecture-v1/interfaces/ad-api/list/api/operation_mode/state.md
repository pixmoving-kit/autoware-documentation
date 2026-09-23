---
title: /api/operation_mode/state
status: v1.0.0
method: notification
type:
  name: autoware_adapi_v1_msgs/msg/OperationModeState
  msg:
    - name: mode
      text: 当前选定的 Autoware 控制指令。
    - name: is_autoware_control_enabled
      text: 如果已启用 Autoware 对车辆的控制，则为 true。
    - name: is_in_transition
      text: 如果操作模式正在切换，则为 true。
    - name: is_stop_mode_available
      text: 如果可以切换到停止模式，则为 true。
    - name: is_autonomous_mode_available
      text: 如果可以切换到自动驾驶模式，则为 true。
    - name: is_local_mode_available
      text: 如果可以切换到本地模式，则为 true。
    - name: is_remote_mode_available
      text: 如果可以切换到远程模式，则为 true。
---

{% extends 'design/autoware-architecture-v1/interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
获取操作模式状态。
详情请参阅[操作模式](../../../features/operation_mode.md)。
{% endblock %}
