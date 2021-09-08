## Redis

#### 1. 레디스란 무엇인가

- "키-값" 구조의 비정형 데이터 저장 관리 도구
- 언제나 키, 값이 존재해야 저장이 되는 구조임.
- 데이터베이스 관리 시스템 (DBMS)

---

#### 2. 레디스의 특징

###### 1) 맵(Map) 데이터 저장소
- 맵: 무한한 키,값 형태의 데이터

###### 2) 인-메모리 데이터베이스
- 데이터베이스냐, 데이터베이스 관리 시스템이냐 
- 인-메모리는 휘발성이다보니, 이것을 데이터베이스라고 해야하나, DBMS라고 해야하냐
- 지금은 영구저장할 수 있는 것이 개발됨.
- 인-메모리: 컴퓨터 안에 데이터가 RAM에 잡혀있음. <br>
 값을 수백만건에서 찾아오는게 아니라 코 앞에서 대기하고 있는 것. <br>
레디스를 끄게 되면 갖다놓았던 모든 정보는 깨끗하게 사라지게 됨

###### 3) 영속성(Persistence) 보장 가능
- 휘발성이라서 날라가는게 정상인데 사용자들이 몇 개는 보장되었으면 좋겠다고 하였음.
- AOP, Snapshot 기능. 

---

#### 3. Collection 알아보기

###### 0) Redis 5대장
- Strings, Lists, Sets, Sorted sets, Hashes

###### 1) Strings
- 숫자임에도 불구하고 string. number라는 타입 자체가 없음
- Strings 명령어: SET, GET, INCR, INCRBY, DECR, DECRBY...
※ 이 명령어는 string에게만 먹힘
- 예시)
> set my_number 10
>> OK

> get my_number
>> "10"

> incr my_number
>> (Integer) 11

> get my_number
>> "11"

###### 2) Lists
- 명령어: LPUSH, RPOP, LPOP, RPUSH, LRAGNE ...
- LPUSH: 리스트의 왼쪽에 아이템 푸시
- RPOP: 오른쪽에서 데이터를 거내오고, 리스트에서는 삭제

- 예시

> lpush my_list 10
>> 1 -리스트의 길이

> lpush my_list 20
>> 2 -리스트의 길이

> lrange my_list 0-1
>> 0) 20
>> 1) 10

> lpush my_list 30 40
>> 4 -리스트의 길이

###### 3) Sets
- 명령어: SADD, SMOVE, SMEMBERS, SCARD, SPOP ...
- SET의 요소는 member라고 부름, 데이터들에 순서가 없음
- SADD: 집합에 member를 추가
- SREM: 집합에서 member를 삭제
- 맴버1을 두 번 넣으면 중복x 

- 예시

> sadd my_set "맴버1"
>> 1 - 추가된 멤버의 숫자

> sadd my_set "맴버2" "맴버3" "맴버4"
>> 3 - 추가된 맴버의 숫자

> smembers my_set
>> 1) 맴버2
>> 2) 맴버4
>> 3) 맴버3
>> 4) 맴버1
>> - 특별한 순서가 적용되진 않는다

###### 4) Sorted Sets(ZSets)

- Sets: 집합의 순서가 없음
- Sorted Sets: 맴버들에게 score를 부여해서, score에 따라 정렬된 값을 return받을 수 있음
- 명령어: ZADD, ZRANGE, ZPOPMIN, ZPOPMAX ...
- ZADD: 집합에 score와 member를 추가
- ZCARD: 집합에 속한 member의 갯수를 조회
- Score의 범위: -9007100254750992(-2^53) ~ 9007100254750992(+2^53)

- 예시

> zadd my_zset 50 "맴버1"
>> 1 -추가된 맴버의 숫자

> zadd my_zset 100 "맴버2"
>> 1

> zrange my_zset 0-1 withscores
>> 1) 100 -> 맴버2
>> 2) 50 -> 맴버1
>> - 스코어에 따라 정렬된 값이 리턴된다.

###### 5) Hashes
- key 안에 여러개의 필드가 있음. (40억개까지 저장 가능)
- 명령어: HSET, HMSET, HGET, HDEL, HINCRBY ...
- HSET: Field와 Value를 저장
- HDEL: Field로 Value를 삭제

- Hashes와 Table과 유사점
Hash key: PK
Field: Column
Value: Value

* MySQL의 데이터를 빠르게 가져올 수 있음. hash를 통해서는 테이블에 맞게 가져올 수 있음

- Hashes와 Table과 차이점
1. 해시 데이터의 필드는 최대 40억개
2. 테이블은 데이터를 변경하기 위해선 먼저 테이블을 변경해야 함
3. 해시는 즉시 필드의 삭제 수정이 가능

###### 6) Bitmaps, Hyperloglogs
- 문자열과 형태는 똑같지만 목적이 다른 형태
- 사실 별로 안쓰임..

###### 7) Streams
- 실시간 정보가 많이 들어오고 있음. resdis에서 처리하기 위해 만듦
- redis 5.0에서 추가됨
- 연속된 데이터 처리 가능함
- 명령어: XADD, XLEN, XRANGE, XREAD, XDEL ...
- 예시

> xadd my_stream * field value
>> 15728830100-0 -명령 결과로 얻은 ID를 리턴
>> <밀리세컨즈시간>-<시퀀스 넘버>

> xadd key ID field value [field 2 value2]

---

#### 4. 레디스 Cache 알아보기

- 레디스에서 제일 중요한 개념 caching. 인-메모리로 작동하기 때문임.
- 페이스북이 대표적으로 관계형 데이터베이스
  - 회원이 20억명인데, 내 ID찾는다고 해도 20억 중에 한 개 찾기. -> DB가 받는 부하가 커질 수 밖에 없음
  - 예를 들어 자주 보는 신문기사가 있을때, 그것을 DB에서 계속 쏴주면 부하받을것. 그것을 인-메모리에 저장하고 쏴주면 DB는 부하를 받지 않음.
  - *반복요청을 캐싱 처리

 ---

#### 5. 레디스의 운영 방법

- 그럼 도대체 레디스는 어떻게 써야할까?

###### 1) 콘텐츠 캐싱

###### 2) 대규모 I/O 처리

- BTS 구독 알람한 사람들이 바로 DB로 가져가면 수억명이 한 번에 보내게 됨.
- Redis로 일단 수많은 요청을 잡아놓음. EX. 좋아요 카운트. -> 5분에 한 번씩 업데이트 해주는 것.(캐싱주기)

###### 3) 세션 관리

- Redis는 휘발성이라 세션꺼지면 날라감.
- 세션: 사용자의 인증 정보를 서버에 저장. 한 번 일치하면 계속 쓸 수 있도록.
- Redis가 갑자기 꺼져서 세션이 유실될때, 사용자가 글쓰다가 갑자기 날라가버림. 결재 중에 날라가버리게 될 경우가 있다보니 jwt를 많이 쓰긴 함.

###### 4) 메세지 큐

- 채팅을 DB에 저장할 경우? 채팅 속도가 빠를 수가 없음.
- 메세지를 벌크로 묶어서 한 번에 저장.

---

#### 6. 레디스와 다른 데이터베이스 비교

|           |레디스|MySQL|MongoDB|
|:------:   |:---:|:---:|:---:|
|언어       |ANSI, C|C, C++|C++, JS, Python|
|스키마 구조|자유로움|고정됨|비교적 자유로움|
|DB 모델    |키 - 벨류|관계형 DB|문서형 DB|
|XML 지원   |X|O|X|
|트리거 지원|X|O|O|
|저장 형태  |휘발성|비휘발성|비휘발성|

- MySQL: RDBS, MongoDB: Document
- 스키마구조: MySQL은 구조대로 넣어야 함. MongoDB는 중간 정도, Redis: NoSQL형태
- XML: HTML 후배 정도. 안드로이드 레이아웃 잡을때 많이 쓰임. Redis, MongoDB는 지원하지 않음
- 트리거: 특정조건이면 트리거가 실행되는 것. Redis는 없음. 사실 트리거가 Risky한 도구임. 모르고 남겨놨다가 트리거 발생해서 버그가 나면 골치 아픔.
- 저장형태: Redis는 조건부 비휘발성. 마지막 파일을 저장해주면서 쓸 수 있기에 휘발성인 동시에 비휘발성 (=조건부 비휘발성)
- 프로젝트에 맞게 선택할 수 있어야 함.
  - 사용자 정보: MySQL
  - 정보가 구체적으로 정해지지 않았다면, MongoDB 
  - 데이터베이스를 도와주는 형태: Redis

---

#### 7. 레디스 실습

https://goni9071.tistory.com/473

redis를 설치한 후 진행하기!


