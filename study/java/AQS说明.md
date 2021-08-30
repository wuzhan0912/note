- 常用的锁是独占锁,共享锁指可以多个线程访问



1. !tryAcquire(arg) Lock实现 +1
2. addWaiter{ enq }
3. acquireQueued 再次尝试tryAcquire, 然后park
4. NonfairSync的tryAcquire方法调用nonfairTryAcquire，调用该方法时，不先进队列，而直接判断能否获取锁，能获取到锁则继续。
5. fair会先判断hasQueuedPredecessors(), 是否有排队
6. 有个保护机制,如果断了unparkSuccessor把最头部的唤醒, 也是prev的作用
7. next常用来唤醒
8. 双链表结构
9. CONDITION  应用没看深,暂时不入队
10. ReentrantLock  对比synchronized
11. 





