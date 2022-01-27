# Maintenance
```note
FOSSLight Hub를 운영하는 데 유용한 가이드입니다.
```

## DB 백업 및 복구하기
### DB 전체 백업
mysqldump -u[아이디] -p[패스워드] [데이터베이스명] > [백업파일명].sql
```
$ mysqldump -ufosslight -pfosslight fosslight > fosslight_backup.sql
```

### FOSSLight 최신 버전으로 업데이트를 위한 DB 백업 (Data만 추출)
1. DBMS를 다운로드 받습니다. (권장 DBMS : HeidiSQL https://github.com/HeidiSQL/HeidiSQL)
2. DB에 접속 후 '데이터베이스를 SQL로 내보내기'를 클릭합니다.
3. DELETE + INSERT로 데이터를 추출합니다. 
    ![config](./images/sql_backup.png)

### DB 복구
mysql -u[아이디] -p[패스워드] [데이터베이스명] < [백업파일명].sql
```
$ mysql -ufosslight -pfosslight fosslight < fosslight_backup.sql
```

