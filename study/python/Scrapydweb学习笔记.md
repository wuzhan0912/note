/usr/local/bin/scrapydweb

执行install后 scrapyd命令变了





java sdk 构想

1. 提供api ： 1 轮训拿到节点，2 返回节点列表
2. 项目启动时遍历spider监听
3. 获取时如果本地获取不到ls zk 异步遍历添加监听
4. 重试机制
5. 
6. 本地维护spiders列表
7. 如果更新的做完了，有可能会被旧的覆盖
8. 线程池遇到同步代码块可能 死锁吧
9. version 最高问题
10. 队列做 每次都查新的



