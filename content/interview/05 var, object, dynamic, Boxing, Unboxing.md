---
title: var, object, dynamic + Boxing/Unboxing
tags:
  - csharp
  - interview
date: 2026-01-13
---
## `var`, `object`, `dynamic`

>C#에서 변수에 값을 담을 때 자주 나오는 키워드가 `var`, `object`, `dynamic`이다. 각각 **타입이 결정되는 시점**과 **검사 방식**이 다르다.

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
  **값형**(int/bool/struct .. ),**참조형**(class/string/array/List/Dictionary ..)
- 하지만 `object`로 담으면 컴파일러는 `object`로만 알기 때문에 꺼내서 사용할 때 **캐스팅이 필요**한 경우가 많다.
- 특히 **값형(`int`, `bool`, `struct`)** 을 `object`에 담으면 **박싱(`Boxing`)** 이 발생할 수 있다.
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
- `dynamic`은 **컴파일러가 타입 검사를 미루고, 실행 중(런타임)에 실제 들어있는 값의 타입을 보고 결정한다.**
- 그래서 캐스팅 없이도 되는 것처럼 보이지만 잘못된 멤버 호출, 오타, 없는 메서드는 **런타임 에러**가 수 있다.
- `dynamic`은 **어떤 타입이든 다시 담을 수 있게 허용한다.**
```csharp
using System;

class Program
{
    static void Main()
    {
        dynamic d = 10;
        Console.WriteLine(d + 1); // 11 (런타임에 int로 동작)
		// 어떤 타입이든 다시 담을 수 있음
        d = "Hello";
        Console.WriteLine(d.Length); // 5 (런타임에 string으로 동작)
    }
}
```

#### `var` / `object` / `dynamic` 요약

- `var`
    - 타입 결정: **컴파일 타임**
    - 캐스팅: 필요 없음(이미 타입이 확정)
    - 특징: 가독성용 문법, **동적 아님**
- `object`
    - 타입 결정: 컴파일 타임에는 `object`, 실제 값은 런타임에 들어감
    - 캐스팅: 보통 **필요**
    - 특징: 값형 담으면 **박싱/언박싱** 연결
- `dynamic`
    - 타입 결정: **런타임**
    - 캐스팅: 겉으로는 필요 없어 보임
    - 특징: 컴파일러 검사가 약해져서 **런타임 에러 위험**
## `Boxing`, `Unboxing`

> **Boxing**: 값형을 `object`로 변환하면서 **힙에 새 객체를 만들고 값이 복사되는 과정**
> **Unboxing**: 박싱된 `object` 안의 값을 **원래 값형으로 꺼내는 과정**

```csharp
using System;

class Program
{
    static void Main()
    {
        int a = 123;

        object box = a;     // 박싱: int → object
        int b = (int)box;   // 언박싱: object → int

        Console.WriteLine(b); // 123
    }
}
```
- 박싱은 힙 할당이 생길 수 있어 **GC 부담/성능 저하**가 발생할 수 있다.
- 반복문에서 박싱이 계속 일어나면 성능 이슈가 커질 수 있다.
#### `Boxing`하면 힙 할당이 생기는 이유
- 값형(`Value Type`)의 변수가 저장되는 위치는 `stack`메모리에 위치하고, 
  참조형(`Reference type`)의 객체는 `heap` 메모리에 위치한다.
  (참조값은 지역 변수면 `stack`, 필드면 `heap` 위치)
- `Boxing`이 발생할 때 `heap`에 공간을 할당해서(`box`) `stack`에 있는 값을 복사해 넣는다.
- `stack` 메모리에서는 값이 저장되어있는 객체를 가리키는 `heap` 메모리의 주소를 저장하게 됩니다.
#### `Unboxing`에서 캐스팅
- `Unboxing`은 **박싱된 `object` 안에 들어있는 값형 값을 다시 꺼내는 과정**이다.  
- 이때 `object`는 무슨 타입이 들어있는지를 컴파일 타임에 확정할 수 없기 때문에 **명시적으로 타입을 지정(캐스팅)** 해줘야 한다.
```csharp
object obj = 10;   // Boxing: int -> object

int x = (int)obj;  // (int)가 캐스팅 문법 + 이 순간 Unboxing 발생
Console.WriteLine(x); // 10
```