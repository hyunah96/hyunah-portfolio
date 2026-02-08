---
title: ResourceDictionary
date: 2026-02-08
tags:
  - wpf
---
## ResourceDictionary

- `ResourceDictionary`는 WPF에서 **재사용 가능한 리소스를 Key로 모아두는 저장소**이다.
- `ResourceDictionary`에 한 번 정의해두면 **여러 화면에서 같은 스타일을 공통으로 재사용**할 수 있다.
- 리소스는 `x:Key`로 등록하고, 사용할 때는 `StaticResource` 또는 `DynamicResource`로 가져온다.
- `StaticResource` : 로드 시점에 한 번 찾아서 고정
- `DynamicResource` : 실행 중 리소스가 바뀌면 UI도 따라 갱신
- 여러 개의 리소스 파일을 합칠 때는 `ResourceDictionary.MergedDictionaries`를 사용한다.  


### ResourceDictionary 생성
폴더 오른쪽 마우스 - 추가 - 리소스 사전
### ResourceDictionary 코드 작성
#### `CommonStyle.xaml`
- `x:Key="CommonRibbonControl"`
    해당 스타일 리소스의 **Key** 
    XAML에서 `Style="{StaticResource CommonRibbonControl}"` **key 적용**
- `TargetType="{x:Type dxr:RibbonControl}"`
	`x:Type`은 타입을 **dxr:RibbonControl** 로 전달
- `<Setter Property="RibbonStyle" Value="Office2019"/>`
    `RibbonStyle`은  **DevExpress**의 `dxr:RibbonControl` **클래스에 정의된 속성**
    `RibbonControl`의 `RibbonStyle` 속성 값을 `Office2019`로 세팅
    그 아래에 있는 `RibbonPage / RibbonPageGroup / BarButtonItem`들도 `Office2019` 스타일로 렌더링
```csharp
    <Style x:Key="CommonRibbonControl"
       TargetType="{x:Type dxr:RibbonControl}">
        <Setter Property="RibbonStyle"
            Value="Office2019"/>
    </Style>
```

#### `MainWindow.xaml`
```csharp
<dx:ThemedWindow.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <ResourceDictionary Source="/Common/Styles/CommonStyle.xaml"/>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</dx:ThemedWindow.Resources>
```

