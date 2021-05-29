
# Linux System
## 1 select,poll和epoll
其实所有的I/O都是轮询的方法,只不过实现的层面不同罢了.

这个问题可能有点深入了,但相信能回答出这个问题是对I/O多路复用有很好的了解了.其中tornado使用的就是epoll的.

[selec,poll和epoll区别总结](https://www.cnblogs.com/Anker/p/3265058.html)

基本上select有3个缺点:

连接数受限
查找配对速度慢
数据由内核拷贝到用户态
poll改善了第一个缺点

epoll改了三个缺点.

关于epoll的: http://www.cnblogs.com/my_life/articles/3968782.html

## 2 Redis原理
Redis是什么？
1. 是一个完全开源免费的key-value内存数据库
2. 通常被认为是一个数据结构服务器，主要是因为其有着丰富的数据结构 strings、map、 list、sets、 sorted sets
Redis数据库
​ 通常局限点来说，Redis也以消息队列的形式存在，作为内嵌的List存在，满足实时的高并发需求。在使用缓存的时候，redis比memcached具有更多的优势，并且支持更多的数据类型，把redis当作一个中间存储系统，用来处理高并发的数据库操作

### Redis数据库

> ​	通常局限点来说，Redis也以消息队列的形式存在，作为内嵌的List存在，满足实时的高并发需求。在使用缓存的时候，redis比memcached具有更多的优势，并且支持更多的数据类型，把redis当作一个中间存储系统，用来处理高并发的数据库操作

- 速度快：使用标准C写，所有数据都在内存中完成，读写速度分别达到10万/20万 
- 持久化：对数据的更新采用Copy-on-write技术，可以异步地保存到磁盘上，主要有两种策略，一是根据时间，更新次数的快照（save 300 10 ）二是基于语句追加方式(Append-only file，aof) 
- 自动操作：对不同数据类型的操作都是自动的，很安全 
- 快速的主--从复制，官方提供了一个数据，Slave在21秒即完成了对Amazon网站10G key set的复制。 
- Sharding技术： 很容易将数据分布到多个Redis实例中，数据库的扩展是个永恒的话题，在关系型数据库中，主要是以添加硬件、以分区为主要技术形式的纵向扩展解决了很多的应用场景，但随着web2.0、移动互联网、云计算等应用的兴起，这种扩展模式已经不太适合了，所以近年来，像采用主从配置、数据库复制形式的，Sharding这种技术把负载分布到多个特理节点上去的横向扩展方式用处越来越多
- 
### Redis缺点

- 是数据库容量受到物理内存的限制,不能用作海量数据的高性能读写,因此Redis适合的场景主要局限在较小数据量的高性能操作和运算上。
- Redis较难支持在线扩容，在集群容量达到上限时在线扩容会变得很复杂。为避免这一问题，运维人员在系统上线时必须确保有足够的空间，这对资源造成了很大的浪费。

## 用户空间和内核空间 

共享内存是在用户空间还是内核空间？是在用户空间

## 进程process和线程thread区别


1.Process特点

(1)进程在执行过程中有内存单元的初始入口点，并且进程存活过程中始终拥有独立的内存地址空间；

(2)进程的生存期状态包括创建、就绪、运行、阻塞和死亡等类型；

(3)从应用程序进程在执行过程中向CPU发出的运行指令形式不同，可以将进程的状态分为用户态和核心态。处于用户态下的进程执行的是应用程序指令、处于核心态下的应用程序进程执行的是操作系统指令。

2.引入Thread目的：

在串行程序基础上引入线程和进程是为了提高程序的并发度，从而提高程序运行效率和响应时间。

3.相同点：

都是按特定顺序执行的指令序列/有自己的执行控制块-->拥有自己的寄存器、状态及调度策略等。

4.关系以及不同点

(1)进程存在于操作系统内，并对应于用户可看作为程序或应用程序的事物。另一方面，线程存在于进程内。因此，线程有时也称作“轻量进程”。一个进程至少包括一个线程，通常将该线程称为主线程，一个进程从主线程的执行开始进而创建一个或多个附加线程，就是所谓的多线程。

(2)多个进程的存在使得计算机能够一次执行多个任务。而多个线程的存在使得进程能够分解工作以便并行执行。在多处理器计算机上，进程或线程可以在不同的处理器中运行。这使得真正的并行处理成为可能。

(3)并行处理处理并不总是能够成功。有时候必须要同步线程。一个线程可能必须等待另一个线程的结果，或者一个线程可能需要独占访问另一个线程正在使用的资源。同步问题是多线程应用程序中出现bug的一个常见原因。有时候线程可能最终等待的是永远不会变得可用的资源。这导致了一种称为“死锁”的状况。

(4)进程有独立的地址空间，一个进程崩溃后，在保护模式下不会对其它进程产生影响；线程有自己的堆栈和局部变量，但线程之间没有单独的地址空间，一个线程死掉就等于整个进程死掉。所以多进程的程序要比多线程的程序健壮，但在进程切换时，耗费资源大效率差，不适合并发。

# Coding(shell&python)
## 检查有效电话号码
```bash
#Assume that file.txt has the following content:
987-123-4567
123 456 7890
(123) 456-7890
#Your script should output the following valid phone numbers:
987-123-4567
(123) 456-7890


sed -n -r '/^([0-9]{3}-|([0-9]{3}) )[0-9]{3}-[0-9]{4}$/p' file.txt

grep -P '^(\d{3}-|\(\d{3}\) )\d{3}-\d{4}$' file.txt
```
## 改变文件输出顺序
```bash
#If file.txt has the following content:
name age
alice 21
ryan 30

#Output the following:
name alice ryan
age 21 30

1.
awk '
{
    for (i = 1; i <= NF; ++i) {
        if (NR == 1) s[i] = $i;
        else s[i] = s[i] " " $i;
    }
} 
END {
    for (i = 1; s[i] != ""; ++i) {
        print s[i];
    }
}' file.txt

2.
num_row=$(head -1 file.txt | wc -w)
i=1;
while [[ $i -le $num_row ]] ; do
#Print the ith column of each row and join together
awk "{print \$$i}" file.txt | paste -s -d ' ';
((i=$i+1))
done

```
## 打印单词出现的次数
```bash
#Assume that words.txt has the following content:
the day is sunny the the
the sunny is is
#Your script should output the following, sorted by descending frequency:
the 4
is 3
sunny 2
day 1


awk '{for(i=1;i<=NF;++i){++m[$i]}}END{for(k in m){print k,m[k]}}' words.txt | sort -nr -k 2

cat words.txt | tr -s ' ' '\n' | sort | uniq -c | sort -rn | awk '{print $2, $1}'
```



## 打印文本第十行内容

```bash
#Assume that file.txt has the following content:
Line 1
Line 2
Line 3
Line 4
Line 5
Line 6
Line 7
Line 8
Line 9
Line 10

awk 'NR==10' file.txt

sed -n '10p' file.txt

tail -n +10 file.txt  | head -n 1
```


```
1. dictionary 找出两个list的intersection
2. given array, find k most frequent element
```

## 找K出现最多次的元素 

```python
'''
	Given a non-empty array of integers, return the k most frequent elements.
	For example,
	Given [1,1,1,2,2,3] and k = 2, return [1,2]
'''

class Solution(object):
    def topKFrequent(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: List[int]
        """
        if not nums:
        	return []
        frequency = {}
        for num in nums:
        	if num in frequency:
        		frequency[num] += 1
        	else:
        		frequency[num] = 1

       	result = []
       	import heapq
       	heap = []

       	for key, value in frequency.iteritems():
       		heapq.heappush(heap, (-value, key))

       	for _ in range(k):
       		result.append(heapq.heappop(heap)[1])
       	return result
```
# network

## OSI和TCP/IP模型

| OSI层  | TCP/IP 层 | 名字  | 协议 |
| ----- | --- | ---- | --- |
|第七层   |应用层 |应用层 | FTP,TFTP,HTTP,DNS,TELNET,SMTP    |
|第六层   |应用层 | 表示层 | NA   |
|第五层   |应用层 | 会话层 |  NA  |
|第四层   |传输层 | 传输层 | TCP,UDP   |
|第三层   |网络层 | 网络层 | IP,ICMP,RIP,OSPF,BGP,IGMP   |
|第二层   |数据链路层 | 数据链路层 | ARP,PPP,MTU   |
|第一层   | 数据链路层| 物理层  | ISO02110,IEEE802     |


## HTTP 响应code
```
1xx：指示信息–表示请求已接收，继续处理。
2xx：成功–表示请求已被成功接收、理解、接受。
3xx：重定向–要完成请求必须进行更进一步的操作。
4xx：客户端错误–请求有语法错误或请求无法实现。
5xx：服务器端错误–服务器未能实现合法的请求。
```

# Cloud