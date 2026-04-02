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
 uname -a
Darwin c5r6s1.codyssey.kr 24.6.0 Darwin Kernel Version 24.6.0: Mon Jan 19 22:00:10 PST 2026; root:xnu-11417.140.69.708.3~1/RELEASE_X86_64 x86_64
환경 정보: 
OS: macOS (Darwin)
커널 버전: 24.6.0
아키텍처: x86_64
 docker --version
Docker version 28.5.2, build ecc6942
 docker info
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
 git --version
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

## 6. 학습 내용

- ✅ OrbStack 설치 및 Docker 호환성
- ✅ Dockerfile 작성
- ✅ Docker 이미지 빌드
- ✅ 컨테이너 실행 및 포트 매핑
- ✅ 바인드 마운트 실습
- ✅ 한글 인코딩 처리
- ✅ Git 커밋 및 GitHub 푸시

## 7. 실습 증거

### 파일 권한 변경 (index.html)

**변경 전:**
```bash
ls -l index.html
-rw-r--r--  1 c1aa20243409  c1aa20243409  272 Apr  2 20:41 index.html
```

**변경**
```bash
chmod 755 index.html
```

**변경 후:**
```bash
ls -l index.html
-rwxr-xr-x  1 c1aa20243409  c1aa20243409  272 Apr  2 20:41 index.html
```

### 디렉토리 권한 변경 (test-dir)

**변경 전:**
```bash
ls -ld test-dir
drwxr-xr-x  2 c1aa20243409  c1aa20243409  64 Apr  2 20:53 test-dir
```

**변경**
```bash
chmod 644 test-dir
```

**변경 후:**
```bash
ls -ld test-dir
drw-r--r--  2 c1aa20243409  c1aa20243409  64 Apr  2 20:53 test-dir
```

### Docker 버전 확인

```bash
docker --version
Docker version 28.5.2, build ecc6942
```

### Docker 전체 정보

```bash
Client:
 Version:    28.5.2
 Context:    orbstack
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.29.1
    Path:     /Users/1aa20243409/.docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v2.40.3
    Path:     /Users/1aa20243409/.docker/cli-plugins/docker-compose

Server:
 Containers: 1
  Running: 1
  Paused: 0
  Stopped: 0
 Images: 1
 Server Version: 28.5.2
 Storage Driver: overlay2
  Backing Filesystem: btrfs
  Supports d_type: true
  Using metacopy: false
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 CDI spec directories:
  /etc/cdi
  /var/run/cdi
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 1c4457e00facac03ce1d75f7b6777a7a851e5c41
 runc version: d842d7719497cc3b774fd71620278ac9e17710e0
 init version: de40ad0
 Security Options:
  seccomp
   Profile: builtin
  cgroupns
 Kernel Version: 6.17.8-orbstack-00308-g8f9c941121b1
 Operating System: OrbStack
 OSType: linux
 Architecture: x86_64
 CPUs: 6
 Total Memory: 7.81GiB
 Name: orbstack
 ID: 23d916e3-c95a-43e7-8ac3-9da1aa57cdd3
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Experimental: false
 Insecure Registries:
  ::1/128
  127.0.0.0/8
 Live Restore Enabled: false
 Product License: Community Engine
 Default Address Pools:
   Base: 192.168.97.0/24, Size: 24
   Base: 192.168.107.0/24, Size: 24
   Base: 192.168.117.0/24, Size: 24
   Base: 192.168.147.0/24, Size: 24
   Base: 192.168.148.0/24, Size: 24
   Base: 192.168.155.0/24, Size: 24
   Base: 192.168.156.0/24, Size: 24
   Base: 192.168.158.0/24, Size: 24
   Base: 192.168.163.0/24, Size: 24
   Base: 192.168.164.0/24, Size: 24
   Base: 192.168.165.0/24, Size: 24
   Base: 192.168.166.0/24, Size: 24
   Base: 192.168.167.0/24, Size: 24
   Base: 192.168.171.0/24, Size: 24
   Base: 192.168.172.0/24, Size: 24
   Base: 192.168.181.0/24, Size: 24
   Base: 192.168.183.0/24, Size: 24
   Base: 192.168.186.0/24, Size: 24
   Base: 192.168.207.0/24, Size: 24
   Base: 192.168.214.0/24, Size: 24
   Base: 192.168.215.0/24, Size: 24
   Base: 192.168.216.0/24, Size: 24
   Base: 192.168.223.0/24, Size: 24
   Base: 192.168.227.0/24, Size: 24
   Base: 192.168.228.0/24, Size: 24
   Base: 192.168.229.0/24, Size: 24
   Base: 192.168.237.0/24, Size: 24
   Base: 192.168.239.0/24, Size: 24
   Base: 192.168.242.0/24, Size: 24
   Base: 192.168.247.0/24, Size: 24
   Base: fd07:b51a:cc66:d000::/56, Size: 64
```

### 운영 명령

```bash
docker images
REPOSITORY     TAG       IMAGE ID       CREATED       SIZE
my-webserver   latest    a24ccd86594d   2 hours ago   62.2MB

docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED       STATUS       PORTS                                     NAMES
38ef82b9ed2e   my-webserver   "/docker-entrypoint.…"   2 hours ago   Up 2 hours   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   jovial_meninsky

docker logs 38ef82b9ed2e 
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/04/02 11:35:59 [notice] 1#1: using the "epoll" event method
2026/04/02 11:35:59 [notice] 1#1: nginx/1.29.7
2026/04/02 11:35:59 [notice] 1#1: built by gcc 15.2.0 (Alpine 15.2.0) 
2026/04/02 11:35:59 [notice] 1#1: OS: Linux 6.17.8-orbstack-00308-g8f9c941121b1
2026/04/02 11:35:59 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 20480:1048576
2026/04/02 11:35:59 [notice] 1#1: start worker processes
2026/04/02 11:35:59 [notice] 1#1: start worker process 30
2026/04/02 11:35:59 [notice] 1#1: start worker process 31
2026/04/02 11:35:59 [notice] 1#1: start worker process 32
2026/04/02 11:35:59 [notice] 1#1: start worker process 33
2026/04/02 11:35:59 [notice] 1#1: start worker process 34
2026/04/02 11:35:59 [notice] 1#1: start worker process 35
192.168.215.1 - - [02/Apr/2026:11:36:59 +0000] "GET / HTTP/1.1" 200 179 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/18.6 Safari/605.1.15" "-"
192.168.215.1 - - [02/Apr/2026:11:36:59 +0000] "GET /favicon.ico HTTP/1.1" 404 153 "http://localhost:8080/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/18.6 Safari/605.1.15" "-"
2026/04/02 11:36:59 [error] 31#31: *1 open() "/usr/share/nginx/html/favicon.ico" failed (2: No such file or directory), client: 192.168.215.1, server: localhost, request: "GET /favicon.ico HTTP/1.1", host: "localhost:8080", referrer: "http://localhost:8080/"
192.168.215.1 - - [02/Apr/2026:11:38:11 +0000] "GET / HTTP/1.1" 200 245 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/18.6 Safari/605.1.15" "-"
192.168.215.1 - - [02/Apr/2026:11:38:13 +0000] "GET / HTTP/1.1" 200 245 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/18.6 Safari/605.1.15" "-"
192.168.215.1 - - [02/Apr/2026:11:40:57 +0000] "GET / HTTP/1.1" 200 245 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/18.6 Safari/605.1.15" "-"
192.168.215.1 - - [02/Apr/2026:11:40:58 +0000] "GET / HTTP/1.1" 200 245 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/18.6 Safari/605.1.15" "-"
192.168.215.1 - - [02/Apr/2026:11:41:32 +0000] "GET / HTTP/1.1" 200 272 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/18.6 Safari/605.1.15" "-"

CONTAINER ID   NAME              CPU %     MEM USAGE / LIMIT    MEM %     NET I/O           BLOCK I/O         PIDS 
38ef82b9ed2e   jovial_meninsky   0.00%     5.801MiB / 7.81GiB   0.07%     7.42kB / 5.18kB   12.3MB / 8.19kB   7 
