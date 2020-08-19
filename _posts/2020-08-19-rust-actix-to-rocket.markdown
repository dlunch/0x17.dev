---
layout: post
title: "actix-web에서 rocket으로 갈아타기"
date: 2020-08-19 17:51:00 +0900
---

개인 프로젝트에서 actix-web를 쓰다가 rocket이 드디어 stable에서 빌드가 된다는 소식([Rocket Issue #19](https://github.com/SergioBenitez/Rocket/issues/19)) 을 듣고 갈아타봤다. 아직 정식 릴리즈는 나오기 전이지만 git master을 가져다 쓰면 잘 된다.

좀 자세하게 어떻게 바꿨는지 쓸라고 했지만.. 막상 써볼라니 귀찮아져서 대충만 적어봄..

[actix_web::web::Data](https://docs.rs/actix-web/3.0.0-beta.3/actix_web/web/struct.Data.html) 는 [rocket.manage](https://api.rocket.rs/async/rocket/struct.Rocket.html#method.manage) 로 바꿨고, CORS 헤더를 넣기 위해 넣은 [middleware](https://docs.rs/actix-web/3.0.0-beta.3/actix_web/struct.App.html#method.wrap_fn) 는 [rocket.attach](https://api.rocket.rs/async/rocket/struct.Rocket.html#method.attach)에 [Adhoc::on_response](https://api.rocket.rs/async/rocket/fairing/struct.AdHoc.html#method.on_response) 로 수정, 다른 모듈로 route 분리를 위한 [configure](https://docs.rs/actix-web/3.0.0-beta.3/actix_web/struct.App.html#method.configure) 는 [rocket.attach](https://api.rocket.rs/async/rocket/struct.Rocket.html#method.attach)에 [Adhoc::on_launch](https://api.rocket.rs/async/rocket/fairing/struct.AdHoc.html#method.on_launch) 로 수정.

rocket route 핸들러 메소드에서는 request header를 직접 못 가져와서 (https://github.com/SergioBenitez/Rocket/issues/178) [FromRequest](https://api.rocket.rs/async/rocket/request/trait.FromRequest.html) 구현을 직접 만들어서 사용했다.

`futures-rs`의 `Stream` 을 사용해서 응답하는 핸들러가 있었는데, rocket의 response stream은 `tokio`의 `AsyncRead` trait을 요구해서 `futures-rs`의 `Stream` trait의 [into_async_read](https://docs.rs/futures/0.3.5/futures/stream/trait.TryStreamExt.html#method.into_async_read) 을 사용할라고 봤으나 사용할 수 없었다.. 일단 전부 버퍼링해서 리턴하게 만들어놨는데, 추가로 더 찾아봐야 할 듯.
