---
title: event, delegate
tags:
  - csharp
  - interview
date: 2026-01-15
---
## `event`, `delegate`

> C#에서 `delegate`는 **메서드를 담는 타입** 이고,  
> `event`는 그 `delegate`를 **구독/발행 용도로 안전하게 쓰도록 제한한 키워드**다.  
> `EventHandler` / `EventHandler<TEventArgs>`는 .NET에서 가장 흔히 쓰는 **표준 이벤트 delegate 형식**이다.


### `delegate`
- **메서드에 대한 참조를 저장하는 타입**
- 선언한 시그니처(반환형/매개변수)가 **완전히 같은 메서드만** 연결할 수 있다.
- `delegate` 변수는
    - `=`로 **통째 대입(덮어쓰기)** 가능
    - `+=`, `-=`로 **여러 메서드 연결/해제** 가능(멀티캐스트)
    - **직접 호출** 가능
```csharp
using System;

public delegate int Calc(int a, int b);

class Program
{
    static int Plus(int a, int b) => a + b;
    static int Minus(int a, int b) => a - b;

    static void Main()
    {
        Calc calc = Plus;
        Console.WriteLine(calc(5, 10));   // 15

        calc = Minus;                      // 통째로 덮어쓰기 가능
        Console.WriteLine(calc(20, 10));  // 10
    }
}
```
내일..ㄱ