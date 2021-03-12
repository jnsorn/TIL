# 비정상종료 

배치 실행시 아래와 같은 에러 발생

```a job execution for this job is already running job instance id = ? ```

서버가 비정상 종료된 경우 결과를 테이블에 저장하지 못해 발생(SimpleJobLauncher)
```java
/*
* validate here if it has stepExecutions that are UNKNOWN, STARTING, STARTED and STOPPING
* retrieve the previous execution and check
  */
  for (StepExecution execution : lastExecution.getStepExecutions()) {
  BatchStatus status = execution.getStatus();
  if (status.isRunning() || status == BatchStatus.STOPPING) {
  throw new JobExecutionAlreadyRunningException("A job execution for this job is already running: "
  + lastExecution);
  } else if (status == BatchStatus.UNKNOWN) {
  throw new JobRestartException(
  "Cannot restart step [" + execution.getStepName() + "] from UNKNOWN status. "
  + "The last execution ended with a failure that could not be rolled back, "
  + "so it may be dangerous to proceed. Manual intervention is probably necessary.");
  }
  }
  ```
메타 정보를 저장하는 테이블을 수동으로 업데이트하여 해결 시도했지만 똑같은 오류가 계속 발생

```sql
UPDATE 
    BATCH_JOB_EXECUTION 
SET 
    END_TIME = SYSTIMESTAMP, 
    STATUS = 'FAILED', 
    EXIT_CODE = 'COMPLETED' 
WHERE 
   JOB_EXECUTION_ID =
      (SELECT 
         MAX(JOB_EXECUTION_ID) 
       FROM 
         BATCH_JOB_EXECUTION 
       WHERE 
         JOB_INSTANCE_ID = #{jobInstanceId}
      );

  ```

BATCH_JOB_EXECUTION 테이블 외의 모든 메타 테이블을 찾아서 해당 작업에 대한 레코드를 삭제.

삭제하다보니 BATCH_STEP_EXECUTION 에도 이미 작업이 시작된 레코드가 존재.

아마 이 레코드 때문에 계속 걸린 것 같은데 이미 삭제를 진행하고 있어서 그냥 다 삭제. 정상 동작

