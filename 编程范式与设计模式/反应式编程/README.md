# 反应式编程

# Backpressure（反压）

首先，Backpressure 并不是响应式编程（Reactive Programming，或者有的人喜欢按字直译为「反应式编程」）独有的；其次，Backpressure 并不是一种「机制」，也不是一种「策略」。Backpressure 其实是一种现象：在数据流从上游生产者向下游消费者传输的过程中，上游生产速度大于下游消费速度，导致下游的 Buffer 溢出，这种现象就叫做 Backpressure 出现。

编程中的 Backpressure 这个概念源自工程概念中的 Backpressure：在管道运输中，气流或液流由于管道突然变细、急弯等原因导致由某处出现了下游向上游的逆向压力，这种情况称作「back pressure」。Backpressure 和 Buffer 是一对相生共存的概念，只有设置了 Buffer，才有 Backpressure 出现；只要设置了 Buffer，一定存在出现 Backpressure 的风险。

例如你是开发服务器后端的，有一个 Socket 不断地接收来自用户的 http 请求来把用户需要的网页返回给用户。你的服务器所能承受的同时访问用户数是有上限的吧？比如说，你的服务器主机的处理器和内存情况决定了，它最多只能承受 5000~6000 个用户同时访问，再多的话服务器就有当掉的风险了。那么你决定：把用户数上限设置为 5000，当超出 5000 用户数的时候，再有新的访问就把它丢弃或者拒绝。那么对于这个案例，5000 就是你对于用户访问数设置的 Buffer；第 5001 个用户的访问，就叫做造成了 Backpressure 的产生；而你的「丢弃或拒绝」的行为，就是对于 Backpressure 的处理。

生产速度大于消费速度，所以需要 Buffer；外部条件有限制，所以 Buffer 需要有上限；Buffer 达到上限这个现象，有一个简化的等价词叫做 Backpressure；Backpressure 的出现其实是一种危险边界，唯一的选择是丢弃新事件。总而言之，Backpressure 指的是在 Buffer 有上限的系统中，Buffer 溢出的现象；它的应对措施只有一个：丢弃新事件。Backpressure 只是一种现象，而不是一种机制；至于你说的 throttleFirst、debounce ，更不是某个机制中的一环，它们只是可以通过人为过滤的方式来降低生产速度，从而降低 Backpressure 出现的几率罢了。
