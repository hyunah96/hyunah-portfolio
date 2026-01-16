---
title: Binding Mode
tags:
  - csharp
  - wpf
date: 2025-12-15
---
### `Binding Mode`
- 바인딩은 **UI ↔ ViewModel**을 연결하는 기능이다.
- `Binding Mode`는 **데이터가 어느 방향으로 흐를지**를 정하는 옵션이다.
#### `OneWay`
- **ViewModel → UI** 한 방향
- VM 값이 바뀌면 UI는 갱신되지만, UI에서 값을 바꿔도 VM에는 반영되지 않는다.
- 읽기 전용 화면에 쓰인다.
```csharp
<TextBlock Text="{Binding StatusText, Mode=OneWay}" />
```
#### `TwoWay`
- **ViewModel ↔ UI** 양방향
- UI에서 입력하면 VM 값이 바뀌고, VM이 바뀌어도 UI가 갱신된다.
```csharp
<TextBox Text="{Binding WorkerName, Mode=TwoWay}" />
<CheckBox IsChecked="{Binding IsRunning, Mode=TwoWay}" />
```
#### `OneTime`
- **처음 한 번만 ViewModel → UI**로 가져온다.
- 이후에 VM 값이 바뀌어도 UI 업데이트는 반영되지 않는다.
```csharp
<TextBlock Text="{Binding EquipmentId, Mode=OneTime}" />
```
