# 모두의 Docker - AWSKRUG Container Hands-on 1st Session
Docker의 기본 사용법을 익히고, 컨테이너를 통해 바로 사용할 수 있는 서비스를 띄워봅니다.
docker-compose를 통해 서비스들을 연결해보고 실사용 가능한 서비스를 구현해 봅니다.
CI/CD 환경을 구성하여 컨테이너 배포를 실습해 봅니다.

## 필수 지식
Hands-on에 앞서 사용자는 다음과 같은 기본지식이 있어야 합니다.
 - 기본 linux command 명령어
 - SSH를 통한 AWS EC2 Amazon Linux 접속방법
 - VIM 에디터 기본 사용법

## 준비 사항
 - 개인 노트북(Mac/Windows 모두가능)
 - AWS 개인계정
 - 터미널 접속환경(Windows는 putty등 접속툴 설치)
 
## Hands-On
### Docker를 통해 컨테이너 이미지 띄워보기
#### Amazon Linux 준비
- AWS 개인계정에 접속하여 Seoul 리전선택
- EC2 콘솔로 들어가서 AMI Amazon Linux 안정화 버전으로 EC2 생성
- Security Group 22번 Port 내 위치에서 오픈
- 다운받은 Keypair로 SSH를 통해 EC2 접속

#### Docker 설치
```
curl -fsSL https://get.docker.com/ | sudo sh
```
기본적으로 docker는 root 권한이 필요하기 때문에 매번 sudo를 칠 필요없이
현재 사용자를 docker 그룹에 추가한다.
```
sudo usermod -aG docker $USER
```


### Docker Image 둘러보기

### Image pull
