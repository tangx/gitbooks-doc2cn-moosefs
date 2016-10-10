# Moosefs的安装

moosefs的所有组件都是默认安装的。

## fuse 依赖

其中客户端mfsmount的安装，系统需要提供FUSE依赖。默认安装时，当configure无法找到fuse则会跳过mfsmount；使用选项 ` --enable-mfsmount ` 强制安装mfsmount是，如果找不到fuse，编译时则会报错。
在CentOS6.4中，可以通过 ` yum -y install fuse ` 安装，但在编译moosefs时，提示无法找到fuse。
在编译安装fuse后，也可能在在编译mfs时提示找不到fuse。
可能是因为 ` PKG_CONFIG_PATH ` 变量没有包含fuse路径

```bash

# mfsmount compile
# configure: error: mfsmount build was forced, but fuse library is too old or not installed
# solution: http://bbs.chinaunix.net/thread-1643863-1-1.html

```

因此，解决方法即指定 ` PKG_CONFIG_PATH ` 路径

```bash
#export  PKG_CONFIG_PATH=/usr/local/lib/pkgconfig:$PKG_CONFIG_PATH
```


## 安装脚本
moosefs支持标准的 ` ./configure && make && make install ` 模式
通过执行选项指定需要安装的组件

访问github并获取 ` moosefs 1.6.27-5 ` [安装脚本](https://github.com/octowhale/bash-scripts/blob/master/shell_scripts/moosefs_installer.sh)



修改脚本中的主要参数

```bash

# software package store path 
SRCDIR=/opt/src
# setup MooseFS prefix and working user
MOOSEFS_PATH=/usr/local/mfs
WORKING_USER=mfs
WORKING_GROUP=mfs
# Install or not following components
# enable / disable
MFSMASTER=disable
MFSCHUNKSERVER=disable
MFSMOUNT=enable

```

执行安装脚本

```bash
sh moosefs_install.sh
```

安装脚本见：附录A
