---
sort: 1
published: true
---
# Developer Documentation
```note
FOSSLight System을 다운로드 받아 직접 설치하고 실행할 수 있습니다. 
```

## 설치
### 요구사항
- JAVA 1.8 이상
- MariaDB 10.0 이상 또는 MySql 5.6 이상

### 다운로드 & 설치
1. JAVA를 설치합니다.: https://www.java.com/ko/download/
2. DDL : https://github.com/fosslight/fosslight/blob/main/install/db/fosslight_create.sql 
3. MariaDB 또는 Mysql 설치합니다. : https://mariadb.org/download/
4. Database 생성 및 초기 Data 등록
```
mysql -u root -p < fosslight_create.sql
```
만약 Database가 이미 존재하거나 Database 이름을 변경하려면 상단의 create database 문과 USE 'fosslight' 문을 변경합니다.
```
mysql -u root -p <DATABASE_NAME> < fosslight_create.sql
```
접속 계정이 이미 존재하거나, 다른 계정을 사용하는 경우 CREATE USER 및 GRANT 부분을 삭제(또는 변경) 합니다.


### IDE Configuration
1. [Spring tool suite][spring]를 다운로드합니다.
2. Git Source를 연동합니다.

[spring]: https://spring.io/tools

### Project Import
※ STS (Spring Tool Shuit) 4.x 기준 설명입니다.
1. lombok 설치: https://projectlombok.org/setup/eclipse
2. File > Import > Gralde > Existing Gradle Project
3. Git Source Directory를 설정하고 Import 합니다.
4. Project > Properties > Resource > Text file encoding 에서 UTF-8 설정
5. application.properties 파일에서 실행 옵션 변경 (port, datasource, logging.path, root.dir 등)

## Build & Run
1. Project Directory에서 gradle build 또는 run 할 수 있습니다.
 - build (war 파일 생성)
```
$ gradlew build
```
 - run (직접 실행)
 ```
 $ gradlew bootRun
 ```
 - ex) 빌드 후 어플리케이션 실행
 ```
 $ gradlew clean build && java -jar build/libs/FOSSLight-0.0.2.war
 ```
 2. IDE 에서 직접 실행
  - Boot Dashboard > local > FOSSLight 선택, 우클릭 start (Crtl + Alt + Shift + B, R)
  