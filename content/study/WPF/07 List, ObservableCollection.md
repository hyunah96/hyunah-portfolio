---
title: List<T>, ObservableCollection<T> 차이
tags:
  - csharp
  - wpf
date: 2025-12-17
---
### `List<T>`, `ObservableCollection<T>`차이
- `List<T>`: 데이터는 바뀌지만, **UI에게 바뀜을 알리는 기능이 없다.**  
    → `list.Add()` 해도 화면이 그대로일 수 있다.
- `ObservableCollection<T>`: 내부적으로 `CollectionChanged` 이벤트를 발생  
    → `Add/Remove` 하면 **UI가 바로 반영**

### `List<T>`
- `List<T>`는 C#의 기본 **동적 배열** 이다.
- WPF에서 `ListBox`, `ListView`, `DataGrid` 같은 `ItemsControl`의 `ItemsSource`로 연결해서 표시할 수 있다.
- **변경 알림 기능이 없어서** `Add/Remove`해도 UI가 **자동 갱신되지 않을 수 있다.**
##### XAML
```csharp
<StackPanel Margin="12">
    <Button Content="List에 Add" Height="32" Click="BtnAdd_List_Click"/>
    <Button Content="ListBox 새로고침" Height="32" Margin="0,6,0,0" Click="BtnRefresh_Click"/>
    <ListBox x:Name="MyList" Height="160" Margin="0,10,0,0"/>
</StackPanel>

```
##### C# (code-behind)
```csharp
using System.Collections.Generic;
using System.Windows;

namespace WpfApp1
{
    public partial class MainWindow : Window
    {
        private List<string> _listItems = new List<string>();
        private int _count = 1;

        public MainWindow()
        {
            InitializeComponent();

            // 처음 연결은 됨 (초기 표시)
            MyList.ItemsSource = _listItems;

            _listItems.Add("Item-01");
            _listItems.Add("Item-02");
        }

        private void BtnAdd_List_Click(object sender, RoutedEventArgs e)
        {
            // List는 Add 해도 UI에 변경됨을 알리지 않아서
            // ListBox가 자동으로 갱신되지 않을 수 있음
            _listItems.Add($"Item-{_count++:00}");
        }

        private void BtnRefresh_Click(object sender, RoutedEventArgs e)
        {
            // 강제로 새로고침
            MyList.ItemsSource = null;
            MyList.ItemsSource = _listItems;
        }
    }
}
```
### `ObservableCollection<T>`
- `ObservableCollection<T>`는 C#에서 제공하는 **가변 길이 컬렉션**이다.
- WPF에서 `ListBox`, `ListView`, `DataGrid` 같은 `ItemsControl`의 `ItemsSource`로 연결해서 표시할 수 있다.
- `Add/Remove/Clear` 같은 **컬렉션 변경이 발생하면 `CollectionChanged` 이벤트로 UI에 알림**을 보내서,  `ItemsControl`이 **자동으로 갱신**된다.

##### XAML
```csharp
    <StackPanel Margin="12">
        <Button x:Name="BtnAdd" Content="추가" Height="32" Click="BtnAdd_Click"/>
        <ListBox x:Name="MyList" Height="160" Margin="0,10,0,0"/>
    </StackPanel>
</Window>
```
##### C# (code-behind)
```csharp
using System.Collections.ObjectModel;
using System.Windows;

namespace WpfApp1
{
    public partial class MainWindow : Window
    {
        // UI 자동 갱신되는 컬렉션
        private ObservableCollection<string> _items = new ObservableCollection<string>();

        private int _count = 1;

        public MainWindow()
        {
            InitializeComponent();
            // ListBox에 데이터 소스 연결
            MyList.ItemsSource = _items;

            // 초기 데이터
            _items.Add("Item-01");
            _items.Add("Item-02");
        }

        private void BtnAdd_Click(object sender, RoutedEventArgs e)
        {
            // Add 하면 ListBox가 자동으로 업데이트 됨
            _items.Add($"Item-{_count++:00}");
        }
    }
}
```
<p>
  <img src="../img/ObservableCollection1.JPG" alt="1" width="48%">
  <img src="../img/ObservableCollection2.JPG" alt="2" width="48%">
</p>
##### 2) `$"Item-{_count++:00}"`
- 문자열 보간 + 숫자 포맷(format)
- `:00` = 숫자를 **최소 2자리로 출력**하고, 부족하면 앞을 **0으로 채움**
    - 1 → `"01"`
    - 9 → `"09"`
    - 10 → `"10"`
```csharp
int n = 1;
string s = $"{n:00}"; // "01"
```