---
title: 几种常见SQL分页方式
date: 2017-12-23 12:37:03
tags: 技术
---

## 几种典型的分页sql，下面例子是每页50条，198*50=9900，取第199页数据

### 1.not in/top

```sql
select top 50 * from pagetest 
where id not in (select top 9900 id from pagetest order by id)
order by id
```

### 2.not exists

```sql
select top 50 * from pagetest 
where not exists 
(select 1 from (select top 9900 id from pagetest order by id)a  where a.id=pagetest.id)
order by id
```

### 3.max/top

```sql
select top 50 * from pagetest
where id>(select max(id) from (select top 9900 id from pagetest order by id)a)
order by id
```

### 4.row_number()

```sql
select top 50 * from 
(select row_number()over(order by id)rownumber,* from pagetest)a
where rownumber>9900

select * from 
(select row_number()over(order by id)rownumber,* from pagetest)a
where rownumber>9900 and rownumber<9951

select * from 
(select row_number()over(order by id)rownumber,* from pagetest)a
where rownumber between 9901 and 9950

select *
from (
    select row_number()over(order by tempColumn)rownumber,*
    from (select top 9950 tempColumn=0,* from pagetest where 1=1 order by id)a
)b
where rownumber>9900
```

>https://www.cnblogs.com/songjianpin/articles/3489050.html

>http://bbs.csdn.net/topics/340134909