---
title: 예상 질문 모음
tags:
  - csharp
  - interview
  - SQL
  - wpf
  - mvvm
  - async
  - await
  - db
date: 2026-01-14
---
## A) C# 기본 개념

1. `Value Type`, `Reference Type` 차이
2. `Stack`, `Heap` 개념
   - `Stack`: 메서드 호출 동안만 쓰는 **지역변수/매개변수/참조형 변수의 참조값**이 쌓이는 곳으로 메서드가 반환되면 즉시 정리가 됩니다.
   - `Heap`: `new`로 만든 **객체의 실체(데이터)** 가 놓이는 곳으로 참조가 끊어지면 GC가 관리합니다.
 
3. 기본 전달 방식(`Pass-by-value`) : `Call by value`, `Call by reference`(=`ref / out / in`)
   
4. `TryParse`, `TryGetValue` Try 패턴 의미
   - `TryParse`, `TryGetValue`는 실패가 자주 날 수 있는 상황에서 `bool`로 `true`/`false`를 반환해서 **예외를 방지합니다. 그리고 성공했을 때의 결과 값을 `out` 매개변수로 전달합니다.**
  
5. `string` immutable(불변) 개념과 성능 포인트
6. `static`의 의미 (static 클래스/멤버, 상태 공유 위험)
7. `struct` vs `class` 차이(언제 struct 쓰나)
   
8. `Boxing`, `Unboxing`차이
   - `Boxing`은 값 형식을 `object`로 변환하면서 **힙에 새 객체를 만들고 값이 복사되는 과정**을 말합니다.
     참조 형식으로 변환하는 과정에서 `heap` 메모리에 공간을 할당해서 `stack`에 있는 참조값을 복사해 새로운 객체에게 넣기때문에 GC 부담이 발생할 수 있습니다.
   - `Unboxing`은 박싱된 `object` 안의 값을 **원래 값형으로 꺼내는 과정**을 말합니다. 
     원래 값 형식 타입으로의 캐스팅이 필요하고 타입이 맞지 않으면 예외가 발생합니다.
   
9. `var`
   - `var`는 **컴파일러가 오른쪽 값을 보고 타입을 결정**합니다.
     동적이 아니라서 한 번 정해진 타입은 바뀌지 않습니다.

10. `object`, `dynamic` 차이(런타임 바인딩 위험)
   - `object`는 **최상위 타입**이라 무엇이든 담지만, 사용할 땐 **캐스팅이 필요**하고 값형을 담으면 **박싱/언박싱**이 발생할 수 있습니다.
   - `dynamic`은 실행 중(런타임)에 실제 들어있는 값의 타입을 보고 결정합니다. 잘못된 멤버 호출, 오타, 없는 메서드는 **런타임 에러**가 수 있습니다.

10. nullable(`int?`)와 null 처리 연산자(`?.`, `??`, `??=`)
11. `const` vs `readonly` 차이
12. 다형성(Polymorphism) 개념(컴파일타임/런타임)
    - 다형성은 **같은 메서드 호출이 타입에 따라 다르게 동작하는 것을**을 말합니다.
      구현 방식으로는 **컴파일 타임 다형성(오버로딩)** 과 **런타임 다형성(오버라이딩,인터페이스)** 로 나뉩니다.
13. `Overloading`, `Overriding` 차이
    - 오버로딩은 **메서드 이름은 같지만**, **매개변수(개수/타입/순서)를 다르게 해서** 컴파일 타임에 어떤 메서드를 호출할지 결정하는 방식입니다.
    - 오버라이딩은 **부모의 메서드를 자식이 재정의 하는 것입니다.** 부모 메서드와 시그니처가 같아야 하고 **런타임에 결정됩니다.**
      
14. `virtual / override / new` 차이
15. 인터페이스, 추상 클래스 차이
	- 인터페이스는 **기능을 반드시 구현해야 하는 규약**입니다. 클래스가 여러개의 인터페이스를 다중구현할 수 있습니다.
	- 추상 클래스는 **공통 필드나 공통 로직같은 기본 구현을 제공하는** 클래스입니다. 일부는 추상 메서드로 남겨서 자식이 반드시 구현하게 할 수 있지만 단일 상속이라 하나만 상속 가능합니다. 따라서 다형성 중심이면 인터페이스를, 공통 코드 재사용이나 공통적인 기본 흐름이 필요하면 추상 클래스를 사용합니다.

16. OOP 4대 특성(캡슐화/상속/다형성/추상화)
    - **객체지향 4대 특성은 캡슐화, 상속, 다형성, 추상화입니다.**  
      1. **캡슐화**는 객체의 내부 데이터를 `private`으로 숨기고, 메서드/프로퍼티로만 접근하게 해서 **무결성을 지키는 것**입니다.  
      2. **상속**은 공통 기능을 부모 클래스에 두고, 자식 클래스가 이를 재사용하면서 필요한 기능을 **추가해서 확장**하는 방식입니다.  
      3. **다형성**은 부모 타입으로 다루더라도 실제 객체 타입에 따라 `override`된 메서드가 다르게 동작하는 것입니다. 
      4. **추상화**는 인터페이스/추상 클래스로 **무엇을 할 수 있는지**만 정의하고 상세 구현은 감춰서, 구현체를 바꿔도 사용하는 코드는 그대로인 **교체 가능한 구조**를 만듭니다.
         
17. 캡슐화에서 `private`로 숨기는 이유
      -  **외부에서 직접 수정하지 못하게 막고**, 필요한 기능만 공개해서 **정해진 방식을 통해서만 접근하게 하기 위함입니다.**
    
18. SOLID 5대 원칙
    - **SRP 단일 책임 원칙** - 하나의 클래스는 하나의 기능만 담당하여 하나의 책임만 가져야 합니다.
    - **OCP 개방 폐쇄 원칙** - 새로운 기능이 추가되어도 기존 코드 수정은 최소화 하는 원칙입니다.
    - **LSP 리스코프 치환 원칙** - 서브 타입은 언제나 부모 타입으로 교체할 수 있어야 한다는 원칙입니다. (다향성)
    - **ISP 인터페이스 분리 원칙** - 인터페이스를 각각 사용에 맞게끔 잘게 분리해야 한다는 원칙입니다.
    - **DIP 의존 역전 원칙** - 어떤 `Class`를 참조해서 사용할 때 그 `Class`를 직접 참조하는 것이 아니라 **상위 요소로 참조**하라는 원칙입니다.

19. 상속(Inheritance) vs 합성(Composition)


20. 결합도(Coupling) vs 응집도(Cohesion)

21. 의존성 주입(DI) 기본 개념(왜 필요한가)
22. 예외(Exception) vs 입력 검증(if) 경계
23. try-catch-finally 의미 + finally 목적

24. enum 사용처(상태값 관리) + 주의점
25. GC가 해주는 것, 못해주는 것
    - **GC가 해주는 것** : 더이상 참조되지 않는 메모리를 회수합니다.(`new`로 만든 객체의 메모리)
    - **GC가 못해주는 것**: 파일 핸들, 소켓, DB 연결 같은 **비관리 자원**은 원하는 시점에 확실히 닫아주지 못합니다.

26. `using` / `IDisposable` 개념(자원 해제)
    - `IDisposable` : 파일/소켓/DB 연결처럼 **관리되지 않는 자원**을 `Dispose()`로 해제해줍니다.
    - `using` : `IDisposable` 객체를 사용 후 **스코프가 끝날 때 자동으로 `Dispose()`를 호출합니다. 예외가 발생해도 자원 해제가 보장됩니다.**

27. `event` vs `delegate` 차이 <<< 1.14 목표
28. `IEnumerable` vs `IEnumerable<T>` / 지연 실행(LINQ)
29. 컬렉션: `List<T>` vs `Dictionary<TKey,TValue>` 선택 기준
30. `Equals` / `GetHashCode` 차 (Dictionary 키/값 비교와 연결)
31. LINQ 장단점(성능/디버깅/쿼리 변환)
32. 접근 제한자(public/private/protected/internal) 의미(설계 관점)
---

## B) WPF 기본 개념
- **XAML 역할 vs Code-behind 역할** (View는 UI, 로직은 최대한 VM)
- **Data Binding** (UI ↔ 데이터 연결 개념)
- **DataContext** (바인딩이 바라보는 대상)
- **Binding Mode**: OneWay / TwoWay / OneTime (+ UpdateSourceTrigger는 언급 정도)
- **INotifyPropertyChanged** (속성 값 바뀌면 UI 갱신되는 원리)
- **ObservableCollection** (목록 변경 시 ItemsControl 자동 갱신)
- **Command**: ICommand / CommandBinding 개념 (이벤트 대신 커맨드 쓰는 이유)
- **UI Thread & Dispatcher 기본** ⭐️ (백그라운드 작업 결과를 UI에 반영하는 방식)
---

## C) MVVM 패턴 기본 개념

47. MVVM이란? (View / ViewModel / Model 역할 분리) <<<< 1.15 목표
48. ViewModel에서 UI(Control)를 직접 참조하면 안 되는 이유
49. Command(`ICommand`)는 무엇이고 왜 쓰나(버튼 클릭 처리 방식)
50. Model과 DTO를 분리하는 이유(표현/전송/도메인 분리) 가볍게 
51. Validation을 MVVM에서 처리하는 기본 방법(개념 수준)
52. **Navigation(화면 전환) 기본 구조** (서비스로 CurrentViewModel 바꾸는 방식 “개념”
53. **비동기(Async/Await)를 MVVM에서 쓰는 패턴** ⭐️
54. IsBusy(로딩 표시), 버튼 비활성화, 예외 처리, 취소(CancellationToken)
55. ViewModel 간 통신 방식(Messenger/EventAggregator 같은 개념) 가볍게 
56. **Scenario 모델링(상태 관리) 기본** ⭐️
57. Resource/Style + StaticResource vs DynamicResource
---

## D) 비동기(Async/Await) 기본 개념
55. async/await
56. Async vs Thread 차이
57. `Task`란 무엇이고, 왜 반환형으로 쓰나
58. `async void`가 위험한 이유(예외/대기 불가)
59. `CancellationToken` 개념(취소는 “협조”라는 점)
60. WPF에서 UI 스레드와 await 이후 UI 업데이트(Dispatcher 포함)
61. Task.Run을 언제 쓰면 안 되는지 (I/O 비동기 vs CPU 작업)
---

## E) 멀티스레드 기본 개념
62. Race Condition(경쟁 상태)이란? (왜 가끔만 터지나)
63. 스레드 안전(Thread-safe) 기본 개념 (공유 데이터/불변 객체/컬렉션)
64. `lock`이란? (왜 필요하고 주의점은?)
65. Deadlock(교착상태)이란? 대표 원인(락 순서/중첩 락)
66. UI 스레드/백그라운드 스레드 분리 이유 + Dispatcher 개념(개념 수준)
---
## F) DB/SQL 기본 + 쿼리 단골 개념

67. SELECT 기본 구조(SELECT/FROM/WHERE/ORDER BY) 의미
68. 정규화(정규형) 아주 기본 개념 또는 PK/FK 관계 (JOIN 이해와 직결)
69. JOIN 개념 + INNER JOIN vs LEFT JOIN 차이
70. WHERE vs HAVING 차이(그룹 집계)
71. GROUP BY가 필요한 상황(집계)
72. DISTINCT는 언제 쓰고 주의점은?
73. 인덱스(Index)란? 어떤 쿼리에 효과적인가
74. 트랜잭션(Transaction)과 ACID 개념(왜 필요한가)
75. SQL Injection 개념 + 파라미터 바인딩이 필요한 이유
---
## G) 디자인 패턴 기본 개념

76. **디자인 패턴이란?**
77. **Singleton** (개념 + 남용 문제)
78. **Observer** (event와 연결)
79. **Command** (WPF ICommand와 연결)