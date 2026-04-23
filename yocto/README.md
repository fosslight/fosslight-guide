---
sort: 4
published: true
title: 🚩FOSSLight Yocto Scanner

---
# FOSSLight Yocto Scanner

<img src="https://img.shields.io/pypi/l/fosslight_yocto" alt="FOSSLight Yocto is released under the Apache-2.0." /> <img src="https://img.shields.io/pypi/v/fosslight_yocto" alt="Current python package version." /> <img src="https://img.shields.io/pypi/pyversions/fosslight_yocto" /> <a href="https://github.com/fosslight/fosslight_yocto_scanner"><img src="https://img.shields.io/badge/GitHub-Repository-purple?logo=github" alt="GitHub Repository" /></a> [![REUSE status](https://api.reuse.software/badge/github.com/fosslight/fosslight_yocto_scanner)](https://api.reuse.software/info/github.com/fosslight/fosslight_yocto_scanner)

[**FOSSLight Yocto Scanner**](https://github.com/fosslight/fosslight_yocto_scanner)는 Yocto Project 기반 빌드 과정에서 [bom.bbclass](https://github.com/fosslight/fosslight_yocto_scanner/blob/main/files_for_preparation/bom.bbclass)를 통해 추출된 결과를 활용하여, rootfs 이미지에 포함된 Package의 OSS 정보를 FOSS Report형식으로 출력해주는 Python Script입니다.  

- **출력 내용** 
    - SRC Sheet : Installed package 목록을 추출하고 OSS 정보를 출력합니다.  
    - BIN Sheet : rootfs image를 압축 해제한 폴더에서 binary를 추출한 후 binary별 OSS 정보를 출력합니다.  

- **OSS 정보 수집 기준**
    - Package별 OSS 정보는 Recipe에 정의된 메타데이터를 기반으로 OSS Name(Recipe name), OSS Version, LICENSE, Download Location을 출력합니다.  

- **⚠️주의 사항**
    - rootfs 이미지 외 target에 탑재되는 이미지 (ex- 커널, 부트로더)에 대해서는 Script가 OSS 정보를 출력해주지 않습니다. 이에 대해서는 사용자가 직접 OSS Report에 OSS 정보를 추가해야 합니다.    
   
<br><br>

## 필요 조건
{: .left-bar-title} 
- [**FOSSLight Yocto Scanner**](https://github.com/fosslight/fosslight_yocto_scanner)는 Python 3.10+ 기반에서 동작합니다.    
- OSS 정보(OSS Name, OSS Version, License)를 Binary DB로부터 추출하는 기능을 사용하려면 [DB 세팅 가이드](../scanner/etc/binary_db.md)를 참고하세요.      

<br><br>

## 설치 방법  
{: .left-bar-title}     
0. (windows의 경우) https://visualstudio.microsoft.com/ko/vs/older-downloads/ > 재배포 가능 패키지 및 빌드 도구에서 Microsoft Build Tools 설치
1. [python virtualenv](../scanner/etc/guide_virtualenv.md) 환경 세팅
2. Python package인 fosslight_yocto 설치
    ```
    $ pip3 install fosslight_yocto
    ```
<br><br>

## 실행 방법
{: .left-bar-title} 


### 준비 사항
{: .specific-title}  
1. build directory (ex-poky/build)로 이동한 후, conf/local.conf에 buildhistory와 bom을 inherit시킵니다.
    ```
    $ cd poky/build
    poky/build$ vi conf/local.conf
    INHERIT += "buildhistory"
    BUILDHISTORY_COMMIT = "1"
    
    INHERIT += "bom"
    ```
2. 최상위 directory 아래 meta/classes 디렉토리에 [bom.bbclass](https://github.com/fosslight/fosslight_yocto_scanner/blob/main/files_for_preparation/bom.bbclass) 파일을 다운로드합니다.
    - meta/classes 가 없는 경우 build에 포함되는 meta layer의 classes폴더에 bom.bbclass를 다운로드합니다.
        ```
        poky/meta/classes$ wget -O bom.bbclass "https://github.com/fosslight/fosslight_yocto_scanner/raw/main/files_for_preparation/bom.bbclass"
        ```
    - yocto 2.5 이전 버전의 경우, --runall 기능을 지원하지 않아 build시 bom.bbclass를 출력하기 위하여 bom.bbclass를 하기와 같이 수정합니다.
        ```
        addtask write_bom_info -> addtask write_bom_info before do_build
        ```
3. 이미지를 build한 후, write_bom_info를 실행합니다.
    - yocto 2.5 이후 버전
        ```
        poky/build $ bitbake <image>
        poky/build $ bitbake --runall=write_bom_info <image> (eg. bitbake --runall=write_bom_info  core-image-minimal)
        ```
    - yocto 2.5 이전 버전
        ```
        poky/build $ bitbake <image>
        ```
4. ${TOPDIR}/에 bom.json 파일과 buildhistory 폴더가 생성됩니다.  

### 실행
{: .specific-title}  
fosslight_yocto 명령어를 실행합니다.
```
$ fosslight_yocto -i [installed-package-names.txt] -b [bom.json] -p [buildhistory/packages] -a [path_to_binary_analysis]
```

- Options
    ```
     Usage
    ────────────────────────────────────────────────────────────────────
    fosslight_yocto [options] <arguments>

    📝 Description
    ────────────────────────────────────────────────────────────────────
    FOSSLight Yocto Scanner parses bom.json to extract open source
    information for packages installed on a Yocto-based model.

    📚 Guide: https://fosslight.org/fosslight-guide-en/scanner/7_yocto.html

    ⚙️  General Options
    ────────────────────────────────────────────────────────────────────
    -h                     Show this help message
    -v                     Show version information
    -o <path>              Output directory path (default: current directory)
    -f <format>            Output file format (excel, csv, opossum)

    🔍 Scanner-Specific Options
    ────────────────────────────────────────────────────────────────────
    -p <path>              Path of buildhistory/package directory
    -b <file>              bom.json file path
    -i <file>              installed-package-names.txt file path
    -ip <file>             installed-packages.txt file path
    -y <file>              sbom-info.yaml or oss-pkg-info.yaml file path
    -a <path>              Path to analyze the binaries
    -n                     Print result in BIN(Yocto) format
    -s                     Analyze source code for New Open Source
    -c                     Analyze all the source code
    -e <path>              Top build output path with bom.json to compress
                           all the source code
    -pr                    Print all data of bom.json

    💡 Examples
    ────────────────────────────────────────────────────────────────────
    # Basic scan with required inputs
    fosslight_yocto -p buildhistory/packages -b bom.json \
                    -i installed-package-names.txt -ip installed-packages.txt

    # Scan with sbom-info.yaml and output path
    fosslight_yocto -p buildhistory/packages -b bom.json \
                    -i installed-package-names.txt -ip installed-packages.txt \
                    -y sbom-info.yaml -o results/

    # Scan with binary analysis and source code analysis
    fosslight_yocto -p buildhistory/packages -b bom.json \
                    -i installed-package-names.txt -ip installed-packages.txt \
                    -a /path/to/binaries -s
    ``` 
<br><br>

## 결과
{: .left-bar-title} 
```
$ tree
.
├── fosslight_log_yocto_260413_1443.txt
├── fosslight_report_yocto_260413_1443.xlsx
└── fosslight_opossum_yocto_260413_1443.json

```
- fosslight_log_yocto_[datetime].txt : 실행 log
- fosslight_report_yocto_[datetime].xlsx : FOSSLight Yocto의 결과 (FOSSLight Report 형태)    
   - Binary별 checksum, tlsh 값은 report에 기본적으로 숨김 처리 되어 있음.
- fosslight_opossum_yocto_[datetime].json : [OpossumUI](https://github.com/opossum-tool/OpossumUI)에서 활용 가능한 Binary 분석 결과  

<br><br>

## 추가 기능
{: .left-bar-title} 

### -y 옵션 : SBOM 정보 파일 기반 Recipe/Package별 OSS 정보 로딩 기능 
{: .specific-title}
- Script를 -y 옵션으로 실행하는 경우, Recipe에 정의된 OSS 정보는 사용하지 않으며, SBOM 파일에 포함된 OSS 정보를 기준으로 OSS Report를 생성합니다.  
  단, SBOM 파일에 Recipe/Package 정보가 존재하더라도, 실제로  Install 되지 않는 Recipe/Package인 경우 , OSS Report에 포함되지 않습니다.  

    | OSS Information                     | Exclude                         | Comment                         |
    |------------------------------------|----------------------------------|---------------------------------|
    | Recipe로부터 추출한 정보           | O                                | Excluded by oss-pkg-info.yaml   |
    | oss-pkg-info.yaml로부터 추가된 정보 | oss-pkg-info.yaml에 쓰여진 값   | Info loaded from oss-pkg-info.yaml |

#### SBOM 정보 파일 작성  
{: .under-bar-title}
SBOM 정보 파일 작성을 위해 YAML 파일을 준비하며, 두 가지 포맷 중 하나를 선택하여 작성합니다.  
1. sbom-info.yaml 파일 (Yaml 형식)을 생성하고, OSS 별로 해당하는 yocto_recipe 또는 yocto_package를 입력합니다. ( 예- [sbom-info.yaml](https://github.com/fosslight/fosslight_yocto_scanner/blob/main/test_files/sbom-info.yaml) )
    -  list 타입으로 작성할 수 없는 필드 : version, download location, homepage, exclude  
    <br>

#### 실행 방법   
{: .under-bar-title}  
- Script 실행 시 -y 파라미터로 SBOM 파일을 지정합니다.  
    -  1개의 SBOM 파일 인 경우  
    ```
    (.venv)$ fosslight_yocto -i [installed-package-names.txt] -b [bom.json] -p [buildhistory/packages] -a [path_to_binary_analysis] -y [sbom-info.yaml]  
    ```
    - 2개 이상의 SBOM 파일을 load하는 경우 (,로 구분)   
    ```
    (.venv)$  fosslight_yocto  -i [installed-package-names.txt] -b [bom.json] -p [buildhistory/packages] -a [path_to_binary_analysis] -y [oss-pkg-info.yaml,sbom-info.yaml]
    ```
<br>

### -s, -c 옵션 : Source Code 분석  
{: .specific-title}
Source Code 분석을 실행합니다.  

#### 실행 방법   
{: .under-bar-title}  
- <span style="color:red">(권장)</span> Parameter -s : FOSSLight Hub에 저장되지 않은 OSS인 Recipe에 대해서만 분석합니다.  
    ```
    (.venv)$ fosslight_yocto -i [installed-package-names.txt] -b [bom.json] -p [buildhistory/packages] -s
    ```
- Parameter -c : Install되는 모든 Recipe에 대하여 Source Code 분석합니다.  
    ```
    (.venv)$ fosslight_yocto -i [installed-package-names.txt] -b [bom.json] -p [buildhistory/packages] -c  
    ```

#### 결과 파일     
{: .under-bar-title}  
- source_analysis_report.xlsx : Source Code 분석 결과 파일  
- scancode_result 폴더 : Recipe 별 결과 파일  

<br>

### -e 옵션 : Recipe별 Source Code를 복사 및 압축 기능
{: .specific-title}
- 설치되는 Recipe별 Source Code를 [recipe_name].zip으로 압축하여 package_zips 폴더에 저장하고, 이를 하나로 재압축하는 기능입니다. 단, socket, FIFO, binary 파일은 제외됩니다.  

#### 준비 사항     
{: .under-bar-title} 
- bom.bbclass의 do_write_bom_info() 함수 내 jsondata 딕셔너리에 아래 항목을 추가한 후 bom.json 파일을 생성합니다.  
    > jsondata["file_path"] = d.getVar("FILESPATH", True)

#### 실행 방법     
{: .under-bar-title} 
- Parameter -e를 추가합니다.  
    ```
    (.venv)$ fosslight_yocto -i [installed-package-names.txt] -b [bom.json] -p [buildhistory/packages] -e
    ```

#### 결과 파일      
{: .under-bar-title}  
- package_zips : recipe별 Source code를 복사하여 [recipe_name].zip 형태로 모은 폴더  
- packages.tar.gz : package_zips를 압축한 파일  
- oss_source_path.txt : SRC_URI = file://로 시작하는 Recipe에 한하여 Source code 취합한 Path를 출력한 파일  
- failed_to_compress_list.txt : 일부 또는 전체 소스에 대하여 압축이 실패한 내역을 출력한 파일  