---
published: true
---
# FOSSLight Binary Scanner

<img src="https://img.shields.io/pypi/l/fosslight_binary" alt="FOSSLight Binary is released under the Apache-2.0." /> <img src="https://img.shields.io/pypi/v/fosslight_binary" alt="Current python package version." /> <img src="https://img.shields.io/pypi/pyversions/fosslight_binary" /> [![REUSE status](https://api.reuse.software/badge/github.com/fosslight/fosslight_binary_scanner)](https://api.reuse.software/info/github.com/fosslight/fosslight_binary_scanner)

[**FOSSLight Binary Scanner**](https://github.com/fosslight/fosslight_binary_scanner)는 Binary를 찾아 출력하고 Binary DB에 동일하거나 비슷한 Binary가 있으면 해당 OSS 정보를 출력합니다. 

## Contents

- [Prerequisite](#-prerequisite)
- [How to install](#-how-to-install)
- [How to run](#-how-to-run)
- [How it works](#-how-it-works)
- [Result](#-result)
- [Development](#-development)


## 📋 Prerequisite
- [**FOSSLight Binary Scanner**](https://github.com/fosslight/fosslight_binary_scanner)는 Python 3.6+ 기반에서 동작합니다.  
- OSS 정보(OSS Name, OSS Version, License)를 Binary DB로 부터 추출하는 기능을 사용하려면 [DB 세팅 가이드](etc/binary_db.md)를 참고하세요.

## 🎉 How to install
FOSSLight Binary Scanner는 pip3를 이용하여 설치할 수 있습니다.     
[python 3.6 + virtualenv](etc/guide_virtualenv.md) 환경에서 설치할 것을 권장합니다.

```
$ pip3 install fosslight_binary
```

## 🚀 How to run
````
$ fosslight_binary [option] <arg>
````    

### About Options
```` 
    Mandatory:
        -p <binary_path>              Binary를 추출할 경로

    Options:
        -h                            Help Message 출력
        -a <target_architecture>      특정 Target Architecture(x86-64, ARM, MIPS, Mach-O, and etc.)인 Binary만 추출
        -o <output_path>              결과물 저장할 경로
        -f <customized_file_name>     결과물 파일명, 파일 확장자는 제외합니다 (기본값: 'binary_[datetime].txt')
        -d <db_url>                   Binary DB 접속 정보(format :'postgresql://username:password@host:port/database_name')
```` 


## 🧐 How it works
1. 하기 사항을 제외하고 Binary를 추출합니다.    
    1-0. symbolic link, FIFO 파일    
    1-1. 파일 extension : ['png', 'gif', 'jpg', 'bmp', 'jpeg', 'qm', 'xlsx', 'pdf', 'ico', 'pptx', 'jfif', 'docx',
                         'doc', 'whl', 'xls', 'xlsm', 'ppt', 'mp4', 'pyc', 'plist']            
    1-2. 파일 Type : ['data','timezone data', 'apple binary property list']    
    1-3. 경로 : ['.git']    
2. 하기 사항에 대하여 Output Report에 "Exclude"를 체크합니다.     
     - Binary가 ['fosslight_bin', 'fosslight_bin.exe']에 포함되는 경우           
     - 경로가 ["test", "tests", "doc", "docs"]에 포함되는 경우     
3. -a 옵션의 경우 file 커맨드에 해당 값이 포함되는 경우만 최종 result에 포함합니다.         
4. Binary별 checksum과 tlsh를 출력합니다.     
5. OSS 정보를 Binary DB로 부터 불러옵니다.       
6. Output 파일을 생성합니다.        

## 📁 Result

```
$ tree
.
├── binary_20210601_201646.txt
├── fosslight_bin_log_20210601_201646.txt
├── FOSSLight-Report_20210601_201646_BIN.csv
└── FOSSLight-Report_20210601_201646.xlsx

```
- FOSSLight-Report_[datetime].xlsx : FOSSLight binary의 결과 (FOSSLight Report 형태)         
- FOSSLight-Report_[datetime]_BIN.csv : FOSSLight binary의 결과 (csv 형태. windows는 생성안 함)
- fosslight_bin_log_[datetime].txt : 실행 log
- binary_[datetime].txt : Binary별 checksum, tlsh 값이 출력된 결과

## 💻 Development
### How to make an executable  
````  
$ pip install .  
$ pyinstaller --onefile cli.py -n cli --additional-hooks-dir=hooks
````
### How to test
````  
$ pip install requiremets-dev.txt
$ tox
````
