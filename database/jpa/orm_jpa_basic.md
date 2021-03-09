# 자바 ORM 표준 JPA 프로그래밍 - 기본편

## 객체지향 쿼리 언어1 - 기본 문법

### 페이징

- JPA는 페이징을 다음 두 API로 추상화
    - setFirstResult(int startPosition) : 조회 시작 위치(0부터 시작)
    - setMaxResults(int maxResult) : 조회할 데이터 수 
