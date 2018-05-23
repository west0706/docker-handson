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
 
## Hands-On Start!
### 1) Docker를 통해 컨테이너 이미지 띄워보기
>이번 헨즈온에서는 말로만 듣던 도오커를 설치해보고 설치하자마자 바로 서비스를 띄워봅니다. 많은 시간을 들이지 않아도 도커를 이용하면 바로 사용할 수 있는 서비스를 몇초만에 만들 수 있습니다.

#### Amazon Linux 준비
- AWS 개인계정에 접속하여 Seoul 리전선택
- EC2 콘솔로 들어가서 AMI Amazon Linux 안정화 버전으로 EC2 생성
- Security Group 22번 Port 내 위치에서 오픈
- 다운받은 Keypair로 SSH를 통해 EC2 접속

#### Docker 설치
- 아래 명령어를 실행하여 Docker부터 설치합니다.
```
curl -fsSL https://get.docker.com/ | sudo sh
```
- 기본적으로 docker는 root 권한이 필요합니다. 매번 sudo를 칠 필요없이 현재 사용자를 docker 그룹에 추가합니다.
```
sudo usermod -aG docker $USER
```
- 설치와 권한설정이 완료되었다면 버전을 확인해 봅니다.
```
docker version
```
- 아래와 같은 정보가 나오면 성공!
```
Client:
 Version:	17.12.1-ce
 API version:	1.35
 Go version:	go1.9.4
 Git commit:	3dfb8343b139d6342acfd9975d7f1068b5b1c3d3
 Built:	Tue Apr  3 23:37:44 2018
 OS/Arch:	linux/amd64

Server:
 Engine:
  Version:	17.12.1-ce
  API version:	1.35 (minimum version 1.12)
  Go version:	go1.9.4
  Git commit:	7390fc6/17.12.1-ce
  Built:	Tue Apr  3 23:38:52 2018
  OS/Arch:	linux/amd64
  Experimental:	false
```
여기까지 도커를 사용할 기본 준비는 다 되었습니다.

### Docker Image 둘러보기

### Image pull

---------------
### Docker Container CI/CD 맛보기
CI/CD 의 대표적인 Tool인 Jenkins를 사용하여 Container를 배포하는 실습을 진행합니다.
개인 Git Repository를 준비하고 소스가 Commit 될때 
개인 Git Repository를 준비하고 이벤트 
개인 Git Repository를 준비하고 
