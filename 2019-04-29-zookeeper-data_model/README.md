## The ZooKeeper Data Model
ZooKeeper 有一个类似于文件系统的层次化的命名空间，但有些不同的是，文件系统中的每个节点只能是文件或者文件夹，而 ZooKeeper 命名空间中的每个节点即可以拥有数据，又可以拥有子节点（好比于文件系统中的每个节点即可以是文件，又可以是文件夹）。

ZooKeeper 命名空间中的每个节点的路径都是规范的、绝对的、斜杠（/）分隔的地址，并且节点的路径不可以是相对地址。任意的 unicode 字符都可以在以下条件约束下，被用于表示节点的路径：
* `\u0000`（null 字符）不可以被用于表示节点的路径（在 C 语言中使用 null 字符会出现问题）；
* `\u0001` - `\u001F` 和 `\u007F` - `\u009F` 字符不可以被用于表示节点的路径（这些字符不能很好地显示，或者会显示混乱）；
* `\ud800` - `uF8FF` 和 `\uFFF0` - `\uFFFF` 字符不可以被用于表示节点的路径；
* `.` 字符可以被用于表示路径，但是 `.` 和 `..` 字符不可以被用于表示相对路径；
* `zookeeper` 关键字不可以被用于表示节点的路径；

### Znode
ZooKeeper 中的节点被称为 *znode*。znode 维护了一个节点的信息结构，这个信息结构包括了节点的数据和 ACL 变更时的版本号，除此之外，znode 信息结构还包括了节点数据的创建和修改时间戳。每次更新 znode 数据的时候，znode 的版本号都会增加。ZooKeeper 会结合版本号和时间戳，用来校验数据缓存和协调更新操作。例如，当客户端在检索 znode 数据的时候，ZooKeeper 也会返回 znode 数据的版本号给客户端，随后在客户端需要更新或者删除 znode 的时候，ZooKeeper 要求客户端必须提供 znode 数据的版本号。如果客户端提供的 znode 数据的版本号和 ZooKeeper 中真实 znode 数据的版本号对应不上（znode 数据已经被其它客户端更新），那么这次更新操作就会执行失败。

*znode* 是开发者们在 ZooKeeper 中访问的最主要的资源，它们有以下几个特性值得被关注：

#### Watches
客户端可以在 znode 上设置 watch。修改或者删除 znode 会 **触发并删除** 这个 watch，当 watch 被触发时，ZooKeeper 会向客户端发送一个通知。

#### Data Access
在 ZooKeeper 命名空间中读取和写入 znode 数据是以原子方式执行的，读取操作会获取 znode 关联的所有字节数据，写入操作会替换 znode 的所有数据。每个 znode 都有一个对应的 ACL（Access Control List），ACL 决定了哪个客户端可以做哪些操作。

ZooKeeper 并不是被设计用来作为一个数据库或者存储很大的对象的，相反的，它只是用来管理分布式系统之间的协调数据。这些协调数据以配置信息、状态信息等等的形式存在，它们一般都很小，通常以 KB 为衡量单位。ZooKeeper 客户端和服务器会校验 znode 的数据是否小于 1M，但实际的使用情况中，znode 数据应当需要远小于 1M。因为计算机在网络上需要使用更多的时间来传输更多的数据，所以在操作数据量比较大的 znode 时将会导致 ZooKeeper 花费更多的时间，从而这会影响 ZooKeeper 操作其它正常数据量大小的 znode 的延迟。如果确实需要在 znode 上存储非常大的数据，通常的做法是将数据保存在大容量的存储系统（例如 NFS 或 HDFS）上，然后在 ZooKeeper znode 中只保存数据存储位置的指针。

#### Ephemeral Nodes
ZooKeeper 同时也有临时节点的概念。这些临时节点只会在 session 处于活跃状态时存在，当 session 终结的时候，这些临时节点也会被删除。也正是由于这个特性，ZooKeeper 中临时节点是不允许创建子节点的。

#### Sequence Nodes -- Unique Naming
创建节点的时候，客户端还可以请求 ZooKeeper 在 znode 路径的末尾上附加一个单调递增的计数器。计数器的值对于父节点来说是唯一的，以 `%010d` 的形式创建（这是一个以 0 为填充的 10 位数字，例如 `0000000001`）。

### Time in ZooKeeper
ZooKeeper 以多种方式记录时间：
* **zxid** 每次对 ZooKeeper 信息结构的修改都会被标记上一个 zxid（ZooKeeper Transaction Id），zxid 对外暴露了 ZooKeeper 中所有更新操作的总排序。每次的更新操作都会有一个 zxid，并且如果 zxid1 小于 zxid2，那么 zxid1 就会在 zxid2 之前被执行；
* **version numbers** 每次对 znode 的修改都会导致其中一种的节点版本号增加。znode 一共有三种版本号：version、cversion、aversion；
* **ticks** 当使用 ZooKeeper 集群模式时，每台 ZooKeeper 服务之间使用 tickTime 来定义例如 status uploads、session timeouts、connection timeouts 等等的事件的间隔时间。ZooKeeper 最小 sessionTimeout 是 tickTime 的两倍；
* **real time** ZooKeeper 只会在 znode 创建或修改时，把时间戳记录到 znode 的信息结构中，除此之外 ZooKeeper 都不会使用真实世界的时间数据；

### ZooKeeper Stat Structure
ZooKeeper 中每个节点的信息结构是由以下字段组成的：
* **czxid** 创建 znode 的 zxid；
* **mzxid** 最后一次修改 znode 的 zxid；
* **pzxid** 最后一次修改 znode 子节点的 pzxid；
* **ctime** 创建 znode 的时间戳；
* **mtime** 最后一次修改 znode 的时间戳；
* **version** 修改 znode 数据的次数；
* **cversion** 修改 znode 子节点的次数；
* **aversion** 修改 znode ACL 的次数；
* **ephemeralOwner** 临时节点所属的 sessionId。如果此节点不是临时节点，那么该值为 0；
* **dataLength** znode 数据的字节长度；
* **numChildren** znode 的子节点数量；

---

原文链接：http://zookeeper.apache.org/doc/r3.4.14/zookeeperProgrammers.html#ch_zkDataModel 。