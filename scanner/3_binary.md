---
published: true
title: "  ㄴ FOSSLight Binary Scanner"
---
# FOSSLight Binary Scanner

<img src="https://img.shields.io/pypi/l/fosslight_binary" alt="FOSSLight Binary is released under the Apache-2.0." /> <img src="https://img.shields.io/pypi/v/fosslight_binary" alt="Current python package version." /> <img src="https://img.shields.io/pypi/pyversions/fosslight_binary" /> [![REUSE status](https://api.reuse.software/badge/github.com/fosslight/fosslight_binary_scanner)](https://api.reuse.software/info/github.com/fosslight/fosslight_binary_scanner)

[**FOSSLight Binary Scanner**](https://github.com/fosslight/fosslight_binary_scanner)는 Binary를 찾아 출력하고 Binary DB에 동일하거나 비슷한 Binary가 있으면 해당 OSS 정보를 출력합니다.    
jar 파일에 대한 오픈 소스 분석 시, 오픈 소스인 [**Dependency-check-py**](https://github.com/jhermann/dependency-check-py)를 이용합니다.   
   
**Github Repository** : [https://github.com/fosslight/fosslight_binary_scanner](https://github.com/fosslight/fosslight_binary_scanner)  
**License** : [Apache-2.0](https://github.com/fosslight/fosslight_binary_scanner/blob/main/LICENSE)

## 목차
- [필요 조건](#-필요-조건)
- [설치 방법](#-설치-방법)
- [실행 방법](#-실행-방법)
- [결과](#-결과)
- [동작 방식](#-동작-방식)


## 📋 필요 조건
[**FOSSLight Binary Scanner**](https://github.com/fosslight/fosslight_binary_scanner)는 Python 3.10+ 기반에서 동작합니다.  
OSS 정보(OSS Name, OSS Version, License)를 Binary DB로부터 추출하는 기능을 사용하려면 [DB 세팅 가이드](etc/binary_db.md)를 참고하세요.    

Jar 파일에 대한 분석을 위해서는 [**Java 11+**](https://openjdk.java.net)를 설치해야 합니다.(Open Source JDK를 설치)    

## 🎉 설치 방법    
### 방법 1. 실행 파일 다운로드
OS(Operating System)에 맞는 실행 파일을 다운로드 받습니다.    
    - [FOSSLight Binary Scanner - Release](https://github.com/fosslight/fosslight_binary_scanner/releases)    

단, 지원하지 않는 OS인 경우 '방법 2'로 설치합니다.

### 방법 2. Python 환경 기반 fosslight_binary 설치
0. (windows의 경우) https://visualstudio.microsoft.com/ko/vs/older-downloads/ > 재배포 가능 패키지 및 빌드 도구에서 Microsoft Build Tools 설치
1. [python 3.10 + virtualenv](etc/guide_virtualenv.md) 환경 세팅
2. Python package인 fosslight_binary 설치
```
$ pip3 install fosslight_binary
```

## 🚀 실행 방법
### 방법 1. windows에서 실행 파일로 실행하는 경우
binary 분석할 path에 fosslight_bin_windows.exe 파일 위치시킨 후, 더블 클릭하여 실행합니다.

### 방법 2. 그 외, command로 실행하는 경우
````
$ fosslight_binary [option] <arg>
````    

### Options
````
    Options:
        -p <binary_path>                    Path to analyze binaries (Default: current directory)
        -h                                  Print help message
        -v                                  Print FOSSLight Binary Scanner version
        -s                                  Extract only the binary list in simple mode
        -e <path>                           Path to exclude from analysis (files and directories, pattern matching is available)
                                            * IMPORTANT: Always wrap patterns in quotes("") to avoid shell expansion.
                                              Example) fosslight_bin -e "test/abc.py" "*.jar" "test/"
        -o <output_path>                    Output path
                                            (If you want to generate the specific file name, add the output path with file name.)
        -f <format> [<format> ...]          Output file formats
                                            (excel, csv, opossum, yaml, spdx-yaml, spdx-json, spdx-xml, spdx-tag, cyclonedx-json, cyclonedx-xml)
                                            Multiple formats can be specified separated by space.
        -d <db_url>                         DB Connection(format :'postgresql://username:password@host:port/database_name')
        --notice                            Print the open source license notice text.
        --no_correction                     Enter if you don't want to correct OSS information with sbom-info.yaml
        --correct_fpath <path>              Path to the sbom-info.yaml file
```` 
- -e 옵션 관련 [Pattern 매칭 가이드](https://scancode-toolkit.readthedocs.io/en/stable/cli-reference/scan-options-pre.html?highlight=ignore#glob-pattern-matching)
   - ⚠️ 사용 시 반드시 쌍 따옴표("")를 이용하여 입력하시기 바랍니다.
       - 예시) fosslight_binary -e "*.png" "tests/"
   - ⚠️ 입력 시 파일명과 확장자는 대소문자를 정확히 구분해야 합니다.


## ⚙️ 환경 변수
FOSSLight Binary Scanner 실행 시, 아래 환경 변수를 설정하여 동작을 제어할 수 있습니다.

### 1. FOSSLIGHT_SKIP_AUTO_INSTALL
- 실행 환경(특히 배포된 실행 파일)에서 dependency-check의 자동 다운로드/설치를 비활성화합니다.
- '1' 또는 'true'일 경우 자동 다운로드/설치가 비활성화됩니다.
- 사용 방법:
   ```
   - Linux/macOS:
      bash export FOSSLIGHT_SKIP_AUTO_INSTALL=1
   - Windows (cmd):
      cmd set FOSSLIGHT_SKIP_AUTO_INSTALL=1
   - Windows (PowerShell):
      powershell $env:FOSSLIGHT_SKIP_AUTO_INSTALL='1'
   ```

   
### 2. DEPENDENCY_CHECK_HOME
- dependency-check의 설치 경로입니다. (이미 존재하면 그대로 사용)
- 기존 설치된 경우: dependency-check 디렉토리 자체 (.../dependency-check)
- 신규 설치시: base + 'dependency-check'를 결정 후 저장
  
### 3. DEPENDENCY_CHECK_VERSION
현재 설치된 dependency-check 버전 정보입니다.
- 쓰는 시점: dependency-check 확인 성공 또는 설치 완료 후
   ```
   - 코드에서 자동 설정(e.g. 12.1.7 버전일 경우):
      python os.environ['DEPENDENCY_CHECK_VERSION'] = '12.1.7'
   
   - 외부 스크립트에서 참조:
      - Linux/macOS:    
         bash echo $DEPENDENCY_CHECK_VERSION
      - Windows (cmd):    
         cmd echo %DEPENDENCY_CHECK_VERSION%
  ```

   
## 📁 결과

```
$ tree
.
├── fosslight_log_220904_0912.txt
├── fosslight_report_220904_0912.xlsx
└── fosslight_opossum_220904_0912.json

```
- fosslight_log_[datetime].txt : 실행 log
- fosslight_report_[datetime].xlsx : FOSSLight binary의 결과 (FOSSLight Report 형태)    
   - jar 파일 분석 시, Vulnerability Link Column에 보안취약점 정보가 추가됨.
   - Binary별 checksum, tlsh Column은 기본적으로 숨김 처리 되어 있음.  
- fosslight_opossum_[datetime].json : [OpossumUI](https://github.com/opossum-tool/OpossumUI)에서 활용 가능한 Binary 분석 결과     

## 🧐 동작 방식
1. 아래 항목들은 Binary 분석 과정에서 제외됩니다.    

   |제외 항목                | 설명                                                                                                                         |    
   |------------------------|-------------------------------------------------------------------------------------------------------------------------------|    
   |symbolic link, FIFO 파일| file open으로 읽을 수 없음.   이로 인해 file type이나 binary인지 체크할 때, FOSSLight Binary Scanner가 멈춤.                      |    
   |Binary가 아닌 확장자     | 'qm', 'xlsx', 'pdf', 'pptx', 'jfif', 'docx', 'doc', 'whl', 'xls', 'xlsm', 'ppt', 'mp4', 'pyc', 'plist', 'dat', 'json', 'js' 등|    
   |특정 파일 Type           | 'data','timezone data', 'apple binary property list'로 시작하는 파일들                                                         |    
   |특정 경로                | '.git'의 경로                                                                                                                 |
   
2. 아래 사항에 대하여 FOSSLight Report에 "**Exclude**"를 체크합니다.

   |Exclude 항목                                                          |설명                                                 |
   |----------------------------------------------------------------------|-----------------------------------------------------|
   |Binary가 ['fosslight_bin', 'fosslight_bin.exe']에 포함되는 경우         | -                                                  |
   |경로가 ["test", "tests", "doc", "docs", "intermediates"]에 포함되는 경우| 실제 배포에 포함되는 소스 코드 / 바이너리의 결과만 출력 |
   |directory가 숨긴 폴더인 경우 (폴더명이 .로 시작하는 경우)                | -                                                   |               
   |특정 확장자인 경우                                                     | 최종 빌드 산출물이 아님(ex, .class)                   |
   
3. Binary별 checksum과 tlsh를 출력합니다.     
4. OSS 정보를 Binary DB로 부터 불러옵니다.       
5. 결과 Report 파일을 생성합니다.    
