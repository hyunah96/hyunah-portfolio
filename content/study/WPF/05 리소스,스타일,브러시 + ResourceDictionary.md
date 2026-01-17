---
title: Resource/Style/Brush, ResourceDictionary
tags:
  - csharp
  - wpf
date: 2025-12-15
---
### `Resource`
- **XAML에서 `x:Key`로 저장해두고 재사용하는 값/객체**를 말한다.
- `Brush`, `Thickness`, `Style`을 리소스로 등록해서 여러 컨트롤에서 공통으로 쓴다.
- 저장할 때 `x:Key`를 주고, 사용할 때 `{StaticResource Key}`로 가져온다.
```csharp
<Window.Resources>
    <Thickness x:Key="CommonMargin">8</Thickness>
</Window.Resources>

<TextBlock Margin="{StaticResource CommonMargin}" Text="Hello"/>
```

#### `StaticResource`, `DynamicResource`
- WPF에서 `x:Key`로 등록한 리소스를 가져오는 방법은 크게 2가지가 있다.
#### `StaticResource`
- XAML이 로드될 때 **한 번만 찾아서 고정**한다.
- **빠르고 일반적으로 권장되는 기본값**
```csharp
<TextBlock Margin="{StaticResource CommonMargin}" Text="Hello"/>
```
#### `DynamicResource`
- 런타임에 `Resources["Key"]`가 바뀌면 **UI가 따라 바뀔 수 있다**.
```csharp
<TextBlock Margin="{DynamicResource CommonMargin}" Text="Hello"/>
```

좋은 자료 :  https://daminkoon.tistory.com/4 
#### `Brush`
- `Background` / `Foreground`에 쓰는 **색 객체**이다.
```csharp
<Window.Resources>
    <SolidColorBrush x:Key="PrimaryBrush" Color="#2D6CDF"/>
</Window.Resources>

<StackPanel Margin="12">
    <TextBlock Text="Title" Foreground="{StaticResource PrimaryBrush}" FontSize="16"/>
    <Button Content="Start" Background="{StaticResource PrimaryBrush}" Foreground="White" Margin="0,8,0,0"/>
</StackPanel>
```
#### `Thickness`
- **두께/여백을 표현하는 타입**이다.
- `Margin/Padding/BorderThickness`에 쓰인다.
- `Margin` : 컨트롤 **바깥 여백**
- `Padding` : 컨트롤 **안쪽 여백**
- `BorderThickness` : 테두리 **두께**
```csharp
<Window.Resources>
    <Thickness x:Key="CommonMargin">8</Thickness>
</Window.Resources>

<Button Content="Start" Margin="{StaticResource CommonMargin}"/>
<Button Content="Stop"  Margin="{StaticResource CommonMargin}"/>
```
#### `Style` 
- 컨트롤에 적용하는 **속성 묶음**이다.
- 같은 모양의 버튼, 텍스트박스가 많아질수록 효과가 크다.
```csharp
<Window.Resources>
    <SolidColorBrush x:Key="PrimaryBrush" Color="#2D6CDF"/>

    <Style x:Key="PrimaryButtonStyle" TargetType="Button">
        <Setter Property="Background" Value="{StaticResource PrimaryBrush}"/>
        <Setter Property="Foreground" Value="White"/>
        <Setter Property="Margin" Value="6"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="Padding" Value="10,6"/>
    </Style>
</Window.Resources>

<StackPanel Margin="12">
    <Button Content="Start" Style="{StaticResource PrimaryButtonStyle}"/>
    <Button Content="Stop"  Style="{StaticResource PrimaryButtonStyle}"/>
</StackPanel>
```
### `ResourceDictionary`
- 리소스를 **따로 파일로 빼서 관리하는 곳이다.**
```csharp
<ResourceDictionary xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
                    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <SolidColorBrush x:Key="PrimaryBrush" Color="#2D6CDF"/>

    <Style x:Key="PrimaryButtonStyle" TargetType="Button">
        <Setter Property="Background" Value="{StaticResource PrimaryBrush}"/>
        <Setter Property="Foreground" Value="White"/>
        <Setter Property="Margin" Value="6"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="Padding" Value="10,6"/>
    </Style>
</ResourceDictionary>
```