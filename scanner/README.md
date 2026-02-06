---
sort: 1
published: true
title: 📦FOSSLight Scanner (All-in-One)
---
# FOSSLight Scanner

<a href="https://github.com/fosslight/fosslight_scanner/blob/main/LICENSE"><img src="https://img.shields.io/pypi/l/fosslight_scanner" alt="FOSSLight Scanner is released under the Apache-2.0." /></a> <a href="https://pypi.org/project/fosslight-scanner/"><img src="https://img.shields.io/pypi/v/fosslight_scanner" alt="Current python package version." /></a> <img src="https://img.shields.io/pypi/pyversions/fosslight_scanner" />

FOSSLight Scanner는 의존성(Dependency), 소스코드, 바이너리에 포함된 오픈소스 정보를 자동으로 분석하기 위한 통합 스캐닝 도구입니다. Git 또는 wget으로 다운로드 가능한 소스뿐 아니라 로컬 소스 경로를 입력하여 분석을 수행할 수 있으며, 그 결과는 SBOM 형식인 FOSSLight Report 형태로 생성됩니다.  
<br />
FOSSLight Scanner는 다음 3가지 스캐너로 구성되어 있으며, 각각 다른 분석 영역을 담당합니다.  

1. [FOSSLight Dependency Scanner](1_dependency.md)   
    Package manager 또는 빌드 시스템을 통해 사용되는 의존성(dependency)을 분석하여 오픈소스 정보를 추출하는 스캐너입니다.   
    npm, pypi, maven, gradle 등 다양한 패키지 매니저를 지원하며, 직접 의존성뿐 아니라 그에 따라 포함되는 하위 의존성까지 함께 분석합니다.     
2. [FOSSLight Source Scanner](2_source.md)  
    소스 코드를 분석하여 라이선스 문구, 저작권 문자열, 그리고 코드 스니펫 등의 오픈소스 관련 정보를 탐지합니다.     
3. [FOSSLight Binary Scanner](3_binary.md)  
    Binary 파일을 대상으로 오픈소스 분석을 수행합니다.   
    바이너리 자체를 리버스 엔지니어링하는 방식이 아니라, 바이너리 목록을 수집한 뒤 내부 DB에 존재하는 OSS 정보와 매칭하여 해당 바이너리에 포함된 오픈소스를 식별합니다.  

**Github Repository** : [https://github.com/fosslight/fosslight_scanner](https://github.com/fosslight/fosslight_scanner)  
**License** : [Apache-2.0](https://github.com/fosslight/fosslight_scanner/blob/main/LICENSE)
<br><br>

## 목차
  - [필요 조건](#필요-조건)
  - [설치 방법](#설치-방법)
  - [실행 방법](#실행-방법)
  - [결과](#결과)
  - [Docker를 이용하여 설치 및 실행 방법](#docker를-이용하여-설치-및-실행-방법)

<br><br>


## 필요 조건

1. [**FOSSLight Scanner**](https://github.com/fosslight/fosslight_scanner)는 Python 3.10 이상(공식 지원 버전: 3.10~3.12) 환경에서 동작하며, pip3 명령을 통해 설치할 수 있습니다.    
2. Jar 파일을 분석하려면 [**Open Source JDK(Java)**](https://openjdk.java.net)를 설치해야 합니다.  
3. (windows의 경우) [Microsoft Build Tools (Microsoft Visual C++ 14.0+)](https://visualstudio.microsoft.com/ko/visual-cpp-build-tools/)를 설치해야 합니다.  
<br><br>


## 설치 방법
{: .left-bar-title} 
### Bee에서 설치(LGE Only)
{: .specific-title}
[Bee]( https://docs.bee0.lge.com/docs/dev-tools/fosslight/)에서 FOSSLight Scanner를 설치하여 사용할 수 있습니다.  

### 일반적인 설치 방법
{: .specific-title}   
FOSSLight Scanner는 pip3를 이용하여 설치할 수 있습니다.      
[python3 virtualenv](etc/guide_virtualenv.md) 환경에서 설치할 것을 권장합니다.  

```
$ pip3 install fosslight_scanner
```

### 설치 에러 발생 시
{: .specific-title}
'Cargo, the Rust package manager, is not installed or is not on PATH.' 에러 발생 시, cargo, rust를 아래와 같이 설치한 이후, 다시 FOSSLight Scanner를 설치합니다.
- Linux, macOS
  ```
  $ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
  $ export PATH="$HOME/.cargo/bin:$PATH"
  ```
- Windows :
[https://www.rust-lang.org/tools/install](https://www.rust-lang.org/tools/install)에서 rust-init.exe 파일 다운로드 후 설치
<br><br> 

## 실행 방법
{: .left-bar-title} 
### Mode별 실행 방법 및 Parameters
{: .specific-title}
```
    📖 Usage
    ────────────────────────────────────────────────────────────────────
    fosslight [mode] [options] <arguments>

    📝 Description
    ────────────────────────────────────────────────────────────────────
    FOSSLight Scanner performs comprehensive open source analysis by running
    multiple modes (Source, Dependency, Binary) together. It can download
    source code from URLs (git/wget) or analyze local paths, and generates
    results in OSS Report format.

    📚 Guide: https://fosslight.org/fosslight-guide/scanner/

    🔧 Modes
    ────────────────────────────────────────────────────────────────────
    all (default)          Run all modes (Source, Dependency, Binary)
    source                 Run FOSSLight Source analysis only
    dependency             Run FOSSLight Dependency analysis only
    binary                 Run FOSSLight Binary analysis only
    compare                Compare two FOSSLight reports

    Note: Multiple modes can be specified separated by comma
          Example: fosslight source,binary -p /path/to/analyze

    ⚙️  General Options
    ────────────────────────────────────────────────────────────────────
    -p <path>              Path to analyze
                           • Compare mode: path to two FOSSLight reports (excel/yaml)
    -w <url>               URL to download and analyze (git clone or wget)
    -f <format>            Output format (excel, csv, opossum, yaml, spdx-yaml, spdx-json, spdx-xml, spdx-tag, cyclonedx-json, cyclonedx-xml)
                           • Compare mode: excel, json, yaml, html
                           • Multiple formats: ex) -f excel yaml json (separated by space)
    -e <pattern>           Exclude paths from analysis (files and directories)
                           ⚠️  IMPORTANT: Always wrap in quotes to avoid shell expansion
                           Example: fosslight -e "test/" "*.jar"
    -o <path>              Output directory or file name
    -c <number>            Number of processes for source analysis
    -r                     Keep raw data from scanners
    -t                     Hide progress bar
    -h                     Show this help message
    -v                     Show version information
    -s <path>              Apply settings from JSON file(check format with 'setting.json' in this repository)
                           Note: CLI flags override settings file
                           Example: -f yaml -s setting.json → output is .yaml
    --no_correction        Skip OSS information correction with sbom-info.yaml
                           (Correction only supports excel format)
    --correct_fpath <path> Path to sbom-info.yaml file for correction
    --ui                   Generate UI mode result file
    --recursive_dep        Recursively analyze dependencies

    🔍 Mode-Specific Options
    ────────────────────────────────────────────────────────────────────
    For 'all' or 'binary' mode:
      -u <db_url>          Database connection string
                           Format: postgresql://username:password@host:port/database

    For 'all' or 'dependency' mode:
      -d <args>            Additional arguments for dependency analysis

    💡 Examples
    ────────────────────────────────────────────────────────────────────
    # Scan current directory with all scanners
    fosslight

    # Scan specific path with exclusions
    fosslight -p /path/to/source -e "test/" "node_modules/" "*.pyc"

    # Generate output in specific format
    fosslight -p /path/to/source -f yaml

    # Run specific modes only
    fosslight source,dependency -p /path/to/source

    # Download and analyze from git repository
    fosslight -w https://github.com/user/repo.git -o result_dir

    # Compare two FOSSLight reports
    fosslight compare -p report_v1.xlsx report_v2.xlsx -f excel

    # Run with database connection for binary analysis
    fosslight binary -p /path/to/binary -u "postgresql://user:pass@localhost:5432/sample"

```
- Ex.1 Local의 Path를 분석하는 방법  
```
fosslight -p /home/source_path
```

- Ex.2 링크를 다운로드 받고 분석하는 방법    
```
fosslight -o test_result_wget -w "https://github.com/LGE-OSS/example.git"
```

- Ex.3 FOSSLight Report SBOM 결과 비교하여 변경/추가/삭제 내역 확인하는 방법  
```
fosslight compare -p FOSSLight_before_proj.yaml FOSSLight_after_proj.yaml -o test_result
```

### 실행 Parameter를 json으로 저장하여 호출하는 방법  
{: .specific-title}
1. [setting.json](https://github.com/fosslight/fosslight_scanner/blob/main/tests/setting.json) 포맷으로 실행 parameter별 값을 json 파일로 작성하여 저장
2. 실행시, -s 로 생성한 setting.json을 호출합니다.
```
fosslight -s setting.json
```
🛈 json 파일에 작성한 parameter보다 실행 시 호출한 값을 우선합니다.      
ex. '-f yaml -s setting.json'로 호출시, yaml 포맷의 output을 출력합니다.
<br><br>

## 결과  
{: .left-bar-title} 
### 오픈소스 분석 결과물 (all 모드)  
{: .specific-title}
<pre>
test_result/
├── fosslight_log
│   └── fosslight_log_all_260204_0925.txt
├── fosslight_report_all_260204_0925.xlsx
└── fosslight_raw_data (-r option 있는 경우)
    ├── fosslight_report_dep_260204_0925.xlsx
    ├── fosslight_report_src_260204_0925.xlsx
    └── fosslight_report_bin_260204_0925.xlsx
</pre>
- fosslight_report_all_(datetime).xlsx : Source 분석, Binary 분석, Dependency 분석 결과가 작성된 FOSSLight Report 형식의 파일
- fosslight_raw_data directory: 분석 결과 Raw Data 파일이 생성되는 폴더 (-r option 있는 경우)
  - fosslight_report_dep_(datetime).xlsx : Dependency 분석 결과 파일
  - fosslight_report_src_(datetime).xlsx : Source 분석 결과 파일
  - fosslight_report_bin_(datetime).xlsx : Binary 분석 결과 파일
 
### fosslight_report_all_(datetime).xlsx  
{: .specific-title}

- **Scanner Info sheet**  
  실행된 스캐너 및 실행 환경 정보를 출력하는 sheet입니다.  
  -  **Tool information** : 실행된 스캐너명 및 버전이 표시됩니다.  
  -  **Start time** : 스캐너 실행 시작 시간이 표시됩니다.  
  -  **Python version** : 스캐너가 실행된 Python 버전이 표시됩니다.   
  -  **Analyzed path** : 분석된 path가 표시됩니다. ('-p' 옵션을 통해 입력된 분석 path 또는 default로 스캐너가 실행된 path)  
  -  **Excluded path** : 분석시 제외된 path가 표시됩니다. ('-e' 옵션을 통해 입력된 path)  
  -  **Comment** : 스캐너별 분석 결과가 표시됩니다.   
      - **fosslight_dependency**
          - 패키지 매니저 manifest 파일 존재하지 않는 경우  
            <pre>
            Ex) [fosslight_dependency v4.1.31] No Package manager detected.
            </pre>
                
          - 패키지 매니저 분석이 성공한 경우  
            -[Success]  {package manager}:
              {project path}: {manifest file}
              <pre>
              Ex) [fosslight_dependency v4.1.31] Dependency Analysis Summary
                  - [Success]  pypi:
                  /home/worker/sample_code/example: requirements.txt
              </pre>  

          - 패키지 매니저 분석 실패한 경우   
            -[Fail]  {package manager}:
              {project path}: {manifest file}
              If analysis fails, see fosslight_log*.txt and the prerequisite guide: https://fosslight.org/fosslight-guide-en/scanner/1_dependency.html#-prerequisite
              <pre>
                Ex) [fosslight_dependency v4.1.31] Dependency Analysis Summary
                    - [Fail]  yarn:
                    /home/worker/sample_code/example: package.json
                    If analysis fails, see fosslight_log*.txt and the prerequisite guide: https://fosslight.org/fosslight-guide-en/scanner/1_dependency.html#-prerequisite
              </pre>
      - **fosslight_source**  
          - Scanned files : 전체 분석된 파일 수  
          - Detected source : 오픈 소스가 검출된 파일 수입니다.  
            - 오픈 소스가 검출되지 않으면 'Detected source : 0'으로 표시됩니다.   
          - KB Enable/KB Unreachable : KB DB의 활성화 여부입니다.    
          - Mode : Source 분석에 사용된 모드입니다.    
          
      - **fosslight_binary**  
          - Detected binaries: 오픈 소스가 발견된 바이너리 수입니다.  
          - Scanned Files : 전체 분석된 파일 수입니다.  

- **DEP_FL_Dependency, SRC_FL_Source, BIN_FL_Binary sheet**   
  sheet명에서 실행된 스캐너를 확인하고 해당 sheet에서 스캐너별 실행된 결과를 확인할 수 있습니다.
  - Exclude 컬럼이 체크된 Row  
    - test(s), doc(s), 숨김 파일 or 폴더는 Exclude 체크됩니다.    
    - sbom-info.yaml을 load한 경우, load한 데이터를 append하고 중복된 파일에 대한 분석 결과는 Exclude 체크됩니다.
  - Comment 컬럼  
    - Add/Loaded by ** : ** 으로부터 load한 Row  
    - Excluded by ** : ** 으로 인해 Exclude된 Row         

### compare 모드 결과
{: .specific-title}  
<pre>
test_result/
├── fosslight_log
│   └── fosslight_log_all_260205_1101.txt
└── fosslight_compare_260205_1101.xlsx
</pre>
- fosslight_compare_(datetime).xlsx : 두 개의 SBOM 비교 결과를 add/delete/change 형태의 테이블로 작성된 파일  
<br><br>  

## Docker를 이용하여 설치 및 실행 방법  
{: .left-bar-title} 
> ⚠️ Docker 환경에서는 FOSSLight Source/Binary Scanner만 실행되며, FOSSLight Dependency Scanner는 지원되지 않습니다.

1. FOSSLight Scanner Docker 이미지 다운로드
   
    선택 1. Dockerhub에서 fosslight_scanner 다운로드 
    ```
    $ docker pull fosslight/fosslight_scanner
    ```
    선택 2. [Dockerfile](https://github.com/fosslight/fosslight_scanner/blob/main/Dockerfile)을 이용하여 이미지 빌드 (선택 1에서 지원하지 않는 OS인 경우)
    ```
    $ docker build -t fosslight_scanner .
    ```
    
2. 빌드한 이미지로 실행합니다.     
ex. Output 경로 : /Users/git/temp/output, 분석 경로 : /Users/git/temp/dir_to_analyze
```
$ docker run -it -v /Users/git/temp/dir_to_analyze:/app/dir_to_analyze -v /Users/git/temp/output:/app/output fosslight_scanner -p dir_to_analyze -o output
```
