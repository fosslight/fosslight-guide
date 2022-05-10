---
published: true
title: FOSSLight Reuse
---
# FOSSLight Reuse

<img src="https://img.shields.io/pypi/l/fosslight-reuse" alt="License" /> <img src="https://img.shields.io/pypi/v/fosslight_reuse" alt="Current python package version." /> <img src="https://img.shields.io/pypi/pyversions/fosslight_reuse" /> [![REUSE status](https://api.reuse.software/badge/github.com/fosslight/fosslight_reuse)](https://api.reuse.software/info/github.com/fosslight/fosslight_reuse)

[**FOSSLight Reuse**](https://github.com/fosslight/fosslight_reuse)는 [reuse-tool][ret]을 이용하여 [소스 코드의 저작권 및 License 표기 규칙][rule]을 준수하는지 확인하고 보완하기 위해 사용할 수 있는 도구입니다.

[ret]: https://github.com/fsfe/reuse-tool
[rule]: https://oss.lge.com/guide/process/osc_process/1-identification/copyright_license_rule.html

**Github Repository** : [https://github.com/fosslight/fosslight_reuse]()  
**License** : [GPL-3.0-only](https://github.com/fosslight/fosslight_reuse/blob/main/LICENSE)

## 목차
  - [필요 조건](#-필요-조건)
  - [설치 방법](#-설치-방법)
  - [실행 방법](#-실행-방법)
  - [결과](#-결과)
  - [동작 방식](#-동작-방식)

## 📋 필요 조건
[**FOSSLight Reuse**](https://github.com/fosslight/fosslight_reuse)는 Python 3.6+ 기반에서 동작합니다.

## 🎉 설치 방법
FOSSLight Reuse는 pip3를 이용하여 설치할 수 있습니다.
[python 3.6 + virtualenv](https://fosslight.org/fosslight-guide-en/scanner/etc/guide_virtualenv.html) 환경에서 설치할 것을 권장합니다.
```
$ pip3 install fosslight_reuse
```

## 🚀 실행 방법
FOSSLight Reuse는 다음 세가지 모드를 가지고 있습니다.
1. `lint` --- [Source Code 내 저작권 및 License 표기 규칙][rule]을 준수하는 지 체크합니다.    
2. `convert` --- [oss-pkg-info.yaml](https://github.com/fosslight/fosslight_reuse/blob/main/tests/convert/oss-pkg-info.yaml)을 FOSSLight-Report.xlsx로 또는 그 반대로 변환합니다.
     - oss-pkg-info.yaml을 [FOSSLight Report](../learn/2_fosslight_report.md)의 SRC Sheet로 변환
     - [FOSSLight Report](../learn/2_fosslight_report.md)의 BIN(Android), BOM Sheet를 oss-pkg-info.yaml로 변환
3. `add` --- Copyright와 License가 없는 파일에 Copyright와 License를 추가합니다.

``` 
$ fosslight_reuse [Mode] [option1] <arg1> [option2] <arg2>...
```

### Mode별 실행 방법 및 Parameters
* Required parameter : **Mode**   
* Optional parameter : **Options**

```
Mode
    lint                  저작권 및 License 표기 규칙 준수 확인
    convert               oss-pkg-info.yaml <-> FOSSLight-Report 변환
    add                   소스 코드에 Copyright와 License 추가
 
Options:
    -h                    설명 메시지 출력
    -p <path>             체크할 소스 경로
    -f <file1,file2,..>   체크할 파일 리스트
    -o <file_name>        결과 파일 이름 지정
    -n                    venv, node_modules, ./ 에 대하여 분석 제외하지 않으려면 추가
 
Options for only 'add' mode
    -l <license>          추가할 라이선스 (SPDX License Identifer)
    -c <copyright>        추가할 저작권 (ex, <year> <copyright holder>)
```

**(Windows인 경우)** 실행 파일을 이용한 방법  
    1. [FOSSLight Reuse - Release](https://github.com/fosslight/fosslight_reuse/releases) 에서 fosslight_reuse_windows.exe를 다운로드  
    2. [oss-pkg-info.yaml](https://github.com/fosslight/fosslight_reuse/blob/main/tests/convert/oss-pkg-info.yaml) 파일 또는 [FOSSLight_OSS-Report*.xlsx](../learn/2_fosslight_report.md) 파일이 위치한 Path에 다운로드 받은 파일을 이동  
    3. 파일을 더블 클릭하여 실행  
    
    
## 📁 결과
### 🔖 lint mode

**1) 특정 경로 내 파일 분석 예시**  
```
(venv)$ fosslight_reuse lint -p /home/test/reuse-example -o result.xml
```
- 실행 결과
    <pre>
        # SUMMARY
        # Open Source Package info: File to which OSS Package information is written.
        # Used licenses: License detected in the path.
        # Files with copyright information: Number of files with copyright / Total number of files.
        # Files with license information: Number of files with license / Total number of files.  
        * Open Source Package info: /home/test/reuse-example/oss-package.info
        * Used licenses: CC-BY-4.0, CC0-1.0, GPL-3.0-or-later
        * Files with copyright information: 6 / 7
        * Files with license information: 6 / 7 </pre>

**2) 특정 파일만 분석 예시**
```
(venv)$ fosslight_reuse lint -p /home/soimkim/test/reuse-example -f "src/load.c,src/dummy.c,src/main.c"
```
- 실행 결과
    <pre>
        # src/load.c
        * License:
        * Copyright: SPDX-FileCopyrightText: 2019 Jane Doe <jane@example.com>
        
        # src/dummy.c
        * License:
        * Copyright:
        
        # src/main.c
        * License: GPL-3.0-or-later
        * Copyright: SPDX-FileCopyrightText: 2019 Jane Doe <jane@example.com> </pre>

<details>
    <summary markdown="span" style="font-weight:bold">Demo 영상 (lint)</summary>
    <img src="images/lint.gif" alt="demo video for lint mode">
</details>


### 🔖 convert mode
**1) Path 내 존재하는 oss-pkg-info.yaml (여러개인 경우 전체 해당) -> FOSSLight-Report 변환 예시**
```
$ fosslight_reuse convert -p /home/test/source
```

**2) FOSSLight Report -> oss-pkg-info.yaml 파일 변환 예시**
```
$ fosslight_reuse convert -f src/FOSSLight-Report.xlsx
```

**3) 실행 결과 파일 예시**

{::options parse_block_html="true" /}
> <details>
> <summary markdown="span">oss-pkg-info.yaml 파일</summary>
```yaml    
Open Source Software Package:
    - name: glibc
    version: 2.3
    source: https://github.com/fsfe/glibc
    license:
    - GPL-3.0
    - LGPL-2.1
    file : 
    - a.c
    - b.c
    - name : dbus
    version : 1.3
    source : https://github.com/fsfe/dbus
    license : GPL-2.0
    file : src/*
    copyright : |
        Copyright (c) 2020 Test
        Copyright (c) 2020 Test
    - name : reuse-tool
    source : https://github.com/fsfe/reuse
    homepage : http://google.com
    license : MIT
    copyright: Copyright (c) 2020 Test
    - name : build-tool
    source : http://gihub.com/bazel
    license : Apache-2.0
    exclude : True
```
> </details>
> <details>
> <summary markdown="span">FOSSLight-Report.xlsx 파일</summary>
<img src="images/fosslight_reuse_report.JPG" alt="FOSSLight Report">
> </details>

<details>
<summary markdown="span" style="font-weight:bold">Demo 영상 (convert)</summary>
<img src="images/convert.gif" alt="demo video for convert mode">
</details>
{::options parse_block_html="false" /}


### 🔖 add mode
**1) 특정 경로 내 파일에 저작권과 라이선스 추가 예시**
```
(venv)$ fosslight_reuse add -p tests/add -c "2019-2021 LG Electronics Inc." -l "GPL-3.0-only"
```

**2) 특정 파일에 저작권과 라이선스 추가 예시**
```
(venv)$ fosslight_reuse add -f "tests/add/test_both_have_1.py,tests/add/test_both_have_2.py,tests/add/test_no_copyright.py,tests/add/test_no_license.py" -c "2019-2021 LG Electronics Inc." -l "GPL-3.0-only"
```

**3) 실행 결과**  
▪️ 파일 변경 사항 : 상단에 저작권과 라이선스 추가  

|Before          |After          |
|:---------------|:--------------|
|![Before](images/fosslight_reuse_add_test.JPG)|![After](images/fosslight_reuse_add_test_result.JPG)|

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

<details>
    <summary markdown="span" style="font-weight:bold">Demo 영상 (add)</summary>
    <img src="images/add.gif" alt="demo video for add mode">
</details>


## 🔍 동작 방식 
### 🔖 lint mode
1. OSS Package Information 파일 존재 여부 체크
    <details>
    <summary markdown="span">하기 파일 중 1개 이상 존재하는지 체크 (대소문자 구분 없음)</summary>
    <ul>
    <li>oss-pkg-info.yaml</li>
    <li>oss-pkg-info.yml</li>
    <li>requirement.txt</li>
    <li>requirements.txt</li>
    <li>package.json</li>
    <li>pom.xml</li>
    <li>build.gradle</li>
    <li>Podfile.lock</li>
    <li>Cartfile.resolved</li>
    <li>oss-package.info </li>
    <li>"MODULE_LICENSE_ "로 시작하는 파일</li>
    </ul>
    </details>

2. fsfe-reuse lint 실행    
    2-1. Project 단위로 실행하는 경우 (-f 없는 경우)   
    - ./reuse/dep5 파일 없으면 생성   
    - ./reuse/dep5 파일이 이미 존재하는 경우 bk 파일을 복사하고 기본 설정값 추가   
    - dep5 파일 생성하여 binary 또는 .json, venv/, node_modules/,. */ 파일을 체크 대상에서 제외시킴   
    - fsfe-reuse lint 실행 (OSS Package Information file이 존재하면, license 정보 없는 파일 목록은 출력하지 않음)   
    - ./reuse/dep5 파일을 원래대로 복구 (원래 존재한 경우 기존 파일로 복구, 존재하지 않은 경우 삭제) 
 
    2-2. 파일 단위로 실행하는 경우 (-f 있는 경우)   
    - 파일별 저작권, License 출력   
    - 단, 파일이 존재하지 않거나 파일이 binary 또는 .json인 경우 출력되지 않음   
3. 결과를 출력하여 xml 파일로 저장

### 🔖 convert mode
1. 변환할 파일의 존재 여부 확인   
   * 파일 예시 : [oss-pkg-info.yaml][yml], [FOSSLight-Report.xlsx][xlsx]   

[yml]: https://github.com/fosslight/fosslight_reuse/blob/main/tests/convert/oss-pkg-info.yaml   
[xlsx]: https://github.com/fosslight/fosslight_reuse/blob/main/tests/convert/OSS-Report-Sample_0.xlsx   

2. 파일을 변환   
    2-1. Path 단위로 실행하는 경우 (-f 없는 경우)   
    - 경로 내 존재하는 oss-pkg-info.yaml 또는 oss-pkg-info.yml 파일을 모두 변환   
    
    2-2. 입력한 파일을 변환  
    - oss-pkg-info.yaml을 FOSSLight-Report.xlsx로 또는 그 반대로 변환   
    - 단, -o 로 output file명을 지정한 경우 해당 이름으로 결과 파일이 생성   
    

### 🔖 add mode
1. 추가할 저작권과 라이선스 확인
2. 저작권과 라이선스 탐색 및 추가
    - 저작권과 라이선스가 모두 존재하는 파일 리스트 출력(Add 대상에서 제외)
    - -c와 -l 옵션을 이용하여 저작권 또는 라이선스가 없는 파일의 상단에 저작권과 라이선스를 추가
    
