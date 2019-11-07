# Disruptor

Disruptor 是一个类似于 Java 中 `BlockingQueue` 的高性能内存队列，相比于 `BlockingQueue`，Disruptor 提供了以下的关键特性：

- 支持广播事件（`BlockingQueue` 只支持单播事件）
- 支持为事件预分配内存
- 内部实现基于无锁并发算法（除了 `BlockingWaitStrategy` 和 `TimeoutBlockingWaitStrategy`）

## 核心概念

### Ring Buffer

`RingBuffer` 通常被认为是 Disruptor 的核心组件，但在 3.0 之后版中，`RingBuffer` 在 Disruptor 中已经被设计成只用于存储和更新数据的容器。

### Sequence

Disruptor 使用 `Sequence` 来标识 `RingBuffer` 中可用的数据段。每个消费者（`EventProcessor`）会维护一个自身的 `Sequence`。Disruptor 中无锁并发的具体实现依赖 `Sequence` 值的移动，`Sequence` 支持许多和 `AtomicLong` 类似的功能，但 `Sequence` 相比于 `AtomicLong` 而言，额外支持了「避免 [伪共享](https://www.cnblogs.com/cyfonly/p/5800758.html)」的特性。

### Sequencer

`Sequencer` 是 Disruptor 真正的核心组件。`Sequencer` 的两个实现类（`SingleProducerSequencer` 和 `MultiProducerSequencer`）实现了 Disruptor 在 Producer 和 Consumer 之间快速和正确地传递数据的所有并发算法。

### Sequence Barrier

`SequenceBarrier` 由 `Sequencer` 产生，并且包含了 `Sequencer` 发布的 `Sequence` 引用和这些 `Sequence` 所依赖的所有 Consumer。`SequenceBarrier` 实现了「是否有可消费的 Event 需要 Consumer 处理」的逻辑。

### Wait Strategy

`WaitStrategy` 决定了 Consumer 如何等待在 Disruptor 中由 Producer 发布的 Event。

### Event

Event 是由 Producer 传递给 Consumer 的数据单元。

### EventProcessor

`EventProcessor` 是处理来自 Disruptor 的 Event 的主要循环程序，并且会持有 Consumer 们的 `Sequence`。在 Disruptor 中，`BatchEventProcessor` 实现了 Event 的循环处理逻辑，并且会回调 `EventHandler` 接口的实现逻辑。

### EventHandler

实现 `EventHandler` 接口的类代表了 Disruptor 中的 Consumer。

### Producer

调用 `Disruptor#publishEvent(EventTranslator)` 的类代表了 Disruptor 中的 Producer。

Disruptor 的核心工作流程如下图：

![image](1.png)

---

参考资料：

- [Disruptor's Blog](http://lmax-exchange.github.io/disruptor/)
- [Disruptor's GitHub](https://github.com/LMAX-Exchange/disruptor)
- [高性能队列——Disruptor](https://tech.meituan.com/2016/11/18/disruptor.html)
- [Log4j 2 Asynchronous Loggers](https://logging.apache.org/log4j/2.x/manual/async.html)
- [伪共享，并发编程无声的性能杀手](https://www.cnblogs.com/cyfonly/p/5800758.html)
