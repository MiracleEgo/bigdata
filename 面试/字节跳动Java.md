**作者：索罗娃·多卡纳 链接：<https://www.nowcoder.com/discuss/203984> 来源：牛客网**

# 一面  

## cookie和session的区别  

* cookie 在客户端
* session在服务端
* 通过cookie实现session

## session的生命周期  

* session调用 inValidate
* session到期,maxAge=-1

## 有n个服务器，怎样去协调他们使用redis

分布式锁

set key value px ... nx 

## redis持久化方式  

* RDB
* AOF
* RDB-AOF(官方推荐)

## Mysql索引结构  

* B树
* B+树
* Hash

## Mysql查询优化  

## 最左匹配原则

例子  有ABCD，建立了index(A,C,D)  

查A = xx and D = xx可以吗  

查C = xx and D = xx可以吗  

查C = xx and A = xx可以吗

## 算法

给定数组nums=[1,2,3,2,3,5,4],和一个常数K，找到有没有可能将nums分成k个子集，使得每个子集的和相等  (5),(1,4),(2,3),(2,3) 

# 二面

https://www.nowcoder.com/discuss/210068

## HTTPS和HTTP的区别  

* SSL加密,更安全
* 端口80与443
* https需要到ca申请证书,一般收费
* HTTP为明文传输

## java GC分为哪几种  

* Minor GC
* Full GC

## TCP/UDP区别  

* 面向连接VS无连接
* 大量数据VS少量数据
* 可靠(三次握手,段标号)VS不可靠
* 慢VS快

## TCP连接建立的过程  

* 三次握手

## 要是服务器发送的最后一个报文时，客户端挂了会怎样  

A B C D 建立了A，C，D三个索引  ,A = x and C = x 用到了几个索引  

* 一次查询只能用到一个索引

## 算法题

求最长不重复子序列  

# 三面

## 堆排序实现

### Inoodb与MyISM的区别

* 行级锁与表级锁
* 支持事务
* 索引

### TCP的TIMED_WAITING  

### java多线程了解过吗，怎么写一个多线程（线程池可以吗）  

### 线程池有哪几个组件（我不知道。。。说了下参数）  

### 在哪个公司实习？讲讲业务逻辑  

