---
sort: 3
published: true
---
# License List
```note
등록된 License 정보를 확인하고, License를 추가, 수정, 삭제할 수 있습니다.
License List의 License Name Column의 cell을 클릭하면 상세정보를 확인할 수 있습니다. 
```
## License List
![LicenseList](../images/license_list.png)  

### License Name
- License Full name으로 SPDX (https://spdx.org/licenses/) 표기 방식을 따르고 있습니다.
- License Name column의 값을 클릭하면, License 별 상세정보를 확인할 수 있습니다.

### Identifier
- standardized short identifier로 License를 더욱 쉽게 식별할 수 있으며 SPDX (https://spdx.org/licenses/) 표기 방식을 따르고 있습니다.

### License Type 
- Permissive : 통상적으로 Copyright Notice와 보증부인 문구를 유지할 것을 요구합니다.
- Weak Copyleft :파생저작물에 동일한 권리가 유지된다는 조건으로 저작물의 복사본과 수정된 버전을 자유롭게 배포할 수 있습니다. 저작물의 복사본과 수정본의 Source Code를 공개해야 합니다.
- Copyleft : 파생저작물에 동일한 권리가 유지된다는 조건으로 저작물의 복사본과 수정된 버전을 자유롭게 배포할 수 있습니다. 저작물의 복사본과 수정본뿐만 아니라 이와 link 되거나 함께 동작하는 프로그램 전체 Source Code를 공개해야 합니다.
- Proprietary :Software 권리자의 허락 없이 사용이 불가능하므로 반드시 source code 사용 여부에 대한 계약 관계를 확인하고 사용하시기 바랍니다.
- Proprietary Free : 추가적인 계약이 필요하지는 않지만 제약된 형태, 특정 이용 약관 또는 조건에서 사용할 수 있습니다.

### Restriction 
- Non-Commercial Use Only : Software의 상업적 사용 및 배포를 금지합니다.
- Network Redistribution : Network 상으로 이용하도록 Service를 제공하는 것만으로도 배포로 간주하여 Open Source 의무 사항을 이행해야 합니다.
- Restricted Modifications : Software의 수정된 버전을 배포할 수 없습니다. 즉 Source code를 수정하지 않고 사용해야 합니다.
- Platform Deployment Restriction : 운영체제, 기술, 사용된 기술 분야, Device Type 등에 따라 Software의 배포를 제한합니다.
- Prohibited Purpose : Software를 특정 목적(분야)을 위하여 사용할 수 없습니다.
- Specification Constraints : 특정 Specification 또는 Standard와 관련하여 Software를 사용해야 합니다.
- Restricted Redistribution : 재배포할 수 있는 Software의 하위 구성 요소(Source Code, Binary file 등)를 제한합니다.
- Common Clause Restriction :Product의 전체 또는 상당 부분이 Common Clause License인 Software으로 부터 가치를 창출하는 경우, 판매 불가합니다. 

### Obligation
- Notice Obligation : 고지 의무가 있습니다.
- Source Code Obligation : Source Code 공개 의무가 있습니다. 

### Web site 
- License 원문의 web site 정보를 제공합니다. URL 클릭 시 해당 사이트로 이동합니다.

### User Guide 
- License 사용 시 주의 사항을 알 수 있습니다.

## (Admin Only) License 추가, 수정, 삭제
### License 추가
![NEW_OSS](../images/3_lic_new.png)  
1. License List에서 우측 상단 Add 버튼을 클릭합니다.
2. "New_License" 탭에서 신규 OSS의 정보를 입력합니다.
    - License Name, Nick Name은 중복될 수 없습니다. 
    - Obligation : 
        - Notice가 체크된 경우 OSS Notice에 포함됩니다. 
        - Source Code가 체크된 경우, Packaging탭에서 소스 코드 취합 OSS 목록으로 표시됩니다.
    - User Guide : 해당 OSS에 대한 정보를 입력합니다.
    - Attribution : OSS Notice 발행시 별도로 포함되어야 하는 문구를 기입합니다.
3. 우측 하단의 Save 버튼을 클릭합니다.

### License 수정
1. License List에서 수정할 License Name을 클릭합니다.
2. License 상세정보 탭에서 수정합니다.
3. 우측 하단의 Save 버튼을 클릭합니다.

### License 삭제
1. License List에서 삭제할 License Name을 클릭합니다.
2. License 상세정보 탭에서 Comment란에 삭제 사유를 기입합니다.
3. 좌측 하단의 Delete 버튼을 클릭합니다.