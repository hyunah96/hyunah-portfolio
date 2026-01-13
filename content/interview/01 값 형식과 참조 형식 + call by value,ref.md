---
title: 값형/참조형 + call by value/ref
tags:
  - csharp
  - interview
date: 2026-01-13
---
## 값형(Value type)과 참조형(Reference type)
>C#의 타입은 크게 **값 형식(Value type)** 과 **참조 형식(Reference type)** 으로 나뉜다. 
> 둘의 차이는 **변수에 데이터가 직접 들어있는가?(값형), 데이터가 있는 곳의 주소를 들고 있는가?(참조형)** 이다.
### 값 형식(Value type)
- 값 형식은 변수가 **실제 데이터 값을 저장**하는 형식이다.
- 그래서 다른 변수에 대입하면 **값이 복사** 된다.
- 복사된 값은 서로 독립적이라, 한쪽을 바꿔도 다른쪽은 안 바뀐다.
#### 값 형식에 해당하는 자료형들
- `short, ushort`
- `int, uint`
- `long, ulong`
- `float, double, decimal`
- `char`
- `bool`
- `struct`
- `enum` 
> **C#에서 struct로 만든 타입은 무조건 값 형식**이다.
>자세한 내용은 [[15 구조체]]를 확인

---
값 형식을 함수의 인자로 전달할 때 내부에서 변수값을 변경해도 원본의 변수값은 변하지 않는다.
**다음 예제를 통해 확인**
```csharp
using System;
using System.Text;
using static System.Runtime.InteropServices.JavaScript.JSType;

namespace ConsoleApp
{
    internal class Program
    {
        public static void TestValue(int a)
        {
            a += 10; //a는 30이 됨 하지만 복사본, 함수가 끝나고 나면 a는 사라짐
        }

        static void Main(string[] args)
        {
            int value = 20;

            Console.WriteLine(value);
            TestValue(value); //value의 값 20이 복사돼서 매개변수 a로 들어감
            Console.WriteLine(value);
        }
    }
}
```
