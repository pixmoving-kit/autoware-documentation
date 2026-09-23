<a id="ros-2-humble-to-jazzy-transition"></a>

# 从 ROS 2 Humble 迁移到 Jazzy

进度跟踪见 [支持 ROS 2 Jazzy Jalisco #6695](https://github.com/autowarefoundation/autoware/issues/6695)。

- ROS 2 Humble 将于 2027 年 5 月结束支持。
- ROS 2 Jazzy 已于 2024 年 5 月发布，支持将持续到 2029 年 5 月。

此时间表用于 Autoware 的平台迁移：

- **原平台：** Ubuntu 22.04 | ROS 2 **Humble**
- **目标平台：** Ubuntu 24.04 | ROS 2 **Jazzy**

```mermaid
timeline
    title ROS 2 Humble to Jazzy Transition
    2026 Feb : Jazzy Docker Beta
             : Optional CI Checks
    2026 Apr : Jazzy Full Support
             : Dual CI Coverage
    2027 Jan : Humble Soft-Freeze
             : Standard Support Ends
    2027 May : Jazzy Exclusive Mode
             : Humble End of Support
```

```mermaid
gantt
    title ROS 2 Transition: Humble to Jazzy
    dateFormat  YYYY-MM-DD
    axisFormat  %b '%y
    tickInterval 1month

    %% Define Milestones (Vertical markers for key dates)
    section Key Events
    Jazzy Docker Beta       :milestone, m1, 2026-02-01, 0d
    Jazzy Full Support    :milestone, m2, 2026-04-01, 0d
    Humble Soft-Freeze      :milestone, m3, 2027-01-01, 0d
    Jazzy Exclusive         :milestone, m4, 2027-05-01, 0d

    %% Section 1: The Outgoing Platform
    section ROS 2 Humble
    Standard Support        :active,    h_std,  2026-01-01, 2027-01-01
    Low Maintenance (Crit Fixes) :crit, h_low,  after h_std, 2027-05-01
    End of Support          :done,      h_end,  after h_low, 1d

    %% Section 2: The Incoming Platform
    section ROS 2 Jazzy
    Docker Base Preview     :           j_beta, 2026-02-01, 2026-04-01
    Full Support (Ubuntu 24.04) :active, j_std,  after j_beta, 2027-06-01

    %% Section 3: Developer Action Items
    section CI Policy
    Optional Checks:           ci_opt, 2026-02-01, 2026-04-01
    Mandatory Dual Coverage :crit,      ci_man, 2026-04-01, 2027-05-01
    Jazzy Only              :active,    ci_jaz, after ci_man, 2027-06-01
```
