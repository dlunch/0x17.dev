---
layout: post
title: "k8s etcd 메모리 줄이기"
date: 2020-08-19 17:51:00 +0900
---

홈서버에 k8s 돌리는데 etcd가 램을 2기가 넘게 먹길래 어차피 옵션을 바꿔서 낮췄다.

`/etc/kubernetes/manifest/etcd.yaml` 에 `--snapshot-count` 옵션을 100으로 줄이고, image를 `3.4.13-0` 으로 바꿔서 `--unsafe-no-fsync` 옵션도 활성화했다. (https://github.com/etcd-io/etcd/pull/11946)

이렇게 하니까 메모리 사용량이 100mb 이하로 줄었다.

순간적인 정전에 안전하지 못하지만 뭐 어차피 대충돌리는거니까 괜찮겠지.
