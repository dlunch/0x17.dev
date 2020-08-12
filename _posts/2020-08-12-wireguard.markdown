---
layout: post
title: "Wireguard 여러 서버에 동시에 연결하기"
date: 2020-08-12 13:25:00 +0900
---

Wireguard 프로필 여러개로 wgcf랑 내서버에 동시에 붙일라고 하는데 윈도용 wireguard 클라에서는 동시에 한 개밖에 연결이 안 된다.

해결책은 Wireguard 설정 파일에 `Peer` 섹션을 여러개 만드는 것. wgcf privkey/ip를 우리가 바꿀수 없으니까 내 서버 설정을 거기에 맞춰서 설정해서 해결.
