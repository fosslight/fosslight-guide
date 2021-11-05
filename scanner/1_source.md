---
published: true
---
# FOSSLight Source Scanner

<img src="https://img.shields.io/pypi/l/fosslight_source" alt="FOSSLight Source is released under the Apache-2.0 License." /> <img src="https://img.shields.io/pypi/v/fosslight_source" alt="Current python package version." /> <img src="https://img.shields.io/pypi/pyversions/fosslight_source" /> [![REUSE status](https://api.reuse.software/badge/github.com/fosslight/fosslight_source_scanner)](https://api.reuse.software/info/github.com/fosslight/fosslight_source_scanner)

[**FOSSLight Source Scanner**](https://github.com/fosslight/fosslight_source_scanner)는 소스 코드 스캐너인 [ScanCode][sc]를 이용하여, 파일 안에 포함된 Copyright과 License 문구를 검출합니다. Build Script, Binary, Directory, 특정 Directory (ex-test) 안의 파일을 제외시킵니다.    
그리고 License 이름에서 "-only", "-old-style"와 같은 문구를 제거합니다. 결과는 spreadsheet, csv 형태로 출력됩니다.

[sc]: https://github.com/nexB/scancode-toolkit

**Github Repository** : [https://github.com/fosslight/fosslight_source_scanner]()  
**License** : [Apache-2.0](https://github.com/fosslight/fosslight_source_scanner/blob/main/LICENSE)

## Contents
  - [Prerequisite](#-prerequisite)
  - [How to install](#-how-to-install)
  - [How to run](#-how-to-run)
    - [1. fosslight_source](#1-fosslight_source)
    - [2. fosslight_convert](#2-fosslight_convert)
  - [Result](#-result)

## 📋 Prerequisite
[**FOSSLight Source Scanner**](https://github.com/fosslight/fosslight_source_scanner)는 Python 3.6+ 기반에서 동작합니다.     
Windows의 경우 [Microsoft Visual C++ Build Tools][ms_build]를 추가로 설치해야 합니다.

[ms_build]: https://visualstudio.microsoft.com/vs/older-downloads/

## 🎉 How to install
FOSSLight Source Scanner는 pip3를 이용하여 설치할 수 있습니다.     
[python 3.6 + virtualenv](etc/guide_virtualenv.md) 환경에서 설치할 것을 권장합니다.

```
$ pip3 install fosslight_source
```

## 🚀 How to run
### 1. fosslight_source     
Source Code 분석을 실행한 후 FOSSLight Report 형식으로 출력합니다.
````
$ fosslight_source [option] <arg>
````  
#### Options
```
  Mandatory
    -p <source_path>               Path to analyze source

  Optional
    -h                             Print help message
    -j                             Generate additional result of executing ScanCode in json format
    -m                             Print the Matched text for each license on a separate sheet
    -o <output_path>               Output path
                                    (If you want to generate the specific file name, add the output path with file name.)
    -f <format>                    Output file format (excel, csv, opossum)

```
#### Example
Source Code 분석 후 FOSSLight Report와 json 형태의 ScanCode 결과 출력
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
  Mandatory
    -p <path_dir>                  Path of ScanCode json files

  Optional
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

## 📁 Result

```
$ tree
.
├── FOSSLight-Report_2021-05-03_00-39-49_SRC.csv
├── FOSSLight-Report_2021-05-03_00-39-49.xlsx
├── scancode_2021-05-03_00-39-49.json
├── fosslight_src_log_2021-05-03_00-39-49.txt
└── Opossum_input_2021-05-03_00-39-49.txt
```
- FOSSLight-Report_[datetime].xlsx : FOSSLight Report 형태의 Source Code 분석 결과
- FOSSLight-Report_[datetime]_[sheet_name].csv : FOSSLight Report를 csv로 출력한 결과
- fosslight_src_log_[datetime].txt: 실행 로그가 저장된 파일
- scancode_[datetime].json : ScanCode 실행 결과 (fosslight_source명령어에 -j 옵션이 포함된 경우에만 생성)
- Opossum_input_[datetime].json : [OpossumUI](https://github.com/opossum-tool/OpossumUI)에서 활용 가능한 Source Code 분석 결과

