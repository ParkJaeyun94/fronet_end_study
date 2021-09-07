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



#### 3. 도커 설치하기

https://docs.docker.com/engine/install/ubuntu/

> 리눅스에서 도커 설치
https://docs.docker.com/engine/install/ubuntu/

> 윈도우에서 도커 설치
https://docs.docker.com/desktop/windows/install/

> 맥에서 도커 설치
https://docs.docker.com/desktop/mac/install/


#### 4. 도커 이미지란

- Linux 가져오면, docker 이미지 켜면 linux가 온 것
- 주의할 것: OFFICAIL인지 확인하고 가져오기.
- 이미지: 개발 환경 그 자체
  - 도커 위에 이미지 올리고, 컨테이너 올리고, 웹 서버에 올리면 개발환경이 달라서 생기는 문제를 해결할 수 있음
  - Local의 개발환경과 실제 환경이 똑같아짐.
- 이미지만 맞춰서는 어려운 부분이 있음
  - node 10ver. 동일한 우분투이더라도 버전이 다르면 작동이 안될 수 있음. 그 안에 돌아가는 sw환경까지 맞추는 것이 바로 docker file임 








