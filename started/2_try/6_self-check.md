---
sort: 6
published: true
---
# Self-Check
```note
Self-Check에서는 검토할 OSS에 대한 License, 보안 취약점 등의 정보를 리뷰 과정 없이 간편하게 확인할 수 있습니다.
```
<iframe width="560" height="315" src="https://www.youtube.com/embed/yfWvm9ZdtEE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Self-Check를 통해 확인할 수 있는 정보

Self-Check Project를 생성하고 검토할 OSS를 입력하면 아래 정보를 확인할 수 있습니다.
- OSS 상세 정보 : 등록된 Version, Version별 License, Copyright, Homepage, Download Location 등
- License 상세 정보 : License의 종류, 의무사항, 제한사항, License 전문 등
- User Guide : 해당 OSS 사용 시 주의사항 등
- Vulnerability : NVD(National Vulnerability Database)에서 제공하는 보안 취약점 정보

## Self-Check을 통한 확인 절차
Self-Check는 아래와 같은 절차를 통해 진행할 수 있습니다.

### 1. Self-Check Project 생성
1. Self-Check List 우측 상단의 Add 버튼을 클릭합니다.
2. 관련 정보를 입력하고 Save합니다.
3. Self-Check List에서 새로 생성한 Self-Check Project를 확인할 수 있고, List에서 더블클릭 시 상세 내용을 확인할 수 있습니다.

### 2. OSS 정보 입력
1. 개별 입력
    - +버튼을 클릭하여 행을 추가한 후 확인하고 싶은 OSS를 입력하고 Save합니다.
2. OSS 보고서를 이용한 일괄 추가
    1. Upload Analysis Result란에 OSS 리스트가 기재된 FOSSLight Report를 업로드합니다. 
        - 업로드 가능한 FOSSLight Report 양식은 Export 버튼을 클릭하여 다운로드 받을 수 있습니다.
    2. OSS List가 작성된 Sheet를 선택하고 OK 클릭합니다.  
    ![select_sheet](../images/6_self_select_sheet.png)
    3. Save 버튼을 클릭합니다. 
 
### 3. OSS 및 License 정보 확인
![oss_table](../images/6_self_oss_table.png)
#### Warning Messages
- Unconfirmed open source : FOSSLight Hub에 동일한 OSS Name이 등록되어 있지 않은 경우 표시됩니다.
- Unconfirmed version : FOSSLight Hub에 동일한 OSS Name은 있으나, 동일 Version이 등록되어 있지 않은 경우 표시됩니다.
- This field is required : License 정보가 기입되어있지 않을 경우에 표시됩니다. (Self-Check에서는 필수 항목이 아닙니다.)
- Non-included license : FOSSLight Hub에 동일 OSS Name, OSS Version이 등록되어 있으나, 기존 등록된 License와 다를 경우 표시됩니다.

#### OSS 및 License 정보
하기 Column의 아이콘을 클릭하면 등록된 OSS의 상세정보, License에 대한 상세정보, 그리고 해당 License에 대한 Guide가 제공됩니다.
단, 등록된 OSS라 할지라도 User Guide가 제공되지 않을 수 있습니다.
- OSS Detail : 등록된 OSS의 여러 Version, 각각의 License, Copyright 등 세부정보가 팝업창으로 제공됩니다.
- License Detail : 해당 OSS가 사용하는 License의 상세 정보와, License Text가 팝업창으로 제공됩니다.
- User Guide : 해당 License 사용 시 참고할 수 있는 정보들에 대한 링크가 제공됩니다.

#### OSS 사용에 따른 의무/제한 사항
❕ 상세 내용은 License List 에서 확인 가능합니다.  
- Obligation > Notify 아이콘: Copyright나 License (혹은 둘 다)에 대한 고지의 의무가 있음을 의미합니다.
- Obligation > Source 아이콘: Source Code 공개 의무가 있음을 의미합니다.
- Restriction 아이콘 : 해당 OSS를 사용하는데 제약사항이 존재함을 의미합니다.
(예 : 수정 제한, 상업적 사용 제한 등)

### 4. Vulnerability 정보 확인
```note
- Vulnerability 열에서 확인 : NIST에서 제공하는 CVE DB에서 해당 OSS가 검색되면 Vulnerability 아이콘이 CVSS Score에 따라 색깔로 구분되어 표시됩니다.
- Export 파일 (.xlsx)로 확인 : 기술된 전체 OSS의 리스트와 취약점 정보가 포함된 엑셀 파일이 다운로드 됩니다.
- Vulnerability 관련 상세 정보는 [Vulnerability](7_vulnerability.md) 에서 확인 가능합니다.
```
1. FOSSLight Hub UI에서 확인  
![self_pop](../images/6_self_pop.png)  
Vulnerability 아이콘을 클릭하면 해당 OSS Name, OSS Version의 취약점 정보가 팝업창으로 제공됩니다.

2. Export 파일로 확인
    - Self-Check Sheet   
    ![self_check_sheet](../images/6_self_sheet1.png)   
    사용자가 입력한 OSS 리스트가 OSS 보고서 양식에 준하여 기술됩니다.  
    이 탭의 정보는 추후 [Project](4_project.md)의 Identification에서 활용될 수 있습니다.  
    - Vulnerability Sheet  
    ![self_check_sheet2](../images/6_self_sheet2.png)  
    취약점 정보가 발견된 OSS의 입력한 버전과 상위 버전의 정보들이 기술됩니다.  
    이 때, Vulnerability Link를 클릭하면 해당 OSS Name, OSS Version의 CVE-ID를 확인 가능합니다.
