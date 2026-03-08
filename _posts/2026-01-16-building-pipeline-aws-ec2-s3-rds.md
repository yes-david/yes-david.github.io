---
layout: post
title: "[AWS] EC2, S3, RDS를 활용한 클라우드 데이터 파이프라인 구축해보기"
date: 2026-01-16
categories: [Data-Engineering-Hands-on]
---

## 개요
클라우드 기반 데이터 아키텍처 구축 실습이다. AWS의 핵심 서비스인 EC2, S3, RDS를 연동하여 데이터 파이프라인을 직접 만들어보는 과정이다. EC2 서버에서 실습용 데이터를 다운로드하여 S3 스토리지에 원본을 보관하고, RDS 관계형 데이터베이스에 정형 데이터 및 비정형 데이터의 메타데이터를 적재하는 흐름을 배운다. 마지막으로 RDS에서 조건에 맞는 데이터를 조회하여 그 결과를 다시 S3에 저장한다. 유의사항으로는, 비용이 계속 발생할 수 있으므로 모든 실습이 끝난 후에는 생성한 클라우드 자원을 반드시 삭제해야 한다.

---

## 실습 진행 순서
### 1단계: EC2 인스턴스 환경 세팅
AWS EC2 콘솔을 켜서 클라우드 가상 서버를 생성한다. 원하는 이름으로 보안 그룹을 만들고 SSH 접속과 MySQL 포트를 열어준다. 터미널을 통해 생성한 인스턴스에 접속한 뒤, 명령어 창에 준비된 링크를 입력하여 실습에 필요한 데이터 압축 파일을 다운로드하고 압축을 푼다.
```bash
mkdir data
cd data
sudo wget -S "https://~~~.s3.ap-northeast-3.amazonaws.com/file/data.zip" -O ./data.zip
unzip data.zip
```
sudo(SuperUser DO. 관리자 권한으로 실행), wget(Web Get. 인터넷에서 파일을 다운로드하는 프로그램 이름이다), -S(Server Response. 파일을 받으면서 서버가 보내는 응답 헤더 정보를 화면에 출력), -O ./data.zip(Output. 받은 파일을 data.zip이라는 이름으로 저장하라는 뜻)

### 2단계: IAM 역할 생성 및 부여
AWS IAM 콘솔을 켜서 EC2 서버가 다른 AWS 서비스에 접근할 수 있도록 권한을 설정한다. S3에 자유롭게 접근할 수 있는 정책을 추가하여 새로운 역할을 만든다. 역할 생성이 끝나면 다시 EC2 설정으로 돌아가 이 역할을 인스턴스에 부여한다.

### 3단계: S3 버킷 및 폴더 생성
AWS S3 콘솔을 켜서 클라우드 저장소를 만든다. 전 세계에서 유일한 이름으로 버킷을 생성한 뒤, 원본 데이터를 저장할 폴더와 나중에 쿼리 결과물을 저장할 폴더를 구조에 맞게 미리 생성해둔다.

### 4단계: RDS 데이터베이스 및 테이블 세팅
AWS RDS 콘솔을 켜서 MySQL 데이터베이스를 프리티어로 생성한다. 데이터베이스 생성이 완료되면 EC2 터미널 창을 열어 MySQL 접속 프로그램을 설치하고 RDS에 연결한다. 접속 후에는 자전거 대여 기록을 담을 정형 데이터 테이블과 이미지 파일의 상세 정보를 담을 메타데이터 테이블을 각각 생성한다.
```bash
sudo yum install -y mariadb105
mariadb --version
mysql -h <RDS_Endpoint> -P 3306 -u admin -p
```
yum(Yellowdog Updater, Modified. Amazon Linux 등 Red Hat 계열 운영체제에서 mariadb 10.5 버전을 자동으로 설치), -y(확인 질문에 모두 예라고 답하라는 것), mysql(DB에 접속하는 프로그램을 실행. MariaDB는 MySQL과 호환되어 mysql 명령어를 그대로 쓴다), -h(Host. 접속할 데이터베이스의 주소를 입력), -P(Port. 데이터베이스가 사용하는 통로 번호. 3306은 MySQL/MariaDB의 기본 번호), -u(User. 접속할 사용자 아이디. 여기서는 admin인 것), -p(Password. 비밀번호를 입력하겠다는 옵션. 엔터를 치면 비밀번호 입력창이 뜬다)

### 5단계: S3로 원본 데이터 업로드
EC2 터미널 창에서 AWS CLI 명령어를 사용하여 아까 다운로드했던 CSV 파일과 이미지 파일을 S3 버킷으로 업로드한다. 그다음 이미지 파일의 메타데이터를 알아내기 위해 관련 명령어를 입력하고, 화면에 출력되는 파일 상세 정보를 따로 메모해둔다.
```bash
aws s3 cp data.csv s3://<버킷명>/~~~/~~~/data.csv
aws s3 cp "~~~.png" s3://<버킷명>/~~~/~~~/~~~.png --content-type image/png
aws s3api head-object --bucket <버킷명> --key ~~~/~~~/~~~.png --query '[ETag, ContentLength]' --output text
```
1. 내 컴퓨터에 있는 data.csv 파일을 S3 버킷의 특정 폴더로 업로드
2. 이미지를 업로드하면서 파일 형식을 image/png로 지정한다. 이렇게 해야 웹 브라우저에서 링크를 열었을 때 다운로드되지 않고 사진이 바로 보인다.
3. 파일을 다운로드하지 않고 서버에 저장된 파일의 크기와 고유한 지문값만 확인. 보통 이 정보를 RDS에 기록할 때 사용한다

### 6단계: RDS에 정형 데이터 및 메타데이터 적재
EC2 터미널 창에서 다시 RDS 데이터베이스에 접속한다. 준비된 명령어를 통해 CSV 파일의 내용을 자전거 대여 테이블에 일괄 적재한다. 이미지 메타데이터 테이블에는 앞선 단계에서 메모해둔 파일 상세 정보를 직접 입력하여 데이터 행을 추가한다.
```bash
mysql -h <RDS_ENDPOINT> -P 3306 -u admin -p
```
```sql
USE ~~~;

LOAD DATA LOCAL INFILE '/home/ec2-user/data/data.csv'
INTO TABLE ~~~
FIELDS TERMINATED BY ','
OPTIONALLY ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 LINES
(@dt, season, holiday, workingday, weather, temp, atemp, humidity, windspeed, casual, registered, count)
SET datetime = STR_TO_DATE(@dt, '%Y-%m-%d %H:%i:%s');
```
```sql
USE ~~~;


INSERT INTO ~~~
(image_id, s3_bucket, s3_key, etag, content_type, size_bytes, created_at)
VALUES
(
  'animal',
  <버킷명>,
  '~~~/~~~/~~~.png',
  <ETag>,
  'image/png',
  <ContentLength>,
  NOW()
);
```

### 7단계: 조건에 맞는 데이터 조회 및 저장 후 마무리
적재가 완료된 자전거 대여 테이블에 특정 조건을 걸어 쿼리를 실행한다. 해당 쿼리의 결과를 S3 버킷의 폴더에 파일 형태로 저장한다. 요금이 나오지 않도록 실습에 사용한 모든 리소스를 삭제하고 마무리한다.
![쿼리](/assets/images/query-2026-01-16-building-pipeline-aws-ec2-s3-rds.png)
