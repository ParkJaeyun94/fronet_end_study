## Docker

#### 1. 도커 컨테이너와 가상 머신

**가상머신

![image](https://user-images.githubusercontent.com/69338643/132276262-082bfb29-1af2-4356-9bb0-3cf35369eddb.png)

- Host OS= window, max, linux
- Hypervisor= 이것을 통해 여러 OS를 컴퓨터에서 사용
- 가상머신: 버츄얼박스, 브이엠웨어 .. 
- GuestOS= Hypervisor 사용하게 될 때, 반드시 둬야 함. 시스템 자원의 일부를 독립된 구간에 분리, 그곳에서 필요한 자원 다시 생성. <br>
  그러다보니 이미지 크기가 굉장히 커짐. Application 용도로는 무거워질 수 밖에 없고, 수정하는 것도 부담. 
  

**도커

![image](https://user-images.githubusercontent.com/69338643/132276538-e3d60f74-ceb0-4947-88c9-6ba71a169fb1.png)

- Hypervisor 위치에 Docker Engine이 오게 됨. Guest level은 존재하지 않음.
- Docker 자체는 Linux에서 움직임. 
- 프로세스 단위의 격리 환경을 만들게 됨. 성능 소실이 거의 발생하지 않고, Host 서버의 computing power를 최대한 활용할 수 있다는 장점
- container ( Guest OS + Bins/Libs + App )의 kernel이 서버의 kernel 공유
  - kernel 하드웨어 장치에 접근, 처리 .. OS에 있어서 굉장히 필요한 프로그램이라는 것
  - 공유를 하기 때문에 전체 이미지가 작아지게 됨. 기존에 가상머신은 GuestOS에 맞는 커널을 넣어야 했는데, 그것이 생략가능


- 도커의 장점
  - 컨테이너 이미지의 용량이 적다
  - 배포 속도가 빠르다 (가상머신은 OS자체가 업데이트 되야할 수도 있음)
  - 가상화된 공간을 사용할 때의 성능손실이 적다! (커널을 공유하고 리소스도 공유하고 프로세스 단위로 사용하기 때문)

---

#### 2. 도커를 사용하는 이유

##### 독립성과 확장성 (마이크로서비스 구조)

- ex. 웹: Node.js 컨테이너 따로, DB 컨테이너 따로. 
- 컨테이너 오케스트레이션 플렛폼: Docker swarm,  Kubernetes를 통해 편리하게 배포, 관리


---
#### 3. 도커 설치하기

https://docs.docker.com/engine/install/ubuntu/

> 리눅스에서 도커 설치
https://docs.docker.com/engine/install/ubuntu/

> 윈도우에서 도커 설치
https://docs.docker.com/desktop/windows/install/

> 맥에서 도커 설치
https://docs.docker.com/desktop/mac/install/


---
#### 4. 도커 이미지란

- Linux 가져오면, docker 이미지 켜면 linux가 온 것
- 주의할 것: OFFICAIL인지 확인하고 가져오기.
- 이미지: 개발 환경 그 자체
  - 도커 위에 이미지 올리고, 컨테이너 올리고, 웹 서버에 올리면 개발환경이 달라서 생기는 문제를 해결할 수 있음
  - Local의 개발환경과 실제 환경이 똑같아짐.
- 이미지만 맞춰서는 어려운 부분이 있음
  - node 10ver. 동일한 우분투이더라도 버전이 다르면 작동이 안될 수 있음. 그 안에 돌아가는 sw환경까지 맞추는 것이 바로 docker file임 

---
#### 5. 도커 파일

- Dockerfile: 이미지를 빌드하는데 필요한 모든 명령을 순서대로 포함하는 텍스트 파일
  - ex. node 버전 설치하고 mysql 버전 뭐하고, 명령은 뭘 실행하고, 등등을 순서대로 적어둠

```dockerfile
FROM ubuntu:18.04
COPY . /app
RUN make /app
CMD python /app/app.py
```

---
#### 6. 도커 컨테이너

- ex. 웹서버: 서버 하나로는 성능이 안된다? 웹서버 컨테이너 늘려 복사해서 컨테이너 늘리고 분산처리해주면 됨. <br>
서로 독립되어있기 때문에 영향을 주지 않음. 컨테이너 하나가 설령 멈춰도 다른 컨테이너가 영향을 받지 않음

---
#### 7. 도커 컨테이너 생성하기

cmd창에
> docker run -i -t ubuntu:14.04

![image](https://user-images.githubusercontent.com/69338643/132297633-80ac015a-abfa-4cf6-858a-d69d149fb14f.png)

이 자체가 ubuntu 환경이 된 것

- 나가는 방법
  - 서버 유지 o: [ctrl] + p + q 
  - 서버 유지 x: exit
---
#### 8. 컨테이너 애플리케이션 구축하기

> docker images

> docker search wordpress

> docker pull wordpress

https://hub.docker.com/_/wordpress

```
docker run --name fastcampus-docker -p 8080:80 -d wordpress
```

---
#### 9. Node.js 앱 만들기

> npm install -g docker

> docker build . -t fastcampus-docker/node-web-app

하면 images에 생기게 됨


[CMD 창에서 할 것]

> docker run --name fc-docker -p 47561:8080 -d fastcampus-docker/node-web-app

- 이후 Containers / Apps에 port가 47561로서 열 수 있게 됨

![image](https://user-images.githubusercontent.com/69338643/132334940-2f34e5ef-563d-46ef-826e-3b215475cc05.png)

