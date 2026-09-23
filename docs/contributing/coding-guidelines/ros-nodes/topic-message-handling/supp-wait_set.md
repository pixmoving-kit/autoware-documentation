<a id="supplement-use-rclcppwaitset"></a>

# 【补充】使用 rclcpp::WaitSet

<a id="what-is-rclcppwaitset"></a>

## 什么是 `rclcpp::WaitSet`

如[_调用 Subscription 对象的 take() 方法_](./index.md#call-take-method-of-subscription-object)所述，`take()` 方法不可逆。一旦执行 `take()`，订阅对象的状态就会改变。由于没有对应的撤销操作，订阅对象无法恢复到之前的状态。可以在调用 `take()` 之前使用 `rclcpp::WaitSet`，检查订阅队列中是否有消息到达。
以下示例代码展示了 `wait_set_.wait()` 如何告知你已经收到消息，且该消息可通过 `take()` 获取。

```c++
      auto wait_result = wait_set_.wait(std::chrono::milliseconds(0));
      if (wait_result.kind() == rclcpp::WaitResultKind::Ready &&
          wait_result.get_wait_set().get_rcl_wait_set().subscriptions[0]) {
            sub_->take(msg, msg_info);
            RCLCPP_INFO(this->get_logger(), "Catch message");
            RCLCPP_INFO(this->get_logger(), "I heard: [%s]", msg.data.c_str());
```

单个 `rclcpp::WaitSet` 对象可以观察多个订阅对象。如果存在多个订阅，分别对应不同话题，就可以逐个订阅检查消息是否到达。自主机器人领域的算法需要多种输入消息，例如传感器数据或执行器状态。对多个订阅使用 `rclcpp::WaitSet`，即可在不取出任何消息的情况下检查所需消息是否已经到达。

```c++
      auto wait_result = wait_set_.wait(std::chrono::milliseconds(0));
      bool received_all_messages = false;
      if (wait_result.kind() == rclcpp::WaitResultKind::Ready) {
        for (auto wait_set_subs : wait_result.get_wait_set().get_rcl_wait_set().subscriptions) {
          if (!wait_set_subs) {
            RCLCPP_INFO_THROTTLE(get_logger(), clock, 5000, "Waiting for data...");
            return {};
          }
        }
        received_all_mesages = true;
      }
```

在上述代码中，如果不使用 `rclcpp::WaitSet`，就无法在不改变订阅对象状态的情况下确认所有所需消息是否到达。

<a id="coding-manner"></a>

## 编码方式

本节结合以下示例代码，介绍如何使用 `rclcpp::WaitSet` 编程。

- [_ros2_subscription_examples/waitset_examples/src/talker_triple.cpp at main · takam5f2/ros2_subscription_examples_](https://github.com/takam5f2/ros2_subscription_examples/blob/main/waitset_examples/src/talker_triple.cpp)
  - 它周期性地发布消息：`/chatter` 每秒一次，`/slower_chatter` 每两秒一次，`/slowest_chatter` 每三秒一次。
- [_ros2_subscription_examples/waitset_examples/src/timer_listener_triple_async.cpp at main · takam5f2/ros2_subscription_examples_](https://github.com/takam5f2/ros2_subscription_examples/blob/main/waitset_examples/src/timer_listener_triple_async.cpp)
  - 它每秒查询一次 `WaitSet`，如果有可用消息，就通过 `take()` 获取。
  - 它为 `/chatter`、`/slower_chatter` 和 `/slower_chatter` 设置了三个订阅。

使用 `rclcpp::WaitSet` 需要以下三个步骤。

<a id="1-declare-and-initialize-waitset"></a>

### 1. 声明并初始化 `WaitSet`

首先必须实例化一个基于 `rclcpp::WaitSet` 的对象。
以下片段来自 [_ros2_subscription_examples/waitset_examples/src/timer_listener_triple_async.cpp at main · takam5f2/ros2_subscription_examples_](https://github.com/takam5f2/ros2_subscription_examples/blob/main/waitset_examples/src/timer_listener_triple_async.cpp)。

```c++
rclcpp::WaitSet wait_set_;
```

??? note

    与 `rclcpp::WaitSet` 类似的类有多种。`rclcpp::WaitSet` 对象可以在运行时配置，但它不是线程安全的，详见 [_`rclcpp::WaitSet` 的 API 规范_](https://docs.ros.org/en/ros2_packages/humble/api/rclcpp/generated/typedef_namespacerclcpp_1ad6fb19c154de27e92430309d2da25ac3.html)。
    rclcpp 包提供了以下可替代 rclcpp::WaitSet 的线程安全类。

    - [_rclcpp::ThreadSafeWaitSet 类型别名_](https://docs.ros.org/en/ros2_packages/humble/api/rclcpp/generated/typedef_namespacerclcpp_1acaec573e71549fd3078644e18e7f7127.html)
      - 订阅、定时器等只能在线程安全的状态下注册到 `ThreadSafeWaitSet`。
      - 示例代码见：[examples/rclcpp/wait_set/src/thread_safe_wait_set.cpp at rolling · ros2/examples](https://github.com/ros2/examples/blob/rolling/rclcpp/wait_set/src/thread_safe_wait_set.cpp)
    - [_rclcpp::StaticWaitSet 类型别名_](https://docs.ros.org/en/ros2_packages/humble/api/rclcpp/generated/typedef_namespacerclcpp_1adb06acf4a5723b1445fa6ed4e8f73374.html)
      - 订阅、定时器等只能在初始化时注册到 `rclcpp::StaticWaitSet`。
      - 示例代码如下：
        - [_ros2_subscription_examples/waitset_examples/src/timer_listener_twin_static.cpp at main · takam5f2/ros2_subscription_examples_](https://github.com/takam5f2/ros2_subscription_examples/blob/main/waitset_examples/src/timer_listener_twin_static.cpp)
        - [_examples/rclcpp/wait_set/src/static_wait_set.cpp at rolling · ros2/examples_](https://github.com/ros2/examples/blob/rolling/rclcpp/wait_set/src/static_wait_set.cpp)

<a id="2-register-trigger-subscription-timer-and-so-on-to-waitset"></a>

### 2. 向 `WaitSet` 注册触发源（Subscription、Timer 等）

需要向基于 `rclcpp::WaitSet` 的对象注册触发源。
以下片段来自 [_ros2_subscription_examples/waitset_examples/src/timer_listener_triple_async.cpp at main · takam5f2/ros2_subscription_examples_](https://github.com/takam5f2/ros2_subscription_examples/blob/main/waitset_examples/src/timer_listener_triple_async.cpp)。

```c++
    subscriptions_array_[0] = create_subscription<std_msgs::msg::String>("chatter", qos, not_executed_callback, subscription_options);
    subscriptions_array_[1] = create_subscription<std_msgs::msg::String>("slower_chatter", qos, not_executed_callback, subscription_options);
    subscriptions_array_[2] = create_subscription<std_msgs::msg::String>("slowest_chatter", qos, not_executed_callback, subscription_options);

    // Add subscription to waitset
    for (auto & subscription : subscriptions_array_) {
      wait_set_.add_subscription(subscription);
    }
```

上述代码中的 `add_subscription()` 方法将已创建的订阅注册到 `wait_set_` 对象。
基于 `rclcpp::WaitSet` 的对象主要处理具有对应回调函数的对象。它不仅可以观察基于 `Subscription` 的对象，也可以观察基于 `Timer`、`Service` 或 `Action` 的对象。单个 `rclcpp::WaitSet` 对象可同时接收多种不同类型的对象。
注册定时器触发源的示例代码如下。

```c++
wait_set_.add_timer(much_slower_timer_);
```

也可以在声明和初始化时注册触发源，如[_示例中的 wait_set_topics_and_timer.cpp_](https://github.com/ros2/examples/blob/rolling/rclcpp/wait_set/src/wait_set_topics_and_timer.cpp#L66) 所示。

<a id="3-verify-waitset-result"></a>

### 3. 检查 WaitSet 结果

`rclcpp::WaitSet` 返回的检查结果采用嵌套数据结构。
可以通过以下两个步骤检查 `WaitSet` 的结果：

1. 检查是否有任意触发源被触发。
2. 检查指定触发源是否被触发。

对于第 1 步，以下示例来自 [_ros2_subscription_examples/waitset_examples/src/timer_listener_triple_async.cpp at main · takam5f2/ros2_subscription_examples_](https://github.com/takam5f2/ros2_subscription_examples/blob/main/waitset_examples/src/timer_listener_triple_async.cpp)。

```c++
      auto wait_result = wait_set_.wait(std::chrono::milliseconds(0));
      if (wait_result.kind() == rclcpp::WaitResultKind::Ready) {
        RCLCPP_INFO(this->get_logger(), "wait_set tells that some subscription is ready");
      } else {
        RCLCPP_INFO(this->get_logger(), "wait_set tells that any subscription is not ready and return");
        return;
      }
```

上述代码中的 `auto wait_result = wait_set_.wait(std::chrono::milliseconds(0))` 检查 `wait_set_` 中是否有触发源被触发。`wait()` 的参数是超时时长。如果该值大于 0 毫秒或 0 秒，此方法会等待接收消息，直到超时。
如果 `wait_result.kind() == rclcpp::WaitResultKind::Ready` 为 `true`，则表示有触发源被触发。

对于第 2 步，以下示例来自 [_ros2_subscription_examples/waitset_examples/src/timer_listener_triple_async.cpp at main · takam5f2/ros2_subscription_examples_](https://github.com/takam5f2/ros2_subscription_examples/blob/main/waitset_examples/src/timer_listener_triple_async.cpp)。

```c++
      for (size_t i = 0; i < subscriptions_num; i++) {
        if (wait_result.get_wait_set().get_rcl_wait_set().subscriptions[i]) {
          std_msgs::msg::String msg;
          rclcpp::MessageInfo msg_info;
          if (subscriptions_array_[i]->take(msg, msg_info)) {
            RCLCPP_INFO(this->get_logger(), "Catch message via subscription[%ld]", i);
            RCLCPP_INFO(this->get_logger(), "I heard: [%s]", msg.data.c_str());
```

上述代码中的 `wait_result.get_wait_set().get_rcl_wait_set().subscriptions[i]` 表示各个触发源是否被触发。结果存储在 `subscriptions` 数组中，其顺序与触发源的注册顺序一致。
