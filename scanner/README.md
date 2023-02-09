---
sort: 5
published: true
title: 🚩FOSSLight Scanner
---
# FOSSLight Scanner

<a href="https://github.com/fosslight/fosslight_scanner/blob/main/LICENSE"><img src="https://img.shields.io/pypi/l/fosslight_scanner" alt="FOSSLight Scanner is released under the Apache-2.0." /></a> <a href="https://pypi.org/project/fosslight-scanner/"><img src="https://img.shields.io/pypi/v/fosslight_scanner" alt="Current python package version." /></a> <img src="https://img.shields.io/pypi/pyversions/fosslight_scanner" />

FOSSLight Scanner는 로컬 소스코드 또는 입력받은 링크를 통해 소스를 다운로드 받은 후 소스코드, 바이너리 및 디펜던시에 대한 오픈 소스 분석을 수행할 뿐만 아니라 저작권/License 표기 규칙 준수 여부 또한 체크할 수 있습니다.  
<br />
이때 오픈 소스 분석과 저작권/License 표기 규칙 확인을 위해 사용하는 툴은 다음과 같습니다.

1. [FOSSLight Prechecker](1_prechecker.md) : 소스 코드 내 저작권 및 License 표기 Rule을 준수하는지 체크합니다. 
2. [FOSSLight Source Scanner](2_source.md) : 소스 코드를 분석하여 오픈 소스 분석 결과를 생성합니다. 
3. [FOSSLight Dependency Scanner](3_dependency.md) : Package manager 또는 빌드 시스템을 통해 사용되는 dependency의 오픈 소스 분석 결과를 생성합니다. 
4. [FOSSLight Binary Scanner](4_binary.md) : Binary를 분석하여 오픈 소스 분석 결과를 생성합니다. 
5. [FOSSLight Yocto Scanner](5_yocto.md) : Yocto Project에 대한 오픈 소스 분석 결과를 생성합니다. (별도 실행 필요)
<br />


**Github Repository** : [https://github.com/fosslight/fosslight_scanner](https://github.com/fosslight/fosslight_scanner)  
**License** : [Apache-2.0](https://github.com/fosslight/fosslight_scanner/blob/main/LICENSE)

## 목차
- [📋 필요 조건](#-필요-조건)
- [🎉 설치 방법](#-설치-방법)
- [🚀 실행 방법](#-실행-방법)
- [📁 결과](#-결과)
- [🐳 Docker를 이용하여 설치 및 실행 방법](#-docker를-이용하여-설치-및-실행-방법)
  
## 📋 필요 조건
1. [**FOSSLight Scanner**](https://github.com/fosslight/fosslight_scanner)는 Python 3.7+ 기반에서 동작합니다.   
2. Jar 파일에 대한 분석을 위해서는 [**Java**](https://openjdk.java.net)를 설치해야 합니다.(Open Source JDK를 설치)
3. (windows의 경우)  https://visualstudio.microsoft.com/ko/vs/older-downloads/ > 재배포 가능 패키지 및 빌드 도구에서 Microsoft Build Tools를 설치해야 합니다.

## 🎉 설치 방법   
FOSSLight Scanner는 pip3를 이용하여 설치할 수 있습니다.     
[python 3.7 + virtualenv](etc/guide_virtualenv.md) 환경에서 설치할 것을 권장합니다.

```
$ pip3 install fosslight_scanner
```

## 🚀 실행 방법
### Mode별 실행 방법 및 Parameters
```
$ fosslight [Mode] [option1] <arg1> [option2] <arg2>...
```
```
 Parameters:
    Mode
        source                  Run FOSSLight Source
        dependency              Run FOSSLight Dependency
        binary                  Run FOSSLight Binary
        prechecker              Run FOSSLight Prechecker
        all                     Run all scanners
        compare                 Compare two FOSSLight reports
 
    Options:
        -h                      Print help message
        -p <path>               Path to analyze (ex, -p {input_path})
                                 * Compare mode input file: Two FOSSLight reports (supports excel, yaml)
                                   (ex, -p {before_name}.xlsx {after_name}.xlsx)
        -w <link>               Link to be analyzed can be downloaded by wget or git clone
        -f <format>             FOSSLight Report file format (excel, yaml)
                                 * Compare mode result file: supports excel, json, yaml, html
        -o <output>             Output directory or file
        -c <number>             Number of processes to analyze source
        -r                      Keep raw data
        -t                      Hide the progress bar
        -v                      Print FOSSLight Scanner version
 
    Options for only 'all' or 'bin' mode
        -u <db_url>             DB Connection(format :'postgresql://username:password@host:port/database_name')
 
    Options for only 'all' or 'dependency' mode
        -d <dependency_argument>        Additional arguments for running dependency analysis
```
- -d 옵션은 FOSSLight Dependency 실행시 argument 입력이 필요한 경우만 입력합니다.[참고](3_dependency.md)

#### Ex.1 Local의 Path를 분석하는 방법
```
fosslight all -p /home/source_path
```

#### Ex.2 링크를 다운로드 받고 분석하는 방법
```
fosslight all -o test_result_wget -w "https://github.com/LGE-OSS/example.git"
```

#### Ex.3 FOSSLight Report BOM 결과 비교하여 변경/추가/삭제 내역 확인하는 방법
```
fosslight compare -p FOSSLight_before_proj.yaml FOSSLight_after_proj.yaml -o test_result
```

## 📁 결과
### 오픈소스 분석 모드 결과 (all, source, dependency, binary)
```
test_result/
├── fosslight_binary_220214_1824.txt
├── fosslight_log
│   └── fosslight_log_220214_1824.txt
├── fosslight_lint_220214_1824.yaml
├── fosslight_report_220214_1824.xlsx
└── fosslight_raw_data
    ├── fosslight_src_220214_1824.xlsx
    ├── fosslight_bin_220214_1824.xlsx
    └── fosslight_dep_220214_1824.xlsx
```
- fosslight_lint_(datetime).yaml : FOSSLight Prechecker의 lint 모드 실행 결과 생성되는 yaml 파일
- fosslight_binary_(datetime).txt : FOSSLight Binary결과 binary 별 checksum, tlsh 값이 추출된 파일
- fosslight_report_(datetime).xlsx : Source code 분석, Binary 분석, Dependency 분석 결과가 작성된 FOSSLight Report 형식의 파일
- fosslight_raw_data directory: 분석 결과 Raw Data 파일이 생성되는 폴더 (-r option 있는 경우)
  - fosslight_src_(datetime).xlsx : Source code 분석 결과 파일
  - fosslight_dep_(datetime).xlsx : Dependency 분석 결과 파일
  - fosslight_bin_(datetime).xlsx : Binary 분석 결과 파일

### compare 모드 결과
```
test_result/
├── fosslight_log
│   └── fosslight_log_20220817_114259.txt
└── fosslight_compare_20220817_114259.xlsx
```
- fosslight_compare_(datetime).xlsx : 두 개의 BOM 비교 결과가 (add/delete/change) 테이블 양식으로 작성된 파일

## 🐳 Docker를 이용하여 설치 및 실행 방법
1. Dockerfile을 이용하여 이미지 빌드
```
$docker build -t fosslight .
```
2. 빌드한 이미지로 실행합니다.     
ex. Output 경로 : /Users/fosslight_scanner/test_output, 분석 경로 : tests/test_files
```
$docker run -it -v /Users/fosslight_scanner/test_output:/app/output fosslight -p tests/test_files -o output
```
