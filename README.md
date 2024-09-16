# aws-saa

aws saa 틀린문제 정리

### EC2 예약 인스턴스를 예약할 수 있는 기간은 얼마인가요?

1년 혹은 3년



### 온프레미스에 호스팅된 OLTP 데이터베이스를 갖춘 전자 상거래 애플리케이션이 있습니다. 이 애플리케이션은 인기가 좋아, 데이터베이스가 초당 수천 개의 요청을 지니게 됩니다. 여러분은 데이터베이스를 EC2 인스턴스로 이전하려 합니다. 이렇게 높은 빈도를 보이는 OLTP 데이터베이스를 처리하기 위해서는 어떤 EC2 인스턴스 유형을 선택해야 할까요?

스토리지 최적화


### 노스버지니아 리전 us-east-1에서 AMI를 사용하면 어떤 AWS 리전에 있는 EC2 인스턴스라도 실행할 수 있습니다

아닙니다.

```
AMI는 특정 AWS 리전에 국한되며, 각 AWS 리전에는 고유한 AMI가 있습니다. 다른 AWS 리전에서 AMI를 사용해 EC2 인스턴스를 실행하는 것은 불가능하지만, 대상 AWS 리전으로 AMI를 복사해 EC2 인스턴스를 생성하는 것은 가능합니다.
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

EC2 인스턴스에서 데이터베이스를 인스턴스 스토어를 사용하여 실행 가능하지만, EC2 인스턴스가 중지 시 데이터가 손실이라는 문제가 있습니다 (문제 없이 다시 시작할 수 있음). 한 가지 솔루션은 인스턴스 스토어가 있는 다른 EC2 인스턴스에서 복제 메커니즘을 설정하여 대기 복사본을 가질 수 있다는 것입니다. 또 다른 솔루션은 데이터에 대한 백업 메커니즘을 설정하는 것입니다. 요구 사항을 검증하기 위해 아키텍처를 설정하는 방법은 모두 사용자에게 달려 있습니다. 이 사용 사례에서는 IOPS 기준이므로 EC2 인스턴스 스토어를 선택해야 합니다.

인스턴스 스토어를 사용하는 EC2 인스턴스에 데이터베이스를 실행할 수는 있으나, EC2 인스턴스가 중단되었을 때 데이터가 소실되는 문제가 발생합니다(재시작에는 아무 문제가 없습니다). 솔루션으로는 대기 복사본을 가질 수 있도록, 인스턴스 스토어를 가진 다른 EC2 인스턴스에 복제 메커니즘을 설정하는 방법이 있습니다. 또다른 솔루션으로는 데이터의 백업 메커니즘을 설정하는 방법이 있습니다. 요구 사항을 검증하기 위한 아키텍처 설정은 온전히 여러분의 몫입니다. 이 사용 사례는 IOPS를 다루고 있으므로, EC2 인스턴스 스토어를 선택해야 합니다.
```
