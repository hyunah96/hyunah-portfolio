---
title: Async, Thread
tags:
  - csharp
  - interview
  - async
  - Thread
date: 2026-01-17
---
## `Async`, `Thread`

> **Async(비동기)**는 “기다림(I/O 대기)을 효율적으로 처리하는 방식이고,  
> Thread(스레드)** 는 **코드를 실제로 실행하는 실행 단위**이다.  
> **async/await = 스레드 생성이 아니다.**


### `Async`
- 보통 **I/O 작업**(파일/네트워크/DB처럼 **기다림**이 많은 작업)에서 효과가 크다.
- `await`를 만나면 **작업이 끝날 때까지 중단했다가**, 끝나면 **이어서 실행한다.**
- **작업을 기다리는 동안 스레드를 붙잡고 있지 않는다.**

```csharp
using System;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        Console.WriteLine("A");
        await Task.Delay(1000); // 1초 타이머(블로킹 X)
        Console.WriteLine("B");
    }
}
```
- `Task.Delay`는 1초 타이머를 **예약**해두고, `await`가 **끝날 때까지 기다렸다가** 다음 줄(`B`)로 넘어감

### `Thread`
- CPU가 실제로 코드를 실행하는 **실행 흐름 1줄**
- 스레드를 새로 만들면 **실제 실행 단위가 늘어난다.**
- `Thread.Sleep`은 **현재 스레드를 그대로 멈춰버린다.(블로킹)**

```csharp
using System;
using System.Threading;

class Program
{
    static void Main()
    {
        Console.WriteLine("A");
        Thread.Sleep(1000); // 현재 스레드 1초 정지(블로킹)
        Console.WriteLine("B");
    }
}
```
- **메인 스레드(Main Thread)** 가 `Main()`을 실행
- `Thread.Sleep(1000)`을 만나면 **메인 스레드 자체가 1초 멈춘다.**
##### 새 스레드를 만들어서 동시에 실행
```csharp
using System;
using System.Threading;

class Program
{
    static void Main()
    {
        var t = new Thread(() =>
        {
            Thread.Sleep(1000);
            Console.WriteLine("작업 스레드: 1초 후");
        });

        t.Start();
        Console.WriteLine("메인 스레드: 바로 출력");
        t.Join(); // 작업 스레드 끝날 때까지 기다림
    }
}
```
1. **메인 스레드(Main Thread)**
	- `Main()` 안의 코드 실행
	- `t.Start()` 호출하고 바로  `Console.WriteLine("메인 스레드: 바로 출력")` 실행
2. **작업 스레드(새로 만든 Thread t)**
	- `t.Start()`가 호출되는 순간 시작됨
	- 람다 안의 코드 실행 `Thread.Sleep(1000)` → 1초 후 `"작업 스레드: 1초 후"` 출력
##### 즉 별도의 스레드라서 독립적으로 동시에 실행된다.