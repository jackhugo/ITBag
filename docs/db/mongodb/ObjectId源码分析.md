分布式系统中，如何产生唯一的资源id，方法很多，今天我们来看看Mongodb的实现方式。

### ObjectId介绍
 ObjectId使用12字节的存储空间，每个字节两位十六进制数字，是一个24位的字符串。结构如下：

  ![](assets/markdown-img-paste-20170930152932619.png)

- 前4位是一个从标准纪元开始的时间戳，是一个int类别，只不过从十进制转换为了十六进制。这意味着这4个字节隐含了文档的创建时间，将会带来一些有用的属性。并且时间戳处于字符的最前面，同时意味着ObjectId大致会按照插入顺序进行排序，这对于某些方面起到很大作用，如作为索引提高搜索效率等等。使用时间戳还有一个好处是，某些客户端驱动可以通过ObjectId解析出该记录是何时插入的，这也解答了我们平时快速连续创 建多个Objectid时，会发现前几位数字很少发现变化的现实，因为使用的是当前时间，很多用户担心要对服务器进行时间同步，其实这个时间戳的真实值并不重要，只要其总不停增加就好。
- 接下来的3个字节，是所在主机的唯一标识符，一般是机器主机名的散列值，这样就确保了不同主机生成不同的机器hash值，确保在分布式中不造成冲突，这也就是在同一台机器生成的objectid中间的字符串都是一模一样的原因。
- 上面的机器字节是为了确保在不同机器产生的ObjectId不冲突，而PID就是为了在同一台机器不同的mongodb进程产生了ObjectId不冲突。
- 前面的9个字节是保证了一秒内不同机器不同进程生成ObjectId不冲突，最后的3个字节是一个自动增加的计数器，用来确保在同一秒内产生的ObjectId也不会冲突，允许2的12次方等于16777216条记录的唯一性。

### 源码分析
源码地址：https://github.com/mongodb/mongo-java-driver

ObjectId类位于org.bson.types包中，本文就不贴完整代码了，讲到的部分会贴出来。

- id由四个部分构成，对应上面的介绍
~~~java
private final int timestamp;
private final int machineIdentifier;
private final short processIdentifier;
private final int counter;
~~~

- timestamp
~~~java
private static int dateToTimestampSeconds(final Date time) {
    //去掉后面的毫秒
    return (int) (time.getTime() / 1000);
}
~~~

- machineIdentifier
~~~java
private static int createMachineIdentifier() {
    // build a 2-byte machine piece based on NICs info
    int machinePiece;
    try {
        StringBuilder sb = new StringBuilder();
        //获得所有的网络接口信息
        Enumeration<NetworkInterface> e = NetworkInterface.getNetworkInterfaces();
        while (e.hasMoreElements()) {
            NetworkInterface ni = e.nextElement();
            sb.append(ni.toString());
            byte[] mac = ni.getHardwareAddress();
            if (mac != null) {
                ByteBuffer bb = ByteBuffer.wrap(mac);
                try {
                    sb.append(bb.getChar());
                    sb.append(bb.getChar());
                    sb.append(bb.getChar());
                } catch (BufferUnderflowException shortHardwareAddressException) { //NOPMD
                    // mac with less than 6 bytes. continue
                }
            }
        }
        machinePiece = sb.toString().hashCode();
    } catch (Throwable t) {
        // exception sometimes happens with IBM JVM, use random
        machinePiece = (new SecureRandom().nextInt());
        LOGGER.warn("Failed to get machine identifier from network interface, using random number instead", t);
    }
    //去掉前一个byte
    machinePiece = machinePiece & LOW_ORDER_THREE_BYTES;
    return machinePiece;
}
~~~

- processIdentifier
~~~java
// Creates the process identifier.  This does not have to be unique per class loader because
// NEXT_COUNTER will provide the uniqueness.
private static short createProcessIdentifier() {
    short processId;
    try {
        String processName = java.lang.management.ManagementFactory.getRuntimeMXBean().getName();
        if (processName.contains("@")) {
            processId = (short) Integer.parseInt(processName.substring(0, processName.indexOf('@')));
        } else {
            processId = (short) java.lang.management.ManagementFactory.getRuntimeMXBean().getName().hashCode();
        }

    } catch (Throwable t) {
        processId = (short) new SecureRandom().nextInt();
        LOGGER.warn("Failed to get process identifier from JMX, using random number instead", t);
    }

    return processId;
}
~~~

- counter
~~~java
private static final AtomicInteger NEXT_COUNTER = new AtomicInteger(new SecureRandom().nextInt());
//直接调用获得自增数
NEXT_COUNTER.getAndIncrement();
~~~

- 最后总装
~~~java
/**
 * Convert to a byte array.  Note that the numbers are stored in big-endian order.
 *
 * @return the byte array
 */
public byte[] toByteArray() {
    ByteBuffer buffer = ByteBuffer.allocate(12);
    putToByteBuffer(buffer);
    return buffer.array();  // using .allocate ensures there is a backing array that can be returned
}
~~~
~~~java
/**
  * Convert to bytes and put those bytes to the provided ByteBuffer.
  * Note that the numbers are stored in big-endian order.
  *
  * @param buffer the ByteBuffer
  * @throws IllegalArgumentException if the buffer is null or does not have at least 12 bytes remaining
  * @since 3.4
  */
public void putToByteBuffer(final ByteBuffer buffer) {
    notNull("buffer", buffer);
    isTrueArgument("buffer.remaining() >=12", buffer.remaining() >= 12);

    //按位置填充
    buffer.put(int3(timestamp));
    buffer.put(int2(timestamp));
    buffer.put(int1(timestamp));
    buffer.put(int0(timestamp));
    buffer.put(int2(machineIdentifier));
    buffer.put(int1(machineIdentifier));
    buffer.put(int0(machineIdentifier));
    buffer.put(short1(processIdentifier));
    buffer.put(short0(processIdentifier));
    buffer.put(int2(counter));
    buffer.put(int1(counter));
    buffer.put(int0(counter));
}
~~~
