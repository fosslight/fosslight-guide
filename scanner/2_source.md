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

**Github Repository** : [https://github.com/fosslight/fosslight_source_scanner](https://github.com/fosslight/fosslight_source_scanner)  
**License** : [Apache-2.0](https://github.com/fosslight/fosslight_source_scanner/blob/main/LICENSE)

## 목차
  - [필요 조건](#-필요-조건)
  - [설치 방법](#-설치-방법)
  - [실행 방법](#-실행-방법)
  - [결과](#-결과)

## 📋 필요 조건
[**FOSSLight Source Scanner**](https://github.com/fosslight/fosslight_source_scanner)는 Python 3.10+ 기반에서 동작합니다.     
  

## 🎉 설치 방법
FOSSLight Source Scanner는 pip3를 이용하여 설치할 수 있습니다.     
[python 3.10 + virtualenv](etc/guide_virtualenv.md) 환경에서 설치할 것을 권장합니다.

```
$ pip3 install fosslight_source
```

## 🚀 실행 방법
Source Code 분석을 실행한 후 FOSSLight Report 형식으로 출력합니다.
````
$ fosslight_source [option] <arg>
````  
#### Options
```
  Optional
      -p <source_path>       Path to analyze source (Default: current directory)
      -h                     Print help message
      -v                     Print FOSSLight Source Scanner version
      -m                     Print additional information for scan result on separate sheets
      -e <path>              Path to exclude from analysis (file and directory, pattern matching is available)
      -o <output_path>       Output path (Path or file name)
      -f <format>            Output file format (excel, csv, opossum, yaml)
  Options only for FOSSLight Source Scanner
      -s <scanner>           Select which scanner to be run (scancode, scanoss, all)
      -j                     Generate raw result of scanners in json format
      -t <float>             Stop scancode scanning if scanning takes longer than a timeout in seconds.
      -c <core>              Select the number of cores to be scanned with ScanCode.
      --no_correction        Enter if you don't want to correct OSS information with sbom-info.yaml
      --correct_fpath <path> Path to the sbom-info.yaml file
```
- -s 옵션이 추가되지 않을 경우 모든 Scanner (ScanCode, SCANOSS)가 동작한 결과가 취합됩니다.
- -e 옵션 관련 [Pattern 매칭 가이드](https://scancode-toolkit.readthedocs.io/en/stable/cli-reference/scan-options-pre.html?highlight=ignore#glob-pattern-matching)
   - ⚠️ 사용 시 반드시 쌍 따옴표("")를 이용하여 입력하시기 바랍니다.
       - 예시) fosslight_binary -e "*.png" "tests/"
   - ⚠️ 입력 시 파일명과 확장자는 대소문자를 정확히 구분해야 합니다.

#### Example
Source Code 분석 후 FOSSLight Report와 json 형태의 ScanCode, SCANOSS 결과 출력
```
$ fosslight_source -p /home/source_path -j
```

## 📁 결과

```
$ tree
.
├── fosslight_log_220103_1540.txt
├── fosslight_opossum_220103_1540.json
├── fosslight_report_220103_1540.xlsx
├── fosslight_report_220103_1540.csv
├── scancode_raw_result.json
├── scanner_output.wfp
└── scanoss_raw_result.json
```
- fosslight_log_[datetime].txt : 실행 로그가 저장된 파일
- fosslight_opossum_[datetime].json : [OpossumUI](https://github.com/opossum-tool/OpossumUI)에서 활용 가능한 Source Code 분석 결과
- fosslight_report_[datetime].xlsx : FOSSLight Report 형태의 Source Code 분석 결과
- fosslight_report_[datetime].csv : FOSSLight Report를 csv로 출력한 결과
- scancode_raw_result.json : ScanCode 실행 결과 (fosslight_source 명령어에 -j 옵션이 포함된 경우에만 생성)
- scanner_output.wfp : SCANOSS 실행 시 생성된 Finger Print (fosslight_source 명령어에 -j 옵션이 포함된 경우에만 생성)
- scanoss_raw_result.json : SCANOSS 실행 결과 (fosslight_source 명령어에 -j 옵션이 포함된 경우에만 생성)
