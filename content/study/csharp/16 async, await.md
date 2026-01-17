---
title: async, await
tags:
  - csharp
  - await
  - async
date: 2025-11-25
---
## async, await
> `async/await`는 **작업을 기다리는 동안 스레드를 막지 않고**, 작업이 끝나면 **이어서 실행**하게 만드는 것을 의미한다.

### `async`
- 메서드에 `async`를 붙이면 **이 메서드 안에서 await를 쓸 수 있다**는 표시다.
- `async` 자체가 비동기로 돌려준다기보다는 **await를 가능하게 해주는 문법 표시**에 가깝다.
### `await`
- `await`는 `Task`가 **끝날 때까지 기다렸다가**, **다음 줄부터 이어서 실행**한다.
- 기다리는 동안 프로그램이 **멈춘 것처럼 보이지만**, 실제로는 **스레드를 붙잡고 기다리는 게 아니다**.
##### `await` 예제 
```csharp
using System;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        Task t = Task.Delay(1000); // 시작만 함(아직 안 기다림)
        Console.WriteLine("바로 출력");

        await t; // 여기서 기다림
        Console.WriteLine("1초 후 출력");
    }
}
```
### `Task` / `Task<T>`
- `Task` : **나중에 끝날 작업(결과 없음)** 을 표현
- `Task<T>` : **나중에 끝날 작업(결과 T 있음)** 을 표현
##### `Task` 예제 (결과 없음)
```csharp
using System;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        await WorkAsync();
    }

    static async Task WorkAsync()
    {
        await Task.Delay(300);
        Console.WriteLine("작업 완료");
    }
}
```
##### `Task<T>` 예제 (결과 있음)
```csharp
using System;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        int sum = await AddAsync(3, 5);
        Console.WriteLine(sum); // 8
    }

    static async Task<int> AddAsync(int a, int b)
    {
        await Task.Delay(300);
        return a + b;
    }
}
```