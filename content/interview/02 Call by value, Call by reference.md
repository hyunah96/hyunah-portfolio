---
title: Call by value, Call by reference
tags:
  - csharp
  - interview
date: 2026-01-13
---
## Call By Value, Call By Reference

>C#에서 메서드에 인자를 넘길 때는 크게 두 가지로 설명할 수 있다.<br>
>**Call By Value (pass-by-value)** → **값을 복사해서 전달**하는 기본 방식<br>
>**Call By Reference (pass-by-reference)** → **변수의 주소를 참조로 전달**하는 방식

### Call By Value (기본: pass-by-value)
- 함수 내부에서 전달 받은 값을 변경해도 **원래 변수의 값은 변경되지 않는다.**
```csharp
class Program
{
  static void SwapStrings(string s1, string s2)
  {
    string temp = s1;
    s1 = s2;
    s2 = temp;
    Console.WriteLine("메소드에서 값 변경: {0} {1}", s1, s2);
    //(2) 메소드에서 값 변경: HyunAh Kim 
  }
     
  static void Main(string[] args)
  {
    string str1 = "Kim";
    string str2 = "HyunAh";
    Console.WriteLine("SwapStrings 메소드 호출 전: {0} {1}", str1, str2);
    //(1) SwapStrings 메소드 호출 전: Kim HyunAh

    SwapStrings(str1, str2);
    Console.WriteLine("SwapStrings 메소드 호출 후: {0} {1}", str1, str2);
    //(3) SwapStrings 메소드 호출 후: Kim HyunAh
  }
}
```
### Call By Reference (ref/out/in: pass-by-reference)
- `ref / out / in`은 **변수의 주소를 참조로 전달**하여, 메서드가 **호출자 변수에 직접 접근**하게 만든다.
- `ref` : **읽기/쓰기**(원본 변경 가능, 초기화 필요)
- `out` : **반드시 할당**(초기화 없이 받기 가능, 결과 반환용)
- `in` : **읽기 전용**(복사 줄이면서 안전하게 전달)
#### `ref`
- **변수 자체를 참조(읽기/쓰기)** 한다. → 함수 안에서 바꾸면 **호출한 쪽 변수에 바로 반영**
- 호출할 때도 `ref`를 붙여야 하고, 받는 쪽도 `ref`여야 한다.
- **반드시 초기화된 변수만 전달 가능**
```csharp
class Program
{
  static void SwapStrings(ref string s1, ref string s2)
  {
    string temp = s1;
    s1 = s2;
    s2 = temp;
    Console.WriteLine("메소드에서 값 변경: {0} {1}", s1, s2);
    //(2) 메소드에서 값 변경: HyunAh Kim 
  }
     
  static void Main(string[] args)
  {
    string str1 = "Kim";
    string str2 = "HyunAh";
    Console.WriteLine("SwapStrings 메소드 호출 전: {0} {1}", str1, str2);
    //(1) SwapStrings 메소드 호출 전: Kim HyunAh

    SwapStrings(ref str1, ref str2);
    Console.WriteLine("SwapStrings 메소드 호출 후: {0} {1}", str1, str2);
    //(3) SwapStrings 메소드 호출 후: HyunAh Kim 
  }
```
#### `out`
- **호출자 변수를 참조로 전달해서** 메서드가 내부에서 **그 변수에 값을 채워 넣는다.**
- 호출하는 쪽은 **초기화를 안 해도 된다.**
- `out`은 보통 `TryParse`, `TryGetValue` 같은 **Try 패턴**과 함께 사용한다.  
  **성공/실패를 `bool`로 반환**해 예외를 남발하지 않고 흐름을 제어하고, **결과는 `out`으로 전달**한다.
```csharp
using System;

class Program
{
    // 입력이 숫자면 true + 결과를 out으로 전달
    static bool TryGetNumber(string input, out int number)
    {
        // out 매개변수는 메서드 안에서 반드시 값이 할당되어야 함
        return int.TryParse(input, out number);
    }

    static void Main()
    {
	    //"123"이라는 문자열을 전달하고 메서드가 결과를 채워서 result로 넘겨줌 
        if (TryGetNumber("123", out int result))
        {
            Console.WriteLine($"성공: {result}"); // 123
        }
        else
        {
            Console.WriteLine("실패");
        }

        // out은 초기화 없이도 호출 가능
        bool ok = TryGetNumber("ABC", out int result2);
        Console.WriteLine(ok);       // False
        Console.WriteLine(result2);  // 0 (TryParse가 넣어줌)
    }
}
```
#### `in`
- **변수 자체를 참조**하지만 `in`은 **읽기 전용(ref + readonly)** 이다.
- 메서드 내부에서 값을 **수정할 수 없다.**
- 주로 **큰 struct를 복사하지 않으면서**, 실수로 바꾸지 못하게 안전하게 전달할 때 사용한다.
```csharp
using System;

struct BigData
{
    public long A, B, C, D, E, F;
}

class Program
{
    static long Sum(in BigData data)
    {
        // data.A = 10; // 컴파일 에러: in은 수정 불가
        return data.A + data.B + data.C + data.D + data.E + data.F;
    }

    static void Main()
    {
        BigData d = new BigData { A=1, B=2, C=3, D=4, E=5, F=6 };

        long total = Sum(in d);
        Console.WriteLine(total); // 21
    }
}
```

**정리**
C#은 기본이 pass-by-value입니다.  
값형은 값이 복사되고, 참조형도 객체가 아니라 참조값이 복사됩니다.  (참조형을 넘겨도 기본은 pass-by-value(참조값이 복사)
그래서 메서드 안에서 참조형의 내부 상태는 바꿀 수 있지만, 매개변수를 다른 객체로 재할당해도 호출자 변수는 안 바뀝니다.  
`ref/out/in`을 사용하면 변수 자체를 참조로 넘겨서 메서드가 호출자 변수를 직접 바꿀 수 있습니다.