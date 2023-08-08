---
title: 详解Redis底层数据结构
date: 2023-08-07 13:48:31
tags: Redis
---

# Redis数据结构

## 动态字符串SDS

我们都知道Redis中保存的Key是字符串，value往往是字符串或者字符串的集合。可见字符串是Redis中最常用的一种数据结构。

不过Redis没有直接使用C语言中的字符串，因为C语言字符串存在很多问题：

- 获取字符串长度的需要通过运算(因为有一个'\0')
- 非二进制安全(也就是说不能包含'\0'，但是可能会有需求在字符串中假如\0这个字符)
- 不可修改

**Redis构建了一种新的字符串结构，称为简单动态字符串（Simple Dynamic String），简称SDS**。
例如，我们执行命令：

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1689684096069-8251fc95-d834-4c70-a2e0-4f7a1ed1236a.png)

那么Redis将在底层创建两个SDS，其中一个是包含“name”的SDS，另一个是包含“虎哥”的SDS。

Redis是C语言实现的，其中SDS是一个结构体，源码如下：

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1689684096141-533bf669-143b-4e15-8421-949f325334e2.png)

你可以发现他有两个长度，一个是已保存的字符串字节数，另一个是申请的字节数，这两个是不一样的。

需要注意的是，上面的结构体只是sds的其中一种结构体，是存储8个位的，能存储255个字节，如果字符串超过255了，会转换成其他的sds结构体，比如这些：![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1691318946825-31992e58-6e05-4e10-80cc-385a3f1b6a22.png)，这样做的好处就可以节约很多内存，大部分的字符串都不会超过255，所以还是第一种比较常用一些。

例如，一个包含字符串“name”的sds结构如下：

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1689684096217-4533e2b3-aa8d-4a22-a4af-31bb0698af52.png)

SDS之所以叫做动态字符串，是因为它具备动态扩容的能力，例如一个内容为“hi”的SDS：

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1689684096293-bf6dcc06-844f-4ce5-a5d5-3568e4df665f.png)

假如我们要给SDS追加一段字符串“,Amy”，这里首先会申请新内存空间：

- 如果新字符串小于1M，则新空间为 扩展后字符串长度的两倍+1；（这个1是结束表示）
- 如果新字符串大于1M，则新空间为 扩展后字符串长度+1M+1。

需要注意的是，下面的新空间的大小是13，但是alloc只显示去掉结束标识'\0'的大小，所以是减去了1。

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1691319431553-ad8f5f65-659a-4b35-95d7-c7a934f1a8ad.png)

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1689684096447-58c90f0a-1047-4ef7-b8b4-1aafb34f829d.png)

## intset

翻译过来就是整数的集合

IntSet是Redis中set集合的一种实现方式，基于整数数组来实现，并且具备**长度可变**、**有序**等特征。
结构如下： 

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1689684096539-a48f6b50-a082-47a3-b064-ccac0c96ec16.png)

注意，上面的结构体是标识存储一个数组，数组中每个元素的大小是32位的。。你可能会问，他写的是int8啊，但是其实contents里面只存储的是指针，这样的话就可以去寻址了。

其中的encoding包含三种模式，表示存储的整数大小不同：

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1689684096605-b4ad8d54-0236-4240-99cd-f39c850288e0.png)

为了方便查找，Redis会将intset中所有的整数按照升序依次保存在contents数组中，结构如图：

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1691320019485-5fb83e79-3f61-4473-9bf0-9f07b64ef47d.png)

现在，数组中每个数字都在int16_t的范围内，因此采用的编码方式是INTSET_ENC_INT16，每部分占用的字节大小为：
encoding：4字节
length：4字节
contents：2字节 * 3 = 6字节

以字节表示是这样的，因为每一个元素都是由encoding规定占2个字节，这样做的好处是能快速找到元素的**地址（注意是地址）**

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1691320569245-a3af3ce9-c6d4-4955-b77e-21c702d28ea2.png)





但是我们向该其中添加一个数字：50000，这个数字超出了int16_t的范围，intset会自动升级编码方式到**合适的大小（通过判断这个数字在哪个范围）**。
以当前案例来说流程如下：

1. 升级编码为INTSET_ENC_INT32, 每个整数占4字节，并按照新的编码方式及元素个数扩容数组
2. **倒序（如果是正序的话，会出现覆盖后面的元素的现象）**依次将数组中的元素拷贝到扩容后的正确位置
3. 将待添加的元素放入数组末尾
4. 最后，将inset的encoding属性改为INTSET_ENC_INT32，将length属性改为4

最后结果：

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1691320625721-502f7e24-844d-4894-b028-8040884e9112.png)



源码如下：

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1691321010618-3191f7a7-473e-4d45-a5d7-c65a43d24180.png)

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1689684096970-e812b31d-6386-4c09-a25f-36d06b04badb.png)

小总结：

Intset可以看做是特殊的整数数组，具备一些特点：

- Redis会确保Intset中的元素**唯一**、**有序**
- 具备**类型升级**机制，可以节省内存空间
- 底层采用**二分查找**方式来查询



但是可以发现他好像没有缩小机制。。。而且它只适合存储少量的数据，因为如果有一个元素超级大，但是大部分元素很小，这样很浪费内存。

## Dict

跟Java中的hashmap很像，但是不是完全一样的，一会我会总结一下。

我们知道Redis是一个键值型（Key-Value Pair）的数据库，我们可以根据键实现快速的增删改查。而键与值的映射关系正是通过Dict来实现的。
Dict由三部分组成，分别是：字典（Dict）、哈希表（DictHashTable）、哈希节点（DictEntry）

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1691322010647-686d39ff-e2c8-4fae-b43f-7b13dd9b225e.png)

当我们向Dict添加键值对时，Redis首先根据key计算出hash值（h），然后利用 h & sizemask来计算元素应该存储到数组中的哪个索引位置（其实就相当于这个数%size的值，这里设计的很巧妙，但是我没听懂）。

我们存储k1=v1，假设k1的哈希值h =1，因此k1=v1要存储到数组角标1位置。

下面的k2也是得到hash值为1，所以需要链化了，这里注意是头插，这样做的好处是不用尾插（因为尾插需要遍历整个链表，找到末尾的指针再添加，这样比较麻烦，头插就直接插入就好了）

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1689684097120-2db946fb-4909-4c52-a3a3-8fa66f8a25d3.png)

Dict由三部分组成，分别是：哈希表（DictHashTable）、哈希节点（DictEntry）、字典（Dict）

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1691322318002-3a8b95d4-947d-49e1-98e8-0b077c6a95d2.png)

其中哈希表的size大小必须的是2^n

最终的图例：

下面有两个hash表，一个存数据，一个是空，在rehash的时候使用。



![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1689684097328-e8cc20cb-cf81-473f-b3c5-199994377b94.png)

**Dict的扩容**

Dict中的HashTable就是数组结合单向链表的实现，当集合中元素较多时，必然导致哈希冲突增多，链表过长，则查询效率会大大降低。
Dict在每次**新增键值对时都会检查负载因子**（LoadFactor = used（entry的个数）/size（数组长度）） ，满足以下两种情况时会触发哈希表扩容：

- 哈希表的 LoadFactor >= 1，并且服务器没有执行 BGSAVE 或者 BGREWRITEAOF 等后台进程（因为这些操作很占用CPU）；
- 哈希表的 LoadFactor > 5 ；

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1689684097397-2e639311-14e0-4c55-a7c5-37d1f448d8f1.png)

**Dict的收缩**

Dict除了扩容以外，每次**删除元素时，也会对负载因子做检查**，当LoadFactor<0.1时，会做哈希表收缩，直至收缩到第一个大于等于元素的2^n

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1689684097465-279bf61f-6967-4a81-82e1-9c8317844291.png)

**Dict的rehash**

不管是扩容还是收缩，必定会创建新的哈希表，**导致哈希表的size和sizemask变化，而key的查询与sizemask有关**。你想想，hash算法都变了，所以需要重新的对每一个key都重新再hash分配到新的hash表里。因此必须对哈希表中的每一个key重新计算索引，插入新的哈希表，这个过程称为rehash。过程是这样的：

1. 计算新hash表的realeSize，值取决于当前要做的是扩容还是收缩：

1. 1. 如果是扩容，则新size为**第一个大于等于**dict.ht[0].used + 1的2^n
   2. 如果是收缩，则新size为**第一个大于等于**dict.ht[0].used的2^n （不得小于4）

1. 按照新的realSize申请内存空间（这个空间其实就是数组，大小得是2^n），创建dictht，并赋值给dict.ht[1]
2. 设置dict.rehashidx = 0，标示开始rehash
3. 将dict.ht[0]中的**每一个**dictEntry都rehash到dict.ht[1]
4. 将dict.ht[1]赋值给dict.ht[0]，给dict.ht[1]初始化为空哈希表，释放原来的dict.ht[0]的内存
5. 将rehashidx赋值为-1，代表rehash结束
6. 在rehash过程中，新增操作，则直接写入ht[1]，查询、修改和删除则会在dict.ht[0]和dict.ht[1]依次查找并执行。这样可以确保ht[0]的数据只减不增，随着rehash最终为空d

**但是这种情况并不是redis采用的，原因在下面，redis对上面这种做法做了优化。**

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1691324026573-ff644ed6-1ef7-496d-8a70-fb6207615594.png)



跟hashmap的不同之处：

1. hash算法不一样
2. hashmap等数据量大的时候会进行树化，但是dict没有

## ZipList

### 基本结构

ZipList 是一种特殊的“双端链表”（它里面没有指针，但是有链表的特点，比如遍历的时候只能一个一个遍历进行查找。最重要的是，它是一片连续的内存空间，所以它不需要通过指针来寻址，但是这样做的坏处就是他不能存储大量的数据。） ，由一系列特殊编码的连续内存块组成。可以在任意一端进行压入/弹出操作, 并且该操作的时间复杂度为 O(1)。

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1690349837263-ecaec63c-75ca-4bba-92b1-0d87591abab1.png)

下面详细介绍一下每个元素都是啥意思

| **属性** | **类型** | **长度** | **用途**                                                     |
| -------- | -------- | -------- | ------------------------------------------------------------ |
| zlbytes  | uint32_t | 4 字节   | 记录整个压缩列表占用的内存字节数                             |
| zltail   | uint32_t | 4 字节   | 记录压缩列表表尾节点距离压缩列表的起始地址有多少字节，通过这个偏移量，可以确定表尾节点的地址。 |
| zllen    | uint16_t | 2 字节   | 记录了压缩列表包含的节点数量。 最大值为UINT16_MAX （65534），如果超过这个值，此处会记录为65535，但节点的真实数量需要遍历整个压缩列表才能计算得出。 |
| entry    | 列表节点 | 不定     | 压缩列表包含的各个节点，节点的长度由节点保存的内容决定。     |
| zlend    | uint8_t  | 1 字节   | 特殊值 0xFF （十进制 255 ），用于标记压缩列表的末端。        |

### **ZipListEntry**

ZipList 中的Entry并不像普通链表那样记录前后节点的指针，因为记录两个指针要占用16个字节，浪费内存。而是采用了下面的结构：

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1689684097769-867c50ed-6ad4-4db0-a017-4bd2ce602044.png)

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1691326549965-8657baf5-0212-435b-ba63-4183e359d597.png)

- previous_entry_length：**前一节点的长度**，占1个或5个字节。

- - 如果前一节点的长度小于254字节，则采用1个字节来保存这个长度值
  - 如果前一节点的长度大于254字节，则采用5个字节来保存这个长度值，第一个字节为固定值0xfe，后四个字节才是真实长度数据。

- encoding：编码属性，记录content的**数据类型**（字符串还是整数）以及**长度**，要么1个字节，要么2个要么5个字节
- contents：负责保存节点的数据，可以是字符串或整数



ZipList中所有存储长度的数值均采用小端字节序，即低位字节在前，高位字节在后。例如：数值0x1234，采用小端字节序后实际存储值为：0x3412

### **Encoding编码**

ZipListEntry中的encoding编码分为字符串和整数两种：
字符串：如果encoding是以“00”、“01”或者“10”开头，则证明content是字符串

| **编码**                                             | **编码长度** | **字符串大小**      |
| ---------------------------------------------------- | ------------ | ------------------- |
| \|00pppppp\|                                         | 1 bytes      | <= 63 bytes         |
| \|01pppppp\|qqqqqqqq\|                               | 2 bytes      | <= 16383 bytes      |
| \|10000000\|qqqqqqqq\|rrrrrrrr\|ssssssss\|tttttttt\| | 5 bytes      | <= 4294967295 bytes |

例如，我们要保存：“ab”和 “bc”这两个entry到ziplist中

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1691325610943-f011adef-68a6-41b1-a17c-62e91854d5b7.png)



之前提到了00、01、10开头表示后面的content存储字符串，那11存啥，答案是存数字。

**ZipListEntry中的encoding编码分为字符串和整数两种：**

- 整数：如果encoding是以“11”开始，则证明content是整数，且encoding固定只占用1个字节

| **编码**                                                     | **编码长度** | **整数类型**                                                 |
| ------------------------------------------------------------ | ------------ | ------------------------------------------------------------ |
| 11000000                                                     | 1            | int16_t（2 bytes）                                           |
| 11010000                                                     | 1            | int32_t（4 bytes）                                           |
| 11100000                                                     | 1            | int64_t（8 bytes）                                           |
| 11110000                                                     | 1            | 24位有符整数(3 bytes)                                        |
| 11111110                                                     | 1            | 8位有符整数(1 bytes)                                         |
| 1111xxxx(这种编码就不需要content部分了，因为直接就在编码中存储了数据) | 1            | 如果这个数字特别小，可以直接在xxxx位置保存数值，范围从0001~1101，减1后结果为实际值 |

例如一个ziplist包含两个整数值：“2”和“5”

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1689684097910-43bb0313-ca8b-4513-8e81-d66867f36bb1.png)

注意上面存储的两个整数都没有content部分，都把数据存储到了encoding部分，因为存的数字太小了

### ZipList的连锁更新问题

ZipList的每个Entry都包含previous_entry_length来记录上一个节点的大小，长度是1个或5个字节：
如果前一节点的长度小于254字节，则采用1个字节来保存这个长度值
如果前一节点的长度大于等于254字节，则采用5个字节来保存这个长度值，第一个字节为0xfe，后四个字节才是真实长度数据
现在，假设我们有N个连续的、长度为250字节的entry，因此entry的previous_entry_length属性用1个字节即可表示，如图所示：

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1691326739961-e469b7c0-4c64-4a03-bfde-9a9f30667f76.png)

之后插入了一个长度等于254字节的entry，由于之前的第一个的大小是250，其中entry中的len的长度是1，但是新加进来需要记录新加进来entry的长度，由于长度是254，所以len需要扩容成5个字节，现在之前第一个的大小也是254，之后的所有的原来大小为250字节的都得改变，直到有一个entry比较小或者比较大，不在250~253这个范围中

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1691326917377-9acb7b9c-271d-4dfd-910e-7a9b1b47f814.png)

ZipList这种特殊情况下产生的连续多次空间扩展操作称之为**连锁更新**（Cascade Update）。新增、删除都可能导致连锁更新的发生。

但是这种概率很小，因为如果不可能**一直有N个连续的、长度为250~253字节之间的entry在链表中**

### **总结**

**ZipList特性：**

- 压缩列表的可以看做一种连续内存空间的"双向链表"
- 列表的节点之间不是通过指针连接，而是记录上一节点和本节点长度来寻址，内存占用较低
- 如果列表数据过多，导致链表过长，可能影响查询性能
- 增或删较大数据时有可能发生连续更新问题

## QuickList

- 问题1：ZipList虽然节省内存，但申请内存必须是连续空间（zipList申请的内存是连续的，但是我们知道大部分内存都是碎片化的，所以申请效率很低），如果内存占用较多，申请内存效率很低。怎么办？

- - 答：为了缓解这个问题，我们必须限制ZipList的长度和entry大小。

- 问题2：但是我们要存储大量数据，超出了ZipList最佳的上限该怎么办？

- - 答：我们可以创建多个ZipList来分片存储数据。

- 问题3：数据拆分后比较分散，不方便管理和查找 ，这多个ZipList如何建立联系？

- - 答：Redis在3.2版本引入了新的数据结构QuickList，它是一个双端链表，只不过链表中的每个节点都是一个ZipList。



下面就是quicklist的示意图，它是一个双端链表，只不过链表中的每个节点都是一个ZipList

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1689684098122-1a346bfe-28f6-41a7-b10c-027ba65c8622.png)



为了避免QuickList中的每个ZipList中entry过多，Redis提供了一个配置项：list-max-ziplist-size来限制。

- 如果值为正，则代表ZipList的允许的**entry个数的最大值**
- 如果值为负，则代表ZipList的**最大内存大小**，分5种情况：

- - -1：每个ZipList的内存占用不能超过4kb
  - **-2：每个ZipList的内存占用不能超过8kb**
  - -3：每个ZipList的内存占用不能超过16kb
  - -4：每个ZipList的内存占用不能超过32kb
  - -5：每个ZipList的内存占用不能超过64kb

其默认值为 -2：

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1689684098200-2b7f5e9c-93e9-4f26-b065-5aeab065a65b.png)



并且为了进一步压缩ziplist的大小，还可以调整是压缩模式（默认是ziplist模式），叫lzf压缩模式，下面的源码里面有体现，但是基本都不使用：![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1691328763331-da761006-498b-4bbc-b178-73140727e6b6.png)







以下是QuickList的和QuickListNode的结构源码：

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1689684098275-8c36baed-510e-4f61-95ea-e7a5b314a20b.png)

我们接下来用一段流程图来描述当前的这个结构

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1689684098342-c24b665a-e31c-4def-8c44-2638f4464656.png)

总结：

QuickList的特点：

- 是一个节点为ZipList的双端链表
- 节点采用ZipList，解决了传统链表的内存占用问题
- 控制了ZipList大小，解决连续内存空间申请效率问题
- 中间节点可以压缩，进一步节省了内存

总结来说QuickList就是利用了ZipList和链表（可以利用内存碎片）的优点，这就解决了内存可以是无限大，解决了之前Ziplist的问题

## SkipList

SkipList（跳表）首先是链表，但与传统链表相比有几点差异：

- 元素按照升序排列存储
- 节点可能包含多个指针，指针跨度不同。

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1689684098413-a849b78f-ae5c-40c6-9fb3-08e970331e56.png)

最多允许32级指针

源码如下：

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1691329605510-77df2bf6-6953-48e4-be7d-75ed5bbe9215.png)



下面图是一个示意图，但是level数组中指向的其实是节点，而不是下一个level[]，这样画图只是为了清晰

![img](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/1689684098556-0a58df5e-69de-4244-86bd-c393000156ec.png)

小总结：

SkipList的特点：

- 跳跃表是一个双向链表，每个节点都包含score和ele值
- 节点按照score值排序，score值一样则按照ele字典排序
- 每个节点都可以包含多层指针，层数是1到32之间的随机数
- 不同层指针到下一个节点的跨度不同，层级越高，跨度越大
- 增删改查效率与红黑树基本一致，**实现却更简单（这就是为什么不用红黑树的原因）**

## RedisObject

Redis中的任意数据类型的键和值都会被封装为一个RedisObject，也叫做Redis对象

### 介绍

从Redis的使用者的角度来看，⼀个Redis节点包含多个database（非cluster模式下默认是16个，cluster模式下只能是1个），而一个database维护了从key space到object space的映射关系。这个映射关系的key是string类型，⽽value可以是多种数据类型，比如：string, list, hash、set、sorted set等。我们可以看到，key的类型固定是string，而value可能的类型是多个。

⽽从Redis内部实现的角度来看，database内的这个映射关系是用⼀个dict来维护的。dict的key固定用⼀种数据结构来表达就够了，这就是动态字符串sds。而value则比较复杂，为了在同⼀个dict内能够存储不同类型的value，这就需要⼀个通⽤的数据结构，这个通用的数据结构就是robj，全名是redisObject。

![img](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/1689684098645-9b9e63ed-9eb9-4254-b0f2-5d44b9578fea.png)

### **Redis的编码方式**

Redis中会根据存储的数据类型不同，选择不同的编码方式，共包含11种不同类型：

| **编号** | **编码方式**            | **说明**               |
| -------- | ----------------------- | ---------------------- |
| 0        | OBJ_ENCODING_RAW        | raw编码动态字符串      |
| 1        | OBJ_ENCODING_INT        | long类型的整数的字符串 |
| 2        | OBJ_ENCODING_HT         | hash表（字典dict）     |
| 3        | OBJ_ENCODING_ZIPMAP     | 已废弃                 |
| 4        | OBJ_ENCODING_LINKEDLIST | 双端链表               |
| 5        | OBJ_ENCODING_ZIPLIST    | 压缩列表               |
| 6        | OBJ_ENCODING_INTSET     | 整数集合               |
| 7        | OBJ_ENCODING_SKIPLIST   | 跳表                   |
| 8        | OBJ_ENCODING_EMBSTR     | embstr的动态字符串     |
| 9        | OBJ_ENCODING_QUICKLIST  | 快速列表               |
| 10       | OBJ_ENCODING_STREAM     | Stream流               |

### **五种数据结构**

Redis中会根据存储的数据类型不同，选择不同的编码方式。每种数据类型的使用的编码方式如下：

| **数据类型** | **编码方式**                                       |
| ------------ | -------------------------------------------------- |
| OBJ_STRING   | int、embstr、raw                                   |
| OBJ_LIST     | LinkedList和ZipList(3.2以前)、QuickList（3.2以后） |
| OBJ_SET      | intset、HT                                         |
| OBJ_ZSET     | ZipList、HT、SkipList                              |
| OBJ_HASH     | ZipList、HT                                        |

# 数据类型

## String

String在Redis中是⽤⼀个robj来表示的，它有三种编码方式，分别是：

1. 其基本编码方式是**RAW**，基于简单动态字符串（SDS）实现，存储上限为512mb。![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1691330324982-8aefb40e-b369-4f13-9171-4b3e75c656b5.png)
2. 如果存储的SDS长度小于44字节（全部加起来刚好是64，这样做的好处是不会产生内存碎片），则会采用**EMBSTR编码**，此时object head与SDS是一段连续空间。申请内存时只需要调用一次内存分配函数（raw需要先申请robj再申请sds的，加起来需要调用两次内存分配函数），效率更高。![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1691330358892-a586a2c5-60b1-44ed-bd3f-52c23094e163.png)
3. 如果存储的字符串是整数值，并且大小在LONG_MAX范围内，则会采用**INT编码**：直接将数据保存在RedisObject的ptr指针位置（刚好8字节），不再需要SDS了。![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1691331225893-6e444ac2-a077-422a-b6e9-45ba48508c36.png)







用来表示String的robj可能编码成3种内部表示：OBJ_ENCODING_RAW，OBJ_ENCODING_EMBSTR，OBJ_ENCODING_INT。也就是上面提到的三种
其中前两种编码使⽤的是sds来存储，最后⼀种OBJ_ENCODING_INT编码直接把string存成了long型。



long型还有的特点是：
在对string进行**incr, decr（+1、-1）**等操作的时候

- 如果它内部是OBJ_ENCODING_INT编码，那么可以直接行加减操作；
- 如果它内部是OBJ_ENCODING_RAW或OBJ_ENCODING_EMBSTR编码，那么Redis会先试图把sds存储的字符串转成long型，如果能转成功，再进行加减操作。



如果对⼀个内部表示成long型的string执行**append, setbit, getrange**这些命令，针对的仍然是string的值。比如字符串”32”，如果按照字符数组来解释，它包含两个字符，它们的ASCII码分别是0x33和0x32 。当我们执行命令`setbit key 7 0`的时候，相当于把字符0x33变成了0x32，这样字符串的值就变成了”22”。⽽如果将字符串”32”按照内部的64位long型来解释，那么它是0x0000000000000020，在这个基础上执⾏setbit位操作，结果就跟之前的不一样了。因此，在这些命令的实现中，会把long型先转成字符串再进行相应的操作。

## List

Redis的List类型可以从首、尾操作列表中的元素：

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1689684098994-38aa9ab9-c759-4565-a196-a23e19803379.png)

哪一个数据结构能满足上述特征？

- LinkedList ：普通链表，可以从双端访问，内存占用较高，内存碎片较多
- ZipList ：压缩列表，可以从双端访问，内存占用低，存储上限低
- QuickList：其实就是**LinkedList + ZipList**，可以从双端访问，内存占用较低，包含多个ZipList，存储上限高



在不同的版本实现起来不一样，quicklist是后面更新出来的数据结构：

- 在3.2版本之前，Redis采用ZipList和LinkedList来实现List，当元素数量小于512并且元素大小小于64字节时采用ZipList编码，超过则采用LinkedList编码。
- 在3.2版本之后，Redis统一采用QuickList来实现List![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1689684099061-86b3cc3a-6c39-4ebf-8e2b-675583320ad6.png)

## Set

Set是Redis中的单列集合，满足下列特点：

- 不保证有序性
- 保证元素唯一
- 求交集、并集、差集

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1689684099128-7a0b9d2c-af15-4f22-812c-e465a5996a2d.png)

可以看出，Set对查询元素的效率要求非常高（因为不管是什么指令，都得先去判断元素是否存在），思考一下，什么样的数据结构可以满足？

- HashTable可以满足条件，也就是Redis中的Dict，不过Dict是双列集合（可以存键、值对），(可以理解为java中的hashset底层就是hashmap)，为了查询效率和唯一性，set采用HT编码（Dict）。Dict中的key用来存储元素，value统一为null。
- 当存储的所有数据都是整数，并且元素数量不超过set-max-intset-entries时，Set会采用IntSet编码，以节省内存

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1689684099218-6dc47fc3-44ca-48a9-96c5-3ff7acd41c00.png)

当添加元素时，会进行校验看是否满足inset的条件（整数、数量不超过max）

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1691375923117-73524ffc-9167-490c-86e8-b930f2caedb1.png)





两种编码的结构如下：

首先全部都是整数的情况，使用intset编码

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1691376128649-f6d0b715-fb3f-444e-92b0-7a616260c61c.png)

之后加了一个字符串类型的数据，编码就由inset转化成dict了

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1689684099286-f4c61f86-bb46-4301-aa46-04b1a35023fe.png)

## ZSET

ZSet也就是SortedSet，其中每一个元素都需要指定一个score值和member值：

- 可以根据score值排序后
- member必须唯一
- 可以根据member查询分数

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1689684099351-79cf1b03-ade7-4726-bffa-dbe5d5379be8.png)

因此，zset底层数据结构必须满足键值存储、键必须唯一、可排序这几个需求。之前学习的哪种编码结构可以满足？

- SkipList：可以排序，并且可以同时存储score和ele值（member）（但是不满足key唯一和根据key找值）
- HT（Dict）：可以键值存储，并且可以根据key找value（无法排序）

可以发现，这两个单个都无法满足我们的需求，所以redis底层是把这两种数据结构进行组合进行使用的。

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1689684099423-fd5600b3-641e-43e1-a7bf-c78159bbb23f.png)



但是你就会发现一个问题，这玩意的数据他存储了两份！！这样做的代价就是用**双重的内存空间**来换取**功能**。

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1689684099518-45b06c0e-95a4-4d2f-af43-c2455c8e7d86.png)







你会发现，使用Dict+skipList很占用内存，所以redis还对他进行了优化，当元素数量不多时，zset会采用ZipList结构（它不能存储太多元素，所以要满足下面的条件）来节省内存，不过需要同时满足两个条件：

- 元素数量小于zset_max_ziplist_entries，默认值128
- 每个元素都小于zset_max_ziplist_value字节，默认值64

图示就是这样

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1689684099668-11eacef4-5ab0-4902-a2b8-fa4c4afc23d3.png)



下面是初始化判断采用那种方法进行编码

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1691378825842-de80b512-b69e-4331-9a1a-3c0a43e247f6.png)

同样的，跟上面说的set集合一样，当增加的时候不满足条件的话，会从ziplist转换成dict+skiplist结构,下面就算add的源码

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1691379033791-a42822be-8729-489f-af6c-9982a7515cec.png)

之前提到过，只有dict+skiplist才有可能满足我们zset的需求，那ziplist底层是怎么实现**排序**和**唯一**和**根据key找value**呢

ziplist本身没有排序功能，而且没有键值对的概念，因此需要有zset通过业务逻辑编码实现：

- ZipList是连续内存，因此score和element是紧挨在一起的两个entry， element在前，score在后
- score越小越接近队首，score越大越接近队尾，按照score值升序排列

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1691379255160-50f7c3cc-a1cf-4c56-a9c9-a7561cd138a9.png)

每次添加都会去检查是否有这个key，如果有就更新，没有就添加。但是你会说，这种性能比hash差啊，其实也不会差多少，这是有原因的，因为这种情况数据量小，所以这些遍历链表进行排序或者插入的业务不会特别影响性能，但是如果数据量大的话，就会很影响了，但是数据量大的话我们就不采用这种方式了，就换成另一种方式了。

## Hash

![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1690374243168-437235b9-ca0f-4bd6-b480-de113ee7f08c.png)

Hash结构与Redis中的Zset非常类似：

- 都是键值存储
- 都需求根据键获取值
- 键必须唯一

区别如下：

- zset的键是member，值是score；hash的键和值都是任意值
- zset要根据score排序；hash则无需排序

**所以可以看出来这个hash和zset超级像，所以他们的底层采用的编码也基本一致，只需要把有关排序的skiplist给去掉就行了**



所以hash的底层实现方式：压缩列表ziplist 或者 字典dict

- 当Hash中数据项比较少的情况下，Hash底层才⽤压缩列表ziplist进⾏存储数据![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1691379984367-a93e5a94-ee34-4efd-9995-3e2e17522b7b.png)
- 随着数据的增加，底层的ziplist就可能会转成dict，当满足下面条件之一就会进行转换。![img](../pic/%E8%AF%A6%E8%A7%A3Redis%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1691380019524-38cc1587-b636-46ff-91b1-be0f7e6f3f46.png)具体配置如下：

- - ziplist中的元素数量 hash-max-ziplist-entries 512
  - ziplist中的任意entry的大小  hash-max-ziplist-value 64






Redis的hash之所以这样设计，是因为当ziplist变得很⼤的时候，它有如下几个缺点：

- 每次插⼊或修改引发的realloc操作会有更⼤的概率造成内存拷贝，从而降低性能。
- ⼀旦发生内存拷贝，内存拷贝的成本也相应增加，因为要拷贝更⼤的⼀块数据。
- 当ziplist数据项过多的时候，在它上⾯查找指定的数据项就会性能变得很低，因为ziplist上的查找需要进行遍历。

总之，ziplist本来就设计为各个数据项挨在⼀起组成连续的内存空间，这种结构并不擅长做修改操作。⼀旦数据发⽣改动，就会引发内存realloc，可能导致内存拷贝。
