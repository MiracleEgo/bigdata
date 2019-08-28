# 一面

```
了不了解线程的局部变量，讲讲线程池参数
8、Cookie和Session的区别，怎么防止Cookie欺骗
9、从用户在浏览器输入域名，到浏览器显示出页面的过程
```

------

## String、StringBuffer、StringBuilder的区别

* String不变性
  - String对象是常量，它的值不能被创建后改变，StringBuffer和StringBuilder可以可变；
  * StringBuilder非线程安全（单线程使用），String与StringBuffer线程安全（多线程使用）；
  * 如果程序不是多线程的，那么使用StringBuilder效率高于StringBuffer。

* 常量池

  常量池是一个内存空间，不同于使用new关键字创建的对象所在的堆空间。
  常量池是为了避免频繁的创建和销毁对象而影响系统性能，其实现了对象的共享。当需要一个对象时，就可以从池中取一个出来（如果池中没有则创建一个），则在需要重复创建相等变量时节省了很多时间。
  在编译期被确定，并被保存在已编译的.class文件中的一些数据，包括类、方法、接口等中的常量和字符串常量。常量池还具备动态性，运行期间可以将新的常量放入池中。java中基本类型的包装类的大部分都实现了常量池技术， 即Byte,Short,Integer,Long,Character,Boolean；

  下面代码创建了几个String对象?

```java
String s1 = new String("s1") ; 
String s2 = new String("s1") ;
```

```java
// 3个，编译期在常量池中创建1个，即“s1”常量对象；运行期堆中创建2个，即s1和s2对象。
String s1 = "s1";  
String s2 = s1;  
s2 = "s2";
```

------

## ==和equals的区别

## 重写equals()不重写hashCode()会发生什么

## volatile怎么保证可见性

## 并发编程的锁机制

* 可重入锁

* 读写锁

  读写锁将对一个资源的访问分成了2个锁，如文件，一个读锁和一个写锁。正因为有了读写锁，才使得多个线程之间的读操作不会发生冲突。`ReadWriteLock`就是读写锁，它是一个接口，ReentrantReadWriteLock实现了这个接口。可以通过readLock()获取读锁，通过writeLock()获取写锁。

* 可中断锁

  可中断锁，即可以中断的锁。在Java中，synchronized就不是可中断锁，而Lock是可中断锁。 如果某一线程A正在执行锁中的代码，另一线程B正在等待获取该锁，可能由于等待时间过长，线程B不想等待了，想先处理其他事情，我们可以让它中断自己或者在别的线程中中断它，这种就是可中断锁。

  Lock接口中的**lockInterruptibly**()方法就体现了Lock的可中断性。

* 公平锁

  `synchronized`是非公平锁，它无法保证等待的线程获取锁的顺序。对于`ReentrantLock`和`ReentrantReadWriteLock`，默认情况下是非公平锁，但是可以设置为公平锁。

## synchronized和lock的区别

- Lock是一个接口，而synchronized是Java中的关键字，synchronized是内置的语言实现；
- synchronized在发生异常时，会自动释放线程占有的锁，因此不会导致死锁现象发生；而Lock在发生异常时，如果没有主动通过unLock()去释放锁，则很可能造成死锁现象，因此使用Lock时需要在finally块中释放锁；
- Lock可以让等待锁的线程响应中断，而synchronized却不行，使用synchronized时，等待的线程会一直等待下去，不能够响应中断；
- 通过Lock可以知道有没有成功获取锁，而synchronized却无法办到。
- Lock可以提高多个线程进行读操作的效率。（可以通过readwritelock实现读写分离）
- 性能上来说，在资源竞争不激烈的情形下，Lock性能稍微比synchronized差点（编译程序通常会尽可能的进行优化synchronized）。但是当同步非常激烈的时候，synchronized的性能一下子能下降好几十倍。而ReentrantLock确还能维持常态。

 到了JDK1.6，发生了变化，对synchronize加入了很多优化措施，有自适应自旋，锁消除，锁粗化，轻量级锁，偏向锁等等。导致在JDK1.6上synchronize的性能并不比Lock差。官方也表示，他们也更支持synchronize，在未来的版本中还有优化余地，所以还是提倡在synchronized能实现需求的情况下，优先考虑使用synchronized来进行同步。

## synchronized的JVM底层实现

## sleep和wait的区别

## 锁池和等待池

## notify和notifyAll有什么区别

## 什么情况会发生死锁，死锁的处理方法

## Cookie和Session的区别

- cookie 和session 的区别：

1、cookie数据存放在客户的浏览器上，session数据放在[服务器](https://www.baidu.com/s?wd=%E6%9C%8D%E5%8A%A1%E5%99%A8&tn=24004469_oem_dg&rsv_dl=gh_pl_sl_csd)上。

2、cookie不是很安全，别人可以分析存放在本地的COOKIE并进行COOKIE欺骗
   考虑到安全应当使用session。

3、session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能
   考虑到减轻服务器性能方面，应当使用COOKIE。

4、单个cookie保存的数据不能超过4K，很多浏览器都限制一个站点最多保存20个cookie。

5、所以个人建议：
   将登陆信息等重要信息存放为SESSION
   其他信息如果需要保留，可以放在COOKIE中

- Cookie

Cookie是客户端技术，程序把每个用户的数据以cookie的形式写给用户各自的浏览器。当用户使用浏览器再去访问服务器中的web资源时，就会带着各自的数据去。这样，web资源处理的就是用户各自的数据了。 

- Session

Session是服务器端技术，利用这个技术，服务器在运行时可以为每一个用户的浏览器创建一个其独享的session对象，由于session为用户浏览器独享，所以用户在访问服务器的web资源时，可以把各自的数据放在各自的session中，当用户再去访问服务器中的其它web资源时，其它web资源再从用户各自的session中取出数据为用户服务。

## 怎么防止Cookie欺骗

[防止cookies欺骗－－相关解决方案](https://blog.csdn.net/sollion/article/details/6769798)

一、网络上提供的解决方案
1、
最简单的是给Cookies加个加密算法。
保险点的是给Cookies加个时间戳和IP戳，实际就是让Cookies在同个IP下多少时间内失效。

2、
实际上是这样的，不管cookies里保存了多少个字段，最后，还要增加一个验证字段，或者称为MAC码。这个码是使用上面所有字段的内容合算出来的摘要再用一种加密算法，如3DES等使用服务器的主密钥进行加密。　这样，在从cookies得到数据后，再判断一下这个MAC码就可以知道整个cookies字段是否被篡改过。

3、
我的个人意见:(想过,正准备用上)
response.COOKIES("LOGIN")("MD5COOKIES")=MD5(服务器IP&客户IP&客户ID) ///等等，看着办吧。
然后 if MD5(服务器IP&客户IP&客户ID)<>request.COOKIES("LOGIN")("MD5COOKIES") then 立即清除全部COOKIES end if
补充：上面我写的那个，客户IP不能带上。我想可以改用网卡的MAC或硬盘的SN码。

4、
做项目的时候研究了一下，理论上应该是杜绝了cookies欺骗,我的做法如下： 
自己做session，不用服务器的session功能，大网站都是这样做的，还可以做到session 
跨站。 
用户第一次访问站点的时候，程序产生一个随机sessionID,然后以cookies的方式发给客户端， 
然后将该md5(sessionID+加上该客户断信息(如IP，端口等))和状态信息一起存入数据库， 
这样就算cookies被盗，也有99%的把握不被欺骗。

5、
要防止这个，在写网页的时候就不要相信cookies，例如用户登陆时，记录用户的账号和密码到cookies里，其他叶面就不能直接使用这个cookies，特别是数据库操作，每个叶面都要把cookies与数据库中的用户名和账号对照，如果不吻合，就清空cookies，按非会员操作，即使吻合，进行数据库操作时也要检查字段，阻断注入。

6、   

参考动网防Cookies欺骗，利用动态密码与Session+Cookies双重验证

------

## [从用户在浏览器输入域名，到浏览器显示出页面的过程](https://blog.csdn.net/qq_24147051/article/details/81115806)



---

# 二面（全程怼项目，压力面）

## [谈谈对UDF的理解，写UDF的目的，代码怎么写的](https://blog.csdn.net/WYpersist/article/details/80314352)

[UDFgithub](https://github.com/sotowang/udf)

```
Hive中有3种UDF：
	UDF：操作单个数据行，产生单个数据行；
	UDAF：操作多个数据行，产生一个数据行。
	UDTF：操作一个数据行，产生多个数据行一个表作为输出。
    
用户构建的UDF使用过程如下：
	第一步：继承UDF或者UDAF或者UDTF，实现特定的方法。
	第二步：将写好的类打包为jar。如hivefirst.jar.
	第三步：进入到Hive外壳环境中，利用add jar /home/hadoop/hivefirst.jar.注册该jar文件
	第四步：为该类起一个别名，create temporary function mylength as 'com.whut.StringLength';这里注意UDF只是为这个Hive会话临时定义的。
	第五步：在select中使用mylength();
```

```
1、看你写过UDF，谈谈对UDF的理解，写UDF的目的，代码怎么写的
2、改造hive表后怎么进行数据一致性校验的，有没有自动化流程
3、看你读过kafka源码，讲讲kafka broker的源码里面你最熟悉的类，以及这个类的主要方法，用的什么设计模式
4、项目里面从数据采集到最终的数据可视化，每个环节都有可能丢数据，怎么判断数据有没有丢，如果丢了如何定位到在哪一个环节丢的
5、项目里面为什么要用kafka stream做实时计算，而不是用spark或者flink，kafka sql和spark sql了解过吗
6、项目里面用到了时序数据库opentsdb，为什么要用这个，有没有跟其它的时序数据库对比过
7、平时逛不逛社区，有没有参与过开源项目
8、看你春招笔试的时候***作系统得了0分是怎么做到的
```

# 三面

```
1、看你写了实时计算的程序，你怎么保证计算的结果肯定是对的
2、数据接入的时候，怎么往kafka topic里面发的，用的什么方式，起了几个线程，producer是线程安全的吗
3、kafka集群有几台机器，怎么确定你们项目需要用几台机器，有评估过吗，吞吐量测过吗
4、spark streaming是怎么跟kafka交互的，具体代码怎么写的，程序执行流程是怎样的，这个过程中怎么确保数据不丢
5、kafka监控是怎么做的，kafka中能彻底删除数据吗，怎么做的
```

# 面委会（全程聊天）

- 平时是怎么学习的，爱看哪些博客，怎么看待加班，有没有成为leader的潜力