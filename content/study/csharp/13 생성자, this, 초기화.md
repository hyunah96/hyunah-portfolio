---
title: 생성자, this, 초기화
tags:
  - csharp
date: 2025-11-19
---
### 생성자(Constructor)란?
- 객체가 `new`로 만들어질 때 자동으로 1번 실행되는 **초기화용 메서드**
- 이름이 **클래스 이름과 같아야** 한다.
- 객체가 생성되자마자 **필드/프로퍼티 초기값을 세팅**할 때 자주 사용한다.
#### 기본 생성자
- 생성자를 직접만들지 않으면 기본 생성자를 자동으로 만들어준다.
```csharp
public class Equipment
{
    public string Name;
    public bool IsRunning;

    // 기본 생성자
    public Equipment()
    {
        Name = "Unknown";
        IsRunning = false;
    }
}
```
```csharp
var eq = new Equipment();
Console.WriteLine(eq.Name);       // Unknown
Console.WriteLine(eq.IsRunning);  // false
```
#### 매개변수 생성자
- 생성자에 파라미터를 만들면 `new Equipment("Press-01", true)`처럼 **값을 넣고 생성**하게 된다.
- `Equipment(string, bool)` 생성자를 직접 정의했기 때문에 **자동 기본 생성자가 생성되지 않는다.**
- 따라서 `var a = new Equipment()`는 **0개 인자를 받는 생성자**를 찾지 못해서 컴파일 에러가 난다.
```csharp
public class Equipment
{
    public string Name;
    public bool IsRunning;

    public Equipment(string name, bool isRunning)
    {
        Name = name;
        IsRunning = isRunning;
    }
}
```
```csharp
var eq = new Equipment("Press-01", true);
var a = new Equipment(); //불가능
Console.WriteLine(eq.Name);       // Press-01
Console.WriteLine(eq.IsRunning);  // true
```
### this란?
- `this`는 **현재 객체 자기 자신** 을 가리키는 키워드다.
- 파라미터 이름과 필드 이름이 같을 때, 구분하려고 자주 쓴다.
- **필드명을 `_name`처럼** 바꿔서 this를 덜 쓰기도 한다.
```csharp
public class Person
{
    public string Name;
    public int Age;

    public Person(string Name, int Age)
    {
        this.Name = Name; // 왼쪽(this.Name)은 필드, 오른쪽(Name)은 파라미터
        this.Age = Age;
    }
}
```
#### this로 생성자끼리 연결하기(중복 제거)
- 생성자를 여러 개 만들다 보면 초기화 코드가 중복되기 쉬운데
- `this(...)`로 **다른 생성자를 호출**해서 중복을 줄일 수 있다.
```csharp
public class Equipment
{
    public string Name;
    public bool IsRunning;
    public int Temperature;

    // (1) 공통 초기화를 담당하는 생성자
    public Equipment(string name, bool isRunning)
    {
        Name = name;
        IsRunning = isRunning;
        Temperature = 25; // 기본 온도
    }

    // (2) 기본 생성자: (1)번을 재사용
    public Equipment() : this("Unknown", false)
    {
    }

    // (3) 온도까지 받는 생성자: (1)번 재사용 + 추가 세팅
    public Equipment(string name, bool isRunning, int temperature) : this(name, isRunning)
    {
        Temperature = temperature;
    }
}
```
```csharp
var a = new Equipment();
var b = new Equipment("Press-01", true);
var c = new Equipment("Press-02", false, 40);

Console.WriteLine($"{a.Name}, {a.IsRunning}, {a.Temperature}"); // Unknown, False, 25
Console.WriteLine($"{b.Name}, {b.IsRunning}, {b.Temperature}"); // Press-01, True, 25
Console.WriteLine($"{c.Name}, {c.IsRunning}, {c.Temperature}"); // Press-02, False, 40
```
### 초기화(Initialization) 방식 3가지

#### (1) 필드/프로퍼티 기본값 주기
- 클래스 선언할 때 바로 초기값을 넣는 방식
- 대부분의 객체가 같은 기본값을 가져도 되는 경우 편하다.
```csharp
public class AlarmSetting
{
    public bool Enabled = true;
    public int Threshold = 80;
}
```
```csharp
var s = new AlarmSetting();
Console.WriteLine(s.Enabled);    // True
Console.WriteLine(s.Threshold);  // 80
```
#### (2) 생성자에서 초기화하기
- 생성 시점에 **입력값 기반으로** 초기화할 수 있다.
```csharp
public class AlarmSetting
{
    public bool Enabled;
    public int Threshold;

    public AlarmSetting(int threshold)
    {
        Enabled = true;
        Threshold = threshold;
    }
}
```
```csharp
var s = new AlarmSetting(90);
Console.WriteLine(s.Enabled);    // True
Console.WriteLine(s.Threshold);  // 90
```
#### (3) 객체 이니셜라이저(Object Initializer)
- - `new Person { Name="철수", Age=25 }` 처럼 객체를 만든 뒤, 프로퍼티/필드에 값을 **한 번에 대입**하는 문법이다.
- 코드가 짧고 가독성이 좋다.
- 또한 `Person` 클래스에 생성자를 따로 만들지 않았기 때문에, C#이 매개변수 없는 기본 생성자 `Person()`을 자동으로 만들어준다. 그래서 `var p2 = new Person()` 형태로도 객체 생성이 가능하다.
- 값을 따로 넣지 않으면, 각 멤버는 **기본값**으로 초기화된다.`Name`은 코드에서 `= ""`로 기본값을 지정했기 때문에 `""`가 되고,`Age`는 `int` 타입의 기본값이 `0`이라서 `0`이 된다.
```csharp
public class Person
{
    public string Name { get; set; } = ""; //프로퍼티
    public int Age { get; set; } //프로퍼티
}
```
```csharp
var p = new Person { Name = "철수", Age = 25 };
var p2 = new Person();  // Name "", Age 0
Console.WriteLine($"{p.Name}, {p.Age}");
```