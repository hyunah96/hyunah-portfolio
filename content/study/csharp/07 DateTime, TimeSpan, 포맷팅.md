---
title: DateTime, TimeSpan, 포맷팅
tags:
  - csharp
date: 2025-11-10
---
### DateTime
- **날짜, 시간**을 다루는 구조체
- 현재 시간, 특정 날짜, 날짜 계산 등에 사용된다.
#### 자주 쓰는 속성(프로퍼티)
- `DateTime.Now` : 로컬 시간(PC 시간대 기준)
- `DateTime.UtcNow` : 전 세계 공통 기준 시간(UTC) 현재 날짜, 시간
- `DateTime.Today` : 오늘 하루의 시작(자정)을 나타내는 값
```csharp
using System;

class Program
{
    static void Main()
    {
        DateTime now = DateTime.Now;
        DateTime utc = DateTime.UtcNow;
        DateTime today = DateTime.Today;

        Console.WriteLine(now); //2025 - 11 - 10 오후 6:21:24
        Console.WriteLine(utc); //2025 - 11 - 10 오전 9:21:24
        Console.WriteLine(today); //2025 - 11 - 10 오전 12:00:00
    }
}
```

#### DateTime 포맷팅 (ToString)
- 표준 포맷
```csharp
using System;

class Program
{
    static void Main()
    {
        DateTime now = DateTime.Now;

        Console.WriteLine(now.ToString("yyyy-MM-dd"));
        Console.WriteLine(now.ToString("yyyy-MM-dd HH:mm:ss"));
        Console.WriteLine(now.ToString("yyyyMMdd_HHmmss"));
    }
}
```


#### DateTime 생성
- `new DateTime(년, 월, 일)`, `new DateTime(년, 월, 일, 시, 분, 초)` 형태로 생성 가능하다.
- `Year/Month/Day/Hour...` 로 접근 가능하다.
```csharp
using System;

class Program
{
    static void Main()
    {
        DateTime dt1 = new DateTime(2025, 11, 10);   // 2025-11-10 00:00:00
        DateTime dt2 = new DateTime(2025, 11, 10, 14, 30, 0); 
        // 2025-11-10 14:30:00

        Console.WriteLine(dt1);
        Console.WriteLine(dt2);

        Console.WriteLine(dt2.Year);
        Console.WriteLine(dt2.Month);
        Console.WriteLine(dt2.Day);
        Console.WriteLine(dt2.Hour);
        Console.WriteLine(dt2.Minute);
        Console.WriteLine(dt2.Second);
    }
}
```


#### DateTime 연산(Add / Subtract)
- `AddXxx()`로 더하고, `AddXxx(-값)`으로 뺄 수 있다.
- 또는 `Subtract(TimeSpan)`으로도 뺄 수 있다.
**Add 예제**
```csharp
using System;

class Program
{
    static void Main()
    {
        DateTime now = DateTime.Now;

        DateTime after10Min = now.AddMinutes(10);
        DateTime yesterday = now.AddDays(-1);
        DateTime nextMonth = now.AddMonths(1);

        Console.WriteLine($"now: {now}");
        Console.WriteLine($"+10 min: {after10Min}");
        Console.WriteLine($"-1 day: {yesterday}");
        Console.WriteLine($"+1 month: {nextMonth}");
    }
}
```
### TimeSpan
- **시간의 간격(기간, duration)** 을 표현하는 타입이다.
- `Subtract`는 숫자만 바로 넣을 수는 없고, `TimeSpan`(시간 간격)을 넣어야 한다.

**Subtract 예제**
```csharp
using System;

class Program
{
    static void Main()
    {
        DateTime now = DateTime.Now;

        DateTime before10Min = now.Subtract(TimeSpan.FromMinutes(10));
        DateTime yesterday = now.Subtract(TimeSpan.FromDays(1));
        DateTime lastMonth = now.Subtract(TimeSpan.FromDays(30)); 

        Console.WriteLine($"now: {now}");
        Console.WriteLine($"-10 min: {before10Min}");
        Console.WriteLine($"-1 day: {yesterday}");
        Console.WriteLine($"-30 days: {lastMonth}");
    }
}
```
