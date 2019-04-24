看看 Guava Cache 的 [源码](https://github.com/google/guava/wiki/CachesExplained)，可以学到很多 Google 工程师的代码设计精髓：
1. `Cache#get(K, Callable<? extends V>)` 可以消除传统的「if cached, return; otherwise create, cache and return」模版代码；
2. 支持线程安全地获取缓存数据、支持缓存数据的异步加载；
3. 支持基于 size、time、reference（引用类型）的缓存失效策略；
5. `CacheStats` 支持统计缓存数据的使用情况；
6. `RemovalListener`、`RemovalNotification` 支持监听缓存失效事件；
7. `ForwardingCache`、`ForwardingLoadingCache` 支持缓存转发（通过这个功能，可以用 Guava API 从 Memcached 或 Redis 等分布式缓存中获取数据）；