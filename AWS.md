### AWS

1. 클라우드 서비스란?
 1) 클라우드라고 하는 이유? 
 - 서버를 ㅇ인 원으로 표현, 이것의 수가 많게 되면 구름 모양.
 - 사용자는 필요한 컴퓨팅 파워만큼만 구름을 때올 수 있음. 
 2) 클라우드 서비스 종류
 * EC2
   - AWS에서 가장 대표적인 서버 단위
   - EC2 instance
 * S3
   - AWS에서 가장 대표적인 파일 저장소
   - 정적페이지인 경우 호스팅으로도 사용 가능함
 * RDS
  - AWS에서 가장 대표적인 관계형 데이터베이스
 
 3) 클라우드 서비스를 사용해야 하는 이유?
 - 비용적, 개발적 측면
 - 개발에만 집중할 수 있음.


2. AWS 가입 및 프로티어 설명
 * 프로티어
  - 가입후 12개월 무료

3. EC2
1) 알아보기
- 탄력적 IP 주소: 특정 IP와 연결
- 인스턴스: 생성후 IP주소 연결하면 비용이 발생하지 않음
- 보안 그룹: 인바운드 규칙 편집, 규칙에 맞지 않은 것은 모두 block
  - 위치무관: 어디서든 열릴 수 있게
  - 내IP: 보안상 개발하려고 만든 곳이면 나만
  - 사용자 지정 
- 로드밸런서: 인스턴스가 일정 부하를 받게 되었을때 스케일링. 똑같은 이미지를 출력하면서 서버를 축소하거나 확대하는 것. 서버 터지지 않도록 막아주는 것.

2) 생성하기
- 네트워크: VPC는 분리해서 쓰기, 하나의 VPC 안에 내가 만든 서버가 다들어가버리게 됨.
- 서브넷: VPC 파티션 같은 것
- 퍼블릭 IP: 
- 스토리지: 몇 GB 쓸 수 있냐

3) 접속하기
-EC2 인스턴스 연결로 접속해봄

4) SSH 클라이언트 사용하기
https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/putty.html

5) 중지, 삭제, 비용
- 중지해도 비용은 나감. 인스턴스를 만든 자체가 시간이 간다.
- 따라서 중지가 아닌 삭제를 해야함.
- But 종료(=삭제)는 백업이 불가능함. 이것에 대한 이미지를 미리 만들어놓는 것이 방법임.

4. S3
- 리전별로 비용이 다름.
- 아직 미국 동부가 제일 저렴, 아시아가 비싼 편, 한국은 나중에 추가되서 풀도 적고 활성화 안된 경우도 있음.

1) 퍼블릭 액세스
- 다 열어두면: 누구나 다운받을 수 있음.
- 퍼블릭을 전부 차단하는 것이 이상적
- 권한의 결정은 IAM 이라는 액세스 관리에서 사용자 추가하면서 설정할 수 있음.

2) 버전관리
- 특정시점으로 롤백할 수 있음.
- 대부분은 비활성화해도 상관은 없음

3) 태그
- 특정내용 기록. 확인하기 좋게끔 해둠


5. aws-sdk로 S3 사용하기

https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/setting-credentials-node.html

1) aws-sdk 설치
> npm install aws-sdk

2) IAM에서 사용자 권한 설정하기

3) aws-sdk를 통해 S3 사용하기

https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/s3-example-creating-buckets.html

문서 참고

```node
const AWS = require("aws-sdk");

AWS.config.update({region: 'us-west-2', accessKeyId:"", secretAccessKey:""});

// Create S3 service object
s3 = new AWS.S3({apiVersion: '2006-03-01'});

// Call S3 to list the buckets
s3.listBuckets(function(err, data) {
    if (err) {
        console.log("Error", err);
    } else {
        console.log("Success", data.Buckets);
    }
});
```

* 문서를 읽으면 나와있음. 

6. RDS
1) MySQL Workbench 다운로드

https://www.mysql.com/products/workbench/

2) 연결전 체크

![image](https://user-images.githubusercontent.com/69338643/132173329-06aff703-534e-4b93-9960-d2eec3be7dc6.png)
![image](https://user-images.githubusercontent.com/69338643/132173352-5f020443-5df7-4e52-a8d3-dca733a42b36.png)



