---
title: SQL Scheduled Jobs Query
date: 2024-4-1 21:05:00 +0500
categories: [ MSSQL]
tags: [sql, scheduled jobs, history]
---

Certainly! Here's an extended query that includes the requested information for SQL Server scheduled jobs:

```sql
SELECT 
    job.name AS JobName,
    job.job_id AS JobID,
    step.step_name AS StepName,
    step.database_name AS StepDatabase,
    step.command AS StepCommand,
    job.notify_level_email,
    job.notify_email_operator_id,
    job.enabled AS IsEnabled,
    job.description AS JobDescription,
    job.date_created AS DateCreated,
    job.date_modified AS DateModified,
    sjh.run_date AS LastRunDate,
    sjh.run_duration AS LastRunDuration,
    jobSchedules.schedule_id AS ScheduleID,
    schedule.name AS ScheduleName,
    schedule.freq_type AS FrequencyType,
    schedule.freq_interval AS FrequencyInterval,
    schedule.active_start_date AS ActiveStartDate,
    schedule.active_end_date AS ActiveEndDate,
    schedule.active_start_time AS ActiveStartTime,
    schedule.active_end_time AS ActiveEndTime
FROM msdb.dbo.sysjobs job
INNER JOIN msdb.dbo.sysjobsteps step ON job.job_id = step.job_id
LEFT JOIN msdb.dbo.sysjobschedules jobSchedules ON job.job_id = jobSchedules.job_id
LEFT JOIN msdb.dbo.sysschedules schedule ON jobSchedules.schedule_id = schedule.schedule_id
LEFT JOIN msdb.dbo.sysjobhistory sjh ON job.job_id = sjh.job_id
ORDER BY job.name, step.step_id;
```

This query retrieves information about each scheduled job, including details about each step, email notifications, job schedules, and the last run details. You can export the results as mentioned earlier to a CSV file using SSMS or other tools.