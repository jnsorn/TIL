# 2020년
## 12월
### 23일 수요일
> _"이벤트의 발행과 구독"_
#### SQS
**의미**
- 분산 시스템을 구성할 때 시스템간 메세지를 주고 받을 수 있는 메세지통
- 전송, 수신, 삭제 3가지 기능을 제공

**구조**
##### Queue(줄)
- Queue 안에 message가 들어가 있다.
- 여러가지용도의 메세지들을 그룹핑하여 관리의 편의성을 도모하기 위해 존재
- Queue에 Message를 올리는 것: SEND
- Queue에서 Message를 읽는 것: RECEIVE
    - 이 때, Queue에서 리시브 된 Message는 보이지 않는 상태로 바뀐다.(사용자가 정해놓은 시간까지 - *visibility timeout: 기본값 30초* )
        - 만약 삭제되지 않는다면 다른 서버가 동일한 작업을 하게 될 수도 있기 때문에(즉, 분산서버 환경이기 때문에!)
    - 그럼 아예 메시지를 삭제하면 되지 visibility timeout을 설정할까?
        - 메세지를 receive한 서버가 장애가 나서 작업을 처리하지 못하는 경우를 대비
        - 그럼 이 때 메세지는 처리가 되지 않은 상태임!
            - 처리할 경우에는 message를 삭제하지만, 지금은 삭제되지 않았기 떄문에 visibility timeout 이후에 queue에 다시 message가 추가됨
    - 메세지를 수신한 서버가 메세지를 처리하게 되면
        - 해당 큐에 메세지 삭제 요청을 보낸다.
        - 다음 작업을 위해 새로운 큐에 메세지를 추가한다. (SEND)

##### Message
- polling: 당겨오는 것
    - start polling for message : 해당 큐에 있는 메세지를 가져오겠다! 이런 의미
- 서버가 정기적으로 큐에 접속해서 큐에 메세지가 있으면 가져오게 되는 것
- Message Available : 큐로부터 받을 수 있는 메세지 수
- Message in Filght : 현재 큐에서 보이지는 않지만 다른 서버로부터 처리되고 있는 메세지 수
- Receice Count : 몇 번이나 리시브가 일어났는지
- 두 개의 서버가 경합
    - 서버1과 서버2가 하나의 큐에서 폴링하고 있을 떄
    - 서버1이 메세지1을 가져가면 서버2는 visibility timeout동안 볼 수 없음

# 2021년
## 1월
### 21일 목요일
> _"SpEL과 condition을 이용해서 분기문을 제거해주세요."_
#### SpEL(Spring Expression Language)
- 런타임에 객체 그래프를 쿼리하고 조작하는 것을 지원하는 표현 언어
- 언어 구문은 Unified EL과 비슷하지만 추가로 메소드 호출과 기본적인 문자열 템플릿 기능을 제공함
- SpEL은 기술에 구애받지 않는 API를 기반으로 하고 있어 필요하다면 다른 표현 언어 구현을 통합할 수 있음
    - 스프링과 직접 연계되지 않아 독자적으로 사용가능
#### Ref
- [공식 문서](https://docs.spring.io/spring-framework/docs/3.0.5.RELEASE/reference/expressions.html)
