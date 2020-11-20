[详细地址](http://blog.codinglabs.org/articles/theory-of-mysql-index.html)

## B-Tree

![image-20200707144010320](C:\Users\YUNTUN\AppData\Roaming\Typora\typora-user-images\image-20200707144010320.png)

## B+Tree

![image-20200707144138796](C:\Users\YUNTUN\AppData\Roaming\Typora\typora-user-images\image-20200707144138796.png)

B+树比B树更适合实现外存储存索引结构

#### 带有顺序访问指针的B+Tree

一般在数据库系统或文件系统中使用的B+Tree结构都在经典B+Tree的基础上进行了优化，增加了顺序访问指针。

![image-20200707144354492](C:\Users\YUNTUN\AppData\Roaming\Typora\typora-user-images\image-20200707144354492.png)

优化的目的是为了提高区间访问的性能



### 为什么使用B-Tree(B+Tree)

​		一般来说，索引本事就很大，不可能全部存储在内存中，因此往往以索引文件的形式存储在磁盘上，这样的话索引查找的过程就要产生**I/O消耗**，相对于内存存取，I/O消耗要高几个数量级，所以，索引的结构组织要尽量减少查找过程中磁盘I/O的存取次数



### B-/+Tree索引的性能分析

由于B+树内节点没有data域，能存储更多，拥有更大的**出度d**，所以性能比B树要更好



## MySql索引实现

#### MyISAM索引实现

data域保存数据记录的**地址**，然后取data值再读取数据记录。称为**非聚集索引**

![image-20200707153622923](C:\Users\YUNTUN\AppData\Roaming\Typora\typora-user-images\image-20200707153622923.png)

#### InnoDB索引实现

​		与MyISAM第一个重大的区别是，InnoDB的数据文件本身就是索引文件地址，MyISAM索引文件和数据文件是分离的，索引文件仅保存数据记录的地址。叶节点包含了**完整的**数据记录。这种索引叫做**聚集索引**。所以InnoDB必须有主键（MyIsAM可以没有），如果没有指定，则会找一个可以唯一标识数据记录的列作为主键，如果不存在，MySQL自动为InnoDB生成一个长度为6字节，类型为长整形的字段作为主键。

​		第二个不同的是，InnoDB的辅助索引data域存储相应主键的值而不是地址。首先检索辅助索引获得主键，然后用主键得到索引中检索获得记录。



所以不建议使用过长的字段作为主键，因为所有辅助索引都引用主索引，过长的主索引会让辅助索引变得太大。再例如，用非单调的字段作为主键在InnoDB中不是个好主意，因为InnoDB数据文件本身是一颗B+Tree，非单调的主键会造成在插入新记录时数据文件为了维持B+Tree的特性而频繁的分裂调整，十分低效，而使用自增字段作为主键则是一个很好的选择。



## 索引使用策略及优化



