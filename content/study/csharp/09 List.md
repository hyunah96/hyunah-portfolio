---
title: List<T>
tags:
  - csharp
date: 2025-11-13
---
### `List<T>`란?
- `List<T>`는 **크기가 자동으로 늘어나는 배열**(동적 배열)이다.
- 배열(`T[]`)은 길이가 고정이지만, `List<T>`는 요소를 **추가/삭제**하면서 크기가 바뀐다.
- `T`는 “자료형 자리(제네릭)”로, 리스트에 담을 타입을 정한다.  
  ex) `List<int>`, `List<string>`

#### `List<T>` 생성
```csharp
using System.Collections.Generic;
var list = new List<int>();            // 비어있는 리스트 생성
var list2 = new List<int> { 1, 2, 3 };  // 초기값 있는 리스트 생성

```
#### `List<T>` 접근, 수정
- `list[index]`로 접근한다. 
- 범위를 벗어나면 에러가 난다. (`ArgumentOutOfRangeException`)
```csharp
var list = new List<int> { 10, 20, 30 };
int first = list[0]; // 10 (접근)
list[0] = 99;        // [99, 20, 30] (수정)

```
### `List<T>` 추가(Add / AddRange / Insert / InsertRange)
#### `Add`
- 순서를 유지하면서 끝에 붙일 때 사용한다.
```csharp
var list = new List<int> { 1, 2, 3 };

list.Add(4); // [1, 2, 3, 4]
list.Add(5); // [1, 2, 3, 4, 5]
```
#### `AddRange(items)`
- 여러 개를 끝에 붙일 때 사용한다.
```csharp
var list = new List<int> { 1, 2 };

list.AddRange(new[] { 3, 4, 5 }); // [1, 2, 3, 4, 5]
```
#### `Insert(index, item)`
- 특정 위치에 끼워 넣을 때 사용한다.
- 그 위치부터 뒤 요소들이 **오른쪽으로 밀린다.**
```csharp
var list = new List<int> { 10, 20, 30 };

// 1번 인덱스 자리에 99 넣기
list.Insert(1, 99); // [10, 99, 20, 30]
```
#### `InsertRange(index, items)`
- 특정 위치에 여러 개를 한 번에 넣을 때 사용한다.
```csharp
var list = new List<int> { 1, 2, 5 };

// 2번 인덱스 위치에 3,4 넣기
list.InsertRange(2, new[] { 3, 4 }); // [1, 2, 3, 4, 5]
```
### `List<T>` 삭제(Remove / RemoveAt / RemoveRange / Clear)
#### `Remove(item)`
- 값이 같은 요소를 **첫 요소만** 삭제한다.
- 성공하면 `true`, 못 찾으면 `false`를 반환한다.
```csharp
var list = new List<int> { 1, 2, 2, 3 };

bool ok = list.Remove(2); // true, 결과: [1, 2, 3]
bool fail = list.Remove(9); // false, 결과 변화 없음
```
#### `RemoveAt(index)`
- **해당 인덱스 위치**의 요소를 삭제한다.
- 범위를 벗어나면 에러가 발생한다.
```csharp
var list = new List<int> { 10, 20, 30 };

list.RemoveAt(1); // [10, 30]
```
#### `RemoveRange(index, count)`
- 인덱스부터 count개를 **한 번에 삭제**한다.
```csharp
var list = new List<int> { 1, 2, 3, 4, 5 };

list.RemoveRange(1, 2); // 1번째 인덱스부터 2개 삭제 => [1, 4, 5]
```
#### `Clear()`
- 전체 삭제
```csharp
var list = new List<int> { 1, 2, 3 };
list.Clear(); // []
```
### `List<T>` 검색(Contains / IndexOf / Find / FindAll)
#### `Contains(item)`
- 값이 있는지 검색한다. 
- 값이 있으면 `true`를 값이 없으면 `false`를 반환한다.
```csharp
var list = new List<string> { "A", "B", "C" };

bool hasB = list.Contains("B"); // true
```
#### `IndexOf(item)`
- 값의 인덱스를 반환한다.
- 없으면 `-1`을 반환한다.
```csharp
var list = new List<int> { 10, 20, 30 };

int idx = list.IndexOf(20); // 1
int none = list.IndexOf(99); // -1
```
#### `Find(predicate)`
- 조건에 맞는 **첫 요소를** 반환한다.
- 못 찾으면 `default(T)`를 반환한다.
  `string`→ `null`, `int` → `0`, `bool` → `false`
```csharp
var list = new List<int> { -5, -2, 0, 7, 9 };

int found = list.Find(x => x > 0); // 7 (처음으로 0보다 큰 값)
```
####  `FindAll(predicate)`
- 조건에 맞는 요소를 **새 List로 반환한다.**
```csharp
var list = new List<int> { 1, 3, 5, 2, 4, 6 };

var evens = list.FindAll(x => x % 2 == 0); // [2, 4, 6]
```
### `List<T>` 정렬, 뒤집기(Sort / Reverse)
#### `Sort()`
- **오름차순**으로 정렬한다.
```csharp
var list = new List<int> { 3, 1, 2 };

list.Sort(); // [1, 2, 3]
```
#### `Reverse()`
- 현재 순서를 뒤집는다. (정렬은 X)
```csharp
var list = new List<int> { 1, 2, 3 };

list.Reverse(); // [3, 2, 1]
```
### `List<T>` 반복(For / Foreach / ForEach)
```csharp
var list = new List<string> { "A", "B", "C" };

// for
for (int i = 0; i < list.Count; i++)
    Console.WriteLine(list[i]);

// foreach
foreach (var item in list)
    Console.WriteLine(item);

// ForEach(람다식)
list.ForEach(item => Console.WriteLine(item));
```
