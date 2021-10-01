---
published: true
title: FOSSLight Reuse
---
# FOSSLight Reuse

<img src="https://img.shields.io/pypi/l/fosslight-reuse" alt="License" /> <img src="https://img.shields.io/pypi/v/fosslight_reuse" alt="Current python package version." /> <img src="https://img.shields.io/pypi/pyversions/fosslight_reuse" /> [![REUSE status](https://api.reuse.software/badge/github.com/fosslight/fosslight_reuse)](https://api.reuse.software/info/github.com/fosslight/fosslight_reuse)
    

[**FOSSLight Reuse**](https://github.com/fosslight/fosslight_reuse)는 [소스 코드의 저작권 및 License 표기 규칙][rule]을 준수하기 위해 사용할 수 있는 도구입니다.
FOSSLight Reuse는 [reuse-tool][ret]을 이용하여 소스 코드의 저작권 및 라이선스 작성 규칙을 준수하는지 확인합니다.

[ret]: https://github.com/fsfe/reuse-tool
[rule]: https://oss.lge.com/guide/process/osc_process/1-identification/copyright_license_rule.html

##  기능
1. `lint` --- [Source Code 내 저작권 및 License 표기 규칙][rule]을 준수하는 지 체크합니다.    
2. `report` --- [oss-pkg-info.yaml](https://github.com/fosslight/fosslight_reuse/blob/main/tests/report/oss-pkg-info.yaml)을 FOSSLight-Report.xlsx로 또는 그 반대로 변환합니다.
     - oss-pkg-info.yaml을 [FOSSLight Report](../learn/2_fosslight_report.md)의 SRC Sheet로 변환
     - [FOSSLight Report](../learn/2_fosslight_report.md)의 BIN(Android), BOM Sheet를 oss-pkg-info.yaml로 변환
3. `add` --- Copyright와 License가 없는 파일에 Copyright와 License를 추가합니다.


## 🎉 설치 방법

FOSSLight Reuse는 pip3를 이용하여 설치할 수 있습니다.
[python 3.6 + virtualenv](https://fosslight.org/fosslight-guide-en/scanner/etc/guide_virtualenv.html)환경에서 설치할 것을 권장합니다.
```
$ pip3 install fosslight_reuse
```

## 🚀 실행 방법
``` 
$ fosslight_reuse lint
```
### Mode별 실행 방법 및 Parameters
* fosslight_reuse [Mode] [option1] <arg1> [option2] <arg2>...

```
Mode
    lint                  저작권 및 License 표기 규칙 준수 확인
    report                oss-pkg-info.yaml <-> FOSSLight-Report.xlsx 변환
    add                   Copyright와 License 추가
 
Options:
    -h                    설명 메시지 출력
    -p <path>             체크할 소스 경로
    -f <file1,file2,..>   체크할 파일 리스트
    -o <file_name>        결과 파일 이름 지정
    -n                    venv, node_modules, ./ 에 대하여 분석 제외하지 않으려면 추가
 
Options for only 'add' mode
    -l <license>          추가할 라이선스 이름(SPDX Format)
    -c <copyright>        추가할 저작권(ex, <year> <holder name>)
```
```
(ex1) $ fosslight_reuse lint -p /home/test/reuse-example -o result.xml
(ex2) $ fosslight_reuse report -p /home/test/source
(ex3 )$ fosslight_reuse add -p tests/add -c "2019-2021 LG Electronics Inc." -l "LicenseRef-LGE-Proprietary"
```

## 📁 실행 결과
### lint
```
# Ex.1) 특정 경로 내 파일을 분석
(venv)$ fosslight_reuse lint -p /home/test/reuse-example -o result.xml
ex.1 실행 결과
    ```baxh

    # SUMMARY
    # Open Source Package info: File to which OSS Package information is written.
    # Used licenses: License detected in the path.
    # Files with copyright information: Number of files with copyright / Total number of files.
    # Files with license information: Number of files with license / Total number of files.

    * Open Source Package info: /home/test/reuse-example/oss-package.info
    * Used licenses: CC-BY-4.0, CC0-1.0, GPL-3.0-or-later
    * Files with copyright information: 6 / 7
    * Files with license information: 6 / 7
    ```

# Ex.2) 특정 파일만 분석
(venv)$ fosslight_reuse lint -p /home/soimkim/test/reuse-example -f "src/load.c,src/dummy.c,src/main.c"
```
<details>
    <summary>lint ex.2 실행 결과 </summary>
    
    # src/load.c
    * License:
    * Copyright: SPDX-FileCopyrightText: 2019 Jane Doe <jane@example.com>

    # src/dummy.c
    * License:
    * Copyright:

    # src/main.c
    * License: GPL-3.0-or-later
    * Copyright: SPDX-FileCopyrightText: 2019 Jane Doe <jane@example.com>

</details>  

### report
```
# Ex.1) Path에 존재하는 oss-pkg-info.yaml 또는 oss-pkg-info.yml 파일을 모두 변환
$ fosslight_reuse report -p /home/test/source
```
<details>
    <summary>oss-pkg-info.yaml -> OSS Report(OSS-Report.xlsx) 결과 </summary>
    oss-pkg-info.yaml
    
    Open Source Package:
    - name: Apache Commons
      version: '2.4'
      source: http://svn.apache.org/repos/asf/commons
      homepage: https://commons.apache.org
      license:
      - Apache-2.0
    - name: dbus
      version: 1.10.20
      source: https://dbus.freedesktop.org/releases/dbus
      copyright: Copyright (c) 2002-2007, Red Hat, Inc.
      homepage: https://www.freedesktop.org
      license:
      - AFL-2.1
    - name: mysql-connector-java
      version: 5.1.38
      source: https://mvnrepository.com/artifact/mysql/mysql-connector-java/5.1.38
      homepage: http://dev.mysql.com/doc/connector-j/en
      license:
      - GPL-2.0
    
</details>


```
# Ex.2) FOSSLight Report를 oss-pkg-info.yaml 파일로 변환
$ fosslight_reuse report -f src/FOSSLight-Report.xlsx
```
 
### add
```
# Ex.1) 특정 경로 내 파일에 저작권과 라이선스를 추가
(venv)$ fosslight_reuse add -p tests/add -c "Copyright 2019-2021 LG Electronics Inc." -l "GPL-3.0-only"
    
# Ex.2) 특정 파일에 저작권과 라이선스를 추가
(venv)$ fosslight_reuse add -f "tests/add/test_both_have_1.py,tests/add/test_both_have_2.py,tests/add/test_no_copyright.py,tests/add/test_no_license.py" -c "2019-2021 LG Electronics Inc." -l "GPL-3.0-only"
```
<details>
    <summary> add 실행 결과 </summary>
    
    * 파일 변경 사항 : 상단에 저작권과 라이선스 추가
<table>
<tr>
    <td>Before</td>
    <td>After</td>
</tr>
<tr>
<td>

 <pre lang="python">
  x = 1
  y = "FOSSLight"
  z = sum(x, 1)

  </pre>
</td>
<td>
  <pre lang="python">
# SPDX-FileCopyrightText: Copyright 2019-2021 LG Electronics Inc.
#
# SPDX-License-Identifier: GPL-3.0-only   


  x = 1
  y = "FOSSLight"
  z = sum(x, 1)
</pre>
</td>
</tr>
</table>    

    # File list that have both license and copyright : 3 / 7
    # __init__.py
    * License:
    * Copyright:

    # test_both_have_1.py
    * License: GPL-3.0-only
    * Copyright: SPDX-FileCopyrightText: Copyright 2019-2021 LG Electronics Inc.

    # test_both_have_2.py
    * License: MIT
    * Copyright: SPDX-FileCopyrightText: Copyright (c) 2011 LG Electronics Inc.

    # Missing license File(s)
    * test_no_license.py
    * Your input license : GPL-3.0-only
    Successfully changed header of tests/add_result/test_no_license.py

    # Missing Copyright File(s)
    * test_no_copyright.py
    * Your input Copyright : Copyright 2019-2021 LG Electronics Inc.
    Successfully changed header of tests/add_result/test_no_copyright.py

</details>   


## 🚀 동작 방법 
### lint
1. OSS Package Information 파일 존재 여부 체크
<details>
    <summary> OSS Package Information 파일 </summary>
    
    하기 파일 중 1개 이상 존재하는지 체크 (대소문자 구분 없음)
    * oss-pkg-info.yaml
    * oss-pkg-info.yml
    * requirement.txt
    * requirements.txt
    * package.json
    * pom.xml
    * build.gradle
    * Podfile.lock
    * Cartfile.resolved
    * oss-package.info 
    * "MODULE_LICENSE_ "로 시작하는 파일 

</details>
2. fsfe-reuse lint 실행
2-1. Project 단위로 실행하는 경우 (-f 없는 경우)
* ./reuse/dep5 파일 없으면 생성
* ./reuse/dep5 파일이 이미 존재하는 경우 bk 파일을 복사하고 기본 설정값 추가
* dep5 파일 생성하여 binary 또는 .json, venv/, node_modules/,. */ 파일을 체크 대상에서 제외시킴
* fsfe-reuse lint 실행 (OSS Package Information file이 존재하면, license 정보 없는 파일 목록은 출력하지 않음)
* ./reuse/dep5 파일을 원래대로 복구
2-2. 파일 단위로 실행하는 경우 (-f 있는 경우)
* 파일별 저작권, License 출력
* 단, 파일이 존재하지 않거나 파일이 binary 또는 .json인 경우 출력되지 않음
3. 결과를 출력하여 xml 파일로 저장

### report
1. 변환할 파일의 존재 여부 확인
    * 파일 예시 : [oss-pkg-info.yaml][yml], [FOSSLight-Report.xlsx][xlsx]

[yml]: https://github.com/fosslight/fosslight_reuse/blob/main/tests/report/oss-pkg-info.yaml
[xlsx]: https://github.com/fosslight/fosslight_reuse/blob/main/tests/report/OSS-Report-Sample_0.xlsx

2. 파일을 변환
    2-1. Path 단위로 실행하는 경우 (-f 없는 경우)
        * 경로 내 존재하는 oss-pkg-info.yaml 또는 oss-pkg-info.yml 파일을 모두 변환
    2-2. 입력한 파일을 변환 
        * oss-pkg-info.yaml을 FOSSLight-Report.xlsx로 또는 그 반대로 변환
        * 단, -o 로 output file명을 지정한 경우 해당 이름으로 결과 파일이 생성
    
### add

- 파일 예시 : [oss-pkg-info.yaml](https://github.com/fosslight/fosslight_reuse/blob/main/tests/report/oss-pkg-info.yaml), [FOSSLight-Report.xlsx](https://github.com/fosslight/fosslight_reuse/blob/main/tests/report/OSS-Report-Sample_0.xlsx)

``` 
$ fosslight_reuse report
```

### Parameters     

| Parameter  | Argument | 필수  | 설명 |
| ------------- | ------------- | ------------- |------------- |
| p | 확인할 경로 | O | 변환할 oss-pkg-info*.yaml 또는 oss-pkg-info*.yml 파일이 위치한 경로 | 
| h | None | X | 설명 메시지 출력 | 
| o | 결과 파일명 | X | 결과 파일명 |    
| f | file1,file2,... | X | 1. FOSSLight Report로 변환할 Yaml 파일 (여러개인 경우 ,로 구분) <br> ex) -f src/oss-pkg-info.yaml,main/setting.yml <br> 2.  oss-pkg-info.yaml로 변환할 FOSSLight Report 파일 |

### Ex 1. oss-pkg-info.yaml 파일을 FOSSLight Report로 변환
1-1. Path에 존재하는 oss-pkg-info*.yaml 또는 oss-pkg-info*.yml 파일을 모두 변환
``` 
$ fosslight_reuse report -p /home/test/source
```

1-2. 특정 oss-pkg-info.yaml 파일들만 변환
``` 
$ fosslight_reuse report -f src/oss-pkg-info.yaml,main/setting.yml
```

### Ex 2. FOSSLight Report 를 oss-pkg-info.yaml 파일로 변환
```
$ fosslight_reuse report -f src/FOSSLight-Report.xlsx
```

## 📁 결과
출력 파일 이름이 -o로 지정되면 해당 이름으로 결과 파일이 생성됩니다.
- FOSSLight-Report_[datetime].xlsx : oss-pkg-info.yaml 파일을 변환한 파일
- oss-pkg-info_[datetime].yaml : FOSSLight-Report.xlsx가 변환된 파일

## 🚀 실행 방법 - report (실행 파일 이용 방법. windows용만 제공)
1. [fosslight_reuse release][release]에서 실행 파일을 다운로드 받습니다.
2. FOSSLight-Report*.xlsx 또는 oss-pkg-info.yaml이 있는 경로에 실행 파일 이동한 후  실행합니다.
3. oss-pkg-info.yaml이 있으면 FOSSLight-Report.xlsx로 변환되고, FOSSLight-Report*.xlsx가 있으면 oss-pkg-info.yaml로 변환됩니다.


## 🚀 실행 방법 - add (Copyright와 License를 추가)
``` 
$ fosslight_reuse add
```

### Parameters      

| Parameter  | Argument | 필수  | 설명 |
| ------------- | ------------- | ------------- |------------- |
| p | 체크할 경로 | O | 체크할 소스 파일 경로 | 
| f | file1,file2,... | X | 저작권, License 를 확인할 파일 목록 |
| c | 저작권 | O | 추가할 저작권('Copyright <year> <holder name>') | 
| l | 라이선스 | O | 추가할 라이선스 이름(SPDX Format) |

### Ex 1. 특정 경로 내 파일에 추가
``` 
$ fosslight_reuse add -p src/ -c "Copyright 2021 LG Electronics Inc." -l "GPL-3.0"
```
    
### Ex 2. 특정 파일에 저작권과 라이선스 추가
``` 
$ fosslight_reuse add -f "src/load.c,src/dummy.c,src/main.c" -c "Copyright 2021 LG Electronics Inc." -l "GPL-3.0"
```


## 동작 방법
1. -p 옵션의 경로가 존재하는 지 체크     
2. 추가할 저작권과 라이선스 확인
3. Reuse Add 실행    
    3-1. Path 단위로 실행하는 경우 (-f 없는 경우)
    - 경로 내 존재하는 모든 파일 중 파일 확장자를 통해 확인할 파일 리스트 추출
    - 저작권과 라이선스가 모두 존재하는 파일 리스트 출력(Add 대상에서 제외)
    - 저작권 또는 라이선스가 없는 파일의 상단에 -c와 -l 옵션으로 추가한 저작권과 라이선스를 추가
    
    3-2. 파일 단위로 실행하는 경우 (-f 있는 경우)
    - 입력한 파일의 저작권과 라이선스를 출력
    - 저작권 또는 라이선스가 없는 파일의 상단에 -c와 -l 옵션으로 추가한 저작권과 라이선스를 추가

## 📁 결과
### 출력 결과
 * File list that have both license and copyright : 파일 내에 저작권과 라이선스가 모두 존재하는 파일 목록
 * Missing License File(s) : 라이선스가 존재하지 않는 파일 목록
 * Missing Copyright File(s) : 저작권이 존재하지 않는 파일 목록

### 파일 변경 사항 : 상단에 저작권과 라이선스 추가
<table>
<tr>
    <td>Before</td>
    <td>After</td>
</tr>
<tr>
<td>

 <pre lang="python">
  x = 1
  y = "FOSSLight"
  z = sum(x, 1)
  
  </pre>
</td>
<td>
  <pre lang="python">
# SPDX-FileCopyrightText: Copyright 2019-2021 LG Electronics Inc.
#
# SPDX-License-Identifier: GPL-3.0-only   


  x = 1
  y = "FOSSLight"
  z = sum(x, 1)
</pre>
</td>
</tr>
</table>

### Ex 1. 특정 경로 내 파일에 저작권과 라이선스를 추가
```
(venv)$ fosslight_reuse add -p tests/add -c "Copyright 2019-2021 LG Electronics Inc." -l "GPL-3.0-only"
```
```bash
# File list that have both license and copyright : 3 / 7
# __init__.py
* License:
* Copyright:

# test_both_have_1.py
* License: GPL-3.0-only
* Copyright: SPDX-FileCopyrightText: Copyright 2019-2021 LG Electronics Inc.

# test_both_have_2.py
* License: MIT
* Copyright: SPDX-FileCopyrightText: Copyright (c) 2011 LG Electronics Inc.

# Missing license File(s)
  * test_no_license.py
  * Your input license : GPL-3.0-only
Successfully changed header of tests/add_result/test_no_license.py

# Missing Copyright File(s)
  * test_no_copyright.py
  * Your input Copyright : Copyright 2019-2021 LG Electronics Inc.
Successfully changed header of /home/jaekwonbang/commit_0915/tests/add_result/test_no_copyright.py
```
    
### Ex 2. 특정 파일에 저작권과 라이선스를 추가
```
(venv)$ fosslight_reuse add -f "tests/add/test_both_have_1.py,tests/add/test_both_have_2.py,tests/add/test_no_copyright.py,tests/add/test_no_license.py" -c "Copyright 2019-2021 LG Electronics Inc." -l "GPL-3.0-only"
```
```bash
# add/test_both_have_1.py
* License: GPL-3.0-only
* Copyright: SPDX-FileCopyrightText: Copyright 2019-2021 LG Electronics Inc.

# add/test_both_have_2.py
* License: MIT
* Copyright: SPDX-FileCopyrightText: Copyright (c) 2011 LG Electronics Inc.

# add/test_no_copyright.py
* License: GPL-3.0-only
* Copyright:

# add/test_no_license.py
* License:
* Copyright: SPDX-FileCopyrightText: Copyright 2019-2021 LG Electronics Inc.

  * Your input license : GPL-3.0-only
Successfully changed header of add/test_no_license.py
  * Your input Copyright : Copyright 2019-2021 LG Electronics Inc.
Successfully changed header of add/test_no_copyright.py
```


[release]: https://github.com/fosslight/fosslight_reuse/releases
