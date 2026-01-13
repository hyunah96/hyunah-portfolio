---
title: var, object, dynamic + Boxing/Unboxing
tags:
  - csharp
  - interview
date: 2026-01-13
---
## `var`, `object`, `dynamic` + Boxing/Unboxing

>C#에서 변수에 값을 담을 때 자주 나오는 키워드가 `var`, `object`, `dynamic`이다. 각각 **타입이 결정되는 시점**과 **검사 방식**이 다르다.
>`var` → **컴파일 타임에 타입이 확정**되는 타입 추론
>`object` → 모든 타입을 담을 수 있는 **최상위 타입**
>`dynamic` → 타입 검사를 **런타임으로 미루는 방식**
>`object`/`dynamic`에 값형을 담았다 꺼낼 때는 **Boxing/Unboxing 비용**까지 같이 이해해야 한다.

### `var`
- `var`는 **컴파일러가 오른쪽 값을 보고 타입을 결정**한다.
- `var`로 선언해도 변수는 **강타입**이며, 타입이 한 번 정해지면 바뀌지 않는다.
- **선언과 동시에 초기화 필수**
```csharp
using System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        var a = 10;                  // int로 결정
        var b = "Hello";             // string으로 결정
        var list = new List<int>();  // List<int>로 결정

        // a = "hi";   // 컴파일 에러: a는 int로 확정됨
        // var x;      // 컴파일 에러: 초기화 없으면 추론 불가

        Console.WriteLine(a.GetType());   // System.Int32
        Console.WriteLine(b.GetType());   // System.String
    }
}
```
### `object
- `object`는 C#의 **최상위 타입**이라 어떤 타입이든 담을 수 있다.
- 하지만 `object`로 담으면 컴파일러는 **object**로만알기 때문에 꺼내서 사용할 때 **캐스팅이 필요**한 경우가 많다.
- 특히 **값형(int, bool, struct)** 을 `object`에 담으면 **박싱(Boxing)** 이 발생할 수 있다.
```csharp
using System;

class Program
{
    static void Main()
    {
        object o1 = 10;        // int 값형이 object로 들어감 → 박싱 발생
        object o2 = "Hello";   // 참조형은 참조만 담김(박싱 X)

        // Console.WriteLine(o1 + 1); // 컴파일 에러: object는 연산 불가

        int n = (int)o1;       // 언박싱 + 캐스팅
        Console.WriteLine(n + 1); // 11
    }
}
```
### `dynamic`
- `dynamic`은 **컴파일러가 타입 검사를 미루고**, 실행 중(런타임)에 결정한다.
- 그래서 캐스팅 없이도 되는 것처럼 보이지만 잘못된 멤버 호출, 오타, 없는 메서드는 **런타임 에러**가 수 있다.
```csharp
using System;

class Program
{
    static void Main()
    {
        dynamic d = 10;
        Console.WriteLine(d + 1); // 11 (런타임에 int로 동작)

        d = "Hello";
        Console.WriteLine(d.Length); // 5 (런타임에 string으로 동작)

        // Console.WriteLine(d.NotExist()); // 컴파일은 되지만 런타임에서 예외 발생 가능
    }
}
```
여기서부턴 내일 수정
준비 잘하자