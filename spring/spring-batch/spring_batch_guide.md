> 1. Spring Batch 가이드 - 배치 어플리케이션이란?  [(기억보단 기록을)](https://jojoldu.tistory.com/324?category=902551)

### 배치 애플리케이션의 조건

- `대용량 데이터` : 대량의 데이터를 가져오거나, 전달하거나, 계산하는 등의 처리를 할 수 있어야 합니다.
- `자동화` : 심각한 문제 해결을 제외하고는 사용자 개입 없이 실행되어야 합니다.
- `견고성` : 잘못된 데이터를 충돌/중단 없이 처리할 수 있어야 합니다.
- `신뢰성` : 무엇이 잘못되었는지를 추적할 수 있어야 합니다.(로깅, 알림)
- `성능` : 지정한 시간 안에 처리를 완료하거나 동시에 실행되는 다른 어플리케이션을 방해하지 않도록 수행되어야 합니다.

## Spring Batch?

- Reader : 데이터를 읽어오는 모듈
- Writer : 데이터를 쓰는 모듈

- 현재 Spring Batch 4.0(Spring Boot 2.0)에서 지원하는 Reader & Writer 

|DataSource|기술|설명| 
|---|---|---|
|DB|JDBC|페이징, 커서, 일괄 업데이트 등 사용 가능|
|DB|Hibernate|페이징, 커서 사용 가능|
|DB|JPA|페이징 사용 가능(현재 버전에선 커서 없음)|
|File|Flat file|지정한 구분자로 파싱 지원|
|File|XML|XML 파싱 지원|

## Quartz
- Quartz : 스케줄러
- Batch : 대용량 배치 처리에 대한 기능 지원
- 따라서 둘이 조합해서 많이 사용함
- 정해진 스케줄마다 Quartz가 Spring Batch를 실행하는 구조

> 2. Spring Batch 가이드 - Batch Job 실행해보기 [(기억보단 기록을)](https://jojoldu.tistory.com/325?category=902551)

- Spring Batch에서 Job은 하나의 배치 작업 단위를 이야기 함
- Job은 여러개의 Step으로 이루어져있음
- Step은 Tasklet 혹은 Reader&Processor&Writer 묶음이 존재
    - Tasklet : Spring MVC의 @Component, @Bean과 비슷한 역할이라고 생각해도 됨,
    명확한 역할은 없지만, 개발자가 지정한 커스텀한 기능을 위한 단위로 보기
      
[Job](../../images/spring-batch/job.png)

