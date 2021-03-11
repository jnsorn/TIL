> 정리의 모든 내용은 [기억보단 기록을](https://jojoldu.tistory.com/324?category=902551) 블로그를 참고하였습니다.

# 7. Spring Batch 가이드 - ItemReader

## ItemReader 소개

- ItemReader : read()를 통해 데이터를 읽어옴
- ItemStream : 주기적으로 상태를 저장하고 오류가 발생하면 해당 상태에서 복원하기 위한 마커 인터페이스
    - 즉, ItemReader의 상태를 저장하고 실패한 곳에서 다시 실행할 수 있게 해주는 역할

## Database Reader

읽기 작업 시 `분할 처리`를 위해 Spring Batch가 지원하는 2가지 Reader 타입

- Cursor : DB와 커넥션을 맺은 후, Cursor를 한칸씩 옮기면서 지속적으로 데이터를 빨아옴
- Paging : 한 번에 개발자가 지정한 pageSize만큼 데이터를 가져옴

# 8. Spring Batch 가이드 - ItemWriter

Spring Batch에서 사용하는 출력 기능

# 9. Spring Batch 가이드 - ItemProcessor

Reader에서 넘겨준 데이터 개별건을 가공/처리해줌

# 10. Spring Batch 가이드 - Spring Batch 테스트 코드

## @SpringBatchTest
ApplicationContext에 테스트에 필요한 여러 유틸 Bean을 등록해줌
- `JobLauncherTestUtils` : 스프링 배치 테스트에 필요한 전반적인 유틸 기능들을 지원
- `JobRepositoryTestUtils` : DB에 생성된 JobExecution을 쉽게 생성/삭제 가능하게 지원
- `StepScopeTestExecutionListener`, `JobScopeTestExecutionListener`
  - 배치 단위 테스트시 StepScope/JobScope 컨텍스트를 생성
  - 해당 컨텍스트를 통해 JobParameter등을 단위 테스트에서 DI 받을 수 있음