# REST API
```note
FOSSLight Hub의 기능을 REST API로 호출할 수 있습니다.
```

## 시작하기
### TOKEN 발행
REST API를 호출하기 위해서 TOKEN을 발행해야 합니다.
1. Admin 계정으로 로그인합니다.
2. System > User Management 탭에서 User별로 Token을 발행할 수 있습니다.

## REST API 테스트 도구

- [http://demo.fosslight.org/swagger-ui.html][swagger] 

[swagger]: http://demo.fosslight.org/swagger-ui.html

## REST API 종류 

1\. OSS & License 정보 조회

| API  | 응답 형식 | 설명 |
| ------------- | ------------- | ------------- |
|/api/v1/downloadlocation_search |	JSON|	Download Location으로 OSS 정보를 조회합니다.|
|/api/v1/license_search|	JSON|	License Name으로 License 정보를 조회합니다.|
|/api/v1/oss_search	|JSON|	OSS Name, Version으로 OSS 정보를 조회합니다. |


2\. 3rd Party 정보 조회

| API  | 응답 형식 | 설명 |
| ------------- | ------------- | ------------- |
|/api/v1/partner_search|	JSON	|보기 권한이 있는 3rd Party에 대하여 하기 정보를 조회합니다. |
|/api/v1/partner_watcher_add|-|3rd Party의 Watcher를 추가합니다.|


3\. Project 정보 조회, 생성, FOSSLight Report 등록, Packaging 파일 업로드, BOM Export, Project 비교

| API  | 응답 형식 | 설명 |
| ------------- | ------------- | ------------- |
|/api/v1/create_project|	JSON|	Project를 생성하고, 생성된 Project ID를 return 받습니다.|
|/api/v1/model_search| JOSN| Project에 대하여 Model List를 조회합니다. (최대 Return Item 수 : 1000) <br>  - Project ID, Category, Model, Name, Release Date|
|/api/v1/model_update|  JSON| Model 정보 문자열 목록을 통해 Project의 Model 정보를 업데이트합니다. <br>  - Model 정보 문자열 목록 (format . MODEL_NAME\|Category\|Release Date) <br>  - ex) MODEL_NAME\|ETC > Etc\|20220428|
|/api/v1/model_update_upload_file	|JSON| Model List 엑셀 파일을 통해 Project의 Model 정보를 업데이트합니다. <br>  - Model List의 엑셀 파일 : Project > Basic Information 탭 > Download 버튼 클릭|
|/api/v1/oss_report_bin	|-	|BIN 탭에 FOSSLight Report를 업로드합니다.이미 OSS Table이 작성된 경우, Reset한 후 업로드하는 FOSSLight Report를 반영합니다. (반영 Sheet Name: "BIN")|
|/api/v1/oss_report_src|	-	|SRC 탭에 FOSSLight Report를 업로드합니다.이미 OSS Table이 작성된 경우, Reset한 후 업로드하는 FOSSLight Report를 반영합니다. (반영 Sheet Name : "SRC")|
|/api/v1/package_upload|-	|Packaging 탭에 Packaging 파일을 업로드합니다. 이미 Packaging 파일이 업로드되어 있는 경우, 추가로 Packaging 파일을 업로드합니다. Packaging 파일 업로드 결과가 Mail로 발송됩니다.|
|/api/v1/prj_bom_compare|	JSON	|두 개의 Project의 BOM의 OSS Name, OSS Version, License를 비교합니다.|
|/api/v1/prj_bom_export	|File	|Project의 BOM에서 Export한 결과 파일을 다운로드 받습니다.|
|/api/v1/prj_bom_export_json|JSON |Project의 BOM에서 Export한 결과를 json 포맷으로 return합니다.|
|/api/v1/prj_search	| JSON |보기 권한이 있는 Project에 대하여 하기 정보를 조회합니다. |
|/api/v1/prj_watcher_add|-|Project의 Watcher를 추가합니다.|

4\. Vulnerability 정보 조회

| API  | 응답 형식 | 설명 |
| ------------- | ------------- | ------------- |
|/api/v1/vulnerability_data|	JSON|	OSS Name, Version별 CVE ID, CVSS Score, NVD Link를 조회합니다. |
|/api/v1/vulnerability_max_data	|JSON	|OSS Name, Version별 max score와 CVE ID를 확인할 링크를 조회합니다.|

5\. Self-Check 생성, FOSSLight Report 등록

| API  | 응답 형식 | 설명 |
| ------------- | ------------- | ------------- |
|/api/v1/create_selfcheck|	JSON	|Self-Check Project를 생성하고, 생성된 Self-Check ID를 return 받습니다.|
|/api/v1/oss_report_selfcheck|	-	|Self-Check에 FOSSLight Report를 업로드합니다. 이미 OSS Table이 작성된 경우, Reset한 후 업로드하는 FOSSLight Report를 반영합니다.(반영 Sheet Name : "Self-Check")|
|/api/v1/export_selfcheck|	File	|Self-Check에서 Export한 결과 파일을 다운로드 받습니다.|
|/api/v1/selfcheck_watcher_add|	-	|Self-Check의 Watcher를 추가합니다.|

6\. API 활용시, Code 값 확인

| API  | 응답 형식 | 설명 |
| ------------- | ------------- | ------------- |
|/api/v1/code_search|	JSON	|Project, 3rd Party 조회, Project 생성시 사용할 하기 Parameter의 값 List를 조회합니다. |

7\. Error code

| Return Code  | Description | Error Message |
| ------------- | ------------- | ------------- |
|200|TOKEN이 존재하지 않습니다.|User does not exist.|
|210|사용자의 TOKEN이 정상적이지 않습니다.|There is an error in the TOKEN value.|
|310|Parameter가 잘못되었습니다.|The parameter is invalid.|
|320|Project, self-check의 create건수가 초과되었습니다.(API를 활용한 생성은 일일 최대 3건)|The number of projects and self-checks that can be created has been exceeded. (Up to 3 per day)|
|330|OSS Report등 파일을 등록할 경우 작성된 data에 대하여 validation check시 오류가 발생하였습니다.|There is an error in the data written in the file.|
|400|업로드할 파일(ex-OSS Report, NOTICE, result.txt, Packaging 파일)을 누락하여 호출을 한 경우|The file to upload is missing.|
|410|OSS Report, Packaging file의 size가 초과된 경우입니다.(최대 Size: OSS Report -5MB, Packaging file- 4GB)|File size exceeded. (Max size: 5MB for oss report, 4GB for packaging file)|
|420|등록한 file들이 지원하지 않은 확장자입니다.|The registered files are extensions that are not supported.|
|430|업로드할 Tab이 활성화되지 않았습니다.Distribution Type 및 업로드할 Tab을 확인하세요. |The tab you are trying to upload is not active.|
|440|Load 할 Sheet 명이 없는 경우|The [Sheet_name] sheet name cannot be found|
|440|업로드한 파일에 load할 row가 없는 경우|There is no data to load.|
|500|호출된 대상에 권한이 없습니다. (ex : public 이 아닌 Project를 Watcher 또는 Creator가 아닌 user가 조회할 경우) |You do not have permission.|
|999|알수 없는 Error. |Unknown error.|

## REST API 활용 Sample
prj_search 를 이용하여 user과 admin 계정의 Project 정보를 조회하는 예제
```python
# SPDX-FileCopyrightText: Copyright 2023 LG Electronics Inc.
# SPDX-License-Identifier: Apache-2.0

import csv
import requests
from datetime import datetime
from collections import OrderedDict

header_list = ['prjId', 'prjName', 'prjVersion', 'createDate', 'updateDate', 'identificationStatus', 'verificationStatus', 'distributionStatus', 'status', 'vulnerabilityScore', 'distributionType', 'notice', 'networkService', 'priority', 'noticePlatform']

def get_data(period, user):
    url = "https://demo.fosslight.org/api/v1/prj_search"

    querystring = {"createDate":period,"creator":user}

    payload = ""
    headers = {"_token": "abCDe...."}

    response = requests.request("GET", url, data=payload, headers=headers, params=querystring, verify=False)

    # print(response.text)
    data = response.json()['data']

    content_list = data['content']

    data_list = []
    for content in content_list:
        data = OrderedDict() 
        for header in header_list:
            if header in content.keys():
                data[header] = content[header]
            else:
                data[header] = ''
        data_list.append(data)

    return data_list

if __name__ == "__main__":
    current_year = datetime.now().year
    
    content_list = []
    content_list.extend(get_data('20220101-{0}1231'.format(current_year), "user"))
    content_list.extend(get_data('20220101-{0}1231'.format(current_year), "admin"))

    content_list.sort(key=lambda x:x['prjId'],reverse=True)
    with open('api_data.csv','w',encoding='utf-8',newline='') as f:
        wr = csv.writer(f)
        wr.writerow(header_list)
        for content in content_list:
            wr.writerow(list(content.values()))

```
