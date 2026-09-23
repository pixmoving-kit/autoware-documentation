<a id="topic-message-handling-guideline"></a>

# 话题消息处理指南

<a id="introduction"></a>

## 简介

本文介绍 Autoware 中话题消息处理的编码指南，其中包含相较于传统方式更推荐的处理方式。[_讨论页面_](https://github.com/orgs/autowarefoundation/discussions/4612)对其进行了概述，请参阅该页面了解推荐方式的基本概念。
本文引用的示例源代码位于 [_ros2_subscription_examples_](https://github.com/takam5f2/ros2_subscription_examples)。

<a id="conventional-message-handling-manner"></a>

## 传统消息处理方式

首先，来看一种常见的传统消息处理方式。
[_ROS 2 教程_](https://docs.ros.org/en/rolling/Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Cpp-Publisher-And-Subscriber.html#write-the-subscriber-node)是包括 Autoware 在内的 ROS 2 应用最常引用的资料之一。
该教程隐含地建议为每个订阅使用专用回调函数，访问并处理接收到的消息。Autoware 全面沿用了这种方式。

```c++
  steer_sub_ = create_subscription<SteeringReport>(
    "input/steering", 1,
    [this](SteeringReport::SharedPtr msg) { current_steer_ = msg->steering_tire_angle; });
```

在上述代码中，当收到名为 `input/steering` 的话题消息时，线程会执行内容为 `{current_steer_ = msg->steering_tier_angle;}` 的匿名函数作为回调。每当收到消息时都会执行回调函数，因此如果并非始终需要该消息，就会浪费计算资源。此外，唤醒线程也会带来计算开销。

<a id="recommended-manner"></a>

## 推荐方式

本节介绍一种推荐方式：仅在需要消息时，使用 `Subscription->take()` 方法获取消息。
以下示例代码展示了如何在某个回调函数执行期间调用 `Subscription->take()` 方法。在大多数情况下，应在主逻辑使用接收消息之前调用 `Subscription->take()`。
此时，话题消息直接从订阅队列（订阅对象内置的队列）中获取，而不通过回调函数。确切地说，需要在代码中确保回调函数不会自动调用。

```c++
  SteeringReport msg;
  rclcpp::MessageInfo msg_info;
  if (sub_->take(msg, msg_info)) {
    // processing and publishing after this
```

采用这种方式有以下好处。

- 可以减少订阅回调函数的调用次数。
- 无须从订阅中获取主逻辑不使用的话题消息。
- 无须为执行回调函数而唤醒线程，从而避免由此带来的多线程编程、数据竞争和互斥锁问题。

<a id="manners-to-handle-topic-message-data"></a>

## 话题消息数据的处理方式

本节介绍四种方式，包括推荐方式。

<a id="1-obtain-data-by-calling-subscription-take"></a>

### 1. 调用 `Subscription->take()` 获取数据

要采用使用 `Subscription->take()` 的推荐方式，主要需要完成以下两项工作。

1. 防止在收到话题消息时调用回调函数。
2. 需要话题消息时，调用订阅对象的 `take()` 方法。

`take()` 方法的典型用法示例见 [_ros2_subscription_examples/simple_examples/src/timer_listener.cpp_](https://github.com/takam5f2/ros2_subscription_examples/blob/main/simple_examples/src/timer_listener.cpp)。

<a id="prevent-calling-a-callback-function"></a>

#### 防止调用回调函数

要防止回调函数被自动调用，必须使其所属回调组中的回调函数不被添加到任何执行器。
根据 [_`create_subscription` 的 API 规范_](http://docs.ros.org/en/iron/p/rclcpp/generated/classrclcpp_1_1Node.html)，必须为基于 `rclcpp::Subscription` 的对象注册回调函数，即使该函数不执行任何操作。
以下是 [_ros2_subscription_examples/simple_examples/src/timer_listener.cpp_](https://github.com/takam5f2/ros2_subscription_examples/blob/main/simple_examples/src/timer_listener.cpp) 中的示例代码片段。

```c++
    rclcpp::CallbackGroup::SharedPtr cb_group_not_executed = this->create_callback_group(
        rclcpp::CallbackGroupType::MutuallyExclusive, false);
    auto subscription_options = rclcpp::SubscriptionOptions();
    subscription_options.callback_group = cb_group_not_executed;

    rclcpp::QoS qos(rclcpp::KeepLast(10));
    if (use_transient_local) {
      qos = qos.transient_local();
    }

    sub_ = create_subscription<std_msgs::msg::String>("chatter", qos, not_executed_callback, subscription_options);
```

上述代码调用 `create_callback_group`，并将第二个参数设为 `false`，创建 `cb_group_not_executed`。执行器不会调用属于该回调组的任何回调函数。
如果将 `subscription_options` 的 `callback_group` 成员设置为 `cb_group_not_executed`，那么收到对应话题 `chatter` 的消息时，就不会调用 `not_executed_callback`。
`create_callback_group` 的第二个参数定义如下。

```c++
rclcpp::CallbackGroup::SharedPtr create_callback_group(rclcpp::CallbackGroupType group_type, \
                                  bool automatically_add_to_executor_with_node = true)
```

当 `automatically_add_to_executor_with_node` 设为 `true` 时，如果节点被添加到执行器，执行器会自动调用该节点包含的回调函数。

<a id="call-take-method-of-subscription-object"></a>

#### 调用 Subscription 对象的 `take()` 方法

要从基于 `Subscription` 的对象中获取话题消息，请在预期时刻调用 `take()` 方法。
以下是 [_ros2_subscription_examples/simple_examples/src/timer_listener.cpp_](https://github.com/takam5f2/ros2_subscription_examples/blob/main/simple_examples/src/timer_listener.cpp) 中使用 `take()` 方法的示例代码片段。

```c++
  std_msgs::msg::String msg;
  rclcpp::MessageInfo msg_info;
  if (sub_->take(msg, msg_info)) {
    RCLCPP_INFO(this->get_logger(), "Catch message");
    RCLCPP_INFO(this->get_logger(), "I heard: [%s]", msg.data.c_str());
```

上述代码由 `rclcpp::Subscription` 类实例化的 `sub_` 对象调用 `take(msg, msg_info)`。调用发生在定时器触发的回调函数中。`msg` 和 `msg_info` 分别表示消息正文及其元数据。如果调用 `take(msg, msg_info)` 时订阅队列中有消息，该消息就会复制到 `msg` 中。
如果成功从订阅中获取消息，`take(msg, msg_info)` 会返回 `true`。此时，上述代码通过 `RCLCPP_INFO` 打印消息中的字符串数据。
如果未能从订阅中获取消息，`take(msg, msg_info)` 会返回 `false`。
调用 `take(msg, msg_info)` 时，如果订阅队列容量大于 1，且队列中有两条或更多消息，则最旧的消息会复制到 `msg` 中。如果队列容量为 1，则总是获取最新消息。

!!! note

    可以根据 `take()` 方法的返回值检查是否有消息到达。但必须注意 take() 方法会改变数据：`take()` 会修改订阅队列，而且该操作不可逆，没有对应的撤销操作。仅使用 `take()` 检查到达消息时，总会改变订阅队列。如果希望在不改变订阅队列的情况下检查消息，建议使用 rclcpp::WaitSet。详情请参阅[_【补充】使用 rclcpp::WaitSet_](./supp-wait_set.md)。

!!! note

    `take()` 方法仅支持获取通过 DDS 进行进程间通信的消息。不得将其用于进程内通信，因为进程内通信基于 `rclcpp` 的另一套软件实现。进程内通信的情况请参阅[_【补充】获取通过进程内通信接收的消息_](./supp-intra-process-comm.md)。

<a id="11-obtain-serialized-message-from-subscription"></a>

#### 1.1 从订阅中获取序列化消息

ROS 2 提供序列化消息功能，支持任意消息类型的通信，详见 [_SerializedMessage 类_](http://docs.ros.org/en/humble/p/rclcpp/generated/classrclcpp_1_1SerializedMessage.html)。Autoware 中的 `topic_state_monitor` 使用了此功能。
要从订阅中获取基于 `rclcpp::SerializedMessage` 的消息，必须使用 `take_serialized()` 方法，而非 `take()` 方法。

以下是 [_ros2_subscription_examples/simple_examples/src/timer_listener_serialized_message.cpp_](https://github.com/takam5f2/ros2_subscription_examples/blob/main/simple_examples/src/timer_listener_serialized_message.cpp) 中的示例代码片段。

```c++
      // receive the serialized message.
      rclcpp::MessageInfo msg_info;
      auto msg = sub_->create_serialized_message();

      if (sub_->take_serialized(*msg, msg_info) == false) {
        return;
      }
```

上述代码通过 `create_serialized_message()` 创建 `msg` 来存储接收的消息，其类型为 `std::shared_ptr<rclcpp::SerializedMessage>`。可以使用 `take_serialized()` 方法获取类型为 `rclcpp::SerializedMessage` 的消息。请注意，`take_serialized()` 的第一个参数需要引用类型的数据。由于 `msg` 是指针，因此应将 `*msg` 作为第一个参数传给 `take_serialized()。

!!! note

    ROS 2 的 `rclcpp` 同时支持 `rclcpp::LoanedMessage` 和 `rclcpp::SerializedMessage`。如果 Autoware 引入[_通过借用消息实现零拷贝通信_](https://design.ros2.org/articles/zero_copy.html)，则借用消息通信应改用 `take_loaned()` 方法。由于目前（2024 年 5 月）Autoware 尚未使用该方法，本文省略对 `take_loaned()` 的说明。

<a id="2-obtain-multiple-data-stored-in-subscription-queue"></a>

### 2. 获取订阅队列中存储的多条数据

如果在 QoS 配置中将队列容量设为多条消息，订阅对象就可以在队列中保存多条消息。传统回调方式要求每条消息都执行一次回调函数。换句话说，它有一个限制：回调函数的一次执行只能处理一条消息。请注意，在传统方式下，只要订阅队列中还有消息，就会取出最旧的一条，并分配线程执行回调函数，直到队列为空。
`take()` 方法可以缓解这一限制。它可以在循环中多次调用，使回调函数在一次执行期间处理多条通过 `take()` 获取的消息。

以下示例代码来自 [_ros2_subscription_examples/simple_examples/src/timer_batch_listener.cpp_](https://github.com/takam5f2/ros2_subscription_examples/blob/main/simple_examples/src/timer_batch_listener.cpp)，它在回调函数的一次执行期间调用 `take()` 方法。

```c++
      std_msgs::msg::String msg;
      rclcpp::MessageInfo msg_info;
      while (sub_->take(msg, msg_info))
      {
        RCLCPP_INFO(this->get_logger(), "Catch message");
        RCLCPP_INFO(this->get_logger(), "I heard: [%s]", msg.data.c_str());
```

上述代码中的 `while(sub->take(msg, msg_info))` 会持续从订阅队列中获取消息，直到队列为空。每次迭代处理一条获取的消息。
请注意，确定订阅队列容量时，必须同时考虑回调函数的执行频率和消息接收频率。例如，如果回调函数以 10Hz 执行，而话题消息以 50Hz 接收，则订阅队列容量至少应为 5，以免丢失接收到的消息。

为每条消息分配线程执行回调函数会带来性能开销。可以使用本节介绍的方式避免这种不必要的开销。
当接收频率与使用频率相差较大时，这种方式尤为有效。例如，即使 CAN 消息等以超过 100 Hz 的频率到达，用户逻辑也可能仅以 10 Hz 等较低频率使用消息。此时，用户逻辑应通过 `take()` 方法获取所需数量的消息，以避免不必要的开销。

<a id="3-obtain-data-by-calling-subscription-take-and-then-call-a-callback-function"></a>

### 3. 调用 `Subscription->take` 获取数据后，再调用回调函数

可以将 `take()`（严格来说是 `take_type_erased()`）方法与回调函数结合，以一致的方式处理接收的消息。这种组合方式无须唤醒线程。
以下是 [_ros2_subscription_examples/simple_examples/src/timer_listener_using_callback.cpp_](https://github.com/takam5f2/ros2_subscription_examples/blob/main/simple_examples/src/timer_listener_using_callback.cpp) 中的示例代码片段。

```c++
      auto msg = sub_->create_message();
      rclcpp::MessageInfo msg_info;
      if (sub_->take_type_erased(msg.get(), msg_info)) {
        sub_->handle_message(msg, msg_info);

```

上述代码先通过 `take_type_erased()` 获取消息，再通过 `handle_message()` 调用已注册的回调函数。请注意，必须使用 `take_type_erased()`，而非 `take()`。`take_type_erased()` 的第一个参数需要 `void` 类型数据。必须使用 `get()` 方法，将类型为 `shared_ptr<void>` 的 `msg` 转换为 `void` 类型。随后使用获取的消息调用 `handle_message()`，已注册的回调函数会在 `handle_message()` 内部调用。
无须关心传给 `take_type_erased()` 和 `handle_message()` 的消息类型。可以将消息变量定义为 `auto msg = sub_->create_message();`。
关于 `create_message()`、`take_type_erased()` 和 `handle_message()`，也可参阅 [_API 文档_](http://docs.ros.org/en/humble/p/rclcpp/generated/classrclcpp_1_1SubscriptionBase.html#_CPPv4N6rclcpp16SubscriptionBase16take_type_erasedEPvRN6rclcpp11MessageInfoE)。

<a id="4-obtain-data-by-a-callback-function"></a>

### 4. 通过回调函数获取数据

仍可使用 ROS 2 应用中常见的传统方式，即通过回调函数访问消息。如果没有使用设置了 `automatically_add_to_executor_with_node = false` 的回调组，那么在收到话题消息时，执行器会自动调用已注册的回调函数。
这种方式的优点之一是无须关心话题消息通过进程间通信还是进程内通信传递。请记住，`take()` 只能用于通过 DDS 的进程间通信，而通过 `rclcpp` 的进程内通信需要使用 `rclcpp` 提供的另一种方式。

<a id="appendix"></a>

## 附录

许多 ROS 2 应用使用回调函数获取话题消息，这似乎已成为一种规则或惯例。如本文所述，可以使用 `Subscription->take()` 方法获取话题消息，而不调用订阅回调函数。
[_Subscription 模板类 — rclcpp 16.0.8 文档_](https://docs.ros.org/en/humble/p/rclcpp/generated/classrclcpp_1_1Subscription.html#_CPPv4N6rclcpp12Subscription4takeER14ROSMessageTypeRN6rclcpp11MessageInfoE)也介绍了这种方式。

许多 ROS 2 用户可能不敢使用 `take()`，因为对它不够熟悉，相关文档也较少。但它在 `rclcpp::Executor` 实现中得到了广泛使用，如下方 [_rclcpp/executor.cpp_](https://github.com/ros2/rclcpp/blob/47c977d1bc82fc76dd21f870bcd3ea473eca2f59/rclcpp/src/rclcpp/executor.cpp#L643-L648) 所示。因此，无论你是否知晓，其实都在间接使用 `take()` 方法。

```c++
    std::shared_ptr<void> message = subscription->create_message();
    take_and_do_error_handling(
      "taking a message from topic",
      subscription->get_topic_name(),
      [&]() {return subscription->take_type_erased(message.get(), message_info);},
      [&]() {subscription->handle_message(message, message_info);});
```

!!!note

    严格来说，执行器调用的是 `take_type_erased()`，而不是 `take()`。

    但 `take_type_erased()` 是 `take()` 的具体实现，`take()` 会在内部调用 `take_type_erased()`。

如果基于 `rclcpp::Executor` 的对象（执行器）被编程为调用回调函数，则由执行器自身决定调用时机。由于执行器本质上以尽力而为的方式调用回调函数，即使消息已接收，也不能保证一定被访问或处理。因此，为确保在预期时刻访问或处理消息，最好直接调用 `take()` 方法。

---

截至 2024 年 5 月，Autoware Universe 已开始采用这些推荐方式。
如需查看 Autoware Universe 中的示例，请参阅以下 PR。

[_feat(tier4_autoware_utils, obstacle_cruise): change to read topic by polling #6702_](https://github.com/autowarefoundation/autoware_universe/pull/6702)
