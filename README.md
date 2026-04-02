# My Web Server

OrbStack을 사용한 Docker 웹서버 프로젝트입니다.

## 프로젝트 구조

codyssey-practice/
├── Dockerfile
├── index.html
└── README.md

## 실행 방법

### 1. Docker 이미지 빌드
docker build -t my-webserver .

### 2. 컨테이너 실행
docker run -d -p 8080:80 -v $(pwd):/usr/share/nginx/html my-webserver

### 3. 브라우저 접속
http://localhost:8080

## 바인드 마운트

-v $(pwd):/usr/share/nginx/html 옵션으로 로컬 파일과 컨테이너를 연동합니다.
- 로컬에서 index.html 수정 → 즉시 브라우저에 반영됨

## 컨테이너 중지

docker stop <CONTAINER_ID>

## 학습 내용

- Docker 기본 개념
- Dockerfile 작성
- 이미지 빌드 및 컨테이너 실행
- 바인드 마운트를 통한 실시간 파일 동기화
- UTF-8 인코딩 처리
