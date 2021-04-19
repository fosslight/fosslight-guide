---
sort: 2
published: true
---
# Try FOSSLight System
```note
FOSSLight 메뉴별 기능을 설명합니다.
```
## Sign In & Sign Up

### Sign In
![SignIn](images/sign_in.png)
- 처음 접속하는 경우 Sign Up 버튼을 클릭하여 계정을 등록합니다.

### Sign Up 
![SignUp](images/sign_up.png)  
- FOSSLight에 처음 접속하는 경우 계정을 등록합니다.

## OSS List
등록된 OSS(Open Source Software) 정보를 확인할 수 있습니다.
![OssList](images/oss_list.png)  

### ID : 
- OSS를 식별하는 숫자입니다.
- OSS의 버전이 여러 개 등록된 경우 '+'가 표시되며 최상위 버전이 표시됩니다. '+' 버튼을 클릭하면 하위 버전의 Open Source 정보를 확인할 수 있습니다.

### OSS Type : 
- M : Multi License로 하나의 OSS에 여러 License의 Source Code가 포함된 경우입니다.
- D : Dual License로 여러 개의 OSS  License 중 하나를 선택할 수 있습니다.
- V : Version different License로 버전 별로 License가 다른 경우입니다.

### OSS Name :
- Nick 표시된 OSS는 하나의 OSS가 여러 개의 Name을 갖고 있습니다.    
    예) "bison"의 Nick name은 "Bison parser", "GNU bison" 로 모두 같은 OSS를 표현하고 있습니다.
      
### Version : 
- OSS 버전을 의미합니다.

### License Name :
- OSS의 License 정보를 알 수 있습니다.  
- Multi License는 OSS에 포함되는 모든 License가 AND로 표시됩니다.
- Dual License는 OSS의 License를 복수 개 중 선택할 수 있고 OR로 표시됩니다.

### License Type
- Permissive : BSD-like 또는 BSD-style License로 불리며 Software 재배포 방법 관련 최소한의 요구사항이 있는 License입니다. 통상적으로 Copyright Notice 와 보증부인 문구를 유지할 것을 요구합니다.
- Weak Copyleft : 파생저작물에 동일한 권리가 유지된다는 조건으로 저작물의 복사본과 수정된 버전을 자유롭게 배포할 수 있습니다.
      저작물의 복사본과 수정본의 Source Code를 공개해야 합니다.
- Copyleft : 파생저작물에 동일한 권리가 유지된다는 조건으로 저작물의 복사본과 수정된 버전을 자유롭게 배포할 수 있습니다. 저작물의 복사본과 수정본뿐만 아니라 이와 link되거나 함께 동작하는 프로그램 전체 Source Code를 공개해야 합니다.
- Proprietary : 3rd Party가 Open Source를 사용하지 않고 자체 개발한 Software로 해당 3rd Party와 계약된 경우에만 사용 가능합니다.
- Proprietary Free : 3rd Party가 Open Source를 사용하지 않고 자체 개발한 Software로 추가적인 계약을 필요로 하진 않지만 일부 제약된 형태로만 사용 가능합니다.

### Obligation :
- Notice Obligation : 고지 의무가 있습니다.
- Source Code Obligation : Source Code 공개 의무가 있습니다. 

### Download Location :
- Open Source를 다운로드 받을 수 있는 URL이 Link로 표시되며, 클릭 시 해당 사이트로 이동하거나 파일을 다운로드 받을 수 있습니다.

### Hoempage :
- Open Source 공식 Site가 있으면, 로 표시되며 클릭 시 해당 사이트로  이동합니다.
- 아이콘에 마우스 오버 시 상세주소를 확인할 수 있습니다.

### Description :
- Open Source 사용 시 주의 사항을 확인할 수 있습니다.

### Vulnerability :
- NIST에서 제공하는 CVE DB에서 해당 OSS가 검색되면 취약 정도 (CVE Score)에 따라 Vulnerability 아이콘 색깔로 구분되어 표시됩니다.

## License List
등록된 License 정보를 확인할 수 있습니다.
![LicenseList](images/license_list.png)  

### License Name
- License Full name으로 SPDX (https://spdx.org/licenses/) 표기 방식을 따르고 있습니다.
- License Name column의 값을 클릭하면, License 별 상세정보를 확인할 수 있습니다.

### Identifier
- standardized short identifier로 License를 더욱 쉽게 식별할 수 있으며 SPDX (https://spdx.org/licenses/) 표기 방식을 따르고 있습니다.

### License Type :
- Permissive : 통상적으로 Copyright Notice와 보증부인 문구를 유지할 것을 요구합니다.
- Weak Copyleft :파생저작물에 동일한 권리가 유지된다는 조건으로 저작물의 복사본과 수정된 버전을 자유롭게 배포할 수 있습니다. 저작물의 복사본과 수정본의 Source Code를 공개해야 합니다.
- Copyleft : 파생저작물에 동일한 권리가 유지된다는 조건으로 저작물의 복사본과 수정된 버전을 자유롭게 배포할 수 있습니다. 저작물의 복사본과 수정본뿐만 아니라 이와 link 되거나 함께 동작하는 프로그램 전체 Source Code를 공개해야 합니다.
- Proprietary :Software 권리자의 허락 없이 사용이 불가능하므로 반드시 source code 사용 여부에 대한 계약 관계를 확인하고 사용하시기 바랍니다.
- Proprietary Free : 추가적인 계약이 필요하지는 않지만 제약된 형태, 특정 이용 약관 또는 조건에서 사용할 수 있습니다.

### Restriction :
- Non-Commercial Use Only : Software의 상업적 사용 및 배포를 금지합니다.
- Network Redistribution : Network 상으로 이용하도록 Service를 제공하는 것만으로도 배포로 간주하여 Open Source 의무 사항을 이행해야 합니다.
- Restricted Modifications : Software의 수정된 버전을 배포할 수 없습니다. 즉 Source code를 수정하지 않고 사용해야 합니다.
- Platform Deployment Restriction : 운영체제, 기술, 사용된 기술 분야, Device Type 등에 따라 Software의 배포를 제한합니다.
- Prohibited Purpose : Software를 특정 목적(분야)을 위하여 사용할 수 없습니다.
- Specification Constraints : 특정 Specification 또는 Standard와 관련하여 Software를 사용해야 합니다.
- Restricted Redistribution : 재배포할 수 있는 Software의 하위 구성 요소(Source Code, Binary file 등)를 제한합니다.
- Common Clause Restriction :Product의 전체 또는 상당 부분이 Common Clause License인 Software으로 부터 가치를 창출하는 경우, 판매 불가합니다. 

### Obligation :
- Notice Obligation : 고지 의무가 있습니다.
- Source Code Obligation : Source Code 공개 의무가 있습니다. 

### Web site :
- License 원문의 web site 정보를 제공합니다. URL 클릭 시 해당 사이트로 이동합니다.

### User Guide :
- License 사용 시 주의 사항을 알 수 있습니다.

## Project
### Basic Information
### Identification
### Packaging
## Self-check
