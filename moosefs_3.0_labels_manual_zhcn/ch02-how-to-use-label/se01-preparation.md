# 怎么使用标签？

## 机器配置
在本例中，我们将在 11 台物理机上安装 MooseFS 3.0。
+ 02(Master Server)
+ 03(Master Server)
+ 04(Chunkserver)
+ 05(Chunkserver)
+ 06(Chunkserver)
+ 07(Chunkserver)
+ 08(Chunkserver)
+ 09(Chunkserver)
+ 10(Chunkserver)
+ 11(Chunkserver)
+ 12(Chunkserver)

假定：

1. 所有设备的处理器为 4-core Intel Xeon E5-1620 V2 @3.70GHz CUP
2. 两台 Master Servers(ts02,ts03)拥有 80GB 内存
3. 所有 Chunkserver(ts04..ts12)拥有 8GB 内存、
4. 所有机器的操作系统为 Ubuntu 14.04.1 LTS 64-bit
5. 在 MooseFS 实例上拥有一些原始数据，且数据副本数(goals)设置为 2

