---
layout: post
title: "rust-analyzer에서 system rustc 사용하기"
date: 2020-08-04 09:20:00 +0900
---

WSL 환경에서 Gentoo를 쓰는데 `rustup`이 아닌 `emerge rust` 로 설치한 rust와 vscode wsl remote의 `rust-analyzer` 익스텐션이 그냥 붙지 않는다.

[여기서](https://github.com/rust-analyzer/rust-analyzer/blob/c8e2d67dd4462904f2803d64c651f4630ee595f4/crates/ra_project_model/src/sysroot.rs#L111) 터지는데, 저기서 참조하는 `RUST_SRC_PATH` 환경변수를 `~/.profile`, `~/.bashrc`, `/etc/profile` 등등에 다 해봤는데 안 된다...

그래서 포기하고 그냥 저기서 찾는 경로에 링크를 걸었다.

`sudo ln -s /usr/lib64/rust-1.45.2/rustlib /usr/lib/`

업데이트하면 깨지겠지만.. 자주 안 하니까 고치면 되겠지.
