---
sort: 5
published: true
---
# Security
```note
Security 탭에서는 Identification단계의 BOM 탭 기준 vulnerability score가 기준 점수 이상인 OSS에 대하여 CVE ID별로 확인 및 조치 상태를 관리할 수 있습니다.
- vulnerability score 기준 점수는 Code Management > 760 (Security Vulnerability Score)에서 설정하실 수 있습니다.
```

## Column 정보
- OSS Name, OSS version
    - Identification단계의 BOM 탭에 작성된 OSS 정보가 자동 출력됩니다.
- CVE ID, CVSS Score, Published Date
    - CVE ID 및 해당 CVE ID의 score, 발행일 정보가 자동 출력됩니다. 
- Vulnerability Resolution
    - 기본값으로 Unresolved로 설정되며, 보안취약점 해결 시 Fixed로 변경하실 수 있습니다. 

### OSS version 미입력시
- Security 탭에서는 OSS version 미기입된 CVE ID에 대해 정확한 vulnerability 확인이 어렵기에 전체 CVE ID 리스트를 보여주지 않고 있습니다.
- 탭 진입 시 다음 팝업 화면이 뜨는 경우, OSS version 미기입된 OSS 목록 확인하셔서 Identification 탭에서 정확히 해당 OSS에 대해 사용된 OSS version 입력하신 후 BOM 탭 save and merge 해주시면,
Security탭에서 기입된 OSS version에 대한 보안취약점을 확인하실 수 있습니다.  
![prj](../images/4_project_security1.png)

## Vulnerability Resolution 여부 Identification 단계 반영
Identification 단계의 SRC, BIN, BOM 탭에서 vulnerability score 확인 시, Security 탭에서 vulnerability resolution 값을 'Fixed'로 변경한 CVE ID에 대해서는 제외된 Max score를 확인할 수 있습니다.
SRC, BIN, BOM 탭에서 vulnerabilty icon 클릭 시, 해당 OSS name 및 version에 대한 전체 CVE ID 리스트 창에서 'Fixed'된 CVE ID는 아래와 같이 비활성화 처리된 것을 확인할 수 있습니다.  
![prj](../images/4_project_security2.png)

