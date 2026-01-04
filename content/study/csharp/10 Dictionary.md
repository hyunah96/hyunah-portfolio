---
title: Dictionary<TKey, TValue>
tags:
  - csharp
date: 2025-11-15
---
### Dictionary<TKey, TValue>란?
- Dictionary<TKey, TValue>는 **키(Key)와 값(Value)** 을 한 쌍으로 저장하는 자료구조이다.
- 키로 값을 빠르게 찾는 전화번호부나, 사전 같은 느낌이다.
- `List<T>`는 인덱스로 찾지만 `Dictionary`는 **키로 찾는다.**
- `TKey`는 **중복될 수 없다.**  
- `TKey`는 키의 타입, `TValue`는 값의 타입
ex)
- `Dictionary<string, int>` : 이름(string) → 점수(int)
- `Dictionary<int, string>` : 학번(int) → 이름(string)
#### Dictionary<TKey, TValue> 생성
```csharp
using System.Collections.Generic;

var dict = new Dictionary<string, int>(); // 비어있는 Dictionary

var dict2 = new Dictionary<string, int>   // 초기값 있는 Dictionary
{
    { "Apple", 1000 },
    { "Banana", 2000 }
}; 
```
#### Dictionary<TKey, TValue> 접근, 수정
- Dictionary는 `dict[key]`로 접근한다.
- 키가 없는데 접근하면 예외 발생: `KeyNotFoundException`
```csharp
var prices = new Dictionary<string, int>
{
    { "Apple", 1000 },
    { "Banana", 2000 }
};
int applePrice = prices["Apple"]; // 1000 (접근)

// 수정(키가 있으면 값 변경)
prices["Apple"] = 1200; // Apple 값 변경
```
#### Dictionary<TKey, TValue> 추가(Add / indexer)
#### `Add`
- 키가 이미 있으면 예외가 발생한다.`ArgumentException`
```csharp
var dict = new Dictionary<string, int>();

dict.Add("A", 1);
dict.Add("B", 2);

// dict.Add("A", 99); // ArgumentException (키 중복)
```
#### `indexer`
- 키가 없으면 **추가** 되고, 키가 있으면 **수정(덮어쓰기)** 된다.
```csharp
var dict = new Dictionary<string, int>();

dict["A"] = 1;   // 추가
dict["A"] = 99;  // 수정(덮어쓰기)
```
#### Dictionary<TKey, TValue> 삭제(Remove / Clear)
#### `Remove(key)`
- 성공하면 `true`, 키가 없으면 `false`를 반환한다.
```csharp
var dict = new Dictionary<string, int>
{
    { "A", 1 },
    { "B", 2 }
};

bool ok = dict.Remove("A");   // true
bool fail = dict.Remove("Z"); // false
```
#### `Clear()`
- 전체 삭제
```csharp
dict.Clear();
```
#### Dictionary<TKey, TValue> 검색(ContainsKey / ContainsValue / TryGetValue)
#### `ContainsKey(key)`
- 키가 있는지 확인한다.
```csharp
var dict = new Dictionary<string, int>
{
    { "A", 1 },
    { "B", 2 }
};

bool hasA = dict.ContainsKey("A"); // true
bool hasZ = dict.ContainsKey("Z"); // false
```
#### `### ContainsValue(value)`
- 값이 있는지 확인한다.
```csharp
bool has2 = dict.ContainsValue(2); // true
```
#### `TryGetValue(key, out value)`
- 키가 없을 수 있는 상황에서 사용하기 가장 안전하다.
```csharp
if (dict.TryGetValue("B", out int v))
{
    Console.WriteLine(v); // 2
}
```
#### PLC 통신에서 사용하는 예시
```csharp
// PLC 주소, 현재 상태
var plcState = new Dictionary<string, bool>
{
    { "M500", false },
    { "M550", true }
};

if (plcState.TryGetValue("M550", out bool isOn) && isOn)
{
    Console.WriteLine("M550 ON");
}
```