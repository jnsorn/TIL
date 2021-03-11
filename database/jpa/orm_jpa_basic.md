# 자바 ORM 표준 JPA 프로그래밍 - 기본편

## 객체지향 쿼리 언어1 - 기본 문법

### 페이징

- JPA는 페이징을 다음 두 API로 추상화
    - setFirstResult(int startPosition) : 조회 시작 위치(0부터 시작)
    - setMaxResults(int maxResult) : 조회할 데이터 수 


### JPQL 타입 표현과 기타식

- JPQL 타입 표현 
  - ENUM : 패키지명을 포함한 이름을 넣어야 함
  - 엔티티 타입 : TYPE(m) = Member

- JPQL 기본 함수
  - JPQL에서 제공하는 표준함수 
    - CONCAT
    - SUBSTRING
    - TRIM
    - LOWER, UPPER
    - LENGTH
    - LOCATE
    - ABS, SQRT, MOD
    - SIZE, INDEX(JPA 용도)
  - 사용자 정의 함수
    - MySQLDialect에 이미 등록되어 있는 경우가 많음
    - 사용하는 DB방언을 상속받고 사용자 정의 함수를 등록한다
      ```sql 
      select function('group_concat',i.name') from Item i
      ``` 
      

  
