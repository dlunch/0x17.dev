---
layout: post
title: "다른 프로세스의 메인 스레드에 코드 실행하기"
date: 2021-01-13 18:35:00 +0900
---

다른 프로세스의 주소 공간에서 코드를 그냥 실행하는 것은 참 쉽다. [CreateRemoteThread](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-createremotethread), [SetWindowsHookEx](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-setwindowshookexw) 등등..

하지만 `CreateRemoteThread` 는 메인 스레드(여기서는 메시지 루프가 있는 스레드)가 아닌 다른 스레드에서 실행되고, `SetWindowsHookEx` 는 언제 어떻게 몇번 불릴지 모른다..

그래서 내가 GUI 앱의 메인 스레드에 1회성 코드를 실행할 때 사용하는 방법은 [SetTimer](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-settimer) 를 이용하는 방법이다. `SetTimer` 의 마지막 인자를 보면 `lpTimerFunc` 콜백 인자가 있고 이 콜백은 메시지 루프에서 호출된다.

`SetTimer` 의 첫 번째 인자에 `hWnd`에 다른 프로세스의 창을 넣으면 작동하지 않기 때문에, `CreateRemoteThread` 등지로 해당 프로세스의 주소 공간에서 `SetTimer` 를 실행하고, 콜백 인자에 원하는 함수 주소를 넣으면 원하는 결과를 볼 수 있다.