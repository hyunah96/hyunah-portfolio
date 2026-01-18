---
title: const, readonly
tags:
  - csharp
  - interview
date: 2026-01-13
---
## `const`, `readonly`
> 변하지 않는 값(상수)을 만들 때 자주 나오는 키워드가 `const`, `readonly`이다.  
> 둘 다 변경 불가처럼 보이지만 **언제 결정되는지(컴파일/런타임)** 와 **어디에 둘 수 있는지**, **참조형에서 의미**가 다르다.

### `const`
- **컴파일 타임 상수**
- `const`는 **컴파일 시점에 값이 확정**된다.
- **반드시 선언과 동시에 초기화**해야 한다.
- **컴파일 타임에 계산 가능한 값만 가능**하다.
- 기본적으로 **static처럼 동작**한다.
```csharp
class Config
{
    public const int Port = 502;
    public const string ProtocolName = "ModbusTCP";

    // public const DateTime Now = DateTime.Now; // 컴파일 에러 (런타임 값)
    
    // 빌드(컴파일)하는 순간에 값이 확정되어야 하는데 DateTime.Now는 컴파일 시점에 
    // 정확한 값을 결정할 수 없음
    // 런타임 값 : 프로그램이 실행 중일 때야 비로소 알 수 있는 값
}
```
### readonly
- **런타임 상수**
- `readonly`는 **런타임에 값이 결정**될 수 있다.
- 선언 시 초기화하거나, **생성자에서 딱 한 번** 값 할당 가능하다.
- 필드에만 쓸 수 있다.(지역 변수에는 X)
- `readonly`는 참조형에서 **재할당만 막고 내부 변경은 막지 않는다.**
##### readonly를 쓸 만한 상황
- 실행할 때 정해지는 값인데, **정해진 이후엔 바뀌면 안 되는 값**
**선언 시 초기화**
```csharp
public readonly string MachineName = "Default";
```

**생성자에서 값 할당**
```csharp
using System;

class Config
{
    // 인스턴스마다 한 번 정해지면 바뀌지 않는 값
    public readonly string MachineName;
    public readonly DateTime CreatedAt;

    public Config(string machineName)
    {
        // 생성자에서만 1회 할당 가능
        MachineName = machineName;
        CreatedAt = DateTime.Now;
    }

    public void Change()
    {
        // 생성자 밖에서는 재할당 불가
        // MachineName = "Other";
        // CreatedAt = DateTime.Now;
    }
}

class Program
{
    static void Main()
    {
        var c1 = new Config("M300");
        Console.WriteLine($"{c1.MachineName}, {c1.CreatedAt}");

        var c2 = new Config("M400");
        Console.WriteLine($"{c2.MachineName}, {c2.CreatedAt}");
    }
}
```

**정리**
`const`는 **컴파일 타임 상수**라 선언과 동시에 초기화해야 하고 컴파일 시점에 결정되는 값만 가능합니다.  
`readonly`는 **런타임 상수**라 선언 시 또는 **생성자에서 한 번만** 값을 설정할 수 있고, 이후 재할당이 불가능합니다.  
하지만 참조형에서는 `readonly`는 **재할당만 막고** 내부 변경은 가능합니다.
내부 변경까지 막고 싶을 때는 불변타입(`string`, `DateTime`)을 사용하거나 캡슐화로 필드는 private로 숨기고, 밖에는 **읽기 전용으로만 제공**하면 됩니다.