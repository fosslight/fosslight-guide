---
published: true
---
# User Guide

## Contents

- [Prerequisite](#-prerequisite)
- [How to install](#-how-to-install)
- [How to run](#-how-to-run)
- [Result](#-result)
- [How to report issue](#-how-to-report-issue)
- [License](#-license)


## 📋 Prerequisite

[**FOSSLight Source Scanner**](https://github.com/fosslight/fosslight_source_scanner)는 Python 3.6+ 기반에서 동작합니다.     
Windows의 경우 [Microsoft Visual C++ Build Tools][ms_build]를 추가로 설치해야 합니다.

[ms_build]: https://visualstudio.microsoft.com/vs/older-downloads/

## 🎉 How to install

FOSSLight Source Scanner는 pip3를 이용하여 설치할 수 있습니다.     
[python 3.6 + virtualenv](guide_virtualenv_kor.md) 환경에서 설치할 것을 권장합니다.

```
$ pip3 install fosslight_source
```

## 🚀 How to run

FOSSLight Source Scanner에는 하기 두 가지 명령어가 있습니다. 

### 1. fosslight_source     
Source Code 분석을 실행한 후 FOSSLight Report 형식으로 출력합니다.

| Parameter  | Argument | Description |
| ------------- | ------------- | ------------- |
| h | None | Print help message. | 
| p | String | Path to analyze source. | 
| j | None | As an output, the result of executing ScanCode in json format other than FOSSLight Report is additionally generated. | 
| o | String | Output file name without file extension. | 

#### Ex. Source Code 분석 후 FOSSLight Report와 json 형태의 ScanCode 결과 출력
```
$ fosslight_source -p /home/source_path -j
```
### 2. fosslight_convert     
json형태인 ScanCode 결과를 FOSSLight Report 형식으로 변환합니다.

| Parameter  | Argument | Description |
| ------------- | ------------- | ------------- |
| h | None | Print help message. | 
| p | String | Path of ScanCode json files. | 
| o | String | Output file name without file extension. | 

#### Ex. json 형태의 ScanCode 결과를 FOSSLight Report 형식으로 변환
```
$ fosslight_convert -p /home/jsonfile_dir
```

## 📁 Result

```
$ tree
.
├── FOSSLight-Report_2021-05-03_00-39-49.csv
├── FOSSLight-Report_2021-05-03_00-39-49.xlsx
├── scancode_2021-05-03_00-39-49.json
└── fosslight_src_log_2021-05-03_00-39-49.txt

```
- FOSSLight-Report_[datetime].xlsx : FOSSLight Report형태의 Source Code 분석 결과
- FOSSLight-Report_[datetime].csv : FOSSLight Report를 csv로 출력한 결과 (Windows 제외)
- fosslight_src_log_[datetime].txt: 실행 로그가 저장된 파일
- scancode_[datetime].json : ScanCode 실행 결과 (fosslight_source명령어에 -j 옵션이 포함된 경우에만 생성)

