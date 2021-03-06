VDO(Virtual Data Optimizer)的技术来源于Red Hat收购的Permabit公司，一个专门从事重删技术的公司。

VDO是一个内核模块，目的是通过重删减少磁盘的空间占用，以及减少复制带宽，VDO是基于块设备层之上的，也就是在原设备基础上映射出mapper虚拟设备，然后直接使用即可，功能的实现主要基于以下技术：

* 零区块的排除：

在初始化阶段，整块为0的会被元数据记录下来，这个可以用水杯里面的水和沙子混合的例子来解释，使用滤纸（零块排除），把沙子（非零空间）给过滤出来，然后就是下一个阶段的处理

* 重复数据删除：

在第二阶段，输入的数据会判断是不是冗余数据（在写入之前就判断），这个部分的数据通过UDS内核模块来判断（`U`niversal `D`eduplication `S`ervice），被判断为重复数据的部分不会被写入，然后对元数据进行更新，直接指向原始已经存储的数据块即可

* 压缩：

一旦消零和重删完成，LZ4压缩会对每个单独的数据块进行处理，然后压缩好的数据块会以固定大小4KB的数据块存储在介质上，由于一个物理块可以包含很多的压缩块，这个也可以加速读取的性能

# 参考

* [REDHAT 7.5beta 新推出的VDO功能](https://ceph.com/planet/redhat-7-5beta-%E6%96%B0%E6%8E%A8%E5%87%BA%E7%9A%84vdo%E5%8A%9F%E8%83%BD/) - 这篇文档提供了详细的部署和实践案例