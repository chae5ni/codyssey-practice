# Codyssey Practice

## 1. 프로젝트 개요

이 저장소는 터미널 기본 조작, 파일 권한, Docker 설치 및 운영, 기존 베이스 이미지 기반 커스텀 이미지 제작, 포트 매핑, 바인드 마운트, Docker 볼륨 영속성, Git/GitHub 연동까지 한 번에 검증하는 과제 결과물이다.

이번 구현에서는 `nginx:alpine` 이미지를 베이스로 선택해 정적 웹 서버 이미지를 만들고, 로컬 `index.html`을 바인드 마운트로 연결해 호스트 수정 내용이 컨테이너에 즉시 반영되는 것을 확인했다. 또한 named volume을 별도로 생성해 컨테이너 삭제 후에도 데이터가 유지되는 것을 검증했다.

## 2. 실행 환경

| 항목 | 값 |
| --- | --- |
| OS | macOS 15.7.4 |
| Shell | `/bin/zsh` |
| Terminal | `Apple_Terminal` |
| Docker | `Docker version 28.5.2, build ecc6942` |
| Docker Context | `orbstack` |
| Git | `git version 2.53.0` |

실행 환경 확인 명령:

```bash
sw_vers
echo $SHELL
printenv TERM_PROGRAM
docker --version
git --version
```

## 3. 수행 항목 체크리스트

| 항목 | 상태 | 근거 |
| --- | --- | --- |
| 터미널 기본 조작 | 완료 | [5. 터미널 조작 로그](#5-터미널-조작-로그) |
| 절대 경로 / 상대 경로 설명 | 완료 | [6. 경로 개념 정리](#6-경로-개념-정리) |
| 파일 권한 확인 / 변경 | 완료 | [7. 파일 권한 실습](#7-파일-권한-실습) |
| Docker 설치 점검 | 완료 | [8. docker 설치 및 점검](#8-docker-설치-및-점검) |
| Docker 기본 운영 명령 | 완료 | [9. docker 기본 운영 명령](#9-docker-기본-운영-명령) |
| hello-world 실행 | 완료 | [10. 컨테이너 실행 실습](#10-컨테이너-실행-실습) |
| ubuntu 컨테이너 내부 명령 | 완료 | [10. 컨테이너 실행 실습](#10-컨테이너-실행-실습) |
| 커스텀 Dockerfile 이미지 제작 | 완료 | [11. 커스텀 이미지 제작](#11-커스텀-이미지-제작) |
| 포트 매핑 검증 | 완료 | [12. 포트 매핑 검증](#12-포트-매핑-검증) |
| 바인드 마운트 반영 확인 | 완료 | [13. 바인드 마운트 검증](#13-바인드-마운트-검증) |
| Docker 볼륨 영속성 | 완료 | [14. Docker 볼륨 영속성 검증](#14-docker-볼륨-영속성-검증) |
| Git 원격 저장소 연결 | 완료 | [15. Git / GitHub 정리](#15-git--github-정리) |
| Git 사용자 정보 / 기본 브랜치 설정 | 완료 | [16. Git 사용자 정보 기본 브랜치 설정](#16-git-사용자-정보-기본-브랜치-설정) |
| VSCode GitHub 로그인 증거 | 완료 | [screenshot 폴더](./screenshot) |

## 4. 저장소 구조

```text
codyssey-practice/
├── Dockerfile
├── README.md
├── index.html
├── terminal-lab/
│   └── index-renamed.html
├── test-dir/
└── test-file.txt
```

## 5. 터미널 조작 로그

현재 위치 확인:

```bash
$ pwd
/Users/1aa20243409/Desktop/codyssey-practice
```

목록 확인, 숨김 파일 포함:

```bash
$ ls -la /Users/1aa20243409/Desktop/codyssey-practice
total 32
drwxr-xr-x  10 c1aa20243409  c1aa20243409  320 Apr  2 22:55 .
drwx------+  7 c1aa20243409  c1aa20243409  224 Apr  2 20:41 ..
drwxr-xr-x  13 c1aa20243409  c1aa20243409  416 Apr  2 22:51 .git
-rw-r--r--   1 c1aa20243409  c1aa20243409   68 Apr  2 20:28 Dockerfile
-rw-r--r--   1 c1aa20243409  c1aa20243409  845 Apr  2 20:48 README.md
-rwxr-xr-x   1 c1aa20243409  c1aa20243409  272 Apr  2 20:41 index.html
drwxr-xr-x   2 c1aa20243409  c1aa20243409   64 Apr  2 22:55 terminal-lab
drw-r--r--   2 c1aa20243409  c1aa20243409   64 Apr  2 20:53 test-dir
-rw-r--r--   1 c1aa20243409  c1aa20243409    0 Apr  2 21:05 test-file.txt
```

디렉터리 생성:

```bash
$ mkdir -p /Users/1aa20243409/Desktop/codyssey-practice/terminal-lab
```

빈 파일 생성:

```bash
$ touch /Users/1aa20243409/Desktop/codyssey-practice/terminal-lab/empty.txt
```

복사:

```bash
$ cp /Users/1aa20243409/Desktop/codyssey-practice/index.html \
  /Users/1aa20243409/Desktop/codyssey-practice/terminal-lab/index-copy.html
```

이동 / 이름 변경:

```bash
$ mv /Users/1aa20243409/Desktop/codyssey-practice/terminal-lab/index-copy.html \
  /Users/1aa20243409/Desktop/codyssey-practice/terminal-lab/index-renamed.html
```

파일 내용 확인:

```bash
$ cat /Users/1aa20243409/Desktop/codyssey-practice/terminal-lab/index-renamed.html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>My Web Server</title>
  </head>
  <body>
    <h1>Hello from OrbStack! 🚀</h1>
    <p>My first web server is running!</p>
    <p>바인드 마운트 테스트 - 실시간 반영!</p>
  </body>
</html>
```

삭제:

```bash
$ rm /Users/1aa20243409/Desktop/codyssey-practice/terminal-lab/empty.txt
```

## 6. 경로 개념 정리

- 절대 경로: 루트(`/`)부터 시작하는 전체 경로다. 예: `/Users/1aa20243409/Desktop/codyssey-practice/index.html`
- 상대 경로: 현재 작업 위치를 기준으로 해석되는 경로다. 예: 현재 위치가 저장소 루트일 때 `index.html`, `terminal-lab/index-renamed.html`
- 차이점: 절대 경로는 어느 위치에서 실행해도 동일하지만, 상대 경로는 현재 디렉터리가 바뀌면 결과가 달라진다.

## 7. 파일 권한 실습

권한 의미:

- `r`: read, 읽기
- `w`: write, 쓰기
- `x`: execute, 실행

숫자 표기 규칙:

- `7 = 4 + 2 + 1 = rwx`
- `6 = 4 + 2 = rw-`
- `5 = 4 + 1 = r-x`
- `4 = 4 = r--`

예시 해석:

- `755`: 소유자 `rwx`, 그룹 `r-x`, 기타 사용자 `r-x`
- `644`: 소유자 `rw-`, 그룹 `r--`, 기타 사용자 `r--`

디렉터리 권한 변경 전:

```bash
$ ls -ld /Users/1aa20243409/Desktop/codyssey-practice/test-dir
drw-r--r--  2 c1aa20243409  c1aa20243409  64 Apr  2 20:53 /Users/1aa20243409/Desktop/codyssey-practice/test-dir
```

디렉터리 권한 변경:

```bash
$ chmod 755 /Users/1aa20243409/Desktop/codyssey-practice/test-dir
$ ls -ld /Users/1aa20243409/Desktop/codyssey-practice/test-dir
drwxr-xr-x  2 c1aa20243409  c1aa20243409  64 Apr  2 20:53 /Users/1aa20243409/Desktop/codyssey-practice/test-dir
```

파일 권한 변경 전:

```bash
$ chmod 600 /Users/1aa20243409/Desktop/codyssey-practice/test-file.txt
$ ls -ld /Users/1aa20243409/Desktop/codyssey-practice/test-file.txt
-rw-------  1 c1aa20243409  c1aa20243409  0 Apr  2 21:05 /Users/1aa20243409/Desktop/codyssey-practice/test-file.txt
```

파일 권한 변경 후:

```bash
$ chmod 644 /Users/1aa20243409/Desktop/codyssey-practice/test-file.txt
$ ls -ld /Users/1aa20243409/Desktop/codyssey-practice/test-file.txt
-rw-r--r--  1 c1aa20243409  c1aa20243409  0 Apr  2 21:05 /Users/1aa20243409/Desktop/codyssey-practice/test-file.txt
```

## 8. Docker 설치 및 점검

버전 확인:

```bash
$ docker --version
Docker version 28.5.2, build ecc6942
```

데몬 동작 확인:

```bash
$ docker info
Client:
 Version:    28.5.2
 Context:    orbstack
 Debug Mode: false

Server:
 Containers: 1
  Running: 1
  Paused: 0
  Stopped: 0
 Images: 4
 Server Version: 28.5.2
 Storage Driver: overlay2
 Logging Driver: json-file
 Cgroup Version: 2
 Operating System: OrbStack
 OSType: linux
 Architecture: x86_64
 CPUs: 6
 Total Memory: 7.81GiB
 Name: orbstack
 Docker Root Dir: /var/lib/docker
```

해석:

- Docker 클라이언트와 데몬이 모두 정상 동작한다.
- 현재 Docker 실행 환경은 `OrbStack`이다.
- 메모리 7.81GiB, CPU 6개가 Docker 엔진에 할당되어 있다.

## 9. Docker 기본 운영 명령

이미지 목록:

```bash
$ docker images
REPOSITORY     TAG       IMAGE ID       CREATED         SIZE
my-webserver   latest    2ff3506fe055   6 minutes ago   62.2MB
<none>         <none>    a24ccd86594d   2 hours ago     62.2MB
hello-world    latest    e2ac70e7319a   9 days ago      10.1kB
ubuntu         24.04     f794f40ddfff   5 weeks ago     78.1MB
```

컨테이너 목록:

```bash
$ docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED       STATUS       PORTS                                     NAMES
38ef82b9ed2e   my-webserver   "/docker-entrypoint.…"   2 hours ago   Up 2 hours   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   jovial_meninsky
```

로그 확인:

```bash
$ docker logs 38ef82b9ed2e
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/04/02 11:35:59 [notice] 1#1: nginx/1.29.7
192.168.215.1 - - [02/Apr/2026:11:41:32 +0000] "GET / HTTP/1.1" 200 272 "-" "Mozilla/5.0 ..." "-"
```

리소스 사용량 확인:

```bash
$ docker stats --no-stream 38ef82b9ed2e
CONTAINER ID   NAME              CPU %     MEM USAGE / LIMIT    MEM %     NET I/O          BLOCK I/O         PIDS
38ef82b9ed2e   jovial_meninsky   0.00%     5.066MiB / 7.81GiB   0.06%     7.5kB / 5.18kB   12.3MB / 8.19kB   7
```

## 10. 컨테이너 실행 실습

`hello-world` 실행:

```bash
$ docker run --rm hello-world
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

`ubuntu` 컨테이너에서 간단 명령 실행:

```bash
$ docker run --rm ubuntu:24.04 bash -lc "ls / && echo inside-ubuntu"
bin
boot
dev
etc
home
lib
lib64
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
inside-ubuntu
```

유지형 컨테이너 생성 후 `exec` 확인:

```bash
$ docker run -dit --name ubuntu-lab ubuntu:24.04 bash
eb719552f74c3d5b45dd1fb6fc33da189bfc87011916e61c97aa48a431e477d6

$ docker exec ubuntu-lab bash -lc "pwd && ls / && echo exec-ok"
/
bin
boot
dev
etc
home
lib
lib64
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
exec-ok
```

관찰 정리:

- `docker run --rm ...`은 명령이 끝나면 컨테이너가 종료되고 자동 삭제된다.
- `docker run -dit ... bash`는 컨테이너를 계속 살아 있게 두고 나중에 `docker exec`로 다시 들어갈 수 있다.
- `attach`는 메인 프로세스에 직접 붙는 방식이고, `exec`는 실행 중인 컨테이너 안에 새 프로세스를 띄우는 방식이라 실습과 운영에서는 보통 `exec`가 더 안전하다.

## 11. 커스텀 이미지 제작

선택한 베이스 이미지:

- `nginx:alpine`

선택 이유:

- Nginx는 정적 웹 서버 실습에 적합하다.
- `alpine` 태그는 비교적 가볍다.
- 기존 Dockerfile을 기반으로 커스텀 포인트를 설명하기 쉽다.

커스텀 포인트:

- `FROM nginx:alpine`: 기존 베이스 이미지를 사용한다.
- `COPY index.html /usr/share/nginx/html/index.html`: 기본 페이지를 내 HTML로 교체한다.

Dockerfile:

```Dockerfile
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/index.html
```

빌드:

```bash
$ docker build -t my-webserver /Users/1aa20243409/Desktop/codyssey-practice
#0 building with "orbstack" instance using docker driver
#6 [2/2] COPY index.html /usr/share/nginx/html/index.html
#7 naming to docker.io/library/my-webserver 0.0s done
#7 DONE 0.3s
```

실행 예시:

```bash
docker run -d --name my-webserver-container -p 8080:80 my-webserver
```

## 12. 포트 매핑 검증

포트 매핑이 필요한 이유:

- 컨테이너 내부의 `80` 포트는 호스트에서 직접 보이지 않는다.
- `-p 8080:80`을 사용해야 호스트의 `localhost:8080`으로 접속해 컨테이너 내부 웹 서버를 확인할 수 있다.

실행 중 컨테이너 포트 확인:

```bash
$ docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED       STATUS       PORTS                                     NAMES
38ef82b9ed2e   my-webserver   "/docker-entrypoint.…"   2 hours ago   Up 2 hours   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   jovial_meninsky
```

HTTP 응답 확인:

```bash
$ curl -I http://localhost:8080
HTTP/1.1 200 OK
Server: nginx/1.29.7
Date: Thu, 02 Apr 2026 13:57:47 GMT
Content-Type: text/html
Content-Length: 272
Last-Modified: Thu, 02 Apr 2026 11:41:29 GMT
Connection: keep-alive
ETag: "69ce55e9-110"
Accept-Ranges: bytes
```

결론:

- 호스트 `8080` 포트가 컨테이너 `80` 포트로 정상 연결되었다.
- 브라우저 접속 대신 `curl` 응답으로도 포트 매핑 성공을 증명할 수 있다.

## 13. 바인드 마운트 검증

실행 명령:

```bash
docker run -d -p 8080:80 -v $(pwd):/usr/share/nginx/html my-webserver
```

의미:

- 호스트의 현재 디렉터리 파일을 컨테이너 `/usr/share/nginx/html`에 연결한다.
- 따라서 호스트에서 `index.html`을 수정하면 이미 실행 중인 컨테이너 화면도 즉시 바뀐다.

반영 증거:

```bash
$ docker logs 38ef82b9ed2e
192.168.215.1 - - [02/Apr/2026:11:36:59 +0000] "GET / HTTP/1.1" 200 179 "-" "Mozilla/5.0 ..." "-"
192.168.215.1 - - [02/Apr/2026:11:38:11 +0000] "GET / HTTP/1.1" 200 245 "-" "Mozilla/5.0 ..." "-"
192.168.215.1 - - [02/Apr/2026:11:41:32 +0000] "GET / HTTP/1.1" 200 272 "-" "Mozilla/5.0 ..." "-"
```

해석:

- 같은 `/` 요청의 응답 크기가 `179 -> 245 -> 272`로 바뀌었다.
- 이는 호스트 측 `index.html` 수정이 재빌드 없이 컨테이너에 즉시 반영되었음을 보여준다.

## 14. Docker 볼륨 영속성 검증

볼륨 생성:

```bash
$ docker volume create nginx-html-data
nginx-html-data
```

볼륨 목록:

```bash
$ docker volume ls
DRIVER    VOLUME NAME
local     nginx-html-data
```

첫 번째 컨테이너에 연결 후 데이터 작성:

```bash
$ docker run -d --name volume-lab-1 -v nginx-html-data:/data ubuntu:24.04 sleep infinity
2ad7723d865031756e0fad47fdefae25e1f4d9b425f4596d46061e5fe3445a7c

$ docker exec volume-lab-1 bash -lc "echo persistent-data > /data/message.txt && cat /data/message.txt"
persistent-data

$ docker exec volume-lab-1 bash -lc "ls -l /data"
total 4
-rw-r--r-- 1 root root 16 Apr  2 13:58 message.txt
```

첫 번째 컨테이너 삭제:

```bash
$ docker rm -f volume-lab-1
volume-lab-1
```

두 번째 컨테이너에 같은 볼륨 연결 후 데이터 확인:

```bash
$ docker run -d --name volume-lab-2 -v nginx-html-data:/data ubuntu:24.04 sleep infinity
c52b68270b73356fa27ce257be173708cd131a7901ccea5f65ccd9c903f30b68

$ docker exec volume-lab-2 bash -lc "cat /data/message.txt"
persistent-data

$ docker exec volume-lab-2 bash -lc "ls -l /data"
total 4
-rw-r--r-- 1 root root 16 Apr  2 13:58 message.txt
```

볼륨 상세 정보:

```bash
$ docker volume inspect nginx-html-data
[
    {
        "CreatedAt": "2026-04-02T22:58:16+09:00",
        "Driver": "local",
        "Mountpoint": "/var/lib/docker/volumes/nginx-html-data/_data",
        "Name": "nginx-html-data",
        "Scope": "local"
    }
]
```

결론:

- 컨테이너는 삭제했지만 데이터는 volume에 남아 있었다.
- 이것이 bind mount와 구분되는 Docker volume의 핵심 장점이다.

## 15. Git / GitHub 정리

역할 차이:

- Git: 로컬에서 버전 관리, 변경 이력 관리, 브랜치 관리
- GitHub: 원격 저장소 호스팅, 백업, 협업, PR/이슈 관리

현재 원격 저장소 연결 상태:

```bash
$ git remote -v
origin  https://github.com/chae5ni/codyssey-practice.git (fetch)
origin  https://github.com/chae5ni/codyssey-practice.git (push)
```

최근 커밋:

```bash
$ git log --oneline --decorate -5
16cc922 (HEAD -> main, origin/main, origin/HEAD) Add README.md
6210baa 첫 번째 커밋: 웹서버 기본 구조 추가
```

현재 확인 결과:

```bash
$ git config --list
credential.helper=osxkeychain
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
core.ignorecase=true
core.precomposeunicode=true
remote.origin.url=https://github.com/chae5ni/codyssey-practice.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.main.remote=origin
branch.main.merge=refs/heads/main
```

## 16. Git 사용자 정보 기본 브랜치 설정

```bash
$ git config --global --list
user.name=chae5ni
user.email=1aa2024@gmail.com
init.defaultbranch=main
```


## 17. 트러블슈팅

### 사례 1. `zsh: command not found: chomd`

- 문제: 권한 변경 명령을 실행하려는데 `chomd`라고 입력해서 에러가 발생했다.
- 원인 가설: `chmod` 오타일 가능성이 높다.
- 확인: 셸 에러 메시지가 `command not found`였고, 올바른 명령은 `chmod`다.
- 해결: `chmod 755 <directory>`, `chmod 644 <file>`처럼 정확한 명령어로 다시 실행했다.

### 사례 2. `docker logs` 실행 시 `requires 1 argument`

- 문제: `docker logs`만 입력하니 `requires 1 argument` 에러가 발생했다.
- 원인 가설: 로그를 볼 대상 컨테이너 이름 또는 ID가 빠졌을 가능성이 높다.
- 확인: `docker ps -a`로 컨테이너 목록을 보고 `38ef82b9ed2e` 컨테이너가 실행 중인 것을 확인했다.
- 해결: `docker logs 38ef82b9ed2e`처럼 컨테이너 ID를 함께 전달해 해결했다.

### 사례 3. `favicon.ico` 404

- 문제: Nginx 로그에 `/favicon.ico` 요청이 `404`로 남았다.
- 원인 가설: 브라우저가 탭 아이콘 파일을 요청했지만 해당 파일이 이미지 경로에 없을 가능성이 높다.
- 확인: 로그에 `open() "/usr/share/nginx/html/favicon.ico" failed (2: No such file or directory)`가 기록됐다.
- 해결 / 대안: 서비스 자체는 `GET /`에서 `200 OK`로 정상 동작했으므로 우선 무시 가능하다. 필요하면 `favicon.ico` 파일을 추가해 해결할 수 있다.

## 18. 검증 방법 요약

| 확인 대상 | 명령 | 성공 기준 | 결과 위치 |
| --- | --- | --- | --- |
| OS / Shell / Terminal | `sw_vers`, `echo $SHELL`, `printenv TERM_PROGRAM` | 환경 정보 출력 | [2. 실행 환경](#2-실행-환경) |
| 파일 조작 | `pwd`, `ls -la`, `mkdir`, `touch`, `cp`, `mv`, `rm`, `cat` | 명령 수행 및 결과 확인 | [5. 터미널 조작 로그](#5-터미널-조작-로그) |
| 권한 실습 | `ls -ld`, `chmod` | 변경 전/후 권한 차이 확인 | [7. 파일 권한 실습](#7-파일-권한-실습) |
| Docker 설치 | `docker --version`, `docker info` | 버전과 Server 정보 출력 | [8. Docker 설치 및 점검](#8-docker-설치-및-점검) |
| Docker 운영 | `docker images`, `docker ps -a`, `docker logs`, `docker stats --no-stream` | 이미지/컨테이너/로그/자원 사용량 출력 | [9. Docker 기본 운영 명령](#9-docker-기본-운영-명령) |
| hello-world | `docker run --rm hello-world` | Docker 정상 문구 출력 | [10. 컨테이너 실행 실습](#10-컨테이너-실행-실습) |
| Ubuntu 실습 | `docker run --rm ubuntu:24.04 ...`, `docker exec ubuntu-lab ...` | 내부 명령 실행 | [10. 컨테이너 실행 실습](#10-컨테이너-실행-실습) |
| 커스텀 이미지 | `docker build -t my-webserver ...` | 빌드 완료 | [11. 커스텀 이미지 제작](#11-커스텀-이미지-제작) |
| 포트 매핑 | `docker ps -a`, `curl -I http://localhost:8080` | `8080->80`과 `HTTP/1.1 200 OK` 확인 | [12. 포트 매핑 검증](#12-포트-매핑-검증) |
| 바인드 마운트 | `docker run -v $(pwd):...`, `docker logs` | 수정 후 응답 크기 변화 확인 | [13. 바인드 마운트 검증](#13-바인드-마운트-검증) |
| 볼륨 영속성 | `docker volume create`, `docker exec`, `docker rm -f`, `docker volume inspect` | 컨테이너 삭제 후 파일 유지 | [14. Docker 볼륨 영속성 검증](#14-docker-볼륨-영속성-검증) |
