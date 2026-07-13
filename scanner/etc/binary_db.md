---
published: true
---

# FOSSLight Database 연동 가이드
FOSSLight Scanner와 FOSSLight Source Scanner에서 FOSSLight Database에 접속하는 방법과 FOSSLight Binary Scanner에서 별도로 DB를 세팅하는 방법을 안내합니다.

## Scanner Database 연결
OSS Information (OSS Name, OSS Version, License)를 DB로부터 출력하려면 DB 접속 정보가 필요합니다.

### Linux / macOS
현재 터미널 세션에만 적용:
````
export KB_URL="https://kb.example.org/"
export KB_TOKEN="example-token-1234567890abcdef"
````

쉘 시작 파일에 추가하여 계속 사용:
````
echo 'export KB_URL="https://kb.example.org/"' >> ~/.bashrc
echo 'export KB_TOKEN="example-token-1234567890abcdef"' >> ~/.bashrc
source ~/.bashrc
````

zsh 사용자는 `~/.zshrc`에 추가합니다.

### Windows Command Prompt
현재 세션에만 적용:
````
set KB_URL=https://kb.example.org/
set KB_TOKEN=example-token-1234567890abcdef
````

사용자 환경변수로 저장:
````
setx KB_URL "https://kb.example.org/"
setx KB_TOKEN "example-token-1234567890abcdef"
````

### Windows PowerShell
현재 세션에만 적용:
````
$env:KB_URL = "https://kb.example.org/"
$env:KB_TOKEN = "example-token-1234567890abcdef"
````

사용자 환경변수로 저장:
````
[System.Environment]::SetEnvironmentVariable("KB_URL", "https://kb.example.org/", "User")
[System.Environment]::SetEnvironmentVariable("KB_TOKEN", "example-token-1234567890abcdef", "User")
````

## Binary Scanner Database 세팅
FOSSLight Binary Scanner에서 OSS Information (OSS Name, OSS Version, License)를 DB로부터 출력하기 위해 별도의 DB를 세팅하는 방법입니다.

## Prerequisite
1. [PostgreSQL][PostgreSQL]를 설치합니다.
2. 원격으로 접속하기 위해 configuration file을 수정하는 방법 : [reference link][ref_link]

[PostgreSQL]: https://www.postgresql.org/download/    
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


## FOSSLight Binary 실행시, DB와 연동하는 방법
ex)
````
fosslight_binary -p path_to_analyze -d postgresql://username:password@host:port/database_name
````
