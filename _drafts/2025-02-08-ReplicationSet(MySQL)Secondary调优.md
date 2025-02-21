---
layout: post
title:  "ReplicationSet(MySQL)Secondary调优"
date:   2025-02-08 00:00:00 +0800
categories: default
---

在MySQL的ReplicationSet中, Secondary接收从Primary传来的binlog, 并且执行. 

Secondary所在的机器重启后, MySQL的实例状态变化: offline -> recovering -> online. 在一次重启后, 某Secondary一直处于recovering状态.

一些有用的M有SQL命令

```sql
show master status; # 查看Secondary状态
show processlist; # 查看MySQL进程
show full processlist; # 查看MySQL进程
show variables like 'slave_parallel_workers'; # 查看Secondary并行执行线程数
show variables like 'slave_parallel_type'; # 查看Secondary并行执行类型
show variables like 'slave_parallel_mode'; # 查看Secondary并行执行模式
select * from performance_schema.replication_connection_status; # 查看Secondary连接状态
select * from performance_schema.replication_group_members; # 查看ReplicationSet成员
select @@GTID_EXECUTED; # 查看GTID执行情况

```


