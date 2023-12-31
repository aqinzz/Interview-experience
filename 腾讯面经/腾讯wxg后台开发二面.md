> 来源：https://www.nowcoder.com/discuss/523972047945334784

# 腾讯wxg后台开发二面

## 数据库

### 1、MySQL索引数据结构？

1. B-Tree 索引：
   - 优点：B-Tree 索引适用于等值查询、范围查询和排序操作，并且在大多数情况下都能提供良好的性能。
   - 适用场景：适用于大多数查询需求，是 MySQL 默认的索引类型。特别适合于需要按顺序遍历索引值的情况。
2. 哈希索引：
   - 优点：哈希索引对等值查询具有很高的查询速度，适合于对单个列进行快速查找。
   - 缺点：不支持范围查询、排序操作和模糊查询，且只能用于相等比较。
   - 适用场景：适用于需要快速查找特定值的场景，如在内存中进行高速查找。
3. 全文索引：
   - 优点：全文索引用于对文本数据进行全文搜索，支持对文本内容进行模糊匹配和自然语言搜索。
   - 适用场景：适用于需要对文本内容进行全文搜索的场景，如博客系统、论坛等。
4. 空间索引：
   - 优点：空间索引用于对空间数据类型（如地理位置信息）进行快速查询和分析。
   - 适用场景：适用于需要处理地理信息系统（GIS）数据的应用，如地图应用、位置服务等。
5. 前缀索引：
   - 优点：前缀索引适用于对较长的列进行索引，可以节省存储空间和提高查询效率。
   - 适用场景：适用于需要对长文本或大字段进行索引的情况，例如对文章标题进行索引。
6. 复合索引：
   - 优点：复合索引可以通过将多个列组合在一起创建索引来加速多列条件查询。
   - 适用场景：适用于需要在多个列上进行联合查询的情况，可以提高查询效率。

### 2、二叉树和平衡二叉树的区别？

二叉树和平衡二叉树是两种常见的树形数据结构，它们之间的主要区别在于平衡性。

1. 二叉树：
   - 二叉树是一种树形数据结构，其中每个节点最多有两个子节点，分别称为左子节点和右子节点。
   - 二叉树并没有强制要求节点的左右子树高度相等或者相差不大，因此它的形状可以是非常不规则的。
2. 平衡二叉树：
   - 平衡二叉树是一种特殊的二叉树，它要求任意节点的左右子树的高度差不超过1。
   - 平衡二叉树的设计目的是为了保持树的高度平衡，这样可以确保在最坏情况下的查找、插入和删除操作的时间复杂度为O(log n)，其中n是树中节点的数量。

### 3、平衡二叉树的优点？

1. 快速的查找、插入和删除操作：平衡二叉树的高度始终保持在较低水平，因此在最坏情况下，查找、插入和删除操作的时间复杂度为O(log n)，其中n是树中节点的数量。这使得平衡二叉树非常适合需要频繁进行这些操作的场景，如数据库索引。
2. 稳定的性能：平衡二叉树保持了树的平衡性，因此在不同情况下的性能表现比较稳定。不像普通的二叉树，可能出现极端情况下退化成链表的情况，导致操作的时间复杂度变为O(n)。
3. 适用于有序数据：平衡二叉树对于有序数据的插入、删除和查找操作的性能表现良好，因为它能够保持树的平衡，不会出现极端的情况。
4. 适用于高效存储和检索：平衡二叉树通常被用于需要高效存储和检索数据的场景，如数据库索引、有序集合等。

### 4、场景题：1000万的数据，想存到MySQL里，要求查询尽可能快，怎么设计？

假设有一个需求是要存储一个社交网络平台的用户信息，包括用户ID、用户名、年龄、性别等信息，并且要求能够快速查询用户信息。

1. 选择合适的数据表引擎： 鉴于数据量较大且需要快速查询，我们选择使用MySQL的InnoDB引擎，因为它支持事务、行级锁定，并且在大数据量下的查询性能比较好。
2. 设计用户信息表： 我们设计一个名为`users`的数据表，包含以下字段：
   - `user_id`：用户ID，主键，使用自增长方式生成。
   - `username`：用户名，唯一索引。
   - `age`：年龄。
   - `gender`：性别。
   - 其他可能需要的用户信息字段。
3. 添加合适的索引： 对于经常用于查询的字段，我们添加合适的索引以提高查询速度。在这个例子中，我们可以为`username`字段添加唯一索引，以及根据需要为其他经常用于查询的字段添加索引。
4. 合理的分区与分表： 如果用户数据量较大，可以考虑根据用户ID范围进行分区，或者根据用户注册时间进行分表存储，以减少单表的数据量，提高查询效率。
5. 适当的缓存策略： 对于用户信息这类经常被查询的数据，可以考虑使用缓存来提高查询性能。例如，可以使用Redis等内存数据库作为查询结果的缓存，减少对数据库的访问。
6. 优化查询语句： 编写高效的查询语句也是提高查询速度的关键。可以通过分析查询语句的执行计划，优化索引的使用，避免全表扫描等方式来优化查询性能。

### 5、redis的使用场景？

1. 缓存：Redis常用于缓存常用数据，如数据库查询结果、API调用结果等。由于Redis存储在内存中，读取速度非常快，适合于需要快速读取的场景，可以有效减轻后端数据库或其他数据源的压力。
2. 会话缓存：在Web应用中，可以使用Redis来存储用户会话数据。由于Redis的高速读写能力，可以提高会话管理的效率，并且支持设置过期时间，方便实现会话的自动过期与续期。
3. 消息队列：Redis的列表数据结构可以用作简单的消息队列，用于实现发布/订阅模式或者任务队列。生产者可以将消息推送到列表中，消费者则可以从列表中读取消息进行处理，实现了简单的消息传递机制。
4. 计数器：Redis的自增命令可以用于实现计数器功能，例如网站访问量、点赞数等统计。由于自增操作是原子的，可以确保计数的准确性。
5. 分布式锁：Redis的SETNX命令可以用于实现分布式锁，通过在Redis中设置一个键值对来控制资源的访问，保证在分布式环境下的数据一致性。
6. 实时排行榜：利用Redis的有序集合数据结构，可以实现实时排行榜功能。通过将用户的分数作为有序集合的分数，用户ID作为成员，可以轻松地实现排行榜的增删改查操作。
7. 地理位置应用：Redis的地理位置数据类型可以用于存储地理位置信息，例如经纬度坐标。通过这些数据结构，可以实现附近的人、附近的店铺等地理位置相关的应用。
8. 发布/订阅：Redis提供了强大的发布/订阅功能，可以用于实现实时消息推送、事件通知等功能，非常适合于需要实时更新数据的场景。

### 6、缓存雪崩的解决方案？

缓存雪崩是指缓存中大量的数据同时失效或者同时被清空，导致大量的请求直接打到数据库或者后端服务，引起服务的崩溃。

给出以下几种解决方案：

1. 缓存数据的过期时间随机化：如果缓存数据的过期时间设置得太过集中，可能会导致大量的缓存同时过期，引发雪崩。可以在设置缓存数据的过期时间时，加入一定的随机因素，使得过期时间分散开来，减少同时失效的可能性。
2. 使用分布式锁：在缓存失效时，可以通过使用分布式锁来保证只有一个线程去加载数据并更新缓存，其他线程等待该线程完成后再从缓存中获取数据。这样可以避免大量请求同时打到数据库。
3. 缓存预热：在系统启动或者缓存失效之前，可以提前将热门数据加载到缓存中，避免缓存失效后大量请求同时打到数据库。
4. 限流降级：当缓存失效后，可以通过限流降级的方式，控制请求的并发量，避免数据库被大量请求压垮。可以使用类似于Hystrix的熔断器来实现请求的限流和降级。
5. 多级缓存：可以使用多级缓存架构，例如将热点数据放在内存中的本地缓存或者使用Redis等内存数据库，将冷数据放在持久化存储中。这样可以分散缓存失效的风险，提高系统的稳定性。
6. 异步加载数据：可以将缓存失效后的数据加载操作放在异步任务中进行，避免在缓存失效时同步加载数据导致的并发访问压力。
7. 限制并发请求：可以通过限制并发请求的方式，控制缓存失效时的并发访问量，避免请求过多导致系统崩溃。

### 7、出现了缓存被打穿了，大量请求压到后端，服务自我保护机制有哪些？

当缓存被击穿时，即某个热点数据缓存失效，大量请求直接打到后端数据库或者服务，可能会对系统造成较大压力，为了保护后端服务，可以采取以下自我保护机制：

1. 熔断器：使用熔断器来监控后端服务的状态，当服务出现异常或者超时时，熔断器会打开，暂时阻断对该服务的请求，避免对后端服务造成更大的压力。
2. 限流：对请求进行限流，限制单位时间内请求的数量或者速率，防止过多的请求打到后端服务，可以通过限制请求的并发量来保护后端服务。
3. 降级：当系统负载过高时，可以通过降级策略临时关闭某些非关键功能或者服务，减轻系统压力，保证核心功能的正常运行。
4. 热点数据预热：对于可能成为热点的数据，可以在缓存失效前提前将其加载到缓存中，避免缓存失效时大量请求打到后端服务。
5. 异步加载数据：对于缓存失效时需要重新加载的数据，可以采用异步加载的方式，将数据加载操作放在异步任务中进行，避免在缓存失效时同步加载数据导致的并发访问压力。
6. 缓存穿透处理：针对缓存穿透问题（即查询不存在的数据），可以使用布隆过滤器等技术进行过滤，避免无效的查询直接打到后端服务。
7. 多级缓存：使用多级缓存架构，将热点数据放在内存中的本地缓存或者使用Redis等内存数据库，将冷数据放在持久化存储中，分散缓存失效的风险，提高系统的稳定性。

## Kafka

### 8、Kafka的架构？

Kafka是一个分布式流处理平台，最初由LinkedIn开发，并于2011年开源。它被设计用来处理实时数据流，具有高性能、高吞吐量、可扩展性等特点。.

Kafka的架构主要包括以下几个组件：

1. Producer（生产者）：Producer负责向Kafka集群发送消息。它将消息发布到Kafka的Topic中，每个消息包含一个key-value对。
2. Broker（代理）：Broker是Kafka集群中的节点，用于存储消息。一个Kafka集群由多个Broker组成，每个Broker都负责一部分数据的存储和处理。
3. Topic（主题）：Topic是消息的逻辑容器，生产者将消息发布到Topic中，消费者从Topic中订阅消息。Topic可以分区，每个分区可以在集群中的不同Broker上进行复制，以提高容错性和可用性。
4. Partition（分区）：Topic可以分成多个分区，每个分区在物理上对应一个文件，用于存储消息数据。分区可以水平扩展，允许Kafka集群处理大量数据，并且可以提高并发处理能力。
5. Consumer Group（消费者组）：消费者组是消费者的逻辑集合。每个消费者组可以包含多个消费者，每个消费者负责消费一个或多个分区的消息。消费者组可以提高消费者的扩展性和容错性。
6. ZooKeeper：ZooKeeper是Kafka集群的协调者，用于管理和维护Kafka集群的元数据、领导者选举、健康检查等工作。Kafka依赖ZooKeeper来进行集群管理和协调。

Kafka的工作流程：

- 生产者将消息发布到指定的Topic中。
- 消费者通过消费者组订阅Topic中的消息。
- 消费者组中的消费者从Topic的分区中拉取消息并进行处理。
- 消息被存储在Broker中的分区中，可以根据需要进行复制和备份。

### 9、Kafka消费模式？

1. 单消费者模式：
   - 在单消费者模式中，一个消费者实例订阅一个或多个分区的消息。
   - 每个分区的消息只会被单个消费者实例处理，这意味着一个分区内的消息是按顺序处理的。
   - 单消费者模式适用于简单的应用场景，例如只有一个消费者需要处理所有消息的情况。
2. 消费者组模式：
   - 在消费者组模式中，多个消费者实例组成一个消费者组，每个消费者实例订阅一个或多个分区的消息。
   - Kafka 会确保同一个分区的消息只会被消费者组内的一个消费者实例处理，这样可以实现消息的负载均衡和并行处理。
   - 消费者组模式适用于需要水平扩展和高吞吐量的场景，例如大规模数据处理和实时流处理应用。
3. 广播消费模式：
   - 广播消费模式是一种特殊的消费模式，每个消费者实例都会收到所有分区的消息。
   - 这种模式适用于需要所有消费者实例都处理所有消息的场景，例如日志订阅等。
4. 精确一次消费模式：
   - 精确一次消费模式是一种高级的消费模式，保证每条消息只会被消费一次，不会重复消费也不会丢失。
   - 这种模式通常需要结合事务和幂等性等机制来实现，适用于对消息处理的一致性要求较高的场景。

### 10、Kafka消息积压？

Kafka消息积压是指由于某种原因导致消息在Kafka中堆积积累，未能及时被消费处理的情况。消息积压可能会导致消费者无法及时处理消息，从而影响系统的性能和稳定性。

1. 消费者处理能力不足：
   - 原因：消费者的处理能力不足，无法及时处理消息。
   - 解决方案：增加消费者实例数量，提高消费者的处理能力；优化消费者的处理逻辑，提高消费者的效率。
2. 消费者组中消费者数量不均衡：
   - 原因：消费者组中的消费者实例处理能力不均衡，导致消息在分区中积压。
   - 解决方案：监控消费者组中各个消费者实例的状态，根据负载情况调整消费者实例的数量和分配，确保消费者组中的消费者数量均衡。
3. 消息生产速率过高：
   - 原因：消息的生产速率超过了消费者的处理速率。
   - 解决方案：优化生产者的生产速率，调整生产者发送消息的频率，或者增加消费者的处理能力，确保消费者能够及时处理生产者发送的消息。
4. 消费者处理失败未及时处理：
   - 原因：消费者在处理消息时发生错误或者失败，但是没有及时处理失败的消息。
   - 解决方案：监控消费者的处理状态，及时发现并处理消费者的错误或者失败，确保消费者能够正常工作。
5. 网络或者硬件故障：
   - 原因：Kafka 集群中的网络或者硬件出现故障，导致消息无法正常发送或者消费。
   - 解决方案：及时发现并修复网络或者硬件故障，确保 Kafka 集群的稳定性和可用性。

### 11、RocketMQ和Kafka的区别

1. 架构模型：
   - Kafka采用发布-订阅（Publish-Subscribe）模型，消息由Producer发送到Topic，然后由Consumer订阅Topic并消费消息。
   - RocketMQ也采用发布-订阅模型，消息也是由Producer发送到Topic，然后由Consumer订阅Topic并消费消息。
2. 消息顺序保证：
   - 在 Kafka 中，同一个分区内的消息是有序的，但不同分区之间的消息不保证顺序。
   - 在 RocketMQ 中，同一个队列（Queue）内的消息是有序的，不同队列之间的消息是并行的，RocketMQ 提供了顺序消息的支持。
3. 消息存储：
   - Kafka使用文件存储消息，消息被持久化到磁盘，并且支持消息的持久化和复制。
   - RocketMQ 也使用文件存储消息，消息被持久化到磁盘，并且支持消息的持久化和复制。
4. 消息推送方式：
   - Kafka采用基于时间的拉取方式（Pull），Consumer可以根据自己的速率拉取消息。
   - RocketMQ 采用基于推送的方式（Push），Broker会将消息推送给Consumer，Consumer需要保持长连接以接收消息。
5. 社区支持和发展：
   - Kafka 是 Apache 软件基金会的顶级项目，拥有活跃的社区和广泛的用户群体。
   - RocketMQ 也是 Apache 软件基金会的顶级项目，社区规模较小，但在中国有着较为广泛的用户群体。
6. 适用场景：
   - Kafka 适用于大规模数据流处理和实时数据管道等场景，如日志收集、事件处理等。
   - RocketMQ 适用于高吞吐量、低延迟的消息传递场景，如电商交易、金融支付等。

### 12、垃圾回收机制

垃圾回收（Garbage Collection，GC）是指一种自动管理内存的机制，用于在程序运行过程中识别并释放不再使用的内存空间，从而避免内存泄漏和提高内存利用率。

不同的编程语言和运行时环境可能会有不同的垃圾回收机制，下面简要介绍几种常见的垃圾回收机制及其特点：

1. 标记-清除：
   - 标记-清除是一种较为传统的垃圾回收算法，在执行时会遍历所有的内存对象，并标记出所有活跃的对象。然后，清除所有未标记的对象，即认为这些对象是不再使用的，可以被释放。
   - 优点：简单直观，能够有效地回收不再使用的内存。
   - 缺点：会产生内存碎片，可能导致内存分配效率降低。
2. 复制：
   - 复制算法将内存空间分为两个区域，每次只使用其中一个区域。当一个区域的内存占满时，将活跃对象复制到另一个区域中，然后清除原区域中的所有对象。
   - 优点：避免了内存碎片问题，提高了内存分配的效率。
   - 缺点：需要额外的内存空间来存储复制后的对象，可能会增加内存消耗。
3. 标记-整理：
   - 标记-整理算法结合了标记-清除和复制算法的优点，在标记阶段标记出活跃对象后，会将它们整理到内存的一端，然后清除掉整理后的内存区域。
   - 优点：避免了内存碎片问题，减少了内存空间的浪费。
   - 缺点：需要移动对象，可能会增加处理时间。
4. 分代：
   - 分代垃圾回收算法根据对象的存活时间将内存划分为不同的代（Generation），通常将内存分为新生代和老年代。新生代中的对象存活时间较短，老年代中的对象存活时间较长。
   - 优点：根据对象的特性进行不同的回收策略，提高了垃圾回收的效率。
   - 缺点：需要根据对象的存活特性进行适当的分代划分，不同的应用场景可能需要调整不同的分代比例和回收策略。
5. 增量：
   - 增量垃圾回收算法将垃圾回收过程分解成多个阶段，每个阶段只完成部分垃圾回收工作，与应用程序交替执行。
   - 优点：减少了单次垃圾回收的停顿时间，提高了系统的响应性。
   - 缺点：增加了垃圾回收器的复杂性，可能会导致一定的性能损失。

### 13、如何避免内存碎片问题？

内存碎片问题是指内存中存在大量不连续的小块空闲内存，这些小块空闲内存虽然总和足够大，但无法满足大块连续内存的需求，从而导致内存利用率降低、内存分配效率下降的情况。

为了避免内存碎片问题，可以采取以下几种策略：

1. 内存池管理：内存池管理是一种常见的解决内存碎片问题的方法。通过预先分配一定数量的内存块，并在需要时从内存池中分配，可以避免频繁的内存分配和释放操作，从而减少内存碎片的产生。
2. 内存块合并：当有大块内存被释放时，可以尝试将相邻的小块空闲内存合并成更大的内存块，以减少内存碎片的产生。
3. 内存分配算法优化：选择合适的内存分配算法也可以减少内存碎片问题。例如，首次适应算法（First Fit）和最佳适应算法（Best Fit）可以尽量利用小块空闲内存，减少内存碎片的产生。
4. 内存整理：在某些情况下，可以通过内存整理操作来减少内存碎片。内存整理会将内存中的对象重新排列，使得空闲内存块集中在一起，从而减少碎片化。
5. 动态内存分配优化：在使用动态内存分配的情况下，可以通过一些技术手段来优化内存分配，如对象池、复用对象等方式，减少频繁的内存分配和释放操作。
6. 选择合适的数据结构：合适的数据结构可以减少内存碎片问题的产生。例如，使用链表等非连续存储结构可以避免因为内存碎片导致的连续内存分配问题。

## 算法

### 14、手撕1：回文串

#### 问题描述：

给你一个字符串 `s`，找到 `s` 中最长的回文子串。

如果字符串的反序与原始字符串相同，则该字符串称为回文字符串。

#### 思路：

以字符串"babad"为例。

1. 初始化状态：
   - 首先创建一个二维数组 `dp`，其大小为 `n x n`（`n` 为字符串长度），并初始化所有元素为 `false`。
   - 对于长度为 1 的子串，即 `dp[i][i]`，将对应位置的元素设为 `true`，因为单个字符肯定是回文串。
2. 状态转移：
   - 接下来从长度为 2 的子串开始，逐步扩展到长度为 `n` 的子串，计算 `dp[i][j]` 的值。
   - 对于每个长度为 `len` 的子串，枚举起始位置 `i`，计算结束位置 `j = i + len - 1`。
   - 如果 `s[i] == s[j]` 且 `dp[i+1][j-1]` 为 `true`，则说明去掉头尾两个字符后的子串是回文串，即 `dp[i][j] = true`。
3. 记录最长回文子串：
   - 在状态转移的过程中，记录下最长的回文子串的起始位置和长度。
   - 每次更新 `dp[i][j]` 为 `true` 时，更新起始位置 `start = i` 和最大长度 `maxLen = len`。
4. 返回结果：
   - 最后根据记录的起始位置 `start` 和最大长度 `maxLen`，使用 `substr` 方法从原始字符串中取出最长回文子串并返回。

给个表格帮助大家理解：

![image-20231228002424008](E:\GitHub\Interview-experience\腾讯面经\assets\image-20231228002424008.png)

表格中，对角线上的格子都表示长度为 1 的子串，因为单个字符肯定是回文串，所以都标记为 `true`。

然后我们开始计算长度为 2 的子串，例如 `ba` 和 `ab`，如果两个字符相同，则标记为 `true`，否则标记为 `false`。在这个例子中，`ba` 和 `ab` 都不是回文串，所以对应的格子都标记为 `false`。

接着我们计算长度为 3 的子串，例如 `bab` 和 `aba`，如果首尾两个字符相同并且去掉首尾字符的子串是回文串，则标记为 `true`，否则标记为 `false`。在这个例子中，`bab` 是回文串，所以对应的格子标记为 `true`，而 `aba` 也是回文串，所以对应的格子也标记为 `true`。

最后我们计算长度为 4 的子串，例如 `baba`，同样地，如果首尾两个字符相同并且去掉首尾字符的子串是回文串，则标记为 `true`，否则标记为 `false`。在这个例子中，`baba` 不是回文串，所以对应的格子标记为 `false`。

最终，我们可以根据这个表格得到最长的回文子串是 "bab"。

#### 参考代码：

```
#include <iostream>
#include <vector>
#include <string>
using namespace std;

string longestPalindrome(string s) {
    if (s.empty()) return "";
    int n = s.length();
    vector<vector<bool>> dp(n, vector<bool>(n, false));  // 定义二维动态规划数组
    int start = 0, maxLen = 1;  // 记录最长回文子串的起始位置和长度
    for (int i = 0; i < n; ++i) {
        dp[i][i] = true;  // 单个字符肯定是回文串
        if (i < n - 1 && s[i] == s[i + 1]) {
            dp[i][i + 1] = true;  // 相邻字符相同则是回文串
            start = i;
            maxLen = 2;
        }
    }
    for (int len = 3; len <= n; ++len) {  // 枚举子串长度
        for (int i = 0; i + len - 1 < n; ++i) {  // 枚举子串起始位置
            int j = i + len - 1;  // 子串结束位置
            if (s[i] == s[j] && dp[i + 1][j - 1]) {
                dp[i][j] = true;  // 根据状态转移方程计算 dp[i][j]
                start = i;
                maxLen = len;
            }
        }
    }
    return s.substr(start, maxLen);  // 返回最长回文子串
}

int main() {
    string s = " ";
    std::cout << "输入字符串：";
    std::cin >> s;
    std::cout << longestPalindrome(s) << std::endl;  // 输出最长回文子串
    return 0;
}
```

### 15、手撕2：轮转数组

#### 问题描述：

给定一个整数数组 `nums`，将数组中的元素向右轮转 `k` 个位置，其中 `k` 是非负数。

#### 思路：

1. 定义 `reverse` 函数：
   - 首先定义一个 `reverse` 函数，它接受一个整数数组 `nums`、起始索引 `start` 和结束索引 `end` 作为参数，用于翻转数组 `nums` 中从索引 `start` 到索引 `end`（不包括 `end`）的部分。
2. 整体翻转数组：
   - 在 `rotate` 函数中，首先对整个数组进行翻转操作，即调用 `reverse(nums, 0, n)`，其中 `n` 是数组 `nums` 的长度。这一步的目的是将整个数组颠倒过来，以便后续的操作能够正确地进行。
3. 翻转前 k 个元素：
   - 接下来，我们对数组中的前 k 个元素进行翻转操作，即调用 `reverse(nums, 0, k)`。这一步的目的是将前 k 个元素颠倒过来，使它们成为数组的末尾部分。
4. 翻转剩余的 n-k 个元素：
   - 最后，我们对数组中剩余的 n-k 个元素进行翻转操作，即调用 `reverse(nums, k, n)`，其中 `n` 是数组 `nums` 的长度。这一步的目的是将剩余的 n-k 个元素颠倒过来，使它们成为数组的前半部分。
5. 完成右移：
   - 经过以上三步操作，我们就完成了数组的循环右移。最终数组 `nums` 中的元素顺序就是我们期望的结果。

#### 参考代码：

```
#include <iostream>
#include <vector>

// 翻转数组的一部分，从 start 到 end（不包括 end）
void reverse(std::vector<int>& nums, int start, int end) {
    while (start < end) {
        // 交换 start 和 end 位置的元素
        int temp = nums[start];
        nums[start] = nums[end - 1];
        nums[end - 1] = temp;
        // 更新 start 和 end 的位置
        start++;
        end--;
    }
}

class Solution {
public:
    void rotate(std::vector<int>& nums, int k) {
        int n = nums.size();
        k %= n; // 如果 k 大于数组长度，取余数

        // 整体翻转数组
        reverse(nums, 0, n);
        // 翻转前 k 个元素
        reverse(nums, 0, k);
        // 翻转剩余的 n-k 个元素
        reverse(nums, k, n);
    }
};

int main() {
    Solution solution;
    std::vector<int> nums;

    int size;
    std::cout << "Enter the size of the array: ";
    std::cin >> size;
    
    std::cout << "Enter " << size << " integers separated by spaces: ";
    for (int i = 0; i < size; ++i) {
        int num;
        std::cin >> num;
        nums.push_back(num);
    }

    int k;
    std::cout << "Enter the value of k: ";
    std::cin >> k;

    solution.rotate(nums, k);

    std::cout << "Rotated array: ";
    for (int num : nums) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

