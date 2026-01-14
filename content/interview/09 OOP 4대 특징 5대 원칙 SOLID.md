---
title: OOP 4대 특징 5대 원칙 SOLID
tags:
  - csharp
  - interview
  - oop
date: 2026-01-14
---
## OOP 의 정의 

> OOP란 Object-Oriented Programming의 약자로 객체 지향 프로그래밍을 의미한다. 
> 객체 지향은 프로그램을 객체 단위로 나누어 구조화하고 설계하는 것이다.

---

## **OOP의 4대 특징**
### 캡슐화(Encapsulation)
- 객체 내부의 상태(필드)를 **외부에서 함부로 바꾸지 못하게 숨기고 허용된 방법(메서드/프로퍼티)으로만 접근하도록** 만드는 것이다.
- 목적: **무결성 유지**, **유지보수 쉬움**
- 필드를 `public`이 아닌 `private`으로 정의하여 외부에서 함부로 바꾸지 못하게 한다.
```csharp
using System;

class Motor
{
    private int _rpm; // 외부에서 직접 변경 불가

    public int Rpm => _rpm; // 읽기 전용

    public void SetRpm(int rpm)
    {
        if (rpm < 0 || rpm > 3000)
            throw new ArgumentOutOfRangeException(nameof(rpm), "RPM 범위(0~3000)만 허용");
        _rpm = rpm;
    }
}

class Program
{
    static void Main()
    {
        var motor = new Motor();
        motor.SetRpm(1500);
        Console.WriteLine(motor.Rpm);
        // motor._rpm = 9999; // 컴파일 에러 (private)
    }
}
```
### 상속(Inheritance)
- 기존 클래스(부모)의 기능/구조를 **물려받아** 새로운 클래스(자식)를 만드는 것이다.
- 무분별한 상속은 결합도가 올라가서 오히려 유지보수가 어려워질 수 있다.
- 목적: **중복 제거**, **공통 로직 재사용**
```csharp
using System;

class Device
{
    public string Name { get; }

    public Device(string name) => Name = name;

    public void Connect()
    {
        Console.WriteLine($"{Name}: 연결됨");
    }
}

class PlcDevice : Device
{
    public PlcDevice(string name) : base(name) { }

    public void ReadRegister(int addr) //자식 기능 추가
    {
        Console.WriteLine($"{Name}: 레지스터 {addr} 읽기");
    }
}

class Program
{
    static void Main()
    {
        var plc = new PlcDevice("OpenPLC");
        plc.Connect();          // 부모 공통 로직 재사용
        plc.ReadRegister(100);  
    }
}
```
### 다형성(Polymorphism)
- 같은 사용 방식으로 호출해도, 실제 동작은 **객체 타입에 따라 다르게 실행**되는 것이다.
- C#에서는 주로 `virtual/override` (런타임 다형성),인터페이스(설계 다형성)로 구현한다.
```csharp
using System;

class Alarm
{
    public virtual void Notify()
    {
        Console.WriteLine("기본 알림");
    }
}

class EmailAlarm : Alarm
{
    public override void Notify()
    {
        Console.WriteLine("이메일 알림 전송");
    }
}

class SmsAlarm : Alarm
{
    public override void Notify()
    {
        Console.WriteLine("SMS 알림 전송");
    }
}

class Program
{
    static void Main()
    {
        Alarm a1 = new EmailAlarm();
        Alarm a2 = new SmsAlarm();

        a1.Notify(); // EmailAlarm 동작
        a2.Notify(); // SmsAlarm 동작
    }
}
```
- 변수 타입이 `Alarm`이어도, 실제 객체가 `EmailAlarm`이면 **override된 메서드가 실행**된다.
### 추상화(Abstraction)
- 상세 구현은 감추고, **핵심 개념/기능만 뽑아서 인터페이스로 제공**하는 것이다.
- 목적: 복잡도를 낮춤
- C#에서는 주로 `abstract class`, `interface`로 표현한다.
```csharp
using System;

interface IComm
{
    void Connect();
    string Read(string address);
}

class ModbusTcpComm : IComm
{
    public void Connect() => Console.WriteLine("Modbus TCP 연결");

    public string Read(string address)
    {
        return $"Modbus에서 {address} 읽음";
    }
}

class SerialComm : IComm
{
    public void Connect() => Console.WriteLine("Serial 연결");

    public string Read(string address)
    {
        return $"Serial에서 {address} 읽음";
    }
}

class MesService
{
    private readonly IComm _comm;

    public MesService(IComm comm) => _comm = comm;

    public void Run()
    {
        _comm.Connect();
        Console.WriteLine(_comm.Read("D100"));
    }
}

class Program
{
    static void Main()
    {
        var mes1 = new MesService(new ModbusTcpComm());
        mes1.Run();

        var mes2 = new MesService(new SerialComm());
        mes2.Run();
    }
}
```
- `MesService`는 Modbus인지 Serial인지 몰라도 된다.
  → 오직 `IComm`만 보고 사용  
  →구현체를 바꿔도 사용하는 코드는 그대로인 **교체 가능한 구조**

## **SOLID 5대 원칙**

> SOLID는 객체지향 설계를 “유지보수/확장”하기 좋게 만드는 5가지 원칙이다.  
> OOP 4대 특성(캡슐화/상속/다형성/추상화)을 **실제로 설계에 적용하는 기준**이라고 보면 된다.


### SRP 단일 책임 원칙
- 클래스는 **하나의 책임** 만 가져야 한다.
- **하나의 클래스는 하나의 기능 담당하여 하나의 책임을 수행**하는데 집중되도록 클래스를 따로따로 여러개 설계하라는 원칙이다.
### OCP  개방 폐쇄 원칙
- **기능 확장에는 열려 있고, 기존 코드 수정에는 닫혀있어야 한다.**
- 즉, 새로운 기능이 추가돼도 기존 코드 수정은 최소화 하도록 프로그램을 설계하라는 원칙이다.
### LSP 리스코프 치환 원칙
- **서브 타입은 언제나 부모 타입으로 교체할 수 있어야 한다는 원칙이다.**
- **다형성** 원리를 이용하기 위한 원칙이다.
### ISP 인터페이스 분리 원칙
- **인터페이스를 각각 사용에 맞게 끔 잘게 분리**해야한다는 설계 원칙이다
### DIP 의존 역전 원칙
-  어떤 `Class`를 참조해서 사용해야하는 상황이 생긴다면, 그 `Class`를 직접 참조하는 것이 아니라 그 **대상의 상위 요소(추상 클래스 or 인터페이스)로 참조**하라는 원칙이다.
