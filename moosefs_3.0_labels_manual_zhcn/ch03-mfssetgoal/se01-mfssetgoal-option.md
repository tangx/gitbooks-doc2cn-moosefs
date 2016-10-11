## 新 mfssetgoal 选项 
从MooseFS 3.0开始，mfssetgoal支持单独标签和简单数字组合的goal表达式。为了设置带有标签的的goal，你需要使用如下goal表达式： 

```bash
mfssetgoal [-r] [-n|-h|-H|-k|-m|-g] GOAL[+|-]|LABELS| -K KEEP_LABELS [CREATE_LABELS] [-A ARCHIVE_LABELS -d ARCHIVE_DELAY] OBJECT... 

```

 
` mfssetgoal LABELS FILE` ：系统会根据LABELS表达式的值存放文件。LABELS表达式由一组以逗号分隔的子表达式组成，每个子表达式指定了文件的一个副本的储存模式(storage schema)。子表达式可以是：一个星号或一个标签模式(label schema)。标签模式可以是一个标签或一个包含标签的并集 (sums)、交集 (multiplications)和方括号(brackets)的表达式。 
 
LABELS表达式的完整语法 如下： 

```
LABELS -> [1-9] E ',' LABELS | [1-9] E ';' LABELS 
E -> '*' | S 
S -> S '+' M | M 
M -> M T | T 
T -> 'A' .. 'Z' | '[' S ']' 

```

 
+ 并集（语法中的符号S）是指文件可以被存在任意一台包含了任意元素的chunkserver上（逻辑OR） 
+ 交集（语法中的符号M）是指文件只能被存储在包含了所有元素的chunkserver上（逻辑AND） 
+ 星号是指任意chunkserver 
+ 相同的子表达式可以通过使用数字缩短表达式长度；数字的值即为子表达式重复的次数。 
 
### 创建、持有、归档 

```bash 
mfssetgoal -K KEEP_LABELS [-C CREATE_LABELS] [-A ARCHIVE_LABELS -d 
ARCHIVE_DELAY ] FILE 
```

当使用CREATE_LABELS标签时会创建（和写入）一个文件。 
 
当文件被创建成功后，系统会立即使用KEEP_LABELS和ARCHIVE_LABELS及ARCHIVE_DELAY （如果指定了的话）。 
 
当距离文件最后一次修改满足ARCHIVE_DELAY条件时，系统会立即使用ARCHIVE_LABELS。 
 
3.2 举个栗子 
` mfssetgoal A,B FILE  ` 
文件会生成两份副本，其中一份会被存放在含有标签A的chunkserver中，另一份会被存放在含有标签B的chunkserver中。  
 
` mfssetgoal A,* FILE  ` 
文件会生成两份副本，其中一份会被存放在含有标签A的chunkserver中，另一份会被存放在其他任意一台chunkserver中  
 
` mfssetgoal *,* FILE  ` 
文件会生成两份副本，会被存放在不同的任意两台chunkserver中。 
 
` mfssetgoal AB,C+D FILE ` 
文件会生成两份副本，其中一份会被存放在任意一台同时包含标签A和B（multiplication of labels，标签交集）的chunkserver中，另一份会被存放在其他任意一台包含了标签C或D其中
之一（sum of labels，标签并集）的chunkserver中。 
 
` mfssetgoal A,B[X+Y],C[X+Y] FILE ` 
文件会生成三份副本，其中一份会被存放在任意一台含有标签A的chunkserver中，另一份会被存放在任意一台包含了标签B且同时包含了标签X或Y之一的chunkserver中，最后一份会被存放在任意一台包含了标签C且同时包含了标签X或Y之一的chunkserver中。 
 
` mfssetgoal A,A FILE  `
命令等价于 mfssetgoal 2A FILE  
 
` mfssetgoal A,BC,BC,BC FILE  ` 
命令等价于 mfssetgoal A,3BC FILE  
 
` mfssetgoal *,* FILE  ` 
命令等价于 msfssetgoal 2* FILE ，也等价于 mfssetgoal 2 FILE 
 
