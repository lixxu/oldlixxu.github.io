---
layout: post
title: "PostgreSQL 中删除重复记录"
date: 2015-04-08 11:20:20 +0800
comments: true
categories: database
tags: PostgreSQL database
---

来源: <http://stackoverflow.com/questions/1746213/how-to-delete-duplicate-entries>

有的时候我们对一个表不会加唯一性限制, 而是在插入数据时进行检查. 但有的时候程序里的 bug 导致还是有重复的记录被插入到数据库中了, 这时候如何快速地删除重复行呢? 

显然, 用程序来删除会比较耗时, 尤其是记录比较多的时候.
以下的方法只适用于 `PostgreSQL`, 因为不是标准的 SQL 语法.

```
DELETE FROM table USING table alias 
  WHERE table.field1 = alias.field1 AND table.field2 = alias.field2 AND
    table.max_field < alias.max_field
```

比如, 有一个用户表, email 字段本该是唯一的, 需要删除重复的 email 记录.
```
DELETE FROM user_accounts USING user_accounts ua2
WHERE user_accounts.email = ua2.email AND user_accounts.id < ua2.id;
```
