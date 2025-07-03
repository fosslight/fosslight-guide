---
sort: 5
published: true
title: 🚩FOSSLight Scanner
---
# FOSSLight Scanner

<a href="https://github.com/fosslight/fosslight_scanner/blob/main/LICENSE"><img src="https://img.shields.io/pypi/l/fosslight_scanner" alt="FOSSLight Scanner is released under the Apache-2.0." /></a> <a href="https://pypi.org/project/fosslight-scanner/"><img src="https://img.shields.io/pypi/v/fosslight_scanner" alt="Current python package version." /></a> <img src="https://img.shields.io/pypi/pyversions/fosslight_scanner" />

FOSSLight Scanner는 로컬 소스코드 또는 입력받은 링크를 통해 소스를 다운로드 받은 후 소스코드, 바이너리 및 디펜던시에 대한 오픈 소스 분석을 수행할 수 있습니다.  
<br />
오픈 소스 분석을 위해 사용하는 툴은 다음과 같습니다.

1. [FOSSLight Source Scanner](2_source.md) : 소스 코드를 분석하여 오픈 소스 분석 결과를 생성합니다. 
2. [FOSSLight Dependency Scanner](3_dependency.md) : Package manager 또는 빌드 시스템을 통해 사용되는 dependency의 오픈 소스 분석 결과를 생성합니다. 
3. [FOSSLight Binary Scanner](4_binary.md) : Binary를 분석하여 오픈 소스 분석 결과를 생성합니다. 
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
1. [**FOSSLight Scanner**](https://github.com/fosslight/fosslight_scanner)는 Python 3.8+ 기반에서 동작합니다.   
2. Jar 파일에 대한 분석을 위해서는 [**Java**](https://openjdk.java.net)를 설치해야 합니다.(Open Source JDK를 설치)
3. (windows의 경우) Microsoft Build Tools (Microsoft Visual C++ 14.0+) from https://visualstudio.microsoft.com/ko/visual-cpp-build-tools/ 를 설치해야 합니다.

## 🎉 설치 방법   
FOSSLight Scanner는 pip3를 이용하여 설치할 수 있습니다.     
[python 3.8 + virtualenv](etc/guide_virtualenv.md) 환경에서 설치할 것을 권장합니다.

```
$ pip3 install fosslight_scanner
```

### ⚠️ 설치 에러 발생 시
'Cargo, the Rust package manager, is not installed or is not on PATH.' 에러 발생 시, cargo, rust를 아래와 같이 설치한 이후, 다시 FOSSLight Scanner를 설치합니다.
#### Linux, MacOS
```
$ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
$ export PATH="$HOME/.cargo/bin:$PATH"
```
#### Windows
[https://www.rust-lang.org/tools/install](https://www.rust-lang.org/tools/install)에서 rust-init.exe 파일 다운로드 후 설치
 

## 🚀 실행 방법
### Mode별 실행 방법 및 Parameters
```
$ fosslight [Mode] [option1] <arg1> [option2] <arg2>...
```
```
     Parameters:
        Mode: Multiple modes can be entered by separating them with , (ex. source,binary)
            all                     Run all scanners(Default)
            source                  Run FOSSLight Source Scanner
            dependency              Run FOSSLight Dependency Scanner
            binary                  Run FOSSLight Binary Scanner
            compare                 Compare two FOSSLight reports

        Options:
            -h                      Print help message
            -p <path>               Path to analyze (ex, -p {input_path})
                                     * Compare mode input file: Two FOSSLight reports (supports excel, yaml)
                                       (ex, -p {before_name}.xlsx {after_name}.xlsx)
            -w <link>               Link to be analyzed can be downloaded by wget or git clone
            -f <formats> [<format> ...]     FOSSLight Report file format (excel, csv, opossum, yaml, spdx-tag, spdx-yaml, spdx-json, spdx-xml)
                                     * Compare mode result file: supports excel, json, yaml, html
                                     * Multiple formats can be specified separated by space.
            -e <path>               Path to exclude from analysis (ex, -e {dir} {file})
            -o <output>             Output directory or file
            -c <number>             Number of processes to analyze source
            -r                      Keep raw data
            -t                      Hide the progress bar
            -v                      Print FOSSLight Scanner version
            -s <path>               Path to apply setting from file (check format with 'setting.json' in this repository)
                                     * Direct cli flags have higher priority than setting file
                                       (ex, '-f yaml -s setting.json' - result file extension is .yaml)
            --no_correction         Enter if you don't want to correct OSS information with sbom-info.yaml
                                     * Correction mode only supported xlsx format.
            --correct_fpath <path>  Path to the sbom-info.yaml file
            --ui                          Generate UI mode result file

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

### 실행 Parameter를 json으로 저장하여 호출하는 방법
1. [setting.json](https://github.com/fosslight/fosslight_scanner/blob/main/tests/setting.json) 포맷으로 실행 parameter별 값을 json 파일로 작성하여 저장
2. 실행시, -s 로 생성한 setting.json을 호출합니다.
```
fosslight -s setting.json
```
🛈 json 파일에 작성한 parameter보다 실행시 호출한 값을 우선합니다.      
ex. '-f yaml -s setting.json'로 호출시, yaml 포맷의 output을 출력합니다.

## 📁 결과
### 오픈소스 분석 모드 결과 (all, source, dependency, binary)
```
test_result/
├── fosslight_log
│   └── fosslight_log_220214_1824.txt
├── fosslight_report_all_220214_1824.xlsx
└── fosslight_raw_data (-r option 있는 경우)
    ├── fosslight_src_220214_1824.xlsx
    ├── fosslight_bin_220214_1824.xlsx
    └── fosslight_dep_220214_1824.xlsx
```
- fosslight_report_(datetime).xlsx : Source 분석, Binary 분석, Dependency 분석 결과가 작성된 FOSSLight Report 형식의 파일
- fosslight_raw_data directory: 분석 결과 Raw Data 파일이 생성되는 폴더 (-r option 있는 경우)
  - fosslight_src_(datetime).xlsx : Source 분석 결과 파일
  - fosslight_dep_(datetime).xlsx : Dependency 분석 결과 파일
  - fosslight_bin_(datetime).xlsx : Binary 분석 결과 파일
 
#### fosslight_report_(datetime).xlsx
1. Exclude : 체크된 Row
   test(s), doc(s), 숨김 파일 or 폴더는 Exclude 체크됩니다.
2. sbom-info.yaml을 load한 경우, load한 데이터를 append하고 중복된 파일에 대한 분석 결과는 Exclude 체크됩니다.
3. Comment란 :     
   Add/Loaded by ** : ** 으로부터 load한 Row     
   Excluded by ** : ** 으로 인해 Exclude된 Row       

### compare 모드 결과
```
test_result/
├── fosslight_log
│   └── fosslight_log_20220817_114259.txt
└── fosslight_compare_20220817_114259.xlsx
```
- fosslight_compare_(datetime).xlsx : 두 개의 BOM 비교 결과가 (add/delete/change) 테이블 양식으로 작성된 파일

## 🐳 Docker를 이용하여 설치 및 실행 방법
> [!NOTE]  
> Docker로 실행시, FOSSLight Source/Binary Scanner만 동작합니다. FOSSLight Dependency Scanner는 동작하지 않습니다. 

1. FOSSLight Scanner Docker 이미지 다운로드
   
    선택 1. Dockerhub에서 fosslight_scanner 다운로드 
    ```
    $ docker pull fosslight/fosslight_scanner
    ```
    선택 2. [Dockerfile](https://github.com/fosslight/fosslight_scanner/blob/main/Dockerfile)을 이용하여 이미지 빌드 (선택 1에서 지원하지 않는 OS인 경우)
    ```
    $ docker build -t fosslight_scanner .
    ```
    
3. 빌드한 이미지로 실행합니다.     
ex. Output 경로 : /Users/git/temp/output, 분석 경로 : /Users/git/temp/dir_to_analyze
```
$ docker run -it -v /Users/git/temp/dir_to_analyze:/app/dir_to_analyze -v /Users/git/temp/output:/app/output fosslight_scanner -p dir_to_analyze -o output
```
