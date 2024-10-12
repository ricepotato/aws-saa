# aws-saa

## 빈약한 부분

- ASG (Auto Scaling Group)
- Elasticache (memcached, redis 특징등)
- EC2 instance fleet
- S3 버킷 트래버스
- S3 전송 가속화
- S3 배치 작업
- S3 SSE
- S3 객체 잠금 - 거버넌스, 규정 준수 모드
- S3 법적 보존
- 

## aws saa 틀린문제 정리

### EC2 예약 인스턴스를 예약할 수 있는 기간은 얼마인가요?

1년 혹은 3년



### 온프레미스에 호스팅된 OLTP 데이터베이스를 갖춘 전자 상거래 애플리케이션이 있습니다. 이 애플리케이션은 인기가 좋아, 데이터베이스가 초당 수천 개의 요청을 지니게 됩니다. 여러분은 데이터베이스를 EC2 인스턴스로 이전하려 합니다. 이렇게 높은 빈도를 보이는 OLTP 데이터베이스를 처리하기 위해서는 어떤 EC2 인스턴스 유형을 선택해야 할까요?

스토리지 최적화


### 노스버지니아 리전 us-east-1에서 AMI를 사용하면 어떤 AWS 리전에 있는 EC2 인스턴스라도 실행할 수 있습니다

아닙니다.

```
AMI는 특정 AWS 리전에 국한되며, 각 AWS 리전에는 고유한 AMI가 있습니다. 다른 AWS 리전에서 AMI를 사용해 EC2 인스턴스를 실행하는 것은 불가능하지만,
대상 AWS 리전으로 AMI를 복사해 EC2 인스턴스를 생성하는 것은 가능합니다.
```


### 다음 중, EC2 인스턴스를 생성할 때 부팅 볼륨으로 사용할 수 있는 EBS 볼륨 유형은 무엇인가요?

gp2, gp3, io1, io2

```
EC2 인스턴스를 생성할 때, 부팅 볼륨으로는 다음의 EBS 볼륨 유형만을 사용할 수 있습니다: gp2, gp3, io1, io2, Magnetic(표준)
```

### EBS 다중 연결이란 무엇일까요?

동일한 EBS 볼륨을 다수의 EC2 인스턴스에 연결

```
EBS 다중 연결을 사용하면, 동일한 EBS 볼륨을 동일 AZ 상에 있는 다수의 EC2 인스턴스에 연결할 수 있습니다. 각 EC2 인스턴스는 완전한 읽기/쓰기 권한을 갖게 됩니다.
```


### EC2 인스턴스에 연결되어 있는, 암호화되지 않은 EBS 볼륨을 암호화하려 합니다. 어떻게 해야 할까요?

EBS 볼륨의 스냅샷을 생성하고 복사한 뒤 복사된 스냅샷을 암호화 하는 옵션을 체크합니다. 그 후, 암호화된 스냅샷을 사용해서 새로운 EBS 볼륨을 만듭니다.


### EC2 인스턴스에 호스팅된 애플리케이션에 고성능 로컬 캐시를 포함시키려 합니다. EC2 인스턴스 종료 시, 캐시가 소실되어도 문제가 없는 상황입니다. 이런 경우, 솔루션 아키텍트로서 어떤 스토리지 메커니즘을 추천할 수 있을까요?

인스턴스 스토어

```
EC2 인스턴스 스토어는 최적의 디스크 I/O 성능을 제공합니다.
```


### 기반 스토리지에 310,000의 IOPS가 필요한 고성능 데이터베이스를 실행하고 있습니다. 어떤 방법을 추천할 수 있을까요?

인스턴스 스토어

```
EBS io1 혹은 io2 볼륨 유형으로 달성할 수 있는 최대 IOPS는 64,000입니다.

EC2 인스턴스에서 데이터베이스를 인스턴스 스토어를 사용하여 실행 가능하지만,
EC2 인스턴스가 중지 시 데이터가 손실이라는 문제가 있습니다(문제 없이 다시 시작할 수 있음).
한 가지 솔루션은 인스턴스 스토어가 있는 다른 EC2 인스턴스에서 복제 메커니즘을 설정하여 대기 복사본을 가질 수 있다는 것입니다.
또 다른 솔루션은 데이터에 대한 백업 메커니즘을 설정하는 것입니다. 요구 사항을 검증하기 위해 아키텍처를 설정하는 방법은 모두 사용자에게 달려 있습니다.
이 사용 사례에서는 IOPS 기준이므로 EC2 인스턴스 스토어를 선택해야 합니다.

인스턴스 스토어를 사용하는 EC2 인스턴스에 데이터베이스를 실행할 수는 있으나,
EC2 인스턴스가 중단되었을 때 데이터가 소실되는 문제가 발생합니다(재시작에는 아무 문제가 없습니다). 솔루션으로는 대기 복사본을 가질 수 있도록,
인스턴스 스토어를 가진 다른 EC2 인스턴스에 복제 메커니즘을 설정하는 방법이 있습니다. 또다른 솔루션으로는 데이터의 백업 메커니즘을 설정하는 방법이 있습니다.
요구 사항을 검증하기 위한 아키텍처 설정은 온전히 여러분의 몫입니다. 이 사용 사례는 IOPS를 다루고 있으므로, EC2 인스턴스 스토어를 선택해야 합니다.
```


### Elastic Load Balancer가 관리하는 한 세트의 EC2 인스턴스 상에 애플리케이션을 호스팅했습니다. 일주일 후, 사용자들은 가끔씩 애플리케이션이 작동하지 않는다며 호소하기 시작했습니다. 문제점을 조사한 결과, 일부 EC2 인스턴스가 이따금 충돌한다는 문제점이 발견되었습니다. 사용자들이 충돌하는 EC2 인스턴스에 연결되지 않도록 보호하기 위해서는 어떻게 해야 할까요?

ELB 상태 확인(health check) 활성화

```
ELB 상태 확인을 활성화하면, ELB가 비정상(충돌) EC2 인스턴스로는 트래픽을 보내지 않게 됩니다.
```


### Application Load Balancer는 트래픽을 다른 대상 그룹으로 라우팅할 수 있습니다. 이때 확인할 내용으로 사용할 수 없는 것을 고르세요

클라이언트의 위치(지리적)


```
ALB는 URL 경로, 호스트 이름, HTTP 헤더 및 쿼리 문자열을 기반으로 트래픽을 다른 대상 그룹으로 라우팅할 수 있습니다.
```


### 규정 준수를 위해, 고정된 정적 IP 주소를 최종 사용자에게 노출하여 사용자들이 안정적이고, 규제 기관의 승인을 받은 방화벽 규칙을 작성할 수 있도록 하려 합니다. 이런 경우, 다음 중 어떤 종류의 Elastic Load Balancer를 사용해야 할까요?

Network Load Balancer

```
Network Load Balancer는 AZ 당 하나의 정적 IP 주소를 가지며, 여기에 탄력적 IP 주소를 연결할 수 있습니다.
Application Load Balancer와 Classic Load Balancer를 정적 DNS 이름으로 사용할 수 있습니다.
```

### Application Load Balancer 내에 사용자 지정 애플리케이션 기반 쿠키를 생성하려 합니다. 다음 중 쿠키의 이름으로 사용 가능한 것은 무엇인가요?

APPUSERC

```
다음의 쿠키 이름은 ELB가 선점하고 있습니다(AWSALB, AWSALBAPP, AWSALBTG).
```



### us-east-1에 있는 한 세트의 EC2 인스턴스에 트래픽을 배분하는 Network Load Balancer가 있습니다. us-east-1b AZ에 2개의 EC2 인스턴스, us-east-1e AZ에는 5개의 EC2 인스턴스가 있습니다. 여러분은 us-east-1b AZ에 있는 EC2 인스턴스의 CPU 사용률이 더 높다는 것을 발견했습니다. 조사를 거친 결과, 두 개의 AZ에 걸쳐 분배된 트래픽의 양은 동일한 것으로 나타났습니다. 이 경우, 어떻게 문제를 해결해야 할까요?

영역간 로드밸런싱 활성화

```
영역간 로드 밸런싱을 활성화하면, ELB가 모든 AZ에 있는 등록된 EC2 인스턴스 전체에 동등하게 분배됩니다.
```


### 다음과 같은 호스트 이름을 기반으로, 트래픽을 3개의 대상 그룹으로 리다이렉팅하도록 구성된 Application Load Balancer가 있습니다: users.example.com, api.external.example.com, checkout.example.com. 이 각각의 호스트 이름에 HTTPS를 구성하려 합니다. 이런 작업을 위해서는 ALB를 어떻게 구성해야 할까요?

서버 이름 표식(SNI) 사용


```
서버 이름 표식(SNI)을 사용하면 동일한 리스너 상에 있는, 자체 SSL 인증서를 가진 다수의 HTTPS 애플리케이션을 노출시킬 수 있습니다.
더 많은 정보는 여기를 참고하세요: https://aws.amazon.com/blogs/aws/new-application-load-balancer-sni/
```

### Application Load Balancer가 관리하는 오토 스케일링 그룹이 있습니다. ASG가 ALB 상태 확인을 사용하도록 구성을 해둔 상태인데, EC2 인스턴스가 비정상인 것으로 보고되었습니다. EC2 인스턴스에는 무슨 일이 일어나게 될까요?

ASG 가 EC2 인스턴스를 종료함

```
오토 스케일링 그룹이 EC2 상태 확인(기본 설정)이 아닌 Application Load Balancer의 상태 확인을 기반으로 EC2 인스턴스의 상태를 판단하도록 구성할 수 있습니다.
EC2 인스턴스가 ALB의 상태 확인에 실패할 경우, 이는 비정상인 것으로 표시되어 종료되며 ASG는 새로운 EC2 인스턴스를 실행합니다.
```


### 여러분의 상사가 애플리케이션이 데이터베이스로 보내는 분당 요청 수를 기반으로 오토 스케일링 그룹을 스케일링하라고 요청했습니다. 어떻게 해야 할까요?

CloudWatch 사용자 지정 지표를 생성한 후 ASG 를 스케일링 하기 위한 CloudWatch 경보 생성


```
백엔드-데이터베이스 연결에는 ‘분당 요청'에 해당하는 CloudWatch 지표가 존재하지 않습니다. CloudWatch 경보를 생성하려면
CloudWatch 사용자 지정 지표를 먼저 생성해야 합니다.
```


### 한 애플리케이션이 애플리케이션 로드 밸런서(ALB)와 오토 스케일링 그룹(ASG)을 통해 배포되었습니다. 이 상태에서 ASG의 크기를 수동으로 조정해 EC2 인스턴스의 평균 연결 개수가 약 1,000개가 되도록 조정 정책을 정의하려고 합니다. 어떤 조정 정책을 사용해야 합니까?

대상 추적 조정 정책

```
ALB만이 EC2 인스턴스로 액세스할 수 있게 할 수 있는 가장 안전한 방법입니다. 규칙에서 보안 그룹을 참조하는 것은 매우 강력한 규칙으로,
이 내용을 바탕으로 많은 시험 문제가 출제됩니다. 그러니 이에 관련된 내용은 반드시 숙지하도록 하세요!
```


### RDS 데이터베이스가 웹사이트에서 들어오는 요청량을 처리하는 데에 어려움을 겪고 있습니다. 백만 명의 사용자들은 대부분 뉴스를 읽고 있으며, 뉴스가 자주 포스팅되는 편은 아닙니다. 이 문제를 해결하기 위해 사용해서는 안 되는 솔루션은 무엇인가요?

- ✅ RDS 다중 AZ
- ElastiCache 클러스터
- RDS 읽기 다중 복제본

```
시험 문제를 읽을 때는 주의하시기 바랍니다. 이 문제에서는, 이런 문제점을 해결하기 위해 사용해서는 "안 되는" 솔루션이 무엇인지 묻고 있습니다.
ElastiCache와 RDS 읽기 전용 복제본은 읽기 스케일링에 도움이 됩니다.
```


### 다음 RDS (Aurora 아님) 기능들 중, 사용 시 SQL 연결 문자열을 변경하지 않아도 되는 것은 무엇인가요?

다중 AZ

```
다중 AZ는 활성화 상태의 데이터베이스 종류와 상관 없이 동일한 연결 문자열을 유지합니다.
```

### 주 AWS 리전에 재해가 발생했을 때에 대비하여 다른 AWS 리전에 데이터베이스의 복제본을 만들어 두려 합니다. 이런 작업을 쉽게 구현하기 위해서는 어떤 데이터베이스의 사용을 추천할 수 있을까요?


Aurora 글로벌 데이터베이스

```
Aurora 글로벌 데이터베이스를 사용하면 최대 5개의 2차 리전까지 Aurora 복제본을 가질 수 있습니다.
```


### RDS PostgreSQL 데이터베이스의 특정 리전에서 정전이 발생했을 때 데이터베이스가 신속하게 다른 AWS 리전에서 읽고 쓰는 작업을 할 수 있도록 재해 복구 전략을 수립하려고 합니다. DR 데이터베이스는 가용성이 매우 높아야 합니다. 가장 적합한 방식은 무엇입니까?

다른 리전에 읽기 전용 복제본을 만들고 해당 읽기 전용 복제본에서 다중 AZ 를 활성화 한다.

### 다음 중 RDS 읽기 전용 복제본과 다중 AZ로의 복제 작업을 적절하게 묘사한 설명은 무엇인가요?

읽기 전용 복제본은 비동기 복제를 사용하고, 다중 AZ 복제본은 동기 복제를 사용한다.

### RDS 데이터베이스를 위해 최대 *15* 개의 읽기 전용 복제본을 가질 수 있습니다.


### 다음 중 IAM을 이용한 웹 서버 인증을 지원하지 않는 RDS 데이터베이스 기술은 무엇인가요?

Oracle

### 암호화되지 않은 RDS DB 인스턴스가 있는 상태에서 읽기 전용 복제본을 생성하려 합니다. RDS 읽기 전용 복제본이 암호화되도록 구성할 수 있을까요?

아니오

```
암호화되지 않은 RDS DB 인스턴스로는 암호화된 읽기 전용 복제본을 생성할 수 없습니다.
```

### 재해 복구 및 감사 목적으로 Aurora 데이터베이스에 대한 장기 백업을 저장해야 합니다. 어떤 것을 추천하시나요?

Aurora 데이터베이스 복제 사용


### 여러분의 개발팀은 프로덕션 데이터에 최대한 빨리 액세스해야 하기 때문에 프로덕션 Aurora 데이터베이스에 대해 읽기 및 쓰기 테스트를 수행하고자 합니다. 어떤 조언을 해주시겠습니까?

Aurora 복제 기능 사용


### 다음 중 "무상태" 애플리케이션 티어를 설계하는 데에 도움이 되지 않는 것을 고르세요.

- ✅ EBS 볼륨에 세션 데이터 저장하기
- 클라이언트 HTTP 쿠키에 세션 데이터 저장하기
- Amazon Elasticache 에 세션 데이터 저장하기
- 세션 데이터를 Amazon RDS 에 저장하기

### 관리 중인 Linux EC2 인스턴스 100s에 소프트웨어 업데이트를 설치하려 합니다. 이 업데이트를 EC2 인스턴스로 동적으로 로딩되어야 하며, 많은 양의 연산을 요구해서는 안 되는 공유 스토리지에 저장하고자 합니다. 어떤 방법을 사용해야 할까요?

- ✅ EFS 에 소프트웨어 업데이트를 저장하고 스타트업 시 EFS 를 네트워크 드라이브로 마운트하기
- EBS 에 소프트웨어 업데이트를 저장하고 각 AZ 의 한 마스터로부터 데이터 복제 소프트웨어를 사용해 동기화하기
- 소프트웨어 업데이트를 EBS 스냅샷으로 패키징하고 새로운 소프트웨어 업데이트 각각에 EBS 볼륨 생성하기
- Amazon RDS 에 소프트웨어 업데이트 저장하기

```
EFS는 EC2 인스턴스의 100s에 동일한 파일 시스템을 마운트할 수 있게 해주는 네트워크 파일 시스템(NFS)입니다.
EFS에 소프트웨어 업데이트를 저장하면 각 EC2 인스턴스가 이들을 평가할 수 있게 됩니다.
```


### 25GB 크기의 파일을 S3에 업로드하려 시도 중이지만, 오류가 발생하고 있습니다. 이 경우, 가능성이 있는 원인은 무엇일까요?

- ✅ 5GB 보다 크기가 큰 파일을 업로드ㅏㄹ 때는 멀티파트 업로드를 사용함

```
파일 크기가 100MB가 넘는 경우에는 멀티파트 업로드가 권장됩니다.
```


### Amazon RDS PostgreSQL을 사용하여 S3에 파일의 인덱스를 구축하려 합니다. 인덱스 구축을 위해서는 파일 콘텐츠 자체의 메타데이터를 포함하고 있는 S3에 있는 각 객체의 첫 250바이트를 읽는 작업이 필수적입니다. S3 버킷에는 총 50TB에 달하는 100,000개 이상의 파일이 포함되어 있습니다. 이 경우 인덱스를 구축하기 위한 효율적인 방법은 무엇일까요?

- ✅ S3 버킷을 트래버스하고, 파일을 하나씩 읽어 들여 첫 250 바이트를 추출한 후, EDS 에 해당 정보를 저장하는 애플리케이션 생성


### 고객이 S3에서 파일 암호화가 수행되도록 하고 싶지만 암호화 키는 AWS 저장하지 않고 본인이 온전히 관리하고 싶다고 합니다. 이럴 경우 ........................... 를 사용할 것을 권장합니다.

- ✅ SSE-C

### 회사에서 S3 버킷에 데이터와 파일을 저장하고 있습니다. 이 파일 중 일부는 회사 규정 준수 정책에 따라 정해진 기간 동안 보관해야 하며 덮어쓰거나 삭제되지 않도록 보호해야 합니다. 이런 경우에 사용할 수 있는 S3 기능은 무엇입니까?

- ✅ S3 객체 잠금 - 보존 규정 준수 모드
- ❌ S3 객체 잠금 - 보존 거버넌스 모드
- ❌ S3 Glacier 볼트 잠금

### 3개의 서로 다른 애플리케이션으로 동일한 메시지를 보내려 합니다. 3개의 애플리케이션 모두 SQS를 사용하고 있습니다. 이를 위해, 어떤 접근법을 선택하는 것이 가장 적절할까요?

- SNS + SQS 팬아웃 패턴 사용

### 한 Kinesis Data Stream에 6개의 샤드가 프로비저닝되어 있습니다. 이 데이터 스트림은 보통 5MB/초의 속도로 데이터를 수신하며, 8MB/초의 속도로 데이터를 전송합니다. 이따금 트래픽이 2배까지 증가하여  ProvisionedThroughputExceededException 예외 처리가 발생합니다. 이 문제를 해결하려면 어떻게 해야 할까요?

- 더 많은 샤드 추가


### 여러 마이크로 서비스 애플리케이션을 온프레미스로 실행 중이며, 이들은 MQTT 프로토콜을 지원하는 메시지 브로커를 사용해 통신하고 있습니다. 애플리케이션을 새로 엔지니어링하거나 코드를 수정하는 작업 없이, 이 애플리케이션들을 AWS로 이전시키려 합니다. MQTT 프로토콜을 지원하는 관리 메시지 브로커를 활용하기 위해서는 다음 중 어떤 AWS 서비스를 사용해야 할까요?

- Amazon MQ


### 온프레미스로 호스팅된 다중 도커 기반의 애플리케이션이 있으며, 이를 AWS로 이전시키려 합니다. 여러분은 인프라를 프로비저닝하거나 관리할 의향이 없으며, 그냥 컨테이너를 AWS 상에서 실행하려고 합니다. 이 경우, 다음 중 어떤 AWS 서비스를 선택해야 할까요?

- ✅ AWS Fargate
- ❌ AWS ECS

```
AWS ECS 를 사용하는 경우 EC2 인스턴스를 프로비저닝 하고 관리해야 합니다.
```

### EC2 인스턴스로 구성된 ECS 클러스터 상에 애플리케이션을 배포하려 합니다. 현재, 클러스터는 DynamoDB에 대한 API 호출을 성공적으로 발행한 애플리케이션 하나를 호스팅하고 있습니다. S3로의 API 호출을 발행하는 두 번째 애플리케이션을 추가하려는데, 권한 부여 관련 문제가 발생했습니다. 이 문제를 해결하고 적절한 보안을 유지하기 위해서는 어떤 방법을 사용해야 할까요?

- ✅ 새 애플리케이션을 위한 IAM 역할 생성
- ❌ EC2 인스턴스 역할을 수정해 S3 에 대한 권한 추가




