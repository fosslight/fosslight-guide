# Maintenance
```note
FOSSLight Hub를 운영하는 데 유용한 가이드입니다.
```

## DB 백업 및 복구하기
### 1. 백업
#### 선택1. 전체 백업    
mysqldump -u[아이디] -p[패스워드] [데이터베이스명] > [백업파일명].sql
```
$ mysqldump -ufosslight -pfosslight fosslight > fosslight_backup.sql
```

#### 선택2. FOSSLight 최신 버전으로 업데이트를 위한 DB 백업 (Data만 추출)
mysqldump -u[아이디] -p[패스워드] [데이터베이스명] --no-create-info > [백업파일명].sql
```
$ mysqldump -ufosslight -pfosslight fosslight --no-create-info > fosslight_backup.sql
```

### 2. 복구
1. 버전에 따른 Table 구조를 반영하기 위해 빈 DB를 새로 만들고 기본 값을 설정합니다. 
[Developer Documentation - 다운로드 & 설치 - 4. Database 생성 및 Data 초기 등록](https://fosslight.org/fosslight-guide/features/1_developer.html#다운로드--설치)

2. 백업한 파일로 복구합니다.
mysql -u[아이디] -p[패스워드] [데이터베이스명] < [백업파일명].sql
```
$ mysql -ufosslight -pfosslight fosslight < fosslight_backup.sql
```

### 추천 DBMS
- [DBeaver Community edition](https://dbeaver.io/download/) 
- [MySQL workbench Community edition](https://dev.mysql.com/downloads/workbench/)
- [HeidiSQL](https://www.heidisql.com/download.php)

## NVD Data를 2002년 Data부터 다운로드 받기
FOSSLight Hub는 일 1회 NVD(NATIONAL VULNERABILITY DATABASE) 에서 제공되는 [NVD Data Feeds](https://nvd.nist.gov/vuln/data-feeds)를 다운로드하여 Database에 저장하며 저장된 NVD Data는 [Vulnerability List](../started/2_try/7_vulnerability.md)에서 조회할 수 있습니다.      
이 때, 2002년 Data부터 NVD Data를 다운로드 받을 경우 하기와 같이 세팅합니다.     
(최초 1회만 세팅하면 이후 Data는 누적되므로 추가적으로 세팅할 필요가 없습니다.)   
    
        
**DB에서 설정값 변경**    
```
UPDATE T2_CODE_DTL SET CD_DTL_NM = 'Y' WHERE CD_NO = '990' AND CD_DTL_NO = '100';
```
NVD Data Feed initialize flag Code의 Default 값은 "N" 으로 설정되어 있으며, 위와 같이 직접 "Y"로 변경하면 다음 NVD 스케줄 동작 시 모든 NVD Data를 Clean하고 2002년 Data 파일 부터 순차적으로 등록 처리함니다.    
해당 값은 NVD Data 초기화 수행 시 에러 여부와 상관 없이 Default 값 ("N") 으로 변경됩니다.
