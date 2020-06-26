# AWS-Starter
AWS(Amazon Web Services) 시작을 위한 정리

## AWS 기초 정리
### Region, Availability Zone , Edge Location
참조링크: [AWS Region , Availability Zone , Edge Location 이란?](https://interconnection.tistory.com/39)

## AWS IAM
참조링크: [AWS IAM: IAM Policy 알아보기 (이론편)](https://musma.github.io/2019/11/05/about-aws-iam-policy.html)

## AWS Lambda
[[AWS Lambda 초보자를 위한 개략적 설명]]   
람다 실습 영상 자료(유투브): [AWS Lambda - 동빈나](https://www.youtube.com/watch?v=7uEDep9DFJs&list=PLRx0vPvlEmdD_AdG6fEwcfVrq5Qb3q_Ja&index=1)   
[AWS Lambda Layer를 사용하는 방법 (How to use AWS Lambda Layers)](https://medium.com/@rabter/aws-lambda-layer%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95-how-to-use-aws-lambda-layers-c206ba40d4cc)

## AWS Chalice
Chalice 시작하기: [AWS Chalice Quickstart and Tutorial](https://aws.github.io/chalice/quickstart.html)   
[HTTP 압축 (2) : HTTP 압축 작동 원리](http://www.simpleisbest.net/archive/2005/07/18/185.aspx)   
[Amazon S3 버킷에 대한 액세스 권한을 Lambda 실행 역할에 허용하려면 어떻게 해야 합니까?](https://aws.amazon.com/ko/premiumsupport/knowledge-center/lambda-execution-role-s3-bucket/)  

## AWS Best Practice(규칙)
### Security Group(보안 그룹)
* 규칙1: 보안 그룹은 EC2 인스턴스 당 생성이 아닌 Application 당 생성으로 하여 동일 Application을 구동하는 EC2 인스턴스 들에 동일한 Security Group을 붙일 수 있도록 한다.
* 규칙2: HTTP(80) 혹은 HTTPS(443)을 제외한 포트에 대해서는 구체적인 IP만 허용하도록 한다.

## EC2 instance에 접속 계정 추가하기
EC2 instance에 접속을 허용을 추가할 계정으로 로그인

### 키 페어 생성
> AWS Management Console 접속 -> EC2 -> 왼쪽 메뉴바에서 네트워크 및 보안 아래 **키 페어** 클릭 -> 키 페어 생성 -> 키 페어 이름 입력 및 파일 형식을 pem으로 지정
> 다운로드 받은 파일을 적절한 곳에 위치

### 공개키 키 생성
> .pem 파일(개인 키)이 존재하는 디렉터리로 이동 : `cd [pem 존재 디렉터리]`
> .ssh 디렉터리에 공개키 생성: `sudo ssh-keygen -y -f [pem 명].pem > $HOME/.ssh/[public key 명].pub`
> .pem 파일 퍼미션 변경: `sudo chmod 600 [pem 명].pem`
> EC2 instance에 접속이 가능한 계정을 통해서 위에서 생성한 공개키를 EC2에 전송: `scp -l 50000 ~/.ssh/[public key 명].pub ec2-user@[ec2 public IP]:/home/ec2-user/.ssh`
> EC2 instance에 SSH 접속하여 $HOME/.ssh/aws_bastion_key.pub 파일의 내용을 bastion 호스트의 ~/.ssh/authorized_keys의 맨 마지막에 추가
``` shell
cd ~/.ssh
cat [public key 명].pub >> authorized_keys
```

### 추가된 계정의 키로 접속해보기
``` shell
cd [pem 존재 디렉터리]
ssh -ti [pem 명].pem -o "UserKnownHostsFile=/dev/null" -o "StrictHostKeyChecking=no" ec2-user@[ec2 public IP]
```

## SSH 터널링 + bastion Host를 통한 EC2 Instance 접속
``` shell
ssh -t -o ProxyCommand="ssh -W %h:%p ec2-user@[bastion host의 ip] -i [bastion에 접근할 때 사용하는 개인키 명].pem" ec2-user@[터널링을 통해 접속할 서버 IP] -i [터널링을 통해 접속할 서버에 사용하는 개인키 명].pem
```
> 터널링을 통해 접속할 서버의 개인키는 위의 [EC2 instance에 접속 계정 추가하기](https://github.com/JuJin1324/AWS-Starter/blob/master/README.md#ec2-instance%EC%97%90-%EC%A0%91%EC%86%8D-%EA%B3%84%EC%A0%95-%EC%B6%94%EA%B0%80%ED%95%98%EA%B8%B0)에서 자신의 개인키로 공개키를 생성한 이후에 터널링을 통해 접속할 EC2 Instance를 만든 사람에게 생성한 공개키를 전달해준다.   
> 전달 받은 사람은 해당 공개키의 내용을 ~/.ssh/authorized_keys 파일에 추가한다.
