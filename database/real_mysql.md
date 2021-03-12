# 4장 트랜잭션과 잠금
- `잠금` : 동시성을 제어하기 위한 기능
- `트랜잭션` : 데이터의 정합성을 보장하기 위한 기능
- `격리수준` : 하나의 트랜잭션 내에서 또는 여러 트랜잭션 간의 작업 내용을 어떻게 공유하고 차단할 것인지를 결정하는 레벨

## 4.1 트랜잭션
- 트랜잭션을 지원하지 않는 엔진 : MyISAM, MEMORY
- 트랜잭션을 지원하는 엔진 : InnoDB

- _Q. 그럼 그 전에는 원자성?이 필요한 작업을 어떻게 처리한걸까?_
- A. if-else문을 이용하여 만약 1번 작업이 성공했다면 2번 작업을 수행하는 식으로 분기 처리(코드를 이용해 처리하기 때문에 굉장히 번거롭다)  

### 4.1.1 MySQL에서의 트랜잭션
- 트랜잭션 : 논리적인 작업 셋 자체가 100% 적용되거나 아무것도 적용되지 않아야함을 보장
- MyISAM : `부분 업데이트(Partial Update)` 현상은 테이블의 정합성을 맞추는 데 상당히 어려운 문제를 만들어 냄

### 4.1.2 주의사항
- 꼭 필요한 최소의 코드에만 적용하는 것이 좋음
    - 일반적으로 DB 커넥션의 개수는 제한적
        - 각 단위 프로그램이 커넥션을 소유하는 시간이 길어질수록 사용 가능한 여유 커넥션의 개수는 줄어듦
- 네트워크를 통해 원격 서버와 통신하는 작업은 트랜잭션 내에서 제거하는 것이 좋음
    - 외부 서버와 통신할 수 없는 상황 발생 시, DBMS 서버까지 위험해지기 때문 

## 4.2 MySQL 엔진의 잠금
- 스토리지 엔진 레벨
    - 스토리지 엔진 간 상호 영향을 미치지 않음
- MySQL 엔진 레벨 : MySQL 서버에서 스토리지 엔진을 제외한 나머지 부분
    - 모든 스토리지 엔진에 영향을 미침
    - 테이블 락, 유저 락, 네임 락 제공
        - 테이블 락 : 테이블 데이터 동기화를 위한 잠금
        - 유저 락 : 사용자의 필요에 맞게 사용할 수 있는 잠금
        - 네임 락 : 테이블 명에 대한 잠금

### 4.2.1 글로벌 락
- `flush tables with read lock` 명령으로 획득 가능
    - MySQL 서버에 존재하는 모든 테이블의 잠금을 건다.
    - 명령이 실행되기 전에 테이블이나 레코드에 쓰기 잠금을 걸고 있는 SQL이 실행되고 있다면, 이 명령은 해당 테이블의 읽기 잠금을 걸기 위해 먼저 실행된 SQL이 
      완료되고 그 트랜잭션이 완료될 때까지 기다려야 한다.
      - 이 때, 명령이 실행되기 전 테이블을 플러시해야하기 때문에 테이블에 실행되고 있는 모든 종류의 쿼리가 온료되어야만 테이블을 플러시하고 잠금을 걸 수 있다. 
- MySQL에서 제공하는 잠금 가운데 가장 범위가 큼
- 한 세션에서 글로벌 락을 획득하면 다른 세션에서 SELECT를 제외한 대부분의 DDL문장이나 DML문장을 실행하는 경우 글로벌 락이 해ㅐ제될 때까지 해당 문장이 
  대기상태로 남음
- 글로벌 락이 영향을 미치는 범위 : MySQL 서버 전체
    - 작업 대상 테이블이나 데이터베이스가 다르다 하더라도 동일하게 영향을 미침
    - 여러 데이터 베이스에 존재하는 MyISAM이나 MEMORY 테이블에 대해 mysqldump로 일관된 백업을 받아야 할때는 글로벌 락 사용
        - _Q. 왜 MyISAM, MEMORY 테이블에 한해서? INNODB는 안되나? mysqldump는 뭐지?_
        - mysqldump : 백업 프로그램
### 4.2.2 테이블 락
- 명시적인 테이블 락
    - `lock tables table_name [READ | WRITE]` 명령으로 획득 가능
        - 개별 테이블 단위로 설정되는 잠금
    - `unlock tables` 명령으로 잠금 해제
- 묵시적인 테이블 락
    - MyISAM이나 MEMORY 테이블에 데이터를 변경하는 쿼리를 실행하면 발생함
    - InnoDB 테이블은 스토리지 엔진 차원에서 레코드 기반의 잠금을 제공
        - 단순 데이터 변경 쿼리로 인해 묵시적인 테이블 락이 설정되지 않음
    - 즉, DML에 대해서는 레코드 기반 잠금이, DDL에 대해서는 테이블 락이 묵시적으로 설정된다.

### 4.2.3 유저 락
- `GET_LOCK()` 함수를 이용해 임의로 잠금 설정
- 사용자가 지정한 문자열(String)에 대해 획득하고 반납(해제)하는 잠금
    - _Q. 사용자가 지정한 문자열이라는 것이 어떤 걸 말하는 거지?_
- `IS_FREE_LOCK()` : 잠금 설정 확인
- `RELEASE_LOCK()` : 잠금 반납
- 많은 레코드를 한 번에 변경하는 트랜잭션의 경우에 유용하게 사용할 수 있음
    - ex.배치 - 동일 데이터를 변경하거나 참조하는 프로그램끼리 분류해서 유저락을 걸고 쿼리를 실행하면 간단히 해결할 수 있다.
    - _Q. 잘 이해 안감!! 왜 많은 레코드를 변경할때 유저락을걸지? 요거에 대한 예시!! _

### 4.2.4 네임 락
- 데이터베이스 객체(ex. 테이블, 뷰 등)의 이름을 변경하는 경우 획득하는 잠금
- `rename table tab_a to tab_b`와 같이 테이블의 이름 변경 시 자동으로 획득
    - _Q. 왜 아래는 에러가 나고 위에는 안나지?!!? 어디에 잠금이 걸리는거야!! ?_
    
## 4.3 MyISAM과 MEMORY 스토리지 엔진의 잠금
- 자체적인 잠금을 가지지 않고 MySQL 엔진에서 제공하는 테이블 락을 사용
- 쿼리 단위로 필요한 잠금을 한꺼번에 모두 요청해서 획득하기 때문에 데드락이 발생할 수 없다 
    - _Q.->?? 뭔가 추상적!! 예시가 궁금 어떤 상황에서 이노디비는 데드락이 걸리고 
  여긴 안걸리는지??_

### 4.3.1 잠금 획득
- 읽기 잠금 : 테이블에 쓰기 잠금이 걸려있지 않으면 바로 읽기 잠금을 획득하고 읽기 작업을 시작
- 쓰기 잠금 : 테이블에 아무런 잠금이 걸려있지 않아야 쓰기 잠금을 획득할 수 있고, 그렇지 않다면 다른 잠금이 해제될 때까지 대기해야 함

### 4.3.2 잠금 튜닝
- `table_locks_immediate`: 다른 잠금이 풀리기를 기다리지 않고 바로 잠금을 획득한 횟수
- `table_locks_waited` : 다른 잠금이 이미 해당 테이블을 사용하고 있어서 기다린 횟수
- `잠금 대기 쿼리 비율` = Table_locks_waited / (Table_locks_immediate + Table_locks_waited) * 100;
    - 이 값이 크다면 테이블 잠금 때문에 경합이 많이 발생하고 있다는 것이고 처리 성능이 영향을 받고 있다는 것이기 때문에 테이블을 분리한다거나 InnoDB 스토리지 
      엔진으로 변환하는 방법을 고려해야 한다.
      - InnoDB는 레코드 단위의 잠금을 사용
    
### 4.3.3 테이블 수준의 잠금 확인 및 해제
- `LOCK TABLES` : 명시적으로 테이블 잠금을 획득
- SELECT, INSERT, DELETE, UPDATE 및 DDL과 같은 쿼리 : 묵시적으로 테이블 잠금을 획득
- `SHOW OPEN TABLES` : 테이블 잠금 여부 확인
    - In_use : 해당 테이블을 잠그고 있는 클라이언트의 수 + 테이블의 잠금을 기다리는 클라이언트의 수
- `SHOW PROCESSLIST` : 어떤 클라이언트의 커넥션이 잠금을 기다리고 있는지 확인 
    - `State : Locked` : 테이블 락을 기다리고 있음
    - `KILL QUERY 클라이언트 id` : 클라이언트가 실행하고 있는 쿼리 종료

## 4.4 InnoDB 스토리지 엔진의 잠금
- 스토리지 엔진 내부에서 **레코드 기반**의 잠금 방식을 탑재하고 있음
    - MyISAM보다 훨씬 뛰어난 동시성 처리 제공
- INFORMATION_SCHEMA 데이터베이스에 존재하는 INNODB_TRX, INNODB_LOCKS, INNODB_LOCK_WAITS 테이블을 조인해서
  현재 어떤 트랜잭션이 어떤 잠금을 대기하고 있고 해당 잠금을 어느 트랜잭션이 가지고 있는지 확인할 수 있다.

### 4.4.1 InnoDB의 잠금 방식

#### 비관적 작금 
- 현재 트랜잭션에서 변경하고자 하는 레코드에 대해 잠금을 획득하고 변경 작업을 처리하는 방식
- 높은 동시성 처리에는 비관적 잠금이 유리

#### 낙관적 잠금
- 우선 변경 작업을 수행하고 마지막에 잠금 충돌이 있었는지 확인해 문제가 있었다면 ROLLBACK 처리하는 방식

### 4.4.2 InnoDB의 잠금 종류

#### 레코드 락
- 레코드 자체만을 잠그는 것
- InnoDB 스토리지 엔진은 레코드 자체가 아니라 인덱스의 레코드를 잠근다
    - 인덱스가 없는 테이블일 경우 내부적으로 자동 생성된 클러스터 인덱스를 이용해 잠금을 설정
- 프라이머리 키 또는 유니크 인덱스에 의한 변경 작업은 레코드 자체에 대해서는 락을 건다.
    - _Q. 프라이머리 키 또는 유니크 인덱스에 대한 변경 작업의 예시?_
    
#### 갭 락
- 레코드와 바로 인접한 레코드 사이의 간격만을 잠그는 것을 의미
- 역할 : 레코드와 레코드 사이의 간격에 새로운 레코드가 생성되는 것을 제어
- 넥스트 키 락의 일부로 사용됨

#### 넥스트 키 락
- 레코드 락 + 갭 락
- STATEMENT 포맷의 바이너리 로그를 사용하는 MySQL 서버에서는 REPEATABLE READ 격리 수준을 사용해야 한다.
    - _Q. 무슨 말인지 이해가 안감_
- 역할 : 바이너리 로그에 기록되는 쿼리가 슬레이브에서 실행될 때 마스터에서 만들어 낸 결과와 동일한 결과를 만들어내도록 보장
- 문제점 : 넥스트 키 락 때문에 데드락이 발생하거나 다른 트랜잭션을 기다리게 만드는 일이 자주 발생
    - 바이너리 로그 포맷을 ROW 형태로 바꿔서 넥스트 키 락을 줄이자

#### 자동 증가 락

- 자동 증가하는 숫자 값을 채번하기 위해 AUTO_INCREMENT라는 칼럼 속성을 사용
- 동시에 여러 레코드가 INSERT되는 경우, 저장되는 각 레코드는 중복되지 않고 저장된 순서대로 증가한 일련번호 값을 가져야 함
- 이를 위해 사용하는 것이 AUTO_INCREMENT 락이라고하며 테이블 수준의 잠금을 사용
- INSERT, REPLACE와 같이 새로운 레코드를 저장하는 쿼리에서만 필요
    - UPDATE, DELETE는 필요하지 않음
- 작동 방식
    - `innodb_autoinc_lock_mode = 0`
      - 기본 잠금 방식, 모든 INSERT 문장은 자동 증가 락을 사용
    - `innodb_autoinc_lock_mode = 1`
      - 단순히 한 건 또는 여러 건의 레코드를 ㅑㅜㄴㄸㄲ
    - `innodb_autoinc_lock_mode = 2`
    
### 4.4.3 인덱스와 잠금
### 4.4.4 트랜잭션 격리 수준과 잠금
### 4.4.5 레코드 수준의 잠금 확인 및 해제


## 4.5 MySQL의 격리 수준