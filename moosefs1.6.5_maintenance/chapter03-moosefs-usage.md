# moosefs的使用

软件安装完成后，配置文件路径为 ` $prefix/etc/mfs/ `
服务守护程序路径为 ` $prefix/sbin/ `
普通维护程序路径为 ` $prefix/bin/ `
数据存储文件路径为 ` $prefix/var/mfs/ `

> ` $prefix ` 为安装脚本中的变量。

## 配置文件设置

复制目录下对应组件的配置文件并去掉.dist即可，例如：

```bash
cp -a mfsmaster.cfg.dist mfsmaster.cfg
```

基本上，只要其他组件（如metalogger、chunkserver等）能够访问mfsmaster，使用默认的配置文件即可正常运行moosefs了。
默认配置文件中，其他组件访问的是mfsmaster的主机名，即 “mfsmaster” 这个字段。
因此，在其他所有服务器上修改hosts文件，并添加mfsmaster与ip地址对应关机即可，如

```bash
10.0.0.50    mfsmaster
```

## 服务端

###mfsmaster

+ mfsmaster.cfg ：主配置文件，指定metadata存放路径、其他组件连接的IP地址及端口等；一般情况下，不需要修改此文件，监听所有可用IP及默认端口即可。
+ mfsexports.cfg ：客户端挂载配置，指定不同网段/IP的客户端可挂载的目录及权限。
+ mfstopology.cfg ：拓扑结构配置文件。

修改mfsexports.cfg，添加如下行

```bash
10.0.0.0/24             /       rw
```

其他配置文件可以不用修改

如果是第一次启动mfsmaster的话，需要在 ` $prefix/var/mfs/ ` 中添加一个 ` metadata.mfs ` 的空文件

```bash
cd /usr/local/mfs/var/mfs/
cp -a metadata.mfs.empty metadata.mfs
```

启动mfsmaster：

```bash

# /usr/local/mfs/sbin/mfsmaster start
working directory: /usr/local/mfs/var/mfs
lockfile created and locked
initializing mfsmaster modules ...
loading sessions ... ok
sessions file has been loaded
exports file has been loaded
mfstopology: incomplete definition in line: 7
mfstopology: incomplete definition in line: 7
mfstopology: incomplete definition in line: 22
mfstopology: incomplete definition in line: 22
mfstopology: incomplete definition in line: 28
mfstopology: incomplete definition in line: 28
topology file has been loaded
loading metadata ...
loading objects (files,directories,etc.) ... ok
loading names ... ok
loading deletion timestamps ... ok
loading chunks data ... ok
checking filesystem consistency ... ok
connecting files and chunks ... ok
all inodes: 127429
directory inodes: 3
file inodes: 127426
chunks: 135386
metadata file has been loaded
stats file has been loaded
master <-> metaloggers module: listen on *:9419
master <-> chunkservers module: listen on *:9420
main master server module: listen on *:9421
mfsmaster daemon initialized properly

```

### mfsmetalogger
+ mfsmetalogger.cfg ：主配置文件，指定metadata.mfs的路径，mfsmaster的地址等。

修改mfsmetalogger.cfg中主机名地址

```bash
MASTER_HOST=10.0.0.50
```

启动mfsmetalogger

```bash
# /usr/local/mfs/sbin/mfsmetalogger start
working directory: /usr/local/mfs/var/mfs
lockfile created and locked
initializing mfsmetalogger modules ...
mfsmetalogger daemon initialized properly
```

### mfschunkserver

+ mfschunkserver.cfg ：主配置文件，指定mfsmaster地址等
mfshdd.cfg ：指定为MooseFS提供的目录路径。

修改 mfschunkserver.cfg 中的主机名

```bash
MASTER_HSOT=10.0.0.50
```

修改 mfshdd.cfg文件，添加moosefs使用的文件目录

```bash
/mnt/mfschunk1
/mnt/mfschunk2
```

启动mfschunkserver

```bash
# /usr/local/mfs/sbin/mfschunkserver start
working directory: /usr/local/mfs/var/mfs
lockfile created and locked
initializing mfschunkserver modules ...
hdd space manager: data folders '/mnt/mfschunks2/' and '/mnt/mfschunks1/' are on the same physical device (could lead to unexpected behaviours)
hdd space manager: path to scan: /mnt/mfschunks2/
hdd space manager: path to scan: /mnt/mfschunks1/
hdd space manager: start background hdd scanning (searching for available chunks)
main server module: listen on *:9422
stats file has been loaded
mfschunkserver daemon initialized properly
```

> **注意**： 启动日志中指出，为MooseFS的两个目录在同一磁盘上，可能导致无法预料的问题。
> **因此，实际生产中，该目录最好单独的分区或磁盘，仅提供给MooseFS。**

### mfscgiserv

启动监控

```bash
# /usr/local/mfs/sbin/mfscgiserv start
lockfile created and locked
starting simple cgi server (host: any , port: 9425 , rootpath: /usr/local/mfs/share/mfscgi)
```

可以通过命令查看其他选项

```bash
/usr/local/mfs/sbin/mfscgiserv -h
```

通过浏览器查看监控信息
http://10.0.0.50:9425


## 客户端

### 客户端挂载 mfsmount

```bash
/usr/local/mfs/bin/mfsmount  local_mountpoint  [options]
MFS 选项:
    -H HOST        主机地址
    -P PORT        主机端口
```

可以通过命令查看其他选项

```bash
/usr/local/mfs/bin/mfsmount -h
```

启动mfsmount

```bash
# /usr/local/mfs/bin/mfsmount /mnt/mfs/ -H 10.0.0.50
mfsmaster accepted connection with parameters: read-write,restricted_ip ; root mapped to root:root
```

查看挂载情况

```bash
# df -h
Filesystem            Size  Used Avail Use% Mounted on
10.0.0.50:9421         33G  5.9G   27G  18% /mnt/mfs
```

### 文件拷贝数量 goal

通过 ` mfsgetgoal / mfsgetgoal -r ` 查看goal信息
通过 ` mfssetgoal / mfssetgoal -r ` 设置goal信息

### 删除隔离 trash can

通过 ` mfsgettrashtime / mfsgettrashtime -r ` 查看隔离时间
通过 ` mfssettrashtime / mfssettrashtime -r ` 设置隔离时间


