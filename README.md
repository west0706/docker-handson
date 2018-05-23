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
개인 Git Repository를 준비하고 소스가 Push 될때마다 Jenkins의 Event hook을 통해 commit 된 소스를 컨테이너와 함께 배포합니다.

전체적인 구성도는 아래와 같습니다.

먼저 간단한 웹서버를 구성하기 위해 node.js express 로 애플리케이션을 만들고 이를 Docker Image로 만든 뒤 컨테이너로 실행하겠습니다. (참고: https://nodejs.org/ko/docs/guides/nodejs-docker-webapp/)

Git Repository 생성
개인 Github 계정에 빈 Repository를 만듭니다.
 - github 접속 (https://github.com/)
 - “New repository” 버튼 클릭
 - repository 이름을 “awskrug-docker” 로 입력
 - Public 선택 후 Create
 - 자신의 로컬PC에서 다음 명력어를 실행하여 repository 동기화
$ git clone https://github.com/west0706/awskrug-docker.git

Node.js 소스작성
 - 커멘드 실행: cd ./awskrug-docker
 - 애플리케이션 의존성 관련 package.json 파일 생성
 - 아래 코드 삽입 후 저장
{
  "name": "docker_web_app",
  "version": "1.0.0",
  "description": "Node.js on Docker",
  "author": "First Last <first.last@example.com>",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.16.1"
  }
}

 - server.js 파일을 같은 디렉토리 안에 생성하고  아래 코드 삽입 후 저장
'use strict';
const express = require('express');

// 상수
const PORT = 8000;
const HOST = '0.0.0.0';

// 앱
const app = express();
app.get('/', (req, res) => {
  res.send('Hello world\n');
});

app.listen(PORT, HOST);
console.log(`Running on http://${HOST}:${PORT}`);


Dockerfile 생성
 - 역시 같은 디렉터리 안에 Dockerfile 파일 생성

FROM node:carbon

# 앱 디렉터리 생성
WORKDIR /usr/src/app

# 앱 의존성 설치
COPY package*.json ./
RUN npm install

# 앱 소스 추가
COPY *.js ./

EXPOSE 8000
CMD [ "npm", "start" ]


 - Dockerfile 과 같은 디렉터리 안에 .dockerignore 파일 생성
 - .dockerignore 파일 안에 아래 두 줄 추가 후 저장 (도커 이미지의 로컬 npm모듈과 디버깅 로그 복사를 막아줌)
node_modules
npm-debug.log

Docker Image 빌드 테스트
 - 작성한 Dockerfile이 있는 위치에서 아래 명령 실행
$ docker build -t <your username>/node-web-app .
 - 생성된 이미지 확인
$ docker images

REPOSITORY                      TAG        ID              CREATED
node                            carbon     1934b0b038d1    5 days ago
<your username>/node-web-app    latest     d64d3505b0d2    1 minute ago

Container 생성 테스트
 - 웹서버가 제대로 동작하는지 확인하기 위해  컨테이너를 생성해 봅니다.
$ docker run -p 8000:8000 -d <your username>/node-web-app
    $ docker ps
 - 웹 브라우저에서 http://<host name>:8000 으로 접속하여 정상 작동 확인
 - 컨테이너가 정상적으로 실행되었다면 이제 jenkins 설정을 진행합니다.


