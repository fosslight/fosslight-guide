---
published: true
title: "  ㄴ FOSSLight Binary Scanner"
---
# FOSSLight Binary Scanner

<img src="https://img.shields.io/pypi/l/fosslight_binary" alt="FOSSLight Binary is released under the Apache-2.0." /> <img src="https://img.shields.io/pypi/v/fosslight_binary" alt="Current python package version." /> <img src="https://img.shields.io/pypi/pyversions/fosslight_binary" /><a href="https://github.com/fosslight/fosslight_binary_scanner"><img src="https://img.shields.io/badge/GitHub-Repository-purple?logo=github" alt="GitHub Repository" /></a> [![REUSE status](https://api.reuse.software/badge/github.com/fosslight/fosslight_binary_scanner)](https://api.reuse.software/info/github.com/fosslight/fosslight_binary_scanner)

[**FOSSLight Binary Scanner**](https://github.com/fosslight/fosslight_binary_scanner)는 Binary를 식별하여 출력하고, Binary DB에 동일하거나 유사한 Binary가 존재하는 경우 해당 OSS 정보(OSS Name, OSS Version, License)를 제공합니다. 또한 jar 파일 분석 후에 보안취약점 정보도 제공합니다.       
   

## 필요 조건
{: .left-bar-title} 
- [**FOSSLight Binary Scanner**](https://github.com/fosslight/fosslight_binary_scanner)는 Python 3.10+ 환경에서 동작합니다.  
- Binary DB가 구성된 환경에서는 해당 DB로부터 OSS 정보(OSS Name, OSS Version, License)를 추출할 수 있습니다. Binary DB가 사전 구성되어 있지 않은 경우에는 [Binary DB 세팅 가이드](etc/binary_db.md)를 참고하여 환경을 설정해야 합니다.   
- Jar 파일 분석을 위해서는 Open Source JDK 기반의 [**Java 11+**](https://openjdk.java.net)를 설치해야 합니다.    

<br><br> 

## 설치 방법
{: .left-bar-title} 

### 방법 1. 실행 파일을 통한 설치  
{: .specific-title}  
OS(Operating System)에 맞는 FOSSLight Binary Scanner 실행 파일을 다운로드하여 사용하며, 지원되지 않는 OS 환경에서는 '방법 2'로 설치합니다.    
    - [FOSSLight Binary Scanner - Release](https://github.com/fosslight/fosslight_binary_scanner/releases)  

> 🛠️ 에러 메세지에 따른 조치 사항  
>  1. \[PYI-385915:ERROR\] Failed to load Python shared library '/tmp/_MEIpgjz34/libpython3.12.so.1.0': /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /tmp/_MEIpgjz34/libpython3.12.so.1.0) 에러가 발생하는 경우 실행 파일이 지원되지 않는 OS 또는 OS version 입니다. 이 경우 '방법 2'로 설치합니다.   


### 방법 2. Python 환경 기반 fosslight_binary 패키지 설치  
{: .specific-title} 
Windows 환경의 경우 Python 패키지 빌드를 위해 [Visual Studio 공식 사이트](https://visualstudio.microsoft.com/ko/vs/older-downloads/) > '기타 도구, 프레임워크 및 재배포 가능 패키지'에서 Microsoft Build Tools 설치합니다.     
1. [python 3.10 + virtualenv](etc/guide_virtualenv.md) 환경 세팅
2. Python package인 `fosslight_binary` 설치
```
$ pip3 install fosslight_binary  
```

<br><br> 

## 실행 방법
{: .left-bar-title} 

### 방법 1. windows에서 실행 파일로 실행하는 경우   
{: .specific-title}  
1. 실행 파일 더블 클릭을 통한 실행  
   - Binary 분석할 path에 `fosslight_bin_windows.exe` 파일을 위치시킨 후, 더블 클릭하여 실행합니다.     
2. 명령 프롬프트(cmd)에서 실행 파일 실행
 - fosslight_bin_windows.exe -p D:\Download\Dir_to_analyze(분석할 Path)  

### 방법 2. 그 외, command로 실행하는 경우(Python Package로 설치한 경우)  
{: .specific-title} 
````
$ fosslight_binary [option] <arguments>  
````    

### Options
````
   📖 Usage
    ────────────────────────────────────────────────────────────────────
    fosslight_bin [options] <arguments>

    📝 Description
    ────────────────────────────────────────────────────────────────────
    FOSSLight Binary Scanner extracts binaries and retrieves open source
    and license information by comparing similarity with binaries stored
    in the Binary DB using TLSH (Trend Micro Locality Sensitive Hash).

    📚 Guide: https://fosslight.org/fosslight-guide/scanner/3_binary.html

    ⚙️  General Options
    ────────────────────────────────────────────────────────────────────
    -p <path>              Binary path to analyze (default: current directory)
    -o <path>              Output file path or directory
    -f <format>            Output formats: excel, csv, opossum, yaml, spdx-yaml, spdx-json, spdx-xml, spdx-tag, cyclonedx-json, cyclonedx-xml
                           (multiple formats can be specified, separated by space)
    -e <pattern>           Exclude paths from analysis (files and directories)
                           ⚠️  IMPORTANT: Always wrap in quotes to avoid shell expansion
                           Example: fosslight_bin -e "test/" "*.jar"
    -h                     Show this help message
    -v                     Show version information

    🔍 Scanner-Specific Options
    ────────────────────────────────────────────────────────────────────
    -d <db_url>            DB Connection (format: 'postgresql://user:pass@host:port/db')
    --notice               Print the open source license notice text
    --no_correction        Skip OSS information correction with sbom-info.yaml
    --correct_fpath <path> Path to custom sbom-info.yaml file

    💡 Examples
    ────────────────────────────────────────────────────────────────────
    # Scan current directory
    fosslight_bin

    # Scan specific path with exclusions
    fosslight_bin -p /path/to/binaries -e "test/" "*.so"

    # Generate output in specific format
    fosslight_bin -f excel -o results/

    # Connect to Binary DB for OSS information
    fosslight_bin -d "postgresql://user:pass@localhost:5432/exampledb"
```` 
- -e 옵션 관련 [Pattern 매칭 가이드](https://scancode-toolkit.readthedocs.io/en/stable/reference/scancode-cli/cli-pre-scan-options.html#glob-pattern-matching)
   - ⚠️ 사용 시 반드시 쌍 따옴표("")를 이용하여 입력하시기 바랍니다.
       - 예시) fosslight_binary -e "*.png" "tests/"
   - ⚠️ 입력 시 파일명과 확장자는 대소문자를 정확히 구분해야 합니다.

<br><br> 

## 결과
{: .left-bar-title}

```
$ tree
.
├── fosslight_log_bin_260401_1156.txt
└── fosslight_report_bin_260401_1156.xlsx

```
- fosslight_log_bin_[datetime].txt : 실행 log
- fosslight_report_bin_[datetime].xlsx : FOSSLight Report 형태의 binary 분석 결과        
   - jar 파일 분석 시, Vulnerability Link Column에 보안취약점 정보가 추가됨  
   - Binary별 checksum, tlsh Column은 기본적으로 숨김 처리 되어 있음     
