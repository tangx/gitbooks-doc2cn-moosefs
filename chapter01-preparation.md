# 准备工作

## MooseFS介绍
分布式文件系统是. . . 见官方网站：
http://www.moosefs.org/

## MooseFS的读写流程

![读取流程](./images/ch01-moosefs-read-process.png)

![写入流程](./images/ch01-moosefs-write-process.png)

## 测试系统环境

|  功能  |  Ip地址  |  系统环境  |  KEEPALIVED (10.0.0.50)  |   MooseFS  partition  |  mount point  |
|  -----  |  -----  |  -----  |  -----  |   -----  |  -----  |
|  mfsmaster  |  10.0.0.20  |  CentOS6.4x64  |  MASTER  |    |    |
|  mfsmetalogger  |  10.0.0.21  |  CentOS6.4x64  |  BACKUP  |    |    |
|  mfschunkserver1  |  10.0.0.21  |  CentOS6.4x64  |    |  /mnt/mfschunk1 , /mnt/mfschunk2  |     | 
|  mfschunkserver2  |  10.0.0.22  |  CentOS6.4x64  |    |  /mnt/mfschunk1 , /mnt/mfschunk2 |     |
|  mfsmount  |  10.0.0.20  |  CentOS6.4x64  |    |    |  /mnt/mfs  |

## 拓扑图

![拓扑图](./images/ch01-topological-graph.png)

## 软件版本

|  软件名  |  版本  |  
|-------|---------|
|  MooseFS  |  v1.6.27-5  |  
|  fuse  |  v2.9.3  |  


