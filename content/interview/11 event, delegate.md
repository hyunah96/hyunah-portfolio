---
title: event, delegate
tags:
  - csharp
  - interview
date: 2026-01-15
---
## `event`, `delegate`

> `delegate`는 **메서드를 담는 타입** 이고, `event`는 그 `delegate`를 **구독/발행 용도로 안전하게 쓰도록 제한한 키워드**다.  


### `delegate`
- **메서드에 대한 참조를 저장하는 타입**
- 선언한 시그니처(반환형/매개변수)가 **완전히 같은 메서드만** 연결할 수 있다.
- `delegate` 변수는
    - 외부에서도 `=`로 **통째 대입(덮어쓰기)** 가능 
    - 외부에서도 `+=`, `-=`로 **여러 메서드 연결/해제** 가능(멀티캐스트)
    - 외부에서도 **직접 호출** 가능(`Invoke`)
```csharp
using System;

public delegate int Calc(int a, int b);

class Program
{
    static int Plus(int a, int b) => a + b;
    static int Minus(int a, int b) => a - b;

    static void Main()
    {
        Calc calc = Plus; //델리게이트 변수 calc가 Plus 메서드를 참조
        Console.WriteLine(calc(5, 10));   // 15

        calc = Minus;                      // calc가 Plus -> Minus로 참조 교체
        Console.WriteLine(calc(20, 10));  // 10
    }
}
```
##### delegate 멀티캐스트
- `+=`는 **덮어쓰기가 아니라 기존 목록에 추가하는 것이다.**
- `p = A;` → **A만 가리킴(교체)**
- `p += A;` → **기존 목록에 A를 추가(누적)**
```csharp
using System;

public delegate void Print(string msg);

class Program
{
    static void A(string msg) => Console.WriteLine("A: " + msg);
    static void B(string msg) => Console.WriteLine("B: " + msg);

    static void Main()
    {
        Print p = null;
        p += A;
        p += B;

        p?.Invoke("Text"); // A, B 순서대로 호출
    }
}
```
##### `Invoke`
- `delegate`를 실행하는 정식 메서드 호출
- 아래 두 개는 같은 의미다.
```csharp
p("Text");
p.Invoke("Text");
```
### `event`
- 이벤트는 어떤 일이 일어났음을 알리는 **알림(메시지)이며** `delegate`타입을 기반으로 만들어진다.
- `delegate` 타입으로는 `Action`을 쓰는 경우가 많다.
  ex) 버튼 클릭, 값 변경, 작업 완료 등
- `event` 멤버: 외부에서 `+=`, `-=`**만 가능** (구독/해제만)
	- - `+=` 로 구독 등록
	- `-=` 로 구독 해제
    - 외부에서 `=`로 덮어쓰기 **불가능** (내부 클래스에서는 가능)
    - 외부에서 직접 호출(`Invoke`) **불가능** (내부 클래스에서는 가능)
```csharp
using System;

class Counter
{   // event를 붙이면 ThresholdReached는 이벤트가 됨
    public event Action ThresholdReached; 

    public void Add(int total, int threshold)
    {
        if (total >= threshold)
            ThresholdReached?.Invoke();   // 이벤트 발생은 클래스 내부에서만
    }
}

class Program
{
    static void Main()
    {
        var c = new Counter();

        c.ThresholdReached += () => Console.WriteLine("도달!");
        // c.ThresholdReached();           // 외부에서 호출 불가
        // c.ThresholdReached = ...;       // 외부에서 대입 불가
        c.Add(total: 10, threshold: 5);
    }
}
```
##### `() => Console.WriteLine("도달!")` 
이건 **매개변수 없는 함수 하나를 즉석에서 만든 것**으로 람다식이다.
**이벤트가 발생할 때** 출력된다.
- `()` : 매개변수 없음
- `=>` : 이렇게 실행해라
- `Console.WriteLine("도달!")` : 실행 내용
##### 흐름 순서
- `c.Add(total: 10, threshold: 5);` 호출
- `if (total >= threshold)` → `10 >= 5` 이므로 참
- `ThresholdReached?.Invoke();` 실행
- 구독해둔 람다식 `() => Console.WriteLine("도달!")` 호출
- 콘솔에 **도달!** 출력