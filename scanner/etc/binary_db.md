---
published: true
---

# FOSSLight Binary Scanner Database 세팅 방법
OSS Information (OSS Name, OSS Version, License)를 DB로부터 출력하기 위해 DB 세팅이 필요합니다. 

## Prerequisite
1. [PostgreSQL][PostgreSQL]를 설치합니다.
2. 원격으로 접속하기 위해 configuration file을 수정하는 방법 : [reference link][ref_link]

[PostgreSQL]: https://github.com/fosslight/fosslight_binary/blob/main/LICENSE
[ref_link]: https://www.cyberciti.biz/tips/postgres-allow-remote-access-tcp-connection.html


## How to create a database and a table
1. User와 Database를 생성합니다.
````
$ sudo -i -u postgres 
$ psql
postgres=# CREATE USER bin_analysis_script_user PASSWORD 'script_123' ;
postgres=# CREATE DATABASE bat OWNER bin_analysis_script_user ENCODING 'utf-8';
````

2. [fosslight_create.sql][sql_link] 파일을 다운로드합니다.

3. Table을 생성합니다.
````
$ psql -U bin_analysis_script_user -d bat -f fosslight_create.sql
````

[sql_link]: https://github.com/fosslight/fosslight_binary_scanner/blob/main/db/initdb.d/fosslight_create.sql

### Table schema
<img alt="table" src="../images/table_schema.png">


## Example. 데이터 입력을 위한 쿼리
````
INSERT INTO public.lgematching (filename, pathname, checksum, tlshchecksum, ossname, ossversion, license, parentname, platformname, platformversion, updatedate, sourcepath) VALUES
('askalono.exe', 'third_party/askalono/askalono.exe', '3f5c6bbf06ddf53a46634bb21691ab0757f3b80c', 'T138267C12BB86A9EDC06AC470878646225B31B4CA0B25BFFF41C455743E6AAF45F3D39C', 'askalono', '', 'Apache-2.0', '[123]windows app project', 'windows', '10', '2021-02-19 17:21:52.430065', 'third_party/src/askalono')  
````   
- The checksum and tlshchecksum values are output to binary.txt when fosslight_binary is executed.


## FOSSLight Binary 실행시, DB와 연동하는 방법
- When calling fosslight_binary, write your DB information with the -d option.
ex)
````
fosslight_binary -p path_to_analyze -d postgresql://username:password@host:port/database_name
````
