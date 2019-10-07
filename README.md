# AWS-Starter
AWS(Amazon Web Services) 시작을 위한 정리

## 1.AWS 시작하기
### AWS 가입 및 로그인
* [AWS](https://aws.amazon.com/ko/console/) 에서 회원가입 및 로그인

### 이중 인증 로그인 등록하기
* 서비스 들어가기
  * 좌측 상단의 서비스 -> 보안, 자격 증명 및 규정 준수 -> IAM

* IAM User 만들기
  * 루트 계정에서 MFA 활성화 -> MFA 관리 -> Get Started with IAM Users -> 사용자 추가
  
-> 멀티 팩터 인증(MFA) -> MFA 활성화 -> 가상 MFA 디바이스
* 스마트폰의 앱스토에서 google otp 혹은 google authenticator 설치 -> 맨 아래의 '설정 시작하기' 터치 -> 바코드 스캔 -> 스캔 후 나오는 6자리의 숫자를 'MFA 코드 1'과 'MFA 코드 2' 두 곳에 6자리 모두 입력.

### 기초 배워야할 항목
* kinesis
* kinesis firehose
* S3 Data Lake
* EMR
