# Self-Check 생성하기
```note
Self-Check를 생성하고 OSS(Open Source Software)정보를 확인합니다.
1. Self-Check 생성
2. OSS 정보 작성
3. OSS 정보 확인
4. Export하여 파일로 다운로드
```

## 1. Self-Check  생성
1. Self-Check List에서 Add 버튼 클릭합니다.
2. Self-Check 정보를 입력합니다.  
    ![new](images/2_self_new.png)  
3. 생성된 Self-Check를 확인합니다. 
    ![new](images/2_self_new_list.png)

## 2. OSS 정보 작성
1. Self-Check List에서 생성한 Self-Check를 더블 클릭합니다.
2. Self-Check 상세 정보탭에서 OSS 정보를 작성합니다.  
    ![new](images/2_self_add.png)
    1. OSS Table 좌측 상단의 + 버튼을 클릭합니다.
    2. 추가된 Row에 정보 (OSS Name, OSS Version, License)를 입력합니다.  
        - OSS Name, OSS Version를 입력하면 FOSSLight에 저장된 정보인 경우 하기 팝업이 뜹니다. 이 때, OK를 클릭하면 FOSSLight에 저장된 정보 (License, Download location)을 자동으로 불러옵니다.  
        ![new](images/2_self_auto.png)
    3. OSS Table 좌측 상단의 + 버튼을 클릭하여 Row를 추가할 수 있습니다.
    4. Save를 클릭합니다. 
3. Check OSS Name 버튼을 클릭합니다.
![new](images/2_self_check_ossname.png)
작성된 Download location을 기반으로 FOSSLight에 저장된 OSS Name으로 자동 변경합니다.
- 팝업에 자동 변환될 OSS 목록이 표시됩니다.
    - Change OSS Name 버튼 : 체크된 Row에 대하여 OSS Table의 OSS Name이 변경됩니다. 
    - Add Nickname 버튼 : 체크된 Row에 대하여 FOSSLight에 저장된 OSS에 Nickname으로 OSS Table에 쓰여진 OSS Name이 추가됩니다.

## 3. OSS 정보 확인
![new](images/2_self_save.png)
- OSS Detail 아이콘 클릭 : 해당 OSS의 버전별 License, Copyright 등 세부정보 팝업창을 확인합니다.  
    ![new](images/2_self_oss.png)
- License Detail 아이콘 클릭: License의 정보와 License Text가 팝업창으로 제공됩니다.  
    ![new](images/2_self_lic.png)
- User Guide 아이콘 클릭: 작성된 License에 대한 User Guide를 팝업창으로 확인합니다.  
    ![new](images/2_self_lic2.png) 

## 4. Export하여 파일로 다운로드
![new](images/2_self_export.png)
- Self-Check Sheet : OSS Table에 쓰여진 사항을 출력합니다. 이 Sheet를 [Project](../started/2_try/4_project.md)의 Identification 탭에 업로드할 수 있습니다. 
- Vulnerability Sheet : OSS별 Vulnerability 정보를 출력합니다.
    - OSS Name : OSS Table에 작성한 OSS Name
    - Nick Name : OSS Table에 작성한 OSS의 nickname으로 Vulnerability가 조회된 경우, 매칭된 nickname이 표시됩니다. (매칭된 nickname이 없는 경우 -로 표시)
    - OSS Version : Vulnerability 조회된 version
        - OSS 버전이 공란인 경우, Vulnerability 에 존재하는 모든 버전에 대하여 정보를 출력합니다. 
        - OSS Version이 설정되어 있는 경우 해당 Version의 하위 버전은 CSV 에 포함되지 않습니다. (상위 버전은 모두 포함)
    - Max Score : 해당 OSS, Version에 대한 Vulnerability Max Score
    - Vulnerability Link : 해당 OSS Name, OSS Version으로 조회된 Vulnerability 목록을 확인할 수 있는 팝업 링크를 출력합니다.