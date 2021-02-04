> 2017 Naver D2 이응준님의
> [그런 REST API로 괜찮은가?](https://www.youtube.com/watch?v=RP_f5dMoHFc&t=1046s&ab_channel=naverd2)
> 발표를 듣고 정리한 내용입니다.

## WEB(1991)

_"어떻게 인터넷에서 정보를 공유할 것인가?"_

정보들을 하이퍼텍스트로 연결한다.

- 표현 형식: HTML
- 식별자: URI
- 전송 방법: HTTP

## REST가 왜 나오게 되었을까?

로이 필딩은 다른 개발자들과 함께 HTTP/1.0 개발에 참여하게 됨. 하지만 기존에 구축되어 있는 Web server와 호환성의 문제가 생길 것이라고 생각.

Roy T.Fielding : _"How do I improve HTTP without breaking the web"_

REST : _A way of providing interoperability between computer systems on the Internet_

`interoperability` : 상호운영성

# REST API

Roy T.Fielding : _"REST API를 위한 최고의 버저닝 전략은 버저닝을 안 하는 것"_

- REST API : REST를 따르는 API
- REST : 분산 하이퍼미디어 시스템(예:웹)을 위한 아키텍쳐 스타일
- 아키텍쳐 스타일 : 제약조건의 집합

## REST를 구성하는 스타일

- client-server
- stateless
- cache
- uniform interface
- layered system
- code-on-demand(optional) : 서버에서 코드를 클라이언트로 보내서 실행할 수 있어야한다. (JS)

## Uniform Interface

- identification of resources : 리소스가 URI로 식별되면 된다.
- manipulation of resources through representations : representation 전송을 통해 리소스를 조작해야 한다.
- self-descriptive messages : 메세지는 스스로를 설명해야한다.
- hypermedia as the engine of application state(HATEOAS) : 애플리케이션의 상태는 Hyperlink를 이용해 전이되어야한다.
    - Link 라는 헤더로 하이퍼링크를 타고 다른 상태로 갈 수 있다.

## 왜 Uniform Interface를 만족해야 하는가?

- 독립적 진화를 하기 위해
    - 서버와 클라이언트가 가각 독립적으로 진화한다
    - 서버의 기능이 변경되어도 클라이언트를 업데이트 할 필요가 없다.

## REST를 잘 지키고 있는 것 : 웹

- 웹 페이지를 변경했다고 웹 브라우저를 업데이트 할 필요는 없다.
- 웹 브라우저를 업데이트 했다고 해도 웹 페이지를 변경할 필요도 없다.
- HTTP/HTML 명세가 변경되어도 웹은 잘 동작한다.

### 상호운용성(interoperability)에 대한 집착

- Referer 오타, charset 잘못 지은 이름이지만 안 고침 (encoding)
    - 이름을 고치면 웹이 다 깨져!
- 만우절 때 만든 HTTP 상태 코드 416 포기함(I'm a teapot)
- HTTP/0.9 아직도 지원함(크롬, 파이어폭스)

## 그럼 REST는 성공했는가?

- REST는 웹의 독립적 진화를 위해 만들어졌다.
- 웹은 독립적으로 진화하고 있다.

# 그런데 REST API는?

REST API는 REST 아키텍쳐 스타일을 따라야하는데, 사실 REST API라고 주장하고 있는 오늘 날의 REST API들은 REST 아키텍쳐 스타일을 따르지 않고 있다.

## 왜 API는 REST가 잘 안 돼요? : 웹과의 비교

| |웹 페이지|HTTP API|
|---|---|---|
|Protocol|HTTP|HTTP|
|커뮤니케이션|사람-기계|기계-기계|
|Media Type|HTML|JSON|

| |HTML|JSON|
|---|---|---|
|Hyperlink|됨(a 태그 등)|정의되어있지 않음|
|Self-descriptive|됨(HTML 명세)|불완전|

## Self-descriptive와 HATEOAS가 독립적 진화에 어떤 도움을 줄까?

### Self-descriptive

**확장 가능한 커뮤니케이션**

서버나 클라이언트가 변경되더라도 오고가는 메세지는 언제나 self-descriptive하므로 언제나 해석이 가능하다.

### HATEOAS

**애플리케이션 상태 전이의 late binding**

어디서 어디로 전이가 가능한지 미리 결정되지 않는다. 어떤 상태로 전이가 완료되고 나서야 그 다음 전이될 수 있는 상태가 결정된다. 쉽게 말해서 링크는 동적으로 변경될 수 있다.

## REST API로 고쳐보기

### Self-descriptive

- 방법1. Media type
    - 미디어 타입을 정의해서 IANA에 직접 등록하여 사용
- 방법2. Profile
    - Link 헤더에 profile relation으로 해당 명세를 링크

### HATEOAS

- 방법1. data
    - data안에 다양한 방법으로 하이퍼링크를 표현한다.
- 방법2. HTTP 헤더
    - Link, Location 등의 헤더로 링크를 표현한다.

# 정리

- 오늘날 대부분의 "REST API"는 사실 REST를 따르지 않고 있다.
- REST의 제약조건 중에서 특히 Self-descriptive와 HATEOAS를 잘 만족하지 못한다.
- REST는 `긴 시간에 걸쳐(수 십년) 진화하는 Web Application을 위한 것`이다.
- REST를 따를 것인지는 API를 설계하는 이들이 스스로 판단하여 결정해야한다.
- REST를 따르겠다면, `Self-descriptive`와 `HATEOAS`를 만족시켜야한다.
    - Self-descriptive는 custom media type이나 profile link relation 등으로 만족시킬 수 있다.
    - HATEOAS는 HTTP 헤더나 본문에 Link를 담아 만족시킬 수 있다.
- REST를 따르지 않겠다면, "REST를 만족하지 않는 REST API"를 뭐라고 부를지 결정해야 할것.
    - HTTP API라고 부르거나 그냥 이대로 REST API라고 부를 수도 있을거다. 