`sun.misc.Unsafe` 主要方法：
1. `staticFieldOffset(Field)`、`objectFieldOffset(Field)` 获取字段在类/实例中的偏移量；
2. `getInt(Object, long)`、`putInt(Object, long, int)` ... 根据偏移量获取/设置字段；
3. `getIntVolatile(Object, long)`、`putIntVolatile(Object, long, int)` ... 根据偏移量获取/设置字段，具有 volatile 语义；
4. `park()`、`unpark()` 阻塞、唤醒线程；
5. `monitorEnter(Object)`、`monitorExit(Object)` 监视器相关操作；
6. `compareAndSwapInt(Object, long, int , int)` CAS相关操作；
7. `loadFence()`、`storeFence()`、`fullFence()` 内存屏障相关操作；
8. `allocateMemory(long)`、`reallocateMemory(long, long)`、`freeMemory(long)` 内存相关操作；