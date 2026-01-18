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

        await t; // 타이머가 끝날 때까지 대기
        Console.WriteLine("1초 후 출력");
    }
}
```
- `Task t = Task.Delay(5000)`5초짜리 타이머 작업을 시작
- 다음 줄 `Console.WriteLine("바로 출력")`은  **기다리지 않고 즉시 실행**
- 이 둘은 완전히 독립이라기보다 **타이머(Task)는 5초 동안 진행 중이고 현재 코드는 멈추지 않고 다음 줄로 계속 진행**하는 **동시에 진행되는 상태**
- `Task.Delay`는 **타이머를 돌려놓는 것**, 실제로 **기다림을 만드는 건** `await`

### `Task` / `Task<T>`
- `Task` : **나중에 끝날 작업(결과 없음)** 을 표현
- `Task<T>` : **나중에 끝날 작업(결과 T 있음)** 을 표현
- `Task.Delay` = 보통 **스레드 추가 없음(타이머 예약)**  
- `Task.Run` = 보통 **다른 스레드에서 실행될 수 있음(스레드풀 사용)**
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
- 앱이 시작되면 `Main()`이 실행되고, `Main()` 안에서 `WorkAsync()`를 **호출한다.**
```csharp
static async Task WorkAsync()
    {
        await Task.Delay(300);
        Console.WriteLine("작업 완료");
    }
```  
- 위 `Task`가 완료되면, `WorkAsync()`는 `Task`를 반환하고 `await WorkAsync()`는 반환을 받으면 다음 줄로 넘어간다.

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
        await Task.Delay(3000);
        return a + b;
    }
}
```
**실행 순서**
- `Main()`에서 `AddAsync(3, 5)` 호출
- `AddAsync` 내부 실행 시작
- `await Task.Delay(300);`에서 3초 타이머 `Task`를 시작하고, 그동안 `AddAsync`는 **일시 중단**됨
- `Task.Delay(300)` 완료되면 다음 줄로 재개 → `return a + b;` 실행
- 결과 `8`이 만들어지고, `Task<int>`가 **완료 상태가 됨**
- `Main()`의 `await AddAsync(3, 5)`가 **8을 받아옴** → `sum`에 저장
- `Console.WriteLine(sum);` 실행 → `8` 출력
- `Main()` 종료 



https://wjunsea.tistory.com/139