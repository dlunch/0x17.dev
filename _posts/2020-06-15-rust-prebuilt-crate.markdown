---
layout: post
title: "Rust Prebuilt crate 삽질기"
date: 2020-06-15 18:23:00 +0900
---

## Rust Prebuilt crate 삽질기

여러가지 복잡한 사정으로 소스공개가 어려운 라이브러리를 공개 리포지토리에서 사용하고 싶어서 한 삽질

1. 필요한 라이브러리를 rlib으로 빌드하고, target/lib어쩌구.rlib, target/deps/(debug/release)_.rlib, _.rmeta 파일들을 준비
1. 이 라이브러리를 사용할 프로젝트에 동일한 이름의 빈 라이브러리를 만들고 사용할 프로젝트에 참조 추가
1. 사용할 프로젝트의 `build.rs` 에 1번에서 준비한 rlib, rmeta를 target/deps/(debug/release)에 복사하고, target/deps 안에 있는 더미 라이브러리를 1번에서 준비한 lib어쩌구.rlib으로 교체하도록 구현
1. 그러면 더미 라이브러리가 1번에서 미리 빌드한 라이브러리가 되어서 사용할 수 있다
1. rust version, os가 동일해야만 사용할 수 있고, 빌드가 가끔 안 되는 단점이 있지만, 어쨌든 작동은 한다

예제
https://github.com/dlunch/FFXIVTools/blob/master/libs/prebuilt_injector/src/lib.rs
