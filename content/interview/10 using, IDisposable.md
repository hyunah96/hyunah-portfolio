---
title: using, IDisposable
tags:
  - csharp
  - interview
date: 2026-01-15
---
## `using` / `IDisposable` 개념 (자원 해제)

> C#에서 `new`로 만든 객체는 GC가 언젠가 메모리를 회수해주지만,  
> **파일/소켓/DB 연결 같은 자원(핸들)은 GC가 원하는 시점에 바로 정리해주지 못한다.**  
> 그래서 **자원이 끝나는 즉시 확실히 닫는 규약**이 필요하고, 그게 `IDisposable`, `using`이다.

---

### `IDisposable`
- **자원을 정리하는 규약**
- `Dispose()`를 호출하면, 객체가 잡고 있던 자원(파일 핸들, DB 커넥션, 소켓 등)을 **즉시 해제**한다.
- 대표적으로 다음 타입들이 `IDisposable`을 구현한다.
    - `FileStream`, `StreamReader/Writer`
    - DB 관련(`SqlConnection`, `SqlCommand` 등)
    - `Mutex`, `Timer`류 일부
```csharp
public interface IDisposable
{
    void Dispose();
}
```
#### 왜 중요한가?
- 자원을 닫지 않으면 **파일 잠김**, **커넥션 누수**, **핸들 고갈**, **성능 저하** 같은 문제가 생긴다.
### `using`
- `IDisposable` 객체를 **사용 후 자동으로 Dispose 호출**해주는 문법
- 예외가 발생해도 `Dispose()`가 호출되도록 보장한다.
##### using 문
```csharp
using System;
using System.IO;

class Program
{
    static void Main()
    {
        using (var fs = new FileStream("log.txt", FileMode.Create))
        {
            byte[] data = System.Text.Encoding.UTF8.GetBytes("hello");
            fs.Write(data, 0, data.Length);
        } // 여기서 fs.Dispose() 자동 호출
    }
}
```
- `Dispose()`를 입력하지 않아도 `using` 스코프가 끝날 때 자동으로 `fs.Dispose()`를 호출