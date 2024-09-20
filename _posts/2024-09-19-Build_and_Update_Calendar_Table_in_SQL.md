---
title: Build and Update Calendar Table in SQL
date: 2024-09-19 20:35:00 +0500
categories: [MSSQL]
tags: [sql,calendar]
---
To build a calendar table in MSSQL that includes weekdays, months, quarters, biannual dates, days, numbered weeks, and days, you can use the following script:

```sql
-- Create a table to store the calendar data
CREATE TABLE Calendar (
    DateValue DATE PRIMARY KEY,
    DayOfWeek VARCHAR(10),
    Month VARCHAR(15),
    Quarter VARCHAR(10),
    BiannualDate VARCHAR(15),
    DayOfMonth INT,
    WeekOfMonth INT,
    WeekOfYear INT,
    DayOfYear INT
);

-- Populate the calendar table with data
DECLARE @StartDate DATE = '2023-01-01';
DECLARE @EndDate DATE = '2024-12-31';
DECLARE @CurrentDate DATE = @StartDate;

WHILE @CurrentDate <= @EndDate
BEGIN
    INSERT INTO Calendar (DateValue, DayOfWeek, Month, Quarter, BiannualDate, DayOfMonth, WeekOfMonth, WeekOfYear, DayOfYear)
    VALUES (
        @CurrentDate,
        DATENAME(WEEKDAY, @CurrentDate),
        DATENAME(MONTH, @CurrentDate),
        'Q' + CAST(DATEPART(QUARTER, @CurrentDate) AS VARCHAR(2)),
        'BA' + CASE WHEN MONTH(@CurrentDate) <= 6 THEN '1' ELSE '2' END,
        DATEPART(DAY, @CurrentDate),
        DATEPART(WEEK, @CurrentDate) - DATEPART(WEEK, DATEADD(MM, DATEDIFF(MM, 0, @CurrentDate), 0)) + 1,
        DATEPART(WEEK, @CurrentDate),
        DATEPART(DAYOFYEAR, @CurrentDate)
    );

    SET @CurrentDate = DATEADD(DAY, 1, @CurrentDate);
END;
```

This script will create a table called "Calendar" and populate it with data for each date between January 1, 2023, and December 31, 2024. The table includes columns for the date, day of the week, month, quarter, biannual date, day of the month, week of the month, week of the year, and day of the year.

To update the Calendar Table above, run the following query:

```sql
DECLARE @StartDate DATE = "Day after the last day in the table";
DECLARE @EndDate DATE = "Last day that you want to see in the calendar";
DECLARE @CurrentDate DATE = @StartDate;

WHILE @CurrentDate <= @EndDate
BEGIN
    INSERT INTO Calendar (DateValue, DayOfWeek, Month, Quarter, BiannualDate, DayOfMonth, WeekOfMonth, WeekOfYear, DayOfYear)
    VALUES (
        @CurrentDate,
        DATENAME(WEEKDAY, @CurrentDate),
        DATENAME(MONTH, @CurrentDate),
        'Q' + CAST(DATEPART(QUARTER, @CurrentDate) AS VARCHAR(2)),
        'BA' + CASE WHEN MONTH(@CurrentDate) <= 6 THEN '1' ELSE '2' END,
        DATEPART(DAY, @CurrentDate),
        DATEPART(WEEK, @CurrentDate) - DATEPART(WEEK, DATEADD(MM, DATEDIFF(MM, 0, @CurrentDate), 0)) + 1,
        DATEPART(WEEK, @CurrentDate),
        DATEPART(DAYOFYEAR, @CurrentDate)
    );

```

Please note that you can modify the @StartDate and @EndDate variables to generate data for a different date range if needed.