---
layout: post
title: "기가바이트 보드 LED 설정값 직접 바꾸기"
date: 2020-10-01 17:47:00 +0900
---

기가바이트 보드들은 [RGB Fusion](https://www.gigabyte.com/MicroSite/512/rgb2.html) 유틸리티로 led 색을 바꿀 수 있는데, 이 유틸리티가 아래같은 오류로 최신 윈도에서 작동을 안 한다. 드라이버 서명이 날아간 것으로 추정..

```
The gdrv2 service failed to start due to the following error:
A certificate was explicitly revoked by its issuer.
```

그래서 [OpenRGB](https://gitlab.com/CalcProgrammer1/OpenRGB) 을 써봤는데, 이녀석은 재부팅하면 설정값이 날아간다. 그래서.. 좀 찾아봤더니 [https://www.bitservices.io/blog/gigabyte-rgb-hidden-bios-settings/](https://www.bitservices.io/blog/gigabyte-rgb-hidden-bios-settings/) 이 글에서 바이오스 설정값을 직접 건드려서 색을 바꾸고 있길래 따라해봤다.

저기 있는대로 grub-mod-setup_var을 받고, Secure Boot가 켜져있으므로 [shim](https://github.com/rhboot/shim) 을 사용해서 부팅을 해봤다.. 여기까지는 잘 됐는데 저 글에서 테스트한 보드는 X570 PRO고, 내건 X570 Elite인데다가, 바이오스 버전도 달라서 (F30이 얼마전에 나왔음) 글에 있는 `0x2CB` 오프셋에 다른 값이 들어있었다.

고민하다가.. 디폴트 색이 같은 오렌지색이니까 동일한 색상값 (0x00, 0x21, 0xff)가 어딘가 있지 않을까 해서 저 근처 오프셋들을 노가다로 하나씩 다 뒤져보다가.. 값을 찾았다.

0x2b7, 0x2b8, 0x2b9, 0x2ba 오프셋에 디폴트가 있는걸 확인해서 바꿔줘서 해결.

![](/assets/201001.png)
