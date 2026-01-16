---
title: Command(ICommand)
tags:
  - csharp
  - wpf
date: 2025-12-18
---
### `Command(ICommand)`
- WPF에서 버튼 클릭 같은 “동작”을 **이벤트(Click)** 로 처리하지 않고, **Command(명령)** 로 연결해서 실행하는 방식이다.
- **UI가 무슨 일을 할지를 ViewModel로 넘기는 통로**라고 보면 된다.
### code-behind, ICommand 차이

### code-behind
- 장점 : 빠르게 만들기 좋다.
- 단점 : 화면이 커지면 **코드비하인드에 로직이 쌓여서** 유지보수가 어려워질 수 있다.
```csharp
<Button Content="추가" Click="BtnAdd_Click"/>
```
```csharp
private void BtnAdd_Click(object sender, RoutedEventArgs e)
{
    // UI 뒤에 로직이 붙음 (code-behind에 쌓이기 쉬움)
}
```
### ICommand
- WPF가 버튼 클릭 같은 UI 동작에서 자동으로 `CanExecute()`,`Execute()`를 호출하기 위한 표준 인터페이스다.
- View는 **Command만 호출**하고 **실제 로직은 ViewModel이 처리한다.**
- 장점: **분리/테스트/재사용**이 쉬워진다.

#### RelayCommand (ICommand 구현)
```csharp
using System;
using System.Windows.Input;

namespace WpfApp1
{
    internal class RelayCommand : ICommand
    {
        private readonly Action _execute;
        private readonly Func<bool>? _canExecute;

        public RelayCommand(Action execute, Func<bool>? canExecute = null)
        {
            _execute = execute ?? throw new ArgumentNullException(nameof(execute));
            _canExecute = canExecute;
        }

        // WPF가 실행 가능 여부를 물어볼 때 호출
        public bool CanExecute(object? parameter) => _canExecute?.Invoke() ?? true;

        // WPF가 실제로 실행할 때 호출
        public void Execute(object? parameter) => _execute();

        // CanExecute 결과가 바뀌었음을 WPF에 알릴 때 사용
        public event EventHandler? CanExecuteChanged;

        public void RaiseCanExecuteChanged()
            => CanExecuteChanged?.Invoke(this, EventArgs.Empty);
    }
}
```
#### MainViewModel
```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;

namespace WpfApp1
{
    internal class MainViewModel : INotifyPropertyChanged
    {
        public ObservableCollection<object> Items { get; } = new ObservableCollection<object>();

        public event PropertyChangedEventHandler? PropertyChanged;

        private int _count = 1;

        public RelayCommand AddCommand { get; }

        public MainViewModel()
        {
            // 초기 데이터 세팅 (Window가 뜰 때 1번 실행)
            Items.Add("Item-01");
            Items.Add("Item-02");

            // Command 객체 생성 (Window가 뜰 때 1번 실행)
            // _execute 안에 나중에 실행할 코드를 저장해두는 구조
            AddCommand = new RelayCommand(() =>
            {
                Items.Add($"Item-{_count++:00}");
            });
        }
    }
}
```
#### MainWindow.xam
- XAML에서 `DataContext`를 `MainViewModel`로 설정하면, Window 생성 시점에 `new MainViewModel()`이 실행된다.
```csharp
<Window x:Class="WpfApp1.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:WpfApp1"
        mc:Ignorable="d"
        Title="MainWindow" Height="450" Width="500">

    <Window.Resources>
        <Thickness x:Key="CommonMargin">8</Thickness>
    </Window.Resources>

    <!-- Window 생성 시점에 MainViewModel 생성 + 생성자 실행 -->
    <Window.DataContext>
        <local:MainViewModel/>
    </Window.DataContext>

    <StackPanel Margin="12">

        <!-- 버튼 클릭 -> WPF 내부에서 ICommand.Execute() 자동 호출 -->
        <Button Content="추가" Height="32"
                Command="{Binding AddCommand}"/>

        <!-- ItemsSource가 ObservableCollection이라 Add/Remove 시 UI 자동 갱신 -->
        <ListBox ItemsSource="{Binding Items}"
                 Height="160"
                 Margin="0,10,0,0"/>
    </StackPanel>
</Window>
```