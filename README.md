# EyesTalk manifest 파일
각각의 Dev 레파지토리와 Manifest 레파지토리의 워크플로우가 공유되어 이미지 태그가 동기화됩니다


<br>
<br>

# ☀️ CI/CD PipeLine
Kustomize, ArgoCD, ECR, GithubAction 사용
![image](https://user-images.githubusercontent.com/73453283/206051230-c570cbe3-abe7-4329-b7ca-0472fbf87b02.png)
<br>

## 파일구조
<img width="528" alt="image" src="https://user-images.githubusercontent.com/73453283/206051058-ea17e833-a1e1-401f-959a-0784d4b2d351.png">

<br>

## ArgoCD
- GithubAction으로 연결된 코드가 push될 때 ArgoCD가 동기화되어 자동으로 코드가 반영된다
<br>

![2022-12-08 16 00 19](https://user-images.githubusercontent.com/73453283/206380718-62d199f8-b5b5-4f02-ab52-0b044a718cae.gif)



<br>
<br>

# ☀️ EKS 구축
![image](https://user-images.githubusercontent.com/73453283/206074481-88b4e5d3-bd7f-4edb-9ae4-abf1ac1cf8a5.png)
## Terraform
#### 가용영역
- ap-northeast-2a, ap-northeast-2b, ap-northeast-2c, ap-northeast-2d
#### VPC
- VPC CIDR : 10.2.0.0/16
- Public subnet 4 : 10.2.1.0/24, 10.2.2.0/24, 10.2.3.0/24, 10.2.4.0/24
- Private subnet 4 : 10.2.16.0/22, 10.2.20.0/22, 10.2.24.0/22, 10.2.28.0/22
## CloudFormation
- EKS version: 1.23
- Instance Type: m5.large (WorkerNode 4)
  - min size:4
  - max size:10
  - volume size: 50
  
<br>
<br>

# ☀️ 모니터링
## 성능 모니터링
- 클러스터의 상태를 모니터링하기 위해 helm, 메트릭 서버, prometheus, Distro를 설치
- 메트릭로그를 Cloudwatch에 자동으로 수집하여 배포한 애플리케이션의 성능 모니터링이 가능
- 주요 애플리케이션 pod의 CPU, Memory사용량을 대시보드로 만들어 CloudWatch에서 확인
- 비용 절감을 위하여 일정 CPU사용량이 넘어가면 경보알람으로 이메일 알람 설정
<br>

### CloudWatch 대시보드
![image](https://user-images.githubusercontent.com/73453283/206075542-0100fbe2-a6d2-46c6-9f0d-f2a5c0145014.png)
### 경보 알람 메일 확인
![image](https://user-images.githubusercontent.com/73453283/206075685-47769841-ffd8-4554-94f4-31a2ce12dff5.png)
![image](https://user-images.githubusercontent.com/73453283/206077277-48fc42bb-130d-4fb1-9354-05f4c56a5b56.png)

<br>

## 로그 모니터링
- 원하는 로그가 발생되고 있는 python 코드에 수집하고 싶은 로그를 저장한 후 인덱스를 만들어서 elasticSearch에 저장
- 수집한 로그를 시각화하기 위해 elasticSearch와 kibana를 연동해 로그 대시보드를 커스텀하여 모니터링한다

<br>

### 수집한 로그
| 로그 | 설명 |
| --- | --- |
| des | 수집한 로그에 대한 설명 (create room, New member joined, user-disconnect,chatting) |
| user_nickname | 사용자가 방에 입장할 때 설정한 닉네임  |
| chatting message | 사용자가 보낸 채팅 메시지 (암호화 예정 )  |
| room_id | 방 생성 시 설정된 방 이름 |
| @timestamp | 로그가 발생한 시간 |

<br>

### Kibana 대시보드 구성
| 대시보드 | 설명 |
| --- | --- |
| 24H 방 생성 사용량 | 24시간동안 어느 시간대에 방이 많이 생성되는 지를 확인할 수 있다.  |
| Today 방 생성 수 | 하루동안 방이 얼마나 생성되는 지를 확인할 수 있다.  |
| room 별 채팅 개수 | 방에서 채팅의 수가 어느정도 되는지 확인할 수 있다.  |
| room 별 채팅 내역 | (암호화 예정) |
| Today 서비스 접속 인원 | 하루동안 해당 서비스에 몇명의 사람이 접속하는지 확인할 수 있다.  |
<br>

![image](https://user-images.githubusercontent.com/73453283/206076638-28a130f7-bfcc-4e55-bef5-9aa2d637dbf3.png)


