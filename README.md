# ai-codyssey

Github와 Codyssey를 연동하기 위해 만들어진 Repository입니다.

# 프로젝트 개요(미션 목표 요약)

개발자로서 코드를 작성하기 전 필수적으로 갖춰야 할 '로컬 개발 환경'을 구축하는 과정입니다.
터미널(CLI), Docker, Git/GitHub라는 세 가지 핵심 도구를 직접 세팅하며 다루는 방법을 익히게 됩니다.

## 1)실행 환경(OS/쉘/터미널, Docker 버전, Git 버전)

- OS : Windows(PowerShell)
- Docker : 29.9.2, build a82e528
- Git :2.56.0.windows.4

## 2) 수행 체크리스트

<details>
<summary>2. 터미널 조작 로그 기록</summary>

- 현재 위치 확인

  ```bash
  $ pwd
  /mnt/c/Users/HYUKJOO
  ```

- 목록 확인(숨김 파일 포함)

  ```bash
  $ ls -a
  .
  ..
  '2024년 연말정산...'
  20251016_industry_744354000.pdf
  20251031_economy_195423000.pdf
  ```

- 이동

  ```bash
  $ cd Downloads

  $ pwd
  /mnt/c/Users/HYUKJOO/Downloads
  ```

- 생성

  ```bash
  // 파일 생성
  $ touch hello.sh
  hello.sh

  // 디렉토리 생성
  $ mkdir practice_dir
  practice_dir
  ```

- 복사

  ```bash
  $ cp hello.sh practice_dir/hello_copy.sh
  ```

- 이동/이름변경
  ```bash
  $ mv practice_dir/hello_moved.sh
  $ mv hello_copy.sh practice_dir/hello_moved.sh
  ```
- 삭제
  ```bash
  $ rm -r practice_dir
  ```
- 파일 내용 확인

  ```bash
  $ echo "Hello Codyssey" > practice_dir/hello_moved.sh
  $ cat practice_dir/hello_moved.sh
  ```

</details>

<details>
<summary>3. 권한 실습 및 증거 기록</summary>

- 권한 확인/변경 명령 전후 비교

  ```bash
  $ ls -al
  -rwxrwxrwx 1 hyukjoo hyukjoo 0 Oct 31 23:42 hello.sh
  $ chmod 755 hello.sh
  $ ls -al
  -rwxr-xr-x 1 hyukjoo hyukjoo 0 Oct 31 23:42 hello.sh
  ```

- 파일 1개, 디렉토리 1개 권한 변경 실험
  ```bash
  $ ls -ld practice_dir/
  drwxr-xr-x 1 hyukjoo hyukjoo 4096 Nov  1 11:07 practice_dir/
  $ chmod 744 practice_dir/
  $ ls -ld practice_dir/
  drwxr--r-- 1 hyukjoo hyukjoo 4096 Nov  1 11:07 practice_dir/
  ```
  </details>

<details>
<summary>4. Docker 설치 및 기본 점검</summary>

- Docker 버전 확인
  ```bash
  $ docker --version
  Docker version 29.9.2, build a82e528
  ```
- Docker 데몬 동작 여부 확인

  ```bash
  $ docker info
  Client: Docker Engine - Community
  Version:    29.9.2
  Context:    default
  Debug Mode: false
  Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.38.1
    Path:     /usr/libexec/docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v5.6.1
    Path:     /usr/libexec/docker/cli-plugins/docker-compose


  Server:
  permission denied while trying to connect to the docker API at unix:///var/run/docker.sock
  ```

  ```bash
  $ sudo docker run hello-world
  Hello from Docker!
  This message shows that your installation appears to be working correctly.


  To generate this message, Docker took the following steps:

  1.  The Docker client contacted the Docker daemon.
  2.  The Docker daemon pulled the "hello-world" image from the Docker Hub.
      (amd64)
  3.  The Docker daemon created a new container from that image which runs the
      executable that produces the output you are currently reading.
  4.  The Docker daemon streamed that output to the Docker client, which sent it
      to your terminal.

  To try something more ambitious, you can run an Ubuntu container with:
  $ docker run -it ubuntu bash

  Share images, automate workflows, and more with a free Docker ID:
  https://hub.docker.com/

  For more examples and ideas, visit:
  https://docs.docker.com/get-started/

  ```

  ```bash
  $ sudo docker run -it ubuntu bash
  root@6192f0216a20:/#
  root@6192f0216a20:/#
  root@6192f0216a20:/#
  root@6192f0216a20:/# exit
  ```

</details>

<details>
<summary>5. Docker 기본 운영 명령 수행</summary>

- 이미지 다운로드 및 목록 확인
  ```bash
  $ sudo docker pull nginx:latest
  latest: Pulling from library/nginx
  Digest: sha256:5ab5b0b01097420873a3f3330f44248e34a3c2a3e8d5ee30658aeae8cb7435da
  Status: Image is up to date for nginx:latest
  docker.io/library/nginx:latest
  ```
  ```bash
  $ sudo docker images
                                                                                                i Info →   U  In Use
  IMAGE                ID             DISK USAGE   CONTENT SIZE   EXTRA
  hello-world:latest   c567b231f656       25.9kB         9.49kB    U
  nginx:latest         5ab5b0b01097        243MB         66.8MB    U
  ubuntu:latest        6a06111c3b0a        163MB         45.9MB    U
  ```
- 컨테이너 실행, 중지, 목록 확인

  ```bash
  //실행 및 목록 확인
  $ sudo docker run -d -p 8080:80 --name my-web nginx
  7a5d98c515c5c1e7ff59b3db97641c9dd16a83b15e8f7a49308aca352abd9f8b
  $ sudo docker ps
  CONTAINER ID   IMAGE     COMMAND                   CREATED          STATUS          PORTS                                     NAMES
  7a5d98c515c5   nginx     "/docker-entrypoint.…"   20 seconds ago   Up 20 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   my-web
  ```

  ```bash
  //중지 및 목록 확인
   sudo docker stop my-web
  my-web
  $ sudo docker ps
  CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
  ```

- 컨테이너 로그 및 리소스 확인

  ```bash
  $ sudo docker logs my-web
  /docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration

  $ sudo docker stats --no-stream my-web
  CONTAINER ID   NAME      CPU %     MEM USAGE / LIMIT     MEM %     NET I/O         BLOCK I/O    PIDS
  7a5d98c515c5   my-web    0.00%     10.89MiB / 6.692GiB   0.16%     1.16kB / 126B   0B / 4.1kB   13
  ```

</details>

<details>
<summary>6. 컨테이너 실행 실습</summary>

- hello-world 실행 성공 기록
  ```bash
  $ sudo docker run hello-world
  \\
  Hello from Docker!
  ```
- ubuntu 컨테이너 진입 및 간단 명령 수행

  ```bash
  $ sudo docker run -it ubuntu bash
  root@04f723fb27e1:/# ls
  bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
  root@04f723fb27e1:/# echo "Hello Codyssey"
  Hello Codyssey
  root@04f723fb27e1:/# exit
  exit
  ```

- 컨테이너 종료/유지(attach/exec) 차이 정리
  docker attach: 메인 프로세스에 직접 연결

attach 명령어는 이미 실행 중인 컨테이너의 '메인 프로세스(PID 1)'의 표준 입력(STDIN), 표준 출력(STDOUT), 표준 에러(STDERR)에 현재 사용 중인 터미널 화면을 그대로 연결합니다. 마치 이미 돌아가고 있는 프로그램의 메인 화면을 내 모니터로 당겨와서 보는 것과 같습니다. 주로 백그라운드(-d)로 실행해 둔 컨테이너의 실시간 로그를 보거나, 인터랙티브 모드(-it)로 실행 중인 컨테이너의 메인 쉘(Shell)에 다시 들어가고자 할 때 사용됩니다.

사용법: sudo docker attach [컨테이너 이름 또는 ID]

작동 방식: 컨테이너를 구동하는 핵심 프로세스(PID 1)에 직접 접속합니다.

주의 사항 (종료 시의 차이점): 터미널을 빠져나오기 위해 exit 명령어(또는 Ctrl + D)를 입력하면, 연결되어 있던 메인 프로세스가 종료 신호를 받아 함께 종료됩니다. 메인 프로세스가 멈추면 도커 컨테이너의 생명주기도 끝나기 때문에, 결과적으로 컨테이너 전체가 정지(Exited) 상태로 바뀌게 됩니다.

안전하게 빠져나오는 법: 컨테이너를 끄지 않고 연결된 화면(터미널)만 빠져나오려면, exit 대신 반드시 단축키 Ctrl + P, Ctrl + Q를 순서대로 눌러야 합니다. 이 단축키를 사용하면 메인 프로세스는 백그라운드에 남겨둔 채 연결만 부드럽게 끊어(Detach) 컨테이너 실행 상태(Up)를 유지할 수 있습니다.

2. docker exec: 새로운 보조 프로세스 생성 후 접속

exec 명령어는 이미 실행 중인 컨테이너 내부에 완전히 독립적인 새로운 프로세스(보통 bash나 sh 같은 새로운 터미널 쉘)를 추가로 생성하여 실행합니다. 메인 프로세스는 건드리지 않고, 일하기 위한 새로운 방(터미널)을 하나 더 만드는 것과 같습니다. 주로 웹 서버나 데이터베이스 컨테이너가 정상적으로 백그라운드에서 서비스 중일 때, 그 안으로 쏙 들어가서 설정 파일을 수정하거나 내부 로그를 확인하고 파일 구조를 둘러보는 등 점검 작업을 할 때 필수적으로 사용됩니다.

사용법: sudo docker exec -it [컨테이너 이름 또는 ID] [실행할 명령어(예: bash)]

-it: 컨테이너 안에서 키보드 입력을 전달하고(Interactive), 터미널 환경(TTY)을 할당받아 대화형으로 작업하기 위해 반드시 사용하는 필수 옵션입니다.

작동 방식: 메인 프로세스와는 별개로, 지정한 명령어(bash 등)를 새로운 프로세스로 띄워 접속합니다.

안전한 종료 (종료 시의 차이점): 작업을 마치고 exit를 입력하면, 처음에 exec로 새로 만들었던 보조 프로세스(터미널 창)만 깔끔하게 닫힙니다. 컨테이너를 구동하고 있는 원래의 메인 프로세스(예: Nginx 웹 서버)는 전혀 영향을 받지 않으므로, 컨테이너는 계속 실행(Up) 상태를 정상적으로 유지합니다. 따라서 특별한 단축키(Ctrl+P, Q)를 누를 필요 없이 마음 편하게 exit로 빠져나와도 안전합니다.

</details>

<details>
<summary>7. 기존 Dockerfile 기반 커스텀 이미지 제작</summary>

- 커스텀 이미지 제작 방식 선택 (웹 서버 기반 또는 Linux 기반)

  ```bash
  # 1. 커스텀 이미지 제작용 디렉토리 생성 및 이동
  $ mkdir -p ~/codyssey/app && cd ~/codyssey/app

  # 2. 커스텀 포인트: 내 PC의 정적 웹 파일(index.html) 생성
  $ mkdir site
  $ echo "<h1>Hello Codyssey Custom Web</h1>" > site/index.html

  # 3. Dockerfile 작성 (nano 편집기 사용 권장)
  $ nano Dockerfile

  FROM nginx:alpine
  LABEL org.opencontainers.image.title="my-custom-nginx"
  ENV APP_ENV=dev
  COPY site/ /usr/share/nginx/html/

  nano 편집기 내부에서 아래 내용 작성 후 저장: Ctrl+O → Enter → Ctrl+X

  # 4. 작성된 Dockerfile 확인
  $ cat Dockerfile
  FROM nginx:alpine
  LABEL org.opencontainers.image.title="my-custom-nginx"
  ENV APP_ENV=dev
  COPY site/ /usr/share/nginx/html/
  ```

- 이미지 빌드 및 컨테이너 실행 성공 확인

  ```bash
  sudo docker build -t my-web:1.0 .
  [sudo: authenticate] 비밀번호:
  [+] Building 10.0s (7/7) FINISHED

  $ sudo docker run -d -p 8080:80 --name custom-web my-web:1.0
  017ff966fc52ea522f2948d8493d968c206243bb99ec0b3f266c02b7671a2bb3
  ```

- 선택한 베이스 이미지
  nginx:alpine
- 적용한 커스텀 목적 간단 요약
  - LABEL / ENV: 이미지 제목과 개발 환경(APP_ENV=dev) 메타데이터 부여
  - COPY: Nginx의 기본 환영 페이지 대신, 사용자가 직접 작성한 정적 파일(site/index.html)을 제공하도록 변경
- 빌드/실행 명령 및 결과(출력/스크린샷) 포함

  ```bash
  $ curl http://localhost:8080
  <!DOCTYPE html>
  <html>
  <head>
  <title>Welcome to nginx!</title>
  <style>
  html { color-scheme: light dark; }
  body { width: 35em; margin: 0 auto;
  font-family: Tahoma, Verdana, Arial, sans-serif; }
  </style>
  </head>
  <body>
  <h1>Welcome to nginx!</h1>
  <p>If you see this page, nginx is successfully installed and working.
  Further configuration is required for the web server, reverse proxy,
  API gateway, load balancer, content cache, or other features.</p>

  <p>For online documentation and support please refer to
  <a href="https://nginx.org/">nginx.org</a>.<br/>
  To engage with the community please visit
  <a href="https://community.nginx.org/">community.nginx.org</a>.<br/>
  For enterprise grade support, professional services, additional
  security features and capabilities please refer to
  <a href="https://f5.com/nginx">f5.com/nginx</a>.</p>

  <p><em>Thank you for using nginx.</em></p>
  </body>
  </html>
  ```

</details>

<details>
<summary>8. 포트 매핑 및 접속 증거</summary>

- 브라우저 접속 화면 또는 curl 응답 첨부
<img width="940" height="262" alt="image" src="https://github.com/user-attachments/assets/59aeabaf-7f58-4745-8ddd-04174b91faa1" />

  ```bash
  $ sudo docker ps
  CONTAINER ID   IMAGE        COMMAND                   CREATED         STATUS         PORTS                                     NAMES
  aedf5bf17a8e   my-web:1.0   "/docker-entrypoint.…"   4 minutes ago   Up 4 minutes   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   custom-web

  $ curl http://localhost:8080
  <h1>Hello Codyssey Custom Web</h1>
  ```

</details>

<details>
<summary>9. Docker 볼륨 영속성 검증</summary>

- 볼륨 생성 및 컨테이너 연결
  ```bash
  $ sudo docker volume create mydata
  mydata
  $ sudo docker run -d --name vol-test -v mydata:/data ubuntu sleep infinity
  e3c3ec40f11eb50b2dafd9ceb55e5cff2600e392d0793df7c7ab0022ed61fb77
  p$ sudo docker exec -it vol-test bash -lc "echo 'Hello Codyssey Volume Persistence' > /data/hello.txt && cat /data/hello.txt"
  Hello Codyssey Volume Persistence
  ```
- 컨테이너 삭제 전후 데이터 유지 증명

  ```bash
  //삭제
  $ sudo docker rm -f vol-test
  vol-test
  $ sudo docker run -d --name vol-test2 -v mydata:/data ubuntu sleep infinity
  6b2141482b534033e44f60f175d465304616b0d90a66d076a0276f68b3015d2e
  $ sudo docker exec -it vol-test2 bash -lc "cat /data/hello.txt"
  Hello Codyssey Volume Persistence
  ```

</details>

<details>
<summary>10. Git 설정 및 GitHub 연동</summary>

- Git 사용자 및 브랜치 설정 결과 확인
  ```bash
  hyukjoo@KWON:~/codyssey/app$ git config --global user.name "notionai5929-cyber"
  hyukjoo@KWON:~/codyssey/app$ git config --global user.email "notionai5929@gmail.com"
  hyukjoo@KWON:~/codyssey/app$ git config --global init.defaultBranch main
  hyukjoo@KWON:~/codyssey/app$ git config --list
  user.name=notionai5929-cyber
  user.email=notionai5929@gmail.com
  init.defaultbranch=main
  ```
- GitHub 저장소 연동 증거 첨부
  ```bash
  $ git push -u origin main
  Username for 'https://github.com': notionai5929-cyber
  Password for 'https://notionai5929-cyber@github.com':
  오브젝트 나열하는 중: 10, 완료.
  오브젝트 개수 세는 중: 100% (10/10), 완료.

  $ git remote -v
  origin  https://github.com/notionai5929-cyber/ai-codyssey.git (fetch)
  origin  https://github.com/notionai5929-cyber/ai-codyssey.git (push)  
  ```
  <img width="562" height="51" alt="image" src="https://github.com/user-attachments/assets/710ec920-bd41-4e84-8e01-0a5e60d583ad" />

</details>

<details>
<summary>11. 보안 및 개인정보 보호</summary>

- 민감 정보(토큰, 비밀번호, 개인키 등) 마스킹 처리
  GitHub 연동 시 사용된 개인 액세스 토큰(PAT) 및 계정 비밀번호는 문서 및 로그에 일체 기록하지 않음
  노출 방지를 위해 마스킹 처리 규칙을 준수
- 노출 의심 시 즉시 제거 및 재발급 수행
- 
</details>

# 트러블슈팅
1. 포트 충돌 및 기존 컨테이너 이름 중복 문제
  Troubleshooting : chmod 755 명령어를 이용해 실행 권한을 부여
                    ls -al로 확인해 보면 권한이 -rwxrwxrwx 그대로 유지된 채 전혀 바뀌지 않는 현상
   cause : WSL 우분투 환경에서 작업 중이지만, 파일이 저장된 실제 물리적 공간은 윈도우 파일 시스템
           리눅스의 권한 체계(rwx)가 기본적으로 무시되고 윈도우 규칙에 따라 전체 권한으로 고정
   solve : 윈도우 드라이브 마운트 시 리눅스식 파일 권한(메타데이터)을 읽을 수 있도록 WSL 설정 파일인 /etc/wsl.conf를 수정
   ```bash
   sudo nano /etc/wsl.conf

   [automount]
   options = "metadata"
   ```
2. WSL(Windows Subsystem for Linux) 환경에서의 파일 권한(chmod) 미적용 문제
   Troubleshooting : 도커 컨테이너를 실행할 때 sudo docker run -d -p 8080:80 --name my-web nginx 명령어를 입력하자,
                     데몬 에러(Daemon Error)가 발생
   cause : 도커는 하나의 포트에 하나의 프로세스만 연결할 수 있으며,
           컨테이너 이름은 시스템 내에서 고유해야 하므로 이전에 띄웠던 잔여 컨테이너가 남아있으면
           새로운 컨테이너를 같은 이름과 포트로 띄울 수 없습니다.
   solve : 실행 중이거나 정지된 모든 컨테이너를 강제로 정리하고 포트를 확보하는 일괄 삭제 명령어를 사용
   ```bash
   sudo docker rm -f $(sudo docker ps -aq)
   ```
   충돌 없는 고유한 이름과 포트 매핑(-p 8081:80 --name my-web2)을 지정하여 정상적으로 백그라운드 구동

