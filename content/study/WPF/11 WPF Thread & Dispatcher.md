---
title: WPF Thread & Dispatcher
tags:
  - csharp
  - wpf
date: 2025-12-24
---
## WPF Thread
- WPF에서 Thread는 2가지로 나눠서 이해하면 편하다.
- **UI Thread**: 화면 담당
- **Background Thread**: 오래 걸리는 일 담당

> **UI는 UI Thread만 건드릴 수 있다.**  
> 다른 Thread가 UI를 만지면 에러가 날 수 있다.

### UI Thread
- **화면 표시, 업데이트**를 담당하는 Thread
- 버튼 클릭, 키 입력 같은 **이벤트 처리도 여기서** 일어난다.
- `TextBox`, `ListBox` 같은 **WPF 컨트롤은 UI Thread가 아닌 곳에서 접근하면 에러가 날 수 있다.**
- 보통 WPF 앱에서 UI Thread는 1개라고 보면 된다.
### Background Thread
- UI가 아닌 **오래 걸리는 작업**을 처리하는Thread
    - 통신(서버 요청/응답 기다리기)
    - 파일 읽기/쓰기
    - 큰 계산, 반복 작업
- 필요하면 여러 개가 생길 수 있다. (`Task.Run`, ThreadPool 등)
### Dispatcher
- **Background Thread → UI Thread로 UI 업데이트를 요청하는 도구**
- 통신 결과를 화면에 띄우고 싶거나, 리스트에 항목을 추가해서 UI에 보여주고싶을 때 **UI Thread의 작업 큐(할 일 목록)에 작업을 올려서 직접 실행하게 만들어준다.**



