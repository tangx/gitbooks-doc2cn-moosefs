## 创建、持有、归档  
在 MooseFS 3.0（及以上版本）中，添加了一个新功能——goal 类型改变计划。 
In MooseFS 3.0 (and above) a possibility to "plan" changing the goal has been added. 
 
在 MooseFS 中，你可以提前设置数据在不同阶段使用的 goal 类型： 
+ 在文件被创建时，使用何种类型的 goal 类型 在文件被更改时，使用何种类型的 goal 类型 
+ 在文件距离上次被更改(modifiacation)多长时间后，使用何种 goal 类型 
 
### 创建 

“创建 goal 类型” （ ` -C CRATE_LABELS ` ）：一个 chunk 在被创建时使用的 goal 类型。 
 
### 持有 

“持有 goal 类型” （ ` -K KEEP_LABELS ` ）：文件被创建完成使用的 goal 类型，几乎立即在创建完成瞬间被系统自动设置 
 
### 归档 

“归档 goal 类型” （ ` -A ARCHIVE_LABELS -d ARCHIVE_DELAY ` ）：文件在距离最后一次修改或属性（例如，最后一次 ctime 修改）ARCHIVE_DELAY 天后使用的 goal 类型，满足 ARCHIVE_DELAY 后系统自动设置。 
"Archive goal" ( ` -A ARCHIVE LABELS -d ARCHIVE DELAY ` ) means the goal which is set by the system automatically after ARCHIVE DELAY time (in days) since last modification of the file or its attributes (i.e. since last ctime modification). 
 
### 怎样设置标签 goal 类型 

获取更多关于命令的信息， 参考本文第三章：新mfssetgoal选项中的第3.1节: 创建、持有、归档。 
 
