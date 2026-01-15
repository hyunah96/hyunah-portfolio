---
title: INotifyPropertyChanged
tags:
  - csharp
  - wpf
date: 2025-12-13
---
## INotifyPropertyChanged
- `INotifyPropertyChanged`는 **속성 값이 바뀌었음을 외부(WPF 바인딩 엔진)에게 알려주는 표준 인터페이스**다.  
- WPF의 `Binding`은 `Text="{Binding Message}"`처럼 **값을 읽어와서 표시**할 수는 있지만, 
  이후에 `Message`가 바뀌었을 때 **자동으로 알 방법이 없다.**
- 그래서 `INotifyPropertyChanged`를 구현하고 `PropertyChanged` 이벤트를 발생시키면,  
  WPF가 이를 감지해서 **바인딩된 UI를 다시 읽고 갱신**한다.

```csharp
public partial class MainWindow : Window, INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;
    private string _message = "처음 텍스트";
    public string Message
    {
        get => _message;
        set
        {
            if (_message == value) return;
            _message = value;
			PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(Message)));
        }
    }
    public MainWindow()
    {
        InitializeComponent();

        DataContext = this;
    }

    private void Button_Click(object sender, RoutedEventArgs e) {
        Message = "버튼 눌러서 바뀜";
    }
}
```
- `PropertyChanged?.Invoke(...)`가 실행되는 순간, 구독자는 `Message` 변경을 감지
- `Text="{Binding Message}"` 같은 바인딩이 다시 평가됨
- T`extBlock`은 자동으로 갱신

