---
layout: post
title: "Oracle Date 类型数据的那些事"
date: 2019-05-14 23:00:00 +0800
categories: Daily
tags: daily
img: ../images/blog/20190514/head.jpg
describe: Oracle Date 类型数据的各种处理方法
---

# 1.时间数据入库

##### 时间字符串入库方法：

```sql
--时间字符串 2019-05-14 10:49:43.0 入库
INSERT INTO TEST_TABLE 
	(DATE_COL) 
VALUES
	(CAST(TO_TIMESTAMP('2019-05-14 10:49:43.0','YYYY-MM-DD HH24:MI:SS.FF') AS DATE))
```

- 如果时间字符串为其他格式，可以调整时间字符串匹配值（YYYY-MM-DD HH24:MI:SS.FF）为其他的表达形式，如：“YYYYDDHH24MI”，可以匹配“2019141049”时间字符串

- 需要注意的是，由于Oracle不区分大小写，因此时间字符串匹配值的""月份""表示为"MM";“分钟”表示为“MI”

##### 查询某一时间范围内的数据：

```sql
--查询更新时间为25分钟前的数据
--时间线举例：PASTTIME------UPDATETIME--------SYSDATE-25/(24*60)-------SYSDATE
SELECT * FROM TEST_TABLE T WHERE T.UPDATETIME <= ( SYSDATE-25/(24*60) )
--查询更新时间为3小时前的数据
SELECT * FROM TEST_TABLE T WHERE T.UPDATETIME <= ( SYSDATE-3/(24) )
```

- 分母为“24”时，表示含义为“小时”粒度，如果是“24*60”，表示含义为“分钟”粒度，以此类推