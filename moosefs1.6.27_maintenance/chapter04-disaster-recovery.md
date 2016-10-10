# 灾难恢复

mfsmetarestore是moosefs提供的元文件恢复工具。将changelogs中的数据写入到metadata.mfs中。

## 智能恢复

```bash
# /usr/local/mfs/sbin/mfsmetarestore -a
loading objects (files,directories,etc.) ... ok
loading names ... ok
loading deletion timestamps ... ok
loading chunks data ... ok
checking filesystem consistency ... ok
connecting files and chunks ... ok
progress: current change: 0 (first:851672 - last:0 - 100% - ETA:finished)
store metadata into file: /usr/local/mfs/var/mfs/metadata.mfs
```

从系统某处读取mfs的默认配置信息，将metadata.mfs恢复到默认路径，因此该目录可以在系统任意目录执行。

## 手动恢复

```bash
/usr/local/mfs/sbin/mfsmetarestore [-f] [-b] [-i] [-x [-x]] -m <meta data file> -o <restored meta data file> [ <change log file> [ <change log file> [ .... ]]
```

+ ` -m <meta data file> ` ：mfsmaster上的metadata.mfs.back 或 mfsmetalogger上的metadata_ml.mfs.back 两个文件
+ ` -o <restored meta data file> ` ：可以使用任意文件名，方便归档。在使用时，需要将该文件名改为metadata.mfs
+ ` change log file ` ：一般来说，changelogs只需要指定最新的一个即可。如果为了恢复的文件不大，可以使用changelog*泛指，mfs会自动筛选

```bash
# cp -a /usr/local/mfs/var/mfs/ /tmp/mfs_20
# cd /tmp/mfs_20/
# /usr/local/mfs/sbin/mfsmetarestore -m metadata.mfs.back -o mfsmetadata.mfs changelog.*
loading objects (files,directories,etc.) ... ok
loading names ... ok
loading deletion timestamps ... ok
loading chunks data ... ok
checking filesystem consistency ... ok
connecting files and chunks ... ok
progress: current change: 0 (first:851672 - last:0 - 100% - ETA:finished)
store metadata into file: mfsmetadata.mfs
```
 

## 恢复失败:空metadata.mfs

在测试灾难恢复时意外碰到的一个问题，应该算作不是bug的bug。
通过mfsmetarestore恢复的metadata.mfs为空元文件。具体情形如下图

![metadata.mfs为空](./images/ch04-disaster-recovery-failed-metadata.mfs-empty.png)

出现这种情况需要满足以下条件：

> 在进行灾难恢复时，metadata.mfs.back是空源文件，而changelog发生了改变。
> 即，从初次启动mfs到灾难发生在一小时内（每小时mfs会自动将changelog中的内容同步到metadata.mfs中）。

由于mfsmaster的重要性，生产环境中运行mfsmaster的服务器都保障了高稳定性，因此这种情况基本不会出现。

### 如何避免？

在mfsmount正常挂载后，对moosefs的挂载分区进行任意写操作，改变changelog。
再正常重启mfsmaster，如下

```bash
# echo "no error"> /mnt/mfs/tmp.txt
# /usr/local/mfs/sbin/mfsmaster restart
working directory: /usr/local/mfs/var/mfs
sending SIGTERM to lock owner (pid:16445)
waiting for termination ... terminated
initializing mfsmaster modules ...
loading sessions ... ok
sessions file has been loaded
exports file has been loaded
... ...
metadata file has been loaded
stats file has been loaded
master <-> metaloggers module: listen on *:9419
master <-> chunkservers module: listen on *:9420
main master server module: listen on *:9421
mfsmaster daemon initialized properly
```
