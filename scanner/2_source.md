---
published: true
---
# FOSSLight Source Scanner

<img src="https://img.shields.io/pypi/l/fosslight_source" alt="FOSSLight Source is released under the Apache-2.0 License." /> <img src="https://img.shields.io/pypi/v/fosslight_source" alt="Current python package version." /> <img src="https://img.shields.io/pypi/pyversions/fosslight_source" /> [![REUSE status](https://api.reuse.software/badge/github.com/fosslight/fosslight_source_scanner)](https://api.reuse.software/info/github.com/fosslight/fosslight_source_scanner)

[**FOSSLight Source Scanner**](https://github.com/fosslight/fosslight_source_scanner)는 소스 코드 스캐너인 [ScanCode][sc], [SCANOSS][scanoss]를 이용합니다. [ScanCode][sc]를 이용하면 파일 안에 포함된 Copyright과 License 문구를 검출하고, [SCANOSS][scanoss]를 이용하면 OSS Name, OSS Version, Download Location, Copyright, License 정보를 [OSSKB][osskb]에서 검색합니다. 
Build Script, Binary, Directory, 특정 Directory (ex-test) 안의 파일은 제외되고, 그리고 License 이름에서 "-only", "-old-style"와 같은 문구는 제거됩니다. 결과는 spreadsheet, csv 형태로 출력됩니다.

[sc]: https://github.com/nexB/scancode-toolkit
[scanoss]: https://github.com/scanoss/scanoss.py
[osskb]: https://osskb.org/

**Github Repository** : [https://github.com/fosslight/fosslight_source_scanner]()  
**License** : [Apache-2.0](https://github.com/fosslight/fosslight_source_scanner/blob/main/LICENSE)

## 목차
  - [필요 조건](#-필요-조건)
  - [설치 방법](#-설치-방법)
  - [실행 방법](#-실행-방법)
    - [1. fosslight_source](#1-fosslight_source)
    - [2. fosslight_convert](#2-fosslight_convert)
  - [결과](#-결과)
  - [Docker를 이용한 설치](#-docker를-이용하여-설치-및-실행-방법)

## 📋 필요 조건
[**FOSSLight Source Scanner**](https://github.com/fosslight/fosslight_source_scanner)는 Python 3.6+ 기반에서 동작합니다.     
SCANOSS를 사용하기 위해서는 Python 3.7+ 환경을 권장합니다.       
         
⚠️ **windows**와 **mac m1**의 경우 설치가 불가합니다. 이 경우 [Docker를 이용](#-docker를-이용하여-설치-및-실행-방법)하여 설치 및 사용을 권장합니다.      


## 🎉 설치 방법
FOSSLight Source Scanner는 pip3를 이용하여 설치할 수 있습니다.     
[python 3.6 + virtualenv](etc/guide_virtualenv.md) 환경에서 설치할 것을 권장합니다.

```
$ pip3 install fosslight_source
```

## 🚀 실행 방법
### 1. fosslight_source     
Source Code 분석을 실행한 후 FOSSLight Report 형식으로 출력합니다.
````
$ fosslight_source [option] <arg>
````  
#### Options
```
  Optional
    -p <source_path>               Path to analyze source (Default: current directory)
    -h                             Print help message
    -j                             Generate raw result of scanners in json format
    -m                             Print additional information for scan result on separate sheets
    -o <output_path>               Output path
                                   (If you want to generate the specific file name, add the output path with file name.)
    -f <format>                    Output file format (excel, csv, opossum)
    -s <scanner>                   Select which scanner to be run (scancode, scanoss, all)

```
-s 옵션이 추가되지 않을 경우 모든 Scanner (ScanCode, SCANOSS)가 동작한 결과가 취합됩니다.

#### Example
Source Code 분석 후 FOSSLight Report와 json 형태의 ScanCode, SCANOSS 결과 출력
```
$ fosslight_source -p /home/source_path -j
```

### 2. fosslight_convert     
json형태인 ScanCode 결과를 FOSSLight Report 형식으로 변환합니다.
````
$ fosslight_convert [option] <arg>
```` 
#### Options
```
  Optional
    -p <path_dir>                  Path of ScanCode json files (Default: current directory)
    -h                             Print help message
    -m                             Print the Matched text for each license on a separate sheet
    -o <output_path>               Output path
                                   (If you want to generate the specific file name, add the output path with file name.)
    -f <format>                    Output file format (excel, csv, opossum)


```
#### Example
json 형태의 ScanCode 결과를 FOSSLight Report 형식으로 변환
```
$ fosslight_convert -p /home/jsonfile_dir
```

## 📁 결과

```
$ tree
.
├── FOSSLight-Report_20220103_154024_SRC_FL_Source.csv
├── FOSSLight-Report_20220103_154024.xlsx
├── fosslight_src_log_20220103_154024.txt
├── scancode_raw_result.json
├── scanner_output.wfp
├── scanoss_raw_result.json
└── Opossum_input_20220103_154024.json
```
- FOSSLight-Report_[datetime]_[sheet_name].csv : FOSSLight Report를 csv로 출력한 결과
- FOSSLight-Report_[datetime].xlsx : FOSSLight Report 형태의 Source Code 분석 결과
- fosslight_src_log_[datetime].txt: 실행 로그가 저장된 파일
- scancode_raw_result.json : ScanCode 실행 결과 (fosslight_source 명령어에 -j 옵션이 포함된 경우에만 생성)
- scanner_output.wfp : SCANOSS 실행 시 생성된 Finger Print (fosslight_source 명령어에 -j 옵션이 포함된 경우에만 생성)
- scanoss_raw_result.json : SCANOSS 실행 결과 (fosslight_source 명령어에 -j 옵션이 포함된 경우에만 생성)
- Opossum_input_[datetime].json : [OpossumUI](https://github.com/opossum-tool/OpossumUI)에서 활용 가능한 Source Code 분석 결과

## 🐳 Docker를 이용하여 설치 및 실행 방법
1. Dockerfile을 이용하여 이미지 빌드
```
$docker build -t fosslight_source .
```
2. 빌드한 이미지로 실행합니다.     
ex. Output 경로 : /Users/fosslight_source_scanner/test_output, 분석 경로 : tests/test_files
```
$docker run -it -v /Users/fosslight_source_scanner/test_output:/app/output fosslight_source -p tests/test_files -o output
```
