---
sort: 4
published: true
---
# Project
```note
Open Source Software를 포함하는 Software의 개발 및 배포를 위해 수행해야 하는 Process를 수행합니다.  
![prj_status](../images/4_project_process.png)
```

## Project List
![ProjectList](../images/project_list.png)  
Project를 검색하고, 해당 Project의 전체적인 정보를 확인하고 OSS Report, OSS Notice, OSS Package를 다운로드할 수 있습니다.

- Search : Project의 기본 정보, Status, License, OSS Name 등으로 Project를 검색할 수 있습니다.
- Project Name (Version) : Row를 더블 클릭하면 Project 상세 화면으로 이동합니다.
- Status : Project의 상태 정보를 표시합니다.
- Identification, Packaging : 각 항목을 클릭하면 Identification, Packaging 상세 항목으로 이동합니다.
- Download : 각 아이콘을 클릭하면 파일을 다운로드 받을 수 있습니다.
    - OSS Report: Identification에서 입력한 목록을 OSS Report 형식으로 다운로드할 수 있습니다.
    - OSS Notice: Packaging 단계가 완료된 경우 표시되며 발행된 OSS Notice를 다운로드할 수 있습니다.
    - Packaging file: Packaging에서 공개할 Source Code가 업로드된 경우 표시되며 Packaging 파일을 다운로드할 수 있습니다.
- Vulnerability : Project의 Identification에 포함된 전체 Open Source List의(Exclude 제외) Vulnerability 정보 중 가장 높은 Critical Level을 표시합니다.
    - Critical (Critical Score 9.0 ~ 10.0)
    - High (Critical Score 7.0 ~ 8.9)
    - Medium (Critical Score 4.0 ~ 6.9)
    - Low (Critical Score 0.1 ~ 3.9)

### Project의 Status
![prj_status](../images/4_project_status.png)
| Status  | Description |
| ------------- | ------------- |
|Progress|	Creator가 작업하고 있는 상태입니다.	|
|Request|Identification 또는 Packaging 단계에서 Creator가 Reviewer에게 Review를 요청한 상태입니다. 해당 탭의 우측 상단 reject 버튼을 통하여 Progress 상태로 변경할 수 있습니다.	|
|Review|Identification 또는 Packaging 단계에서 Reviewer가 Review 중인 상태입니다. 이 때, Creator는 Identification 또는 Packaging의 정보를 수정할 수 없습니다. 수정이 필요한 경우 Comment를 남겨 Reviewer에게 Reject을 요청합니다. |
|Complete|Project Review가 완료된 상태를 의미합니다. Creator는 Identification 또는 Packaging의 정보를 수정할 수 없습니다. 수정이 필요한 경우 Project Basic Information탭에서 Request to Open을 클릭합니다.	|
| Drop|더 이상 Project의 OSC Process를 진행하지 않는 상태를 의미합니다. Status: Complete가 아닌 경우, Drop 설정을 할 수 있으며, 필요시에는 Open을 클릭하여 직접 Open할 수 있습니다.	|

## Project의 Process

### 1. Create a Project
배포하는 Software에 대하여 Project를 생성합니다.
1. Project List에서 Add 버튼을 클릭합니다.
2. New_Project 탭에서 Project 관련 정보를 입력합니다.
3. 우측 하단의 Save 버튼을 클릭합니다.

#### Basic Information탭
Project에 대한 기본 정보를 수정하거나 Status를 변경하는 탭입니다. 
![prj_basic](../images/4_project_bi.png)
Project List에서 Project Name을 더블 클릭합니다. 
- Delete : Project를 삭제합니다. 
- Drop : Project의 Status를 Drop으로 변경합니다. 다시 Process를 진행하기 위해서는 Open 버튼을 클릭해야 합니다. 
- Copy : Project를 복사하여 새로운 Project를 생성합니다. 
- Save : 기본 정보를 수정한 후에는 클릭해야 저장됩니다. 
- Open : Status가 Drop인 경우 표시되며 클릭하면 Status를 Progress로 변경합니다. 
- Request to Open : Status가 Complete인 Project인 경우 표시되며 Status를 Progress로 변경하여 Process를 재수행할 수 있습니다. 
- (Admin Only)
    - Complete : 모든 Process가 완료된 Project에 대하여 Status를 변경합니다. 
    - Open : Status가 Complete 또는 Drop인 경우 표시되며 Status를 Progress로 변경합니다. 

### 2. Identification
배포하는 Project에 대하여 Open Source Software 분석 결과를 작성합니다.
![Identification](../images/identification.png)
- Identification 작성 Process : 1. 

#### 2-1. 3rd Party Tab
1. 3rd Party 탭 작성 Process
3rd Party Project Load -> Warning message 검토 -> Save

#### 2-2. SRC Tab
1. SRC 탭 작성 Process
Open Source Software 분석 결과 작성 -> Warning message 검토 -> Save

#### 2-3. BIN Tab
1. BIN 탭 작성 Process
Open Source Software 분석 결과 작성 -> Warning message 검토 -> Save

#### 2-1. BOM Tab

### 3. Packaging

#### 3-1. Packaging Tab

#### 3-2. Notice Tab
OSS Notice는 Identification > BOM 탭을 기준으로 자동 생성됩니다. 이 때, 발행하는 OSS Notice의 포맷이나 Contents를 수정할 수 있습니다.