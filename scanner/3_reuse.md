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
```fosslight_reuse [Mode] [option1] <arg1> [option2] <arg2>...```   
* Required parameter : **Mode**   
* Optional parameter : **Options**

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

**(windows인 경우)** 실행 파일을 이용한 방법  
    1. FOSSLight Reuse - Release 에서 fosslight_reuse_windows.exe를 다운로드  
    2. oss-pkg-info.yaml 파일 또는 [FOSSLight|OSS]-Report*.xlsx가 위치한 Path에 다운로드 받은 파일을 이동  
    3. 파일을 더블 클릭하여 실행  
    
    
## 📁 실행 결과
### lint
```
# Ex.1) 특정 경로 내 파일을 분석
(venv)$ fosslight_reuse lint -p /home/test/reuse-example -o result.xml
```
ex.1 실행 결과
```bash
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

```
# Ex.2) 특정 파일만 분석
(venv)$ fosslight_reuse lint -p /home/soimkim/test/reuse-example -f "src/load.c,src/dummy.c,src/main.c"
```
ex.2 실행 결과

```bash    
    # src/load.c
    * License:
    * Copyright: SPDX-FileCopyrightText: 2019 Jane Doe <jane@example.com>

    # src/dummy.c
    * License:
    * Copyright:

    # src/main.c
    * License: GPL-3.0-or-later
    * Copyright: SPDX-FileCopyrightText: 2019 Jane Doe <jane@example.com>
```

### report
```
# Ex.1) Path에 존재하는 oss-pkg-info.yaml 또는 oss-pkg-info.yml 파일을 모두 변환
$ fosslight_reuse report -p /home/test/source
```

oss-pkg-info.yaml -> OSS Report(OSS-Report.xlsx) 결과   
    **_oss-pkg-info.yaml_**   
```yaml    
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
```
    
**_FOSS-Report.xlsx_**   




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
실행 결과
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
    Successfully changed header of tests/add_result/test_no_copyright.py
```


## 🚀 동작 방법 
### lint
1. OSS Package Information 파일 존재 여부 체크
    OSS Package Information 파일  
    * 하기 파일 중 1개 이상 존재하는지 체크 (대소문자 구분 없음)  
    ```
        - oss-pkg-info.yaml
        - oss-pkg-info.yml
        - requirement.txt
        - requirements.txt
        - package.json
        - pom.xml
        - build.gradle
        - Podfile.lock
        - Cartfile.resolved
        - oss-package.info 
        - "MODULE_LICENSE_ "로 시작하는 파일
    ```

2. fsfe-reuse lint 실행<br>
    2-1. Project 단위로 실행하는 경우 (-f 없는 경우)
    - ./reuse/dep5 파일 없으면 생성
    - ./reuse/dep5 파일이 이미 존재하는 경우 bk 파일을 복사하고 기본 설정값 추가
    - dep5 파일 생성하여 binary 또는 .json, venv/, node_modules/,. */ 파일을 체크 대상에서 제외시킴
    - fsfe-reuse lint 실행 (OSS Package Information file이 존재하면, license 정보 없는 파일 목록은 출력하지 않음)
    - ./reuse/dep5 파일을 원래대로 복구
    2-2. 파일 단위로 실행하는 경우 (-f 있는 경우)
    - 파일별 저작권, License 출력
    - 단, 파일이 존재하지 않거나 파일이 binary 또는 .json인 경우 출력되지 않음
3. 결과를 출력하여 xml 파일로 저장

### report
1. 변환할 파일의 존재 여부 확인
    * 파일 예시 : [oss-pkg-info.yaml][yml], [FOSSLight-Report.xlsx][xlsx]

[yml]: https://github.com/fosslight/fosslight_reuse/blob/main/tests/report/oss-pkg-info.yaml
[xlsx]: https://github.com/fosslight/fosslight_reuse/blob/main/tests/report/OSS-Report-Sample_0.xlsx

2. 파일을 변환
    2-1. Path 단위로 실행하는 경우 (-f 없는 경우)
    - 경로 내 존재하는 oss-pkg-info.yaml 또는 oss-pkg-info.yml 파일을 모두 변환
    
    2-2. 입력한 파일을 변환 
    - oss-pkg-info.yaml을 FOSSLight-Report.xlsx로 또는 그 반대로 변환
    - 단, -o 로 output file명을 지정한 경우 해당 이름으로 결과 파일이 생성
    

### add
1. 추가할 저작권과 라이선스 확인
2. 저작권과 라이선스 탐색 및 추가
    - 저작권과 라이선스가 모두 존재하는 파일 리스트 출력(Add 대상에서 제외)
    - -c와 -l 옵션울 이용하여 저작권 또는 라이선스가 없는 파일의 상단에 저작권과 라이선스를 추가
    
