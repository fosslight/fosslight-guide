---
sort: 4
published: true
---
# Project
```note
Open Source Software를 포함하는 Software의 개발 및 배포를 위해 수행해야 하는 Process를 순차적으로 수행합니다.  
![prj_status](../images/4_project_process.png)
```

## Project List
![ProjectList](../images/project_list.png)  
Project를 검색하고, 해당 Project의 전체적인 정보를 확인하고 FOSSLight Report, OSS Notice, OSS Package를 다운로드할 수 있습니다.

- Search : Project의 기본 정보, Status, License, OSS Name 등으로 Project를 검색할 수 있습니다.
- Project Name (Version) : Row를 더블 클릭하면 Project 상세 화면으로 이동합니다.
- Status : Project의 상태 정보를 표시합니다.
- Identification, Packaging : 각 항목을 클릭하면 Identification, Packaging 상세 항목으로 이동합니다.
- Download : 각 아이콘을 클릭하면 파일을 다운로드 받을 수 있습니다.
    - FOSSLight Report: Identification에서 입력한 목록을 FOSSLight Report 형식으로 다운로드할 수 있습니다.
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
|Review|Identification 또는 Packaging 단계에서 Reviewer가 Review 중인 상태입니다. 이 때, Creator는 Identification 또는 Packaging의 정보를 수정할 수 없습니다. 수정이 필요한 경우 [Comment](#comment)를 남겨 Reviewer에게 Reject을 요청합니다. |
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
- Project List의 Identification column 내 버튼을 클릭하여 진입합니다.

<iframe width="560" height="315" src="https://www.youtube.com/embed/zzopYiY2UOA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

#### 2-1. 3rd Party Tab
![prj](../images/4_project_3rd.png)
*3rd Party 탭 작성 Process*
- 3rd Party Software가 포함된 경우 : 사전에 리뷰 완료된 3rd Party를 Load → Save
- 3rd Party Software가 포함되지 않은 경우 : Not Applicable 체크 → Save

*리뷰 완료된 3rd Party를 Load하는 방법*  
하기 방법 중 선택하여 3rd Party Software 정보를 불러올 수 있습니다. 
(💁 3rd Pary는 여러개 불러올 수 있습니다.)
1. 3rd Party Search : 3rd Party List 메뉴에서 Status: confirm인 3rd Party Software를 검색하고 load합니다.
2. Project Search : 다른 Project의 3rd Party 탭을 load합니다.

#### 2-2. SRC Tab
![prj](../images/4_project_src.png)
*SRC 탭 작성 Process*
- Source code별 OSS가 포함된 경우: Source code별 OSS 정보를 작성 -> Save
- Source code별 OSS 분석 대상이 아닌 경우 : Not Applicable 체크 → Save

*Source code별 OSS 정보 작성 방법* 
- OSS Table에 수기로 작성
    - OSS Table의 좌측 상단 + 버튼을 클릭하여 OSS 정보를 기입합니다.
- OSS 정보 일괄 Load 하는 방법
    1. Upload Analysis Result 란에 OSS List를 작성한 FOSSLight Report를 업로드합니다. 
        - Load 가능한 FOSSLight Report 양식은 우측 상단 "Export"버튼을 클릭하면 다운로드 가능합니다. 
    2. Project Search : 다른 Project의 SRC 탭을 Load합니다.

#### 2-3. BIN Tab
![prj](../images/4_project_bin.png)
*BIN 탭 작성 Process*
- Binary가 포함된 경우 : Binary별 OSS 정보를 작성 -> Save
- Binary가 포함되지 않는 경우 : Not Applicable 체크 → Save

*Binary별 OSS 정보 작성 방법*  
- OSS Table에 수기로 작성
    - OSS Table의 좌측 상단 + 버튼을 클릭하여 OSS 정보를 기입합니다.
- OSS 정보 일괄 Load 하는 방법
    1. Upload Analysis Result 란에 OSS List를 작성한 FOSSLight Report를 업로드합니다. 
        - Load 가능한 FOSSLight Report 양식은 우측 상단 "Export"버튼을 클릭하면 다운로드 가능합니다. 
    2. Project Search : 다른 Project의 BIN 탭을 Load합니다.

#### 2-1. BOM Tab
3rd Party, SRC, BIN 탭에 작성된 OSS 목록을 취합하고 리뷰 요청을 합니다.
![prj](../images/4_project_bom.png)

##### Review 요청 방법
1. Merge And Save 버튼을 클릭합니다.
    - 3rd Party, SRC, BIN 탭에 작성한 OSS List를 취합합니다.
2. [Warning message별 검토 사항](#warning) 검토 사항을 확인합니다.
3. Request Review 버튼을 클릭하여 리뷰 요청을 합니다.
    - 단, 빨간색 Warning Message가 있을 경우 리뷰 요청이 불가합니다.

##### (Admin only) Review 방법
1. BOM 탭 우측 상단 Review Start 버튼을 클릭합니다.
2. [Warning message별 검토 사항](#warning) 검토 사항을 확인합니다.
3. Merge And Save 클릭 후 Confirm을 클릭하면 Packaging 탭이 활성화됩니다. 
    - Creator에게 재확인이 필요한 경우 Reject을 클릭하여 Status를 Progress로 변경합니다.

### 3. Packaging
```note
- Packaging 단계에서는 Source Code 공개 의무가 있는 Open Source를 사용한 경우 공개할 Source Code를 취합(OSS Package)하고 이를 FOSSLight에 등록합니다.
- OSS 고지문은 Packaging 단계가 Confirm되면 자동으로 생성됩니다. 만약, OSS 고지문 내용을 변경해야 할 경우, Notice tab에서 수정할 수 있습니다.
- Project List의 Packaging column 내 버튼을 클릭하여 진입합니다.
```

<iframe width="560" height="315" src="https://www.youtube.com/embed/66uWu4qxOog" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

#### 3-1. Packaging Tab
![prj](../images/4_project_pkg.png)
Packaging tab에서는 OSS Package 파일을 Upload하고 이를 Verify합니다. (단, Source Code 공개를 필요로하는 License하의 Open Source를 사용하지 않았다면 이 탭은 비활성화됩니다. )
1. OSS Package Upload  
    - Source code를 취합한 Packaging 파일(압축 파일)을 Upload합니다.
2. "Path of source code in the OSS Package" column을 기입합니다.
    - 공개해야 할 Open Source 종류가 많아 Path 기입을 일일이 하기 어려운 경우 'Export Path'버튼으로 Packaging OSS List 파일을 다운로드 한 후 Path를 기입하고 'Upload Path'버튼으로 upload 하면 Path 정보가 등록됩니다.
    - 'Save' 버튼으로 입력한 Path정보를 저장할 수 있습니다.
    - Path정보는 대소문자를 구분하니 입력 시 주의하시기 바랍니다.
3. 'Verify'버튼을 클릭하여 확인 과정을 수행합니다.
    - Verify 후 OSS Package 내에서 찾은 File은 File Count란에 개수가 표시됩니다. 찾지 못한 Open Source가 있다면 "path not found"라고 표시됩니다.
    - OSS Package 내에서 찾은 README, File List, Banned List를 확인할 수 있습니다.
        - README : OSS Package 내 포함된 README 파일
        - File List : OSS Package 내의 파일 목록
        - Banned List : "Proprietary", "Commercial" 등 공개되지 말아야 할 파일 목록


#### 3-2. Notice Tab
![prj](../images/4_project_notice.png)
OSS Notice는 Identification > BOM 탭을 기준으로 자동 생성됩니다. 이 때, 발행하는 OSS Notice의 포맷이나 Contents를 수정할 수 있습니다.

#### 3-3. Review 요청
- Packaging 탭 우측 상단 Request Review 버튼을 클릭하여 리뷰 요청을 합니다.

#### 3-4. (Admin only) Review 방법
- Packaging 탭 우측 상단 Review Start 버튼을 클릭합니다.
- 우측 상단의 Confirm을 클릭하면 Packaging이 Confirm되고 OSC Process가 완료됩니다. 
- Packaging이 Confirm된 Project에 대해서 Project List에서 발행된 OSS Notice를 다운로드 받을 수 있습니다.
    - Creator에게 재확인이 필요한 경우 Reject을 클릭하여 Status를 Progress로 변경합니다.


## ⭐Tips for Project
### Check OSS Name 버튼 (SRC, BIN Tab)
OSS Table에 작성된 Download location을 기반으로 FOSSLight에 저장된 OSS Name으로 자동 변경합니다.
- 팝업에 자동 변환될 OSS 목록이 표시됩니다.
    - Change OSS Name 버튼 : 체크된 Row에 대하여 OSS Table의 OSS Name이 변경됩니다. 
    - (Admin Only) Add Nickname 버튼 : 체크된 Row에 대하여 FOSSLight에 저장된 OSS에 Nickname으로 OSS Table에 쓰여진 OSS Name이 추가됩니다.

### <a name="comment"></a> Comment 남기기
- 탭별 우측 상단의 Comment Edit 버튼을 클릭하면 Comment를 남기고 해당 Comment를 Reviewer, Watcher, Creator에게 메일로 발송할 수 있습니다.

### <a name="warning"></a> OSS Table's Warning message 
#### Warning message 색깔별 의미
- 빨간색 : 리뷰 요청 또는 Confirm이 불가합니다. 검토 후 수정이 필요합니다.
- 파란색 : 리뷰 요청 또는 Confirm 가능하지만, 검토가 필요한 사항입니다.
- 회색 : 정보 전달을 위한 message입니다.

#### Warning message에 따른 검토 사항

| Message  | Description |
| ------------- | ------------- |
|This field is required| 내용 입력이 필요합니다.|
|Unconfirmed open source|등록되지 않은 신규 OSS입니다.|
|Unconfirmed version|등록되지 않은 신규 버전입니다.|
|Unconfirmed license|등록되지 않은 신규 License 입니다.|
|Dual license: Select a license|Dual License임에도 모두 사용된 것으로 쓰여져 있습니다. Dual License인 경우, 사용할 License만 선택합니다.|
|Specify OSS Name or put 1 license in a row|OSS Name이 - 또는 공란이면서, 여러 License가 하나의 Row에 쓰여져 있습니다. OSS Name이 - 또는 공란인 경우, License별로 Row를 분리하여 작성하여주십시오.|
|The address should be started with www|주소 format이 맞지 않습니다.|
|Formatting error|줄바꿈 문자가 포함되어 있습니다. 여러 줄 작성이 필요한 경우, Row를 추가하여 작성하시기 바랍니다.|
|Not the same as property|입력한 URL이 FOSSLight에 등록된 해당 OSS의 URL과 다릅니다.|

