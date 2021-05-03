---
sort: 1
published: true
---
# Developer Documentation
```note
FOSSLight 소스를 다운로드받아 직접 컴파일하여 실행할 수 있습니다. 
```

## 설치
### 요구사항
- JAVA 1.8 이상
- MariaDB 10.0 이상 또는 MySql 5.6 이상

### 개발 환경
- Framework : Spring Boot 2.1.x
- Build Tool : Gradle 6.x
- Git : [https://github.com/fosslight/fosslight][src]
- IDE : [Spring Tool Suite][spring]
    - [lombock][lb] 설치 필요
- Project Character Set: UTF-8

### 다운로드 & 설치
1. JAVA를 설치합니다.: [https://openjdk.java.net][java]
2. DDL : [https://github.com/fosslight/fosslight/blob/main/install/db/fosslight_create.sql][sql]
3. MariaDB 또는 Mysql 설치합니다. : [https://mariadb.org/download][maria]
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
[Spring Tool Suite][spring]를 다운로드합니다.  

#### Project Import
※ STS (Spring Tool suite) 4.x 기준
1. lombok 설치: [https://projectlombok.org/setup/eclipse][lb]
2. File > Import > Gradle > Existing Gradle Project
3. [Git Source Directory][git_repo]를 설정하고 Import 합니다.
4. Project > Properties > Resource > Text file encoding에서 UTF-8로 설정합니다.

[spring]: https://spring.io/tools
[lb]: https://projectlombok.org/setup/eclipse
[src]: https://github.com/fosslight/fosslight
[sql]: https://github.com/fosslight/fosslight/blob/main/install/db/fosslight_create.sql
[maria]: https://mariadb.org/download/
[java]: https://openjdk.java.net
[git_repo]: https://github.com/fosslight/fosslight

## 실행
### 실행 옵션 변경
[application.properties][props] 파일에서 실행 옵션 변경
 - server.port=8180: 웹서버 포트 (8180으로 설정한 경우 [http://localhost:8180][local])
 - spring.datasource.url=127.0.0.1:3306/fosslight: FOSSLight Database가 설치되어 있는 DB 서버의 IP, Port, Database Name을 설정
 - spring.datasource.username=fosslight: Database 접속자명을 설정
 - spring.datasource.password=fosslight: Database 접속자 패스워드 설정
 - logging.path=./logs : 로그파일 출력 경로 설정 ( Default "./logs" 는 Application 실행 위치를 의미)
 - logging.file=fosslight : 로그를 출력할 로그파일명 (logback-spring.xml 파일 참고)
 - root.dir=./data: 파일 업/다운로드 최상위 경로를 의미

[props]: https://github.com/fosslight/fosslight/blob/main/src/main/resources/application.properties

### Build & Run
하기 두 가지 방법으로 빌드 및 실행할 수 있습니다. 
1. Gradle build & Run
    - build (war 파일 생성)
    ```
    $ gradlew build
    ```
    - run
    ```
    $ gradlew bootRun
    ```
    - build & run - 방법 2 (빌드 후 어플리케이션 실행)
    ```
    $ gradlew clean build && java -jar build/libs/FOSSLight-1.0.0.war
    ```

2. IDE 에서 직접 실행
    - Boot Dashboard > local > FOSSLight 선택, 우클릭 start (Crtl + Alt + Shift + B, R)

### 동작 확인
- 웹브라우저에서 [http://localhost:8180][local]으로 접속하면 로그인 화면이 표시됩니다.
- 초기 로그인 계정은 id: admin, pswd :admin 입니다.

[local]: http://localhost:8180
