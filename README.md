# My Web Server

# OrbStack 웹서버 개발 워크스테이션 구축

## 1. 프로젝트 개요
Docker를 활용하여 웹서버 이미지를 구축하고, 컨테이너 기술의 기본 개념(포트 매핑, 볼륨, 권한)을 이해하며, Git/GitHub를 통해 버전관리 및 협업 환경을 구성하는 프로젝트입니다.

**미션 목표:**
- Docker 기본 개념 이해 및 실습
- 커스텀 웹서버 이미지 제작
- 파일 시스템 및 권한 관리 학습
- Git/GitHub를 통한 버전관리 경험

---

## 2. 실행 환경

#### OS/쉘/터미널

```bash
$ uname -a
Darwin c5r6s1.codyssey.kr 24.6.0 Darwin Kernel Version 24.6.0: Mon Jan 19 22:00:10 PST 2026; root:xnu-11417.140.69.708.3~1/RELEASE_X86_64 x86_64
환경 정보: 
OS: macOS (Darwin)
커널 버전: 24.6.0
아키텍처: x86_64
$ docker --version
Docker version 28.5.2, build ecc6942
$ docker info
Client:
 Version:    28.5.2
 Context:    orbstack
 
Server:
 Containers: 1
  Running: 1
 Images: 1
 Server Version: 28.5.2
 Storage Driver: overlay2
 Operating System: OrbStack
 Architecture: x86_64
 CPUs: 6
 Total Memory: 7.81GiB
$ git --version
git version 2.53.0
```

## 3. 프로젝트 구조

```
codyssey-practice/
├── Dockerfile # 웹서버 이미지 정의
├── index.html # 웹페이지 (UTF-8 인코딩)
├── README.md # 프로젝트 문서
└── test-dir/ # 테스트 디렉토리
```

## 4. 핵심 파일 설명

### Dockerfile
- nginx:alpine 기반 이미지
- UTF-8 인코딩 설정
- 포트 80 노출

### index.html
- 한글 지원 웹페이지
- charset=UTF-8 메타태그

### README.md
- 프로젝트 전체 문서

## 5. 실행 방법

```bash
# 이미지 빌드
docker build -t my-webserver .

# 컨테이너 실행
docker run -p 8080:80 my-webserver

# 브라우저 접속
# http://localhost:8080
```
