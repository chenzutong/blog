## swap介绍
Swap分区， 即交换区  

作用: 当系统的物理内存不够用的时候，就需要将物理内存中的一部分空间释放出来，
以供当前运行的程序使用，那些被释放的空间可能来自一些很长时间没有什么操作的程序，
这些被释放的空间被临时保存到Swap空间中，等到那些程序要运行时，再从Swap中恢复保存的数据到内存中。
这样，系统总是在物理内存不够时，才进行Swap交换。  

Swap空间应大于或等于物理内存的大小，最小不应小于64M，
通常Swap空间的大小应是物理内存的2-2.5倍，Swap的调整对Linux服务器，
特别是Web服务器的性能至关重要，通过调整Swap，有时可以越过系统性能瓶颈，节省系统升级费用。

## 查看已有的swap空间
> free -h  

h: 可读输出  
m: 单位m输出  
g: 单位g输出  


## 新增swap分区空间
### 使用dd创建swapfile
> dd if=/dev/zero of=/swapfile bs=1G count=64

bs单位默认bytes,可以改为M或G， count为计数  
of 表示output file，表示输出的文件，也就是我们要进行扩容的文件所在路径，这里是/swapfile

### mkswap创建交换文件
> mkswap /swapfile

### swapon激活
> swapon /swapfile

### 设置开启启动
在/etc/fstab添加  
/swapfile swap swap default 0 0

## 去掉swap
### 停用
> swapoff /swapfile

### 删除
> rm -rf swapfile

### 删除随即启动swap
删除/etc/fstab里面相关内容