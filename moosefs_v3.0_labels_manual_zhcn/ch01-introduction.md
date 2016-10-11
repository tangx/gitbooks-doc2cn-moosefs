# MooseFS 3.0 标签功能介绍

## 1.1 概述

在最近发布的 MooseFS 版本 3.0 中，新增了一个新功能——标签。现在文件 goal 类型值可以设置文件副本数量或标签表达式。通过标签，你可以指定数据存放在特定的chunkserver(s)中。例如，使用标签可以指定将常用数据存储到含有 SSD 硬盘的 chunkserver 上，或者将数据储存到几个不同的机房3。

## 1.2 什么是标签？

标签就是字母(26 个大写字母，即 A-Z)。每个 chunkserver 可以拥有多个标签(最多 26 个)。
