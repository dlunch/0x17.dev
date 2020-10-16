---
layout: post
title: "wsl2 gentoo openrc 사용하기"
date: 2020-10-16 17:30:00 +0900
---

wsl2 환경에서 init 바이너리가 wsl2의 것으로 고정되어 systemd등의 다른 init을 사용하지 못한다. systemd는 [genie](https://github.com/arkane-systems/genie) 같은게 있는데, openrc는 그런게 없어서 좀 삽질을 해봤다.

openrc는 prefix 환경에서도 돌 수 있어서, 다른 init이 떠있어도 그냥 실행하면 실행을 할 수 있다. wsl2에서 그냥 openrc를 실행하면 다른 init이 떠 있다고 에러가 나지만, `/run/openrc/softlevel` 파일을 만든 뒤에는 정상작동한다.

그래서 아래처럼 스크립트를 만들었다

```
#!/bin/bash
if [ ! -d "/run/openrc" ]
then
        mkdir -p /run/openrc
        touch /run/openrc/softlevel
        openrc default
fi
```

그리고 이걸 매번 실행시켜줘야 하는데, 그냥 .bashrc에 등록했다
`[ ! -d "/run/openrc" ] && sudo /sbin/run_openrc.sh`
sudoers에도 한 줄 추가
`%wheel ALL=(ALL) NOPASSWD: /sbin/run_openrc.sh`

이러면 wsl 창을 켤 때 openrc가 실행돼있지 않으면 새로 실행하게 된다. 일부 부트 서비스들은 켜지는데 오래걸리기만 하고 의미가 없으니 `rc-update` 로 안 켜지게 하면 된다.
