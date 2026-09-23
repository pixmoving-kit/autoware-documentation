<a id="release-notes"></a>

# 发布说明

## v1.9.1

- [变更] 将环岛添加为一种[规划行为](features/planning-factors.md)

## v1.9.0

- [新增] 添加[控制命令 API](features/control.md)
- [新增] 添加 {{ link_ad_api('/api/vehicle/metrics') }}
- [新增] 添加 {{ link_ad_api('/api/system/diagnostics/reset') }}
- [新增] 添加 {{ link_ad_api('/api/fail_safe/list_mrm_description') }}
- [新增] 添加 {{ link_ad_api('/api/fail_safe/mrm_request/list') }}
- [新增] 添加 {{ link_ad_api('/api/fail_safe/mrm_request/send') }}
- [变更] [诊断 API](features/diagnostics.md) 支持更详细的状态
- [变更] 弃用[故障安全 API](features/fail-safe.md) 中的 MRM 行为常量

## v1.8.0

- [新增] 添加[手动控制 API](features/manual-control.md)

## v1.7.0

- [维护] 更改原型实现使用的消息。

## v1.6.0

- [变更] 修正 {{ link_ad_api('/api/vehicle/status') }} 的通信方式
- [变更] 为 {{ link_ad_api('/api/routing/clear_route') }} 添加限制
- [变更] 为 {{ link_ad_api('/api/vehicle/doors/command') }} 添加限制

## v1.5.0

- [新增] 添加 {{ link_ad_api('/api/routing/change_route_points') }}
- [新增] 添加 {{ link_ad_api('/api/routing/change_route') }}

## v1.4.0

- [新增] 添加 {{ link_ad_api('/api/vehicle/status') }}

## v1.3.0

- [新增] 添加[心跳 API](features/heartbeat.md)
- [新增] 添加[诊断 API](features/diagnostics.md)

## v1.2.0

- [新增] 添加[车门 API](features/vehicle-doors.md)
- [变更] 为 {{ link_ad_api('/api/fail_safe/mrm_state') }} 添加靠边停车常量

## v1.1.0

- [新增] 添加 {{ link_ad_api('/api/fail_safe/mrm_state') }}
- [新增] 添加 {{ link_ad_api('/api/vehicle/dimensions') }}
- [新增] 添加 {{ link_ad_api('/api/vehicle/kinematics') }}
- [变更] 为[路线规划 API](features/routing.md) 添加选项

## v1.0.0

- [新增] 添加[接口 API](features/interface.md)
- [新增] 添加[定位 API](features/localization.md)
- [新增] 添加[路线规划 API](features/routing.md)
- [新增] 添加[运行模式 API](features/operation_mode.md)
