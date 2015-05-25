memcached 
-------------------------

## memcached help
{% highlight shell %}
./memcached -help
{% endhighlight %}

## memcaced memory allocation

memcached按slab allocation方式分配管理内存
每一个page是1MB(by default)，然后把一个page划分成多个相同大小的chunk, 所有相同的chunks 组成一个集合slab class．　
即：一个slab class　对应多个pages, 一个pages对应多个大小相同的chunk．
当创建一个新的slab class时，chunk大小会根据chunk growth factor进行递增．默认增长因子为1.25，　chunk的大小将会取the best power of 2 fit for befored chunk'size * 1.25.
在默认场景下，一个page下的chunks会是一个slab class，下一个page中的chunk按增长因子划分成新大小的chunk.
在默认场景下，memcached可用的内存大小为64MB，　见`-m <num>      max memory to use for items in megabytes (default: 64 MB)`


**memcached 中pages的大小设置如下**：
{% highlight shell %}
./memcached -I 1m   
## -I            Override the size of each slab page. Adjusts max item size
##               (default: 1mb, min: 1k, max: 128m)
{% endhighlight %}

**memcached　查看分配的chunks方式**:
{% highlight shell %}
./memcached -u memcached -vv  ##the user is memcached
{% endhighlight %}

**memcached chunk增长因子设置**:
{% highlight shell %}
./memcached -f 1.25  ## default is 1.25
## -f <factor>   chunk size growth factor (default: 1.25)
{% endhighlight %}

**memcached 分配内存的大小设置**:
{% highlight shell %}
./memcached -m 64
## -m <num>      max memory to use for items in megabytes (default: 64 MB)
{% endhighlight %}

问题:如何让一个slab class有多个pages?

**统计slab和item等数据**:
{% highlight shell %}
telnet memcached_ip memcached_port
stats

## http://docs.oracle.com/cd/E19078-01/mysql/mysql-refman-5.0/ha-overview.html#ha-memcached-stats-general
stats slabs

stats items

stats sizes
{% endhighlight %}

## memcached　分布式
Memcached虽然称为“分布式“缓存服务器，但服务器端并没有“分布式”的功能。Memcached的分布式完全是有客户端实现的．
![image](images/memcached-distribute.jpeg)
采用得最多是Consistent Hashing方式，根据key的哈希值来分配相应的memcached node. see:http://blog.sina.com.cn/s/blog_493a845501013ei0.html

## memcached存储的数据安全吗?
15.5.5.2: Is the data inside of memcached secure?

No, there is no security required to access or update the information within a memcached instance, which means that anybody with access to the machine has the ability to read, view and potentially update the information. If you want to keep the data secure, you can encrypt and decrypt the information before storing it. If you want to restrict the users capable of connecting to the server, your only choice is to either disable network access, or use IPTables or similar to restrict access to the memcached ports to a select set of hosts. 


## reference:
+ http://docs.oracle.com/cd/E17952_01/refman-5.6-en/ha-memcached-using-logs.html
+ http://docs.oracle.com/cd/E19078-01/mysql/mysql-refman-5.0/ha-overview.html
