# Installation (Quick Start)
```note
FOSSLight를 빠르고 쉽게 설치 및 실행하는 방법을 설명합니다.
```
## 설치
### 요구사항
- JAVA 1.8 이상
- MariaDB 10.0 이상 또는 MySql 5.6 이상

### 설치
1. JAVA를 설치합니다. : [ https://openjdk.java.net][java]
2. MariaDB 또는 Mysql 설치합니다. : [https://mariadb.org/download][maria]
3. DDL 파일을 다운로드 받습니다. : [fosslight_create.sql][sql]
4. Database 생성 및 초기 Data 등록합니다.
```
mysql -u root -p < fosslight_create.sql
```
만약 Database가 이미 존재하거나 Database 이름을 변경하려면 상단의 create database 문과 USE 'fosslight' 문을 변경합니다.
```
mysql -u root -p <DATABASE_NAME> < fosslight_create.sql
```
접속 계정이 이미 존재하거나, 다른 계정을 사용하는 경우 CREATE USER 및 GRANT 부분을 삭제(또는 변경) 합니다.

[java]: https://openjdk.java.net
[sql]: https://github.com/fosslight/fosslight/blob/main/install/db/fosslight_create.sql
[maria]: https://mariadb.org/download

## 실행
java가 설치되어 있는 환경에서 명령어를 실행합니다. 
```
java -jar fosslight.war --root.dir=/data/fosslight --server.port=8180
```
### 실행 옵션
- 포트 변경
```
--server.port=<PORT>
```
- Work Directory 설정 (Default: /usr/share/fosslight)
```
--root.dir=<WORK_DIRECTORY>
```
- Database 접속정보 변경 (Default: 127.0.0.1:3306/fosslight)
```
--spring.datasource.url=<IP>:<Port>/<Database>
--spring.datasource.username=<USER_NAME>
--spring.datasource.password=<PASSWORD>
```
- log 파일 경로 지정
```
--logging.path=<LOG_PATH>
```

