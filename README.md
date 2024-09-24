# A simple method to measure execution time in SQL Server

Timing SQL calls with millisecond accuracy

## Introduction

Measuring the execution time of a statement or function can be handy when performance tuning. SQL has a ton of features but I often fall back to rough-and-ready methods to at least narrow down the bit that needs optimising. Remember that the first step in optimising is working out what needs to be optimised.

## SQL Execution timing

With SQL Server 2008 Microsoft introduced the `DateTime2 `which provides higher accuracy than the `DateTime `type. While the accuracy is quoted as 100 nanoseconds, the [theoretical accuracy and practical accuracy differ](http://www.sqlphilosopher.com/wp/2012/10/precision-in-datetime-data-types/). 1ms is what you may end up with, which for us is fine.

To time SQL calls simply use 

```sql
Declare @StartTime DateTime2 = SysUTCDateTime()

-- my SQL calls

Print 'Time taken was ' + cast(DateDiff(millisecond, @StartTime, SysUTCDateTime()) as varchar) + 'ms'
```

For SQL Server versions below SQL Server 2008 you'll need to fall back to the traditional `DateTime`:

```sql
Declare @StartTime DateTime = GetDate()

-- my SQL calls

Print 'Time taken was ' + cast(DateDiff(millisecond, @StartTime, GetDate()) as varchar) + 'ms'
```

Note the use of `SysUTCDateTime `instead of `SysDateTime `for the case when you measure time over a daylight Saving time change. OK, rare, but possible! (Thanks Richard). For those working on SQL Server 2005 and under, you're stuck with `GetDate`.
