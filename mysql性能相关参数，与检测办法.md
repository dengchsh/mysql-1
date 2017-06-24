Mysql服务启动之后，我们可以使用`show variables`和`show status` 命令可以查看mysql服务的静态参数值和动态运行状态信息。
## show variables
show variables是查看数据库启动后不会动弹更改的值，比如缓冲区大小、字符集、数据文件名等。
## show status
show status是查看数据库运行期间的动态变化信息，比如锁等待、当前连接数等。

<br>

# 影响MySQL性能的重要参数

    主要介绍的是使用MyISAM存储引擎的key_buffer_size和table_cache
    以及使用使用InnoDB存储引擎的一些以innodb_开头的参数
    
## 1.key_buffer_size

该参数是用来设置索引块（Index Blocks）缓存的大小，它被索引线程共享，此参数只使用MyISAM存储引擎。MySQL5.1之后的版本，可以将指定的表索引缓存入指定的key_buffer,这样可以降低线程之间的竞争。
```
索引缓存概述
<br>
MyISAM存储引擎和其他很多数据库系统一样，采用了一种将最经常访问的表保存在内存中的策略。对应索引区块来说，它维护者一个叫做索引缓存（索引缓冲）的结构体，这个结构体中存放着许多哪些经常使用的索引区块的缓冲区块。对应数据区块来说，Mysql主要依靠系统的本地文件系统缓存。有了索引缓冲后，线程之间不再是串行地访问索引缓存。多个线程可以并行地访问索引缓存。可以设置多个索引缓存，同时也能指定数据表索引到特定的缓存中。
```
### 创建一个索引缓存
```
set global 缓存索引名.key_buffer_size=100*1024;
```
global是全局限制，表示对每一个新的会话（连接）都有效。

### 修改一个索引缓存
和创建一个索引缓存一样，都是
```
set global 缓存索引名.key_buffer_size=100*1024;
```

将相关表的索引放到自己创建的索引缓存中
格式：
```
cache index 表名1,表名2 in 索引缓存
```
将t1、t2、t3表中的索引放到my_cache索引缓存中
因为t1表式InnoDB表，t2，t3表为MyISAM表，故只有t2、t3表中的索引可以放到my_cache缓存中。

### 将索引放到默认的kef_buffer中

可以使用load index into cache +表名

### 删除索引缓存
将其索引缓冲大小设置为了0，就可以删除了，注意不能删除默认的key_buffer。
### 配置mysql服务器启动时自动加载索引缓存
在MySQL配置文件中添加如下内容（在Windows下叫my.ini，在Linux下叫my.cnf）
my_cache.key_buffer_size=1G  #指定索引缓存区大小
init_file=/usr/local/mysql/init_index.sql#在该文件中指定要加载到缓存区德索引

