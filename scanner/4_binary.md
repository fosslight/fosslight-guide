---
published: true
---
# FOSSLight Binary Scanner

<img src="https://img.shields.io/pypi/l/fosslight_binary" alt="FOSSLight Binary is released under the Apache-2.0." /> <img src="https://img.shields.io/pypi/v/fosslight_binary" alt="Current python package version." /> <img src="https://img.shields.io/pypi/pyversions/fosslight_binary" /> [![REUSE status](https://api.reuse.software/badge/github.com/fosslight/fosslight_binary_scanner)](https://api.reuse.software/info/github.com/fosslight/fosslight_binary_scanner)

[**FOSSLight Binary Scanner**](https://github.com/fosslight/fosslight_binary_scanner)는 Binary를 찾아 출력하고 Binary DB에 동일하거나 비슷한 Binary가 있으면 해당 OSS 정보를 출력합니다.    
jar 파일에 대한 오픈 소스 분석 시, 오픈 소스인 [**Dependency-check-py**](https://github.com/jhermann/dependency-check-py)를 이용합니다.   
   
**Github Repository** : [https://github.com/fosslight/fosslight_binary_scanner]()  
**License** : [Apache-2.0](https://github.com/fosslight/fosslight_binary_scanner/blob/main/LICENSE)

## 목차
- [필요 조건](#-필요-조건)
- [설치 방법](#-설치-방법)
- [실행 방법](#-실행-방법)
- [결과](#-결과)
- [동작 방식](#-동작-방식)


## 📋 필요 조건
[**FOSSLight Binary Scanner**](https://github.com/fosslight/fosslight_binary_scanner)는 Python 3.6+ 기반에서 동작합니다.  
OSS 정보(OSS Name, OSS Version, License)를 Binary DB로부터 추출하는 기능을 사용하려면 [DB 세팅 가이드](etc/binary_db.md)를 참고하세요.

## 🎉 설치 방법
Jar 파일에 대한 분석을 위해서는 Java를 설치해야 합니다.    
   - Java 설치 링크 : https://openjdk.java.net (Open Source JDK를 설치)       
FOSSLight Binary Scanner는 pip3를 이용하여 설치할 수 있습니다.     
[python 3.6 + virtualenv](etc/guide_virtualenv.md) 환경에서 설치할 것을 권장합니다.

```
$ pip3 install fosslight_binary
```

## 🚀 실행 방법
````
$ fosslight_binary [option] <arg>
````    

### Options
````
    Mandatory:
        -p <binary_path>              Binary를 추출할 경로

    Options:
        -h                            Help Message 출력
        -o <output_path>              결과물 저장할 경로 (특정 결과 파일명을 원하는 경우에는 파일명까지 입력합니다.)
        -f <format>                   결과 파일 format (excel, csv, opossum) (default: excel and csv (window : excel only)
        -d <db_url>                   Binary DB 접속 정보(format :'postgresql://username:password@host:port/database_name')
```` 

## 📁 결과

```
$ tree
.
├── binary_20210601_201646.txt
├── fosslight_bin_log_20210601_201646.txt
├── FOSSLight-Report_20210601_201646_BIN.csv
├── FOSSLight-Report_20210601_201646.xlsx
└── Opossum_input_20210601_201646.json

```
- binary_[datetime].txt : Binary별 checksum, tlsh 값이 출력된 결과
- fosslight_bin_log_[datetime].txt : 실행 log
- FOSSLight-Report_[datetime]_BIN.csv : FOSSLight binary의 결과 (csv 형태. windows는 생성 안 함)
- FOSSLight-Report_[datetime].xlsx : FOSSLight binary의 결과 (FOSSLight Report 형태)    
   - jar 파일 분석 시, Vulnerability Link Column이 FOSSLight-Report_[datetime].xlsx에 추가 됨.    
- Opossum_input_[datetime].json : [OpossumUI](https://github.com/opossum-tool/OpossumUI)에서 활용 가능한 Binary 분석 결과     

## 🧐 동작 방식
1. 하기 사항을 제외하고 Binary를 추출합니다.    
    1-0. symbolic link, FIFO 파일    
    1-1. 파일 extension : ['png', 'gif', 'jpg', 'bmp', 'jpeg', 'qm', 'xlsx', 'pdf', 'ico', 'pptx', 'jfif', 'docx',
                         'doc', 'whl', 'xls', 'xlsm', 'ppt', 'mp4', 'pyc', 'plist']            
    1-2. 파일 Type : ['data','timezone data', 'apple binary property list']    
    1-3. 경로 : ['.git']    
2. 하기 사항에 대하여 FOSSLight Report에 "Exclude"를 체크합니다.     
     - Binary가 ['fosslight_bin', 'fosslight_bin.exe']에 포함되는 경우           
     - 경로가 ["test", "tests", "doc", "docs"]에 포함되는 경우     
3. Binary별 checksum과 tlsh를 출력합니다.     
4. OSS 정보를 Binary DB로 부터 불러옵니다.       
5. Output 파일을 생성합니다.    
