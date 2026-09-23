<a id="initialize-the-pose"></a>

# 初始化位姿

<a id="related-api"></a>

## 相关 API

- [定位](../features/localization.md)

<a id="sequence"></a>

## 时序

- 使用输入信息初始化位姿。

  ```plantuml
  {% include 'design/autoware-architecture-v1/interfaces/ad-api/use-cases/sequence/initialize-pose-input.plantuml' %}
  ```

- 使用 GNSS 初始化位姿。

  ```plantuml
  {% include 'design/autoware-architecture-v1/interfaces/ad-api/use-cases/sequence/initialize-pose-gnss.plantuml' %}
  ```
