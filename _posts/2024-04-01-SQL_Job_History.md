---
title: SQL Job History
date: 2024-4-1 21:05:00 +0500
categories: [ MSSQL]
tags: [sql, job, history]
---

The following query can be used to retrieve the history for a scheduled job based on its name:

```sql
USE msdb;

DECLARE @JobName NVARCHAR(128) = 'YourJobName';  -- Replace 'YourJobName' with the actual job name

SELECT
    j.name AS JobName,
    h.run_status,
    h.run_date,
    h.run_duration
FROM
    dbo.sysjobs j
INNER JOIN
    dbo.sysjobhistory h ON j.job_id = h.job_id
WHERE
    j.name = @JobName
ORDER BY
    h.run_date DESC, h.run_time DESC;
```

Replace 'YourJobName' with the actual name of the job you're interested in. This query joins the `sysjobs` and `sysjobhistory` tables in the `msdb` database to fetch job history details based on the job name, ordering the results by run date and time in descending order.

This will depend on how much history is stored within the sysjobhistory table.  