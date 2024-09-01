---
title: "Guid Ordering in Csharp and in DB"
date: 2024-09-01T19:00:39+08:00
draft: false
description: ""
---

Say we have a GUID `aaaaaaaa-bbbb-cccc-ddee-ffgghhiijjkk`

The way .NET and SQL Server compare GUIDs are different.  

In .NET it will start the comparision from `a`, `b`.....all the way till `k`.
However in SQL Server, it will take `f~k` to compare first, then `d~e`, then `c`, `b`, lastly `a`.

If we want to achive the same comparing result in both DB and application server, .NET has provided a struct called `SqlGuid`, this struct implement different `ComparedTo` logic.  
We can convert our `Guid()` to `SqlGuid()` then run the sorting, this way you can get the same result as DB returned.


---

Refenrence:
1. [Dark Thread - 冷知識 - Guid 如何排序？](https://blog.darkthread.net/blog/guid-sorting/)
2. [Microsoft Official Document - Comparing GUID and uniqueidentifier values](https://learn.microsoft.com/zh-tw/sql/connect/ado-net/sql/compare-guid-uniqueidentifier-values?view=sql-server-ver16)