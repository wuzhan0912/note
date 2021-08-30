## netty实战

1. 

2. NIO

3. ######完整的Selector案例(Full Selector Example)

   这有一个完整的案例，首先打开一个Selector,然后注册channel，最后锦亭Selector的状态：

```java
      Selector selector = Selector.open();
      SelectionKey key = channel.register(selector, SelectionKey.OP_READ);
      while(true) {

        int readyChannels = selector.select();

        if(readyChannels == 0) continue;

        Set<SelectionKey> selectedKeys = selector.selectedKeys();

        Iterator<SelectionKey> keyIterator = selectedKeys.iterator();
        while(keyIterator.hasNext()) {
        SelectionKey key = keyIterator.next();
        if(key.isAcceptable()) {
            // a connection was accepted by a ServerSocketChannel.
          } else if (key.isConnectable()) {
            // a connection was established with a remote server.
          } else if (key.isReadable()) {
            // a channel is ready for reading
          } else if (key.isWritable()) {
            // a channel is ready for writing
        }
        keyIterator.remove();
        }
      }
```









taskqueue和schulequeue区别