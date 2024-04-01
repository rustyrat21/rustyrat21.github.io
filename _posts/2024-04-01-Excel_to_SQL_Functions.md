---
title: Excel to SQL Functions
date: 2024-4-1 21:05:00 +0500
categories: [ MSSQL, Excel]
tags: [sql,excel,functions]
---
## Sumif statement
### Excel Function
```excel 
=sumif(range, criteria, [sum_range])

=sumif(B:B,A2,C:C)
```
### SQL statement
```sql
select
sum(case when <condition> then <actions> end)
from db.dbo.table

Select
	sum(case when field1 = 100 then 1 end)
from db.dbo.table
```


