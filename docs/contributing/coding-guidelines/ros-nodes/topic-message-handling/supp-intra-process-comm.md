<a id="supplement-obtain-a-received-message-through-intra-process-communication"></a>

# 【补充】获取通过进程内通信接收的消息

<a id="topic-message-handling-in-intra-process-communication"></a>

## 进程内通信中的话题消息处理

`rclcpp` 支持进程内通信。如[_话题消息处理指南_](index.md)所述，进程内通信不能使用 `take()` 方法。`take()` 无法返回通过进程间通信接收的话题消息。  
不过，系统提供了用于进程内通信的方法，与[_调用 Subscription->take 获取数据后，再调用回调函数_](./index.md#3-obtain-data-by-calling-subscription-take-and-then-call-a-callback-function)中介绍的进程间通信方法类似。
在进程内通信中，使用 `take_data()` 方法获取接收到的数据，并且必须通过 `execute()` 方法处理这些数据。由于 `take_data()` 的返回值采用复杂的数据结构，应将 `execute()` 与 `take_data()` 配合使用。
有关 `take_data()` 和 `execute()` 的更多信息，请参阅 [_SubscriptionIntraProcess 模板类 — rclcpp 16.0.8 文档_](http://docs.ros.org/en/humble/p/rclcpp/generated/classrclcpp_1_1experimental_1_1SubscriptionIntraProcess.html#_CPPv4N6rclcpp12experimental24SubscriptionIntraProcess9take_dataEv)。

<a id="coding-manner"></a>

## 编码方式

要处理通过进程内通信传递的消息，请如下先调用 `take_data()`，再调用 `execute()`。

```c++
// Execute any entities of the Waitable that may be ready
std::shared_ptr<void> data = waitable.take_data();
waitable.execute(data);
```

示例程序位于 [_ros2_subscription_examples/intra_process_talker_listener/src/timer_listener_intra_process.cpp at main · takam5f2/ros2_subscription_examples_](https://github.com/takam5f2/ros2_subscription_examples/blob/main/intra_process_talker_listener/src/timer_listener_intra_process.cpp)。
可以按以下方式运行程序。如果将 `use_intra_process_comms` 设为 `true`，则执行进程内通信；如果设为 `false`，则执行进程间通信。

```console
ros2 intra_process_talker_listener talker_listener_intra_process.launch.py use_intra_process_comms:=true
```

以下是 [_ros2_subscription_examples/intra_process_talker_listener/src/timer_listener_intra_process.cpp at main · takam5f2/ros2_subscription_examples_](https://github.com/takam5f2/ros2_subscription_examples/blob/main/intra_process_talker_listener/src/timer_listener_intra_process.cpp) 中的代码片段。

```c++
      // check if intra-process communication is enabled.
      if (this->get_node_options().use_intra_process_comms()){

        // get the intra-process subscription's waitable.
        auto intra_process_sub = sub_->get_intra_process_waitable();

        // check if the waitable has data.
        if (intra_process_sub->is_ready(nullptr) == true) {

          // take the data and execute the callback.
          std::shared_ptr<void> data = intra_process_sub->take_data();

          RCLCPP_INFO(this->get_logger(), " Intra-process communication is performed.");

          // execute the callback.
          intra_process_sub->execute(data);
```

下面逐行解释上述代码。

- `if (this->get_node_options().use_intra_process_comms()){`
  - 此语句通过 `NodeOptions` 检查是否启用了进程内通信。

- `auto intra_process_sub = sub_->get_intra_process_waitable();`
  - 此语句获取执行进程内通信的具体对象。

- `if (intra_process_sub->is_ready(nullptr) == true) {`
  - 此语句检查是否已通过进程内通信接收到消息。
  - `is_ready()` 的参数类型为 `rcl_wait_set_t`，但由于该参数在 `is_ready()` 内部未被使用，因此目前传入 `nullptr`。
    - 使用 `nullptr` 目前只是一种临时处理方式，并无特殊含义。

- `std::shared_ptr<void> data = intra_process_sub->take_data();`
  - 此语句从用于进程内通信的订阅中获取话题消息。
  - `intra_process_sub->take_data()` 不会返回表示是否成功接收消息的布尔值，因此需要先调用 `is_ready()` 进行检查。

- `intra_process_sub->execute(data);`
  - 与接收消息对应的回调函数会在 `execute()` 内部调用。
  - 回调函数由调用 `execute()` 的线程执行，不发生上下文切换。
