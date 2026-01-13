---
title: Overloading, Overriding
tags:
  - csharp
  - interview
date: 2026-01-13
---
## Overloading, Overriding
>**오버로딩(Overloading)** → **같은 이름, 매개변수 목록이 다른 메서드를 여러 개 정의**
>**오버라이딩(Overriding)** → **상속 관계에서 부모의 virtual 메서드를 자식이 재정의**

### Overloading
- **메서드 이름은 같지만**, **매개변수(개수/타입/순서)가 다르면** 여러 개 만들 수 있다.
- 반환형, 접근제한자만 바꾸는건 불가능하다.
- 호출 시점에 전달한 인자에 맞춰 **컴파일러가 어떤 메서드를 쓸지 결정**한다.
```csharp
using System;

class Calculator
{
    // int 2개
    public int Add(int a, int b)
    {
        return a + b;
    }
    //매개변수 목록도 달라야만 성립됨 반환형만 바꾸는건 불가
// 	public double Add(int a, int b)
//	{
//        return a + b;
//	}

    // double 2개
    public double Add(double a, double b)
    {
        return a + b;
    }

    // int 3개
    public int Add(int a, int b, int c)
    {
        return a + b + c;
    }
}

class Program
{
    static void Main()
    {
        var cal = new Calculator();

        Console.WriteLine(cal.Add(1, 2));      
        Console.WriteLine(cal.Add(1.5, 2.3));  
        Console.WriteLine(cal.Add(1, 2, 3)); 
    }
}
```
### Overriding
- 부모 메서드를 자식 메서드에서 **재정의(덮어쓰기)** 하는 것으로 **부모 메서드**에는 `virtual` 을 **자식 메서드**에는 `override`를 붙인다.
- `override`는 **부모 메서드와 시그니처가 완전히 같아야** 한다.
- 부모 타입으로 참조해도 실제 객체의 메서드가 실행되는 **런타임 다형성**이다.
- 결정 시점은 **런타임**
```csharp
using System;

class Animal
{
    public virtual void Speak()
    {
        Console.WriteLine("동물이 소리낸다");
    }
}

class Dog : Animal
{
    public override void Speak()
    {
        Console.WriteLine("멍멍");
    }
}

class Cat : Animal
{
    public override void Speak()
    {
        Console.WriteLine("야옹");
    }
}

class Program
{
    static void Main()
    {
        Animal a1 = new Dog();
        Animal a2 = new Cat();

        a1.Speak(); // 멍멍
        a2.Speak(); // 야옹
    }
}
```
### `new` 
- `new`는 부모의 메서드를 숨기고 새로운 메서드를 정의하는 것이다.
- `override`→ **재정의(다형성)**, `new` → **숨김(hiding)** (부모 메서드를 대체하는 게 아님)
- 재정의가 아니기 때문에 `Parent` 타입으로 참조하면 `Parent` 메서드가 호출되고, `Child` 타입으로 참조하면 `Child` 메서드가 호출된다.
```csharp
using System;

class Parent
{
    public void Hello()
    {
        Console.WriteLine("부모");
    }
}

class Child : Parent
{
    public new void Hello()
    {
        Console.WriteLine("자식");
    }
}

class Program
{
    static void Main()
    {
        Parent p = new Child();
        p.Hello(); // 부모  (참조 타입이 Parent라서 Parent.Hello 호출)

        Child c = new Child();
        c.Hello(); // 자식
    }
}

```