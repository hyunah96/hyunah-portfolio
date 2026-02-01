---
title: 02 WPF, MVVM, DevExpress 연습
date: 2026-02-01
tags:
  - csharp
  - wpf
  - mvvm
  - DevExpress
---
이사하느라 며칠동안 공부를 못했다. ~~그럼 다시!ㅎ~~

### ViewModel
#### ViewModels/MainViewModel.cs
`INotifyPropertyChanged`를 사용하여 선택된 설비 보여주기
```csharp
internal class MainViewModel:INotifyPropertyChanged
{	
	public event PropertyChangedEventHandler? PropertyChanged;
	private Equipment? _selectedEquipment;
	public Equipment? SelectedEquipment
	{
		get => _selectedEquipment;
		set
		{
			if (_selectedEquipment == value) return;
			_selectedEquipment = value;
			OnPropertyChanged();
		}
	}
```
- 필드는 `_`언더바로 시작, 프로퍼티는 대문자로 시작(복합명사일때는 `PascalCase`)

```csharp
public MainViewModel() {
    Equipments.Add(new Equipment { ...

    LogItems.Add(...
    
	//Equipments.FirstOrDefault() == Equipments[0]
    SelectedEquipment = Equipments.FirstOrDefault(); 
}
```
- `FirstOrDefault`는 첫번째 요소를 가져오는 기능을 제공하는 **LINQ**메서드다.
- `Equipments[0]`와 같은 의미

### View
선택된 설비 Binding하기
#### Views/MainWindow.xaml
```csharp
<Border Grid.Column="0" BorderBrush="Gray" BorderThickness="1" Padding="10">
    <StackPanel>
    <TextBlock Text="설비 목록" FontWeight="DemiBold" Margin="0,0,0,8"/>
        <ListBox ItemsSource="{Binding Equipments}"
             DisplayMemberPath="Name"
             SelectedItem="{Binding SelectedEquipment}"
             Height="Auto"/>
    </StackPanel>
</Border>
```
- `ItemsSource`는 화면에 나열할 데이터 목록(컬렉션)을 지정한다.
- `SelectedItem`은 현재 선택된 항목(객체)을 나타낸다.

##### 설비 상세 Binding하기
```CSHARP
<Border Grid.Column="2" BorderBrush="Gray" BorderThickness="1" Padding="10">
    <StackPanel>
    <TextBlock Text="설비 상세" FontWeight="DemiBold" Margin="0,0,0,12"/>
        <TextBlock FontSize="16" FontWeight="SemiBold" Text="{Binding SelectedEquipment.Name}"/>
        <StackPanel Orientation="Horizontal" Margin="0,0,0,6">
            <TextBlock Text="상태 : " Width="40"/>
            <TextBlock Text="{Binding SelectedEquipment.IsConnected}"/>
        </StackPanel>
        <StackPanel Orientation="Horizontal" Margin="0,0,0,6">
            <TextBlock Text="모드 : " Width="40"/>
            <TextBlock Text="{Binding SelectedEquipment.Mode}"/>
        </StackPanel>
        <StackPanel Orientation="Horizontal" Margin="0,0,0,6">
            <TextBlock Text="마지막 확인 : " Width="40"/>
            <TextBlock Text="{Binding SelectedEquipment.LastSeen}"/>
        </StackPanel>
    </StackPanel>
</Border>
```
- 헷갈리는 부분 `Margin = 0,0,0,0` (좌, 상, 우, 하) 
- `Padding`도 마찬가지

![화면](../img/sb.jpg)