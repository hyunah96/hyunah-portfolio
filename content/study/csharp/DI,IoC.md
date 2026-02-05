---
title: DI/IoC
date: 2026-02-05
tags:
  - csharp
---
## IoC

클래스 안에서 `new`로 객체를 직접 만들어서 의존하지 않고 필요한 객체를 외부(컨테이너)가 만들어서 넣어준다.

### 의존성 설치

NuGet 패키지 관리자를 사용하거나 Package Manager Console에서 명령어를 입력하여 설치
```csharp
Install-Package Microsoft.Extensions.DependencyInjection
```
### IoC 컨테이너 설정

서비스 목록을 담아둘 IoC컨테이너 생성
```csharp
using Microsoft.Extensions.DependencyInjection;  
​  
var serviceCollection = new ServiceCollection();
```
### IServiceCollection

서비스 등록 목록 
어떤 타입을 요청하면 무엇을 만들어줄지를 등록(Add...)
#### IServiceCollection 확장 메서드

- `AddTransient<TService, TImpl>()` 
  서비스가 요청할 때마다 **매번 새 객체**를 생성
- `AddScoped<TService, TImpl>()` 
  서비스 요청당 한 번 생성, 연결이 유지되는 동안 재사용
- `AddSingleton<TService, TImpl>()` 
  서비스가 요청될 때 **최초 1회** 만들어서 재사용
- `TService`: 서비스 타입
- `TImpl`: 구현 타입

`IMainView`를 요청하면 `MainViewModel`을 싱글톤으로 반환하는 서비스를 등록
```csharp
services.AddSingleton<IMainView, MainViewModel>()
```
### IServiceProvider

등록된 레시피를 바탕으로 실제 객체를 꺼내주는 곳
- `GetService`: 등록이 안 되어 있으면 `null` 반환
- `GetRequiredService`: 등록이 안 되어 있으면 예외 발생

등록해둔 `IServiceCollection`을 바탕으로 `IServiceProvider`(컨테이너) 가 만들어짐
```csharp
services.BuildServiceProvider();
```
### 최종 정리!!!
1. `IMainView`를 IoC컨테이너에 `Type`값으로 전달 
   `MarkupExtension`을 상속하면 XAML에서 `{extensions:Container ...}` 형태로 마크업 확장처럼 사용할 수 있다.
   ```csharp
   DataContext="{extensions:Container {x:Type vm:IMainView}}" 
   ```

2. `IMainView`를 요청하면 `MainViewModel`을 싱글톤으로 반환하는 서비스를 등록
```csharp
ServiceCollection services = new ServiceCollection();  
.....  
.AddSingleton<IMainView, MainViewModel>()
```

3. `MainViewModel`은 `IMainView`인터페이스에 정의된 `Title`을 구현
    
4. `IModelingFileController` : 기능 이름/시그니처 정의

#### 결론

`View`에서 `MainViewModel` 구현체를 직접 바인딩하지 않고 `IMainView` 인터페이스로만 의존하게 만들어서 `View`와 `ViewModel`을 느슨하게 결합하려는 목적 DI/IoC로 교체 가능하게 하는 게 목적

### IoC 최종 이해 (제어의 역전)

**객체를 누가 만들고, 누가 연결할지에 대한 주도권이 바뀌는 것**을 IoC라고 한다.

- 역전 전에는 `View`가 어떤 `ViewModel`을 쓸지 **결정**함으로써 `View`가 직접 주도한다.
```csharp
DataContext = new MainViewModel();
```

- 역전 후에는 `View`가 주도하지 않고 컨테이너에서 어떤 `ViewModel`을 쓸지 **결정**한다.
  ```csharp
  DataContext="{extensions:Container {x:Type vm:IMainView}}"
  ```

**이렇게 주도권이 View → 컨테이너로 넘어간 상태를 IoC라고 한다.**

### DI 최종 이해 (의존성 주입)

DI는 IoC를 구현하는 방법(DI도 컨테이너 목록에 등록해야함) 
**필요한 의존 객체를 밖에서 넣어주는 것**
```csharp
public FAceService BizFAceService;  
​  
public MainViewModel(FAceService faceService)  
{  
    BizFAceService = faceService;  
    Title = string.Empty;  
}
```


---

이건 내일

Behavior는 이벤트를 Attach/Detach로 확장하여 연결하는 개념 Command는 실행 행위를 추상화한 패턴 개발 구조 방향은 Core 중심으로 화면이 바뀌어도 핵심 로직은 재사용 가능하도록 설계하는 것이 목표