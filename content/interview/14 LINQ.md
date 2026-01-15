---
title: LINQ
tags:
  - csharp
  - interview
  - LINQ
date: 2026-01-15
---
## LINQ (Language Integrated Query)

> **LINQ**는 C# 안에서 컬렉션, 데이터를 **쿼리 형식으로 조회**할 수 있게 해주는 기능이다.  
> (대부분 `IEnumerable<T>` 위에서 동작하고, 보통 **지연 실행** 특성이 있다.)

---
### 쿼리 구문(Query Syntax)
```csharp
using System;
using System.Linq;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        var nums = new List<int> { 1, 2, 3, 4, 5 };

        var evens =
            from n in nums
            where n % 2 == 0
            select n * 10;

        foreach (var n in evens)
            Console.WriteLine(n);
    }
}
```
### 메서드 구문(Method Syntax) 
```csharp
using System;
using System.Linq;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        var nums = new List<int> { 1, 2, 3, 4, 5 };

        var evens = nums
            .Where(n => n % 2 == 0)   // 필터
            .Select(n => n * 10);     // 변환

        foreach (var n in evens)
            Console.WriteLine(n);     // 20, 40 출력
    }
}
```
### LINQ 장점

#### 1) 코드 가독성 향상
- LINQ는 데이터 처리 로직을 간결하고 읽기 쉽게 만들어준다. 복잡한 데이터 처리를 몇 줄의 쿼리로 표현할 수 있으며 SQL 문법과 유사하여 쿼리 작성을 쉽게 이해할 수 있다.
#### 2) 통일된 데이터 쿼리 방식
- LINQ는 컬렉션, 데이터베이스, XML 등 다양한 데이터 소스를 대상으로 통일된 방식으로 쿼리를 작성할 수 있다.
#### 3) 타입 안정성
- LINQ 쿼리는 타입 안정성을 보장한다. 쿼리에서 사용되는 데이터 타입이 컴파일 시에 체크되므로 런타임 오류를 줄이고 안전한 코드를 작성할 수 있다.

### LINQ 단점
#### 1) 성능 문제

- **지연 실행**
  LINQ는 기본적으로 **지연 실행**을 사용한다. 이는 쿼리가 바로 실행되지않고 **열거**가 일어나는 순간에 실행되기 때문에 상황에 따라 성능 문제를 야기할 수 있다.
  
  - **큰 데이터셋에서의 성능 저하**
    LINQ는 간결하고 읽기 쉬운 코드를 제공하지만, 대량의 데이터 처리 시에는 성능이 저하될 수 있다. 특히 반복적으로 데이터를 변환하거나 복잡한 필터링을 수행할 때 전통적인 반복문에 비해 느리게 동작할 수 있다.
#### 2) 복잡한 쿼리에서의 가독성 문제
- LINQ는 단순한 쿼리에서는 코드의 가독성을 높여주지만, 쿼리가 복잡해질수록 가독성은 떨어진다. 특히 중첩된 쿼리를 사용하는 경우 코드가 길고 복잡해져서 유지보수가 어려워진다.
#### 3) 디버깅
- 쿼리가 길면 중간 결과를 눈으로 확인하기 어렵고, 지연 실행으로 인해 쿼리가 언제 실행되는지, 어디서 오류가 발생하는지 명확하게 파악하기 어려울 수 있다.
#### 4) 메모리 사용 증가
- `ToList()`,`ToArray()` 같은 즉시 실행 메서드를 사용하면 메모리에 많은 데이터를 로드할 수 있어 메모리 부족 문제를 유발할 수 있다.