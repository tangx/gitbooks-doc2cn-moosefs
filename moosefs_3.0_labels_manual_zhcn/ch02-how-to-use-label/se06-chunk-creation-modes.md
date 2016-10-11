##  chunk的创建模式 

当你在启用了标签功能的chunkservers上存放文件时，可能会出现chunkserver磁盘空间耗尽（或chunkserver过载）的情况。 当chunkserver磁盘空间耗尽或者chunkserver过载时，需要使用创建chunks模式通知 MooseFS如何处理这种情况。 
你可以为每个文件、目录及其子目录等设置这些模式，因为当为数据设置goal类型的时候，他们可以被设置（或修改）。 
 
创建chunks模式可以分为三种： 
+ loose 模式（ ` -l flag to mfssetgoal ` ）：当服务器过载或磁盘空间耗尽时，允许创建的chunk使用其他标签。 
+ Standard 模式（默认模式）：允许在磁盘空间耗尽时，允许创建的chunk使用其他标签；但在服务器过载时，不允许使用其他标签。 
+ Strict 模式（ ` -s flag to mfssetgoal `  ）：在任何情况下，都不允许新创建的 chunk都使用其他标签。 
 
下表展示了这些模式下MooseFS的行为： 

| 是否使用其他标签 	| 磁盘空间耗尽 |  	服务器过载  |
| --------------- | ---------- | ---------- | 
| Loose模式  | 	使用其他标签  | 	使用其他标签  |
| Standard模式  | 	使用其他标签  | 	无写入操作   |
| Strict模式  | 	无写入操作（返回ENOSPC）  | 	无写入操作  |
 
