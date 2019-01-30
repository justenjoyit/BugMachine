### 一.jvm 参数
`-Xms64m -Xmx128m -XX:+PrintHeapAtGC -XX:+HeapDumpOnOutOfMemoryError`

### 二.情景复原
3个文章收集人员,每个人负责收集100篇文章并装订成杂志样品;
2个杂志审核人员,每个人负责从杂志队列中取杂志审核
以上过程重复进行

### 三.出现问题
OutOfMemoryError

### 四.问题原因
1. article大小为:53379B = 52KB
2. magazine大小为:5.1MB
3. jvm最大内存大小为128M,意味着
`新生代:老年代=1:2,新生代=32MB,老年代=96MB,新生代里,eden:from survivor:to survivor=8:1:1`,
老年代最多能有20个左右magazine,新生代survivor满的时候(存小于2个左右),新生代会将东西复制到老年代,
也就是说在有21个左右magazine时,如果一直持有不释放就会发生`OutOfMemoryError`

### 五.真实情况
当size为小于等于21时不会发生error,当超过21时就会发生error

### 六.优化方法
让循环队列长度小于22

