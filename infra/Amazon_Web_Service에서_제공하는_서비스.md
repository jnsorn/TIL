- 생활코딩 EC2 (2015-12-28) [링크](https://opentutorials.org/module/1946/11273)
- Amazon Web Services로 시작하는 클라우드 입문(WINGS 프로젝트 아사 시호)

# Amazon Web Service에서 제공하는 서비스

## 컴퓨팅 관련 서비스

가상 서버 기능 및 컨테이너 실행 환경에 대한 관리 서비스 제공

### Amazon EC2

- Amazon Elastic Compute Cloud
- 종량제 형태로 과금되는 가상 서버 기능(Linux, Windows 서버 등)
- EC2에서 가동되고 있는 가상 서버를 인스턴스라고 부름

### Amazon EC2 Container Service

- Docker를 운용하는 서비스
    - Docker : 가상화 기술을 사용한 애플리케이션 실행 환경 구축을 위한 툴

### Amazon EC2 Container Registry

- Docker 이미지를 보관하고 공유하는 서비스
    - Docker 이미지 : 애플리케이션 실행 환경을 패키징한 것

### AWS Elastic Beanstalk

- PaaS 서비스
- 앱을 자동으로 AWS에서 배포 할 수 있음
- Web 서버의 부하에 따라서 자동으로 서버를 증가시켜줌

### AWS Lambda

- 클라이언트의 리퀘스트 발생 시점에 임의의 프로그램을 구동시키는 이벤트 드리븐 형태의 서비스

### Auto Scaling

- CPU 사용률 등 사전에 정해진 조건에 의해서 EC2 인스턴스를 자동적으로 증감시키는 서비스
- Web 시스템에서의 급격한 부하 증가에도 유연하게 대응할 수 있음

### Elastic Load Balancing

- 트래픽에 따라서 복수 개의 EC2 인스턴스로 부하를 분산시키는 서비스

## 스토리지 & 콘텐츠 전송 관련 서비스

데이터를 쉽게 관리할 수 있는 서비스

### Amazon S3

- 파일 서버

### Amazon CloudFront

- 전 세계에 콘텐츠를 전송하기 위한 네트워크 서비스
- 동영상 등을 에지 로케이션이라고 불리는 거점에 자동적으로 전달하고 이용자로부터 가장 가까운 에지 로케이션에서 콘텐츠를 전송함

### Amazon EBS

- Amazon EC2의 데이터를 보관하는 스토리지 서비스
- EC2의 하드디스크, SSD와 같은 역할을 함
- 스냅샷 백업 가능

### Amazon Elastic File System

- EC2의 공유 파일 저장 서비스
- 파일의 추가/삭제에 따라서 자동으로 용량을 확장/축소하는 스토리지

### Amazon Glacier

- 저렴한 가격으로 이용할 수 있는 스토리지 서비스
- 백업이나 아카이브등의 용도에 적합
- 사용 빈도는 낮지만 장기 보존이 필요한 데이터에 이용

### AWS Import/Export Snowball

- 페타 바이트 급의 대용량 데이터 전송 서비스
- 데이터 센터의 이전, 재해 시 데이터 전환등에 사용

### AWS Storage Gateway

- 온프레미스 환경과 AWS를 접속하는 스토리지 게이트웨이
    - 온프레미스 : 회사 내에서 자체적으로 데이터센터를 보유하고 시스템 구축에서 운용까지 직접 수행하는 형태

## 데이터베이스 관련 서비스

가상 데이터베이스 기능 제공

### Amazon RDS

- 관계형 데이터베이스(RDBMS)를 구축/운용하는 서비스

### AWS Database Migration Service

- 최소한의 정지 시간에 데이터베이스를 이행할 수 있는 서비스

### Amazon DynamoDB

- NoSQL 데이터베이스 서비스를 구축/운용하는 서비스
- 비정형 데이터 쉽게 취급 가능

### Amazon ElasticCache

- 클라우드에서 메모리의 캐시 관리가 가능한 서비스
- 고속의 메모리 내 캐시로부터 정보를 취득함

### Amazon Redshift

- 빅 데이터를 위한 데이터웨어 하우스
- 페타 바이트 규모의 데이터를 분석할 수 있는 서비스

## 네트워크 관련 서비스

### Amazon VPC

- AWS내에서 사설 네트워크(Private Network)를 구축하기 위한 서비스
- VPC를 사용하면 네트워크 세그먼트를 분할하고 방화벽을 배치하는 것만으로 보안 요건에 따른 제어가 가능하게 됨
    - 네트워크 세그먼트 : 네트워크의 작은 영역(서브 네트워크보다 같거나 작은 것을 의미)

### AWS Direct Connect

- 온프레미스 환경의 네트워크와 AWS의 VPC 네트워크를 직접 접속하기 위한 전용선 서비스

### Amazon Route 53

- 도메인 네임과 IP 주소를 매칭시키는 DNS 시스템을 구축하기 우히ㅏㄴ 서비스

## 개발자용 도구

### AWS CodeDeploy

- 개발한 애플리케이션을 운영 환경에 자동으로 배피

## 관리툴

시스템들의 상태를 관리하기 위한 툴

### Amazon CloudWatch

- AWS 리소스를 감시하는 서비스
- 사전에 설정된 값을 초과하면 경보를 발생시키는 운용 관리 기능이 있음

## 분석툴

### Amazon Elasticsearch Service

- ElasticSearch 클러스터를 실행하고 자동으로 확장 또는 축소할 수 있는 스케일링 서비스

## 애플리케이션 서비스

### Amazon SNS

- 푸쉬 알림 서비스

### Amazon SQS

- 메시지 큐 서비스