---
sort: 4
published: true
title: 🚩FOSSLight Yocto Scanner

---
# FOSSLight Yocto Scanner

<img src="https://img.shields.io/pypi/l/fosslight_yocto" alt="FOSSLight Yocto is released under the Apache-2.0." /> <img src="https://img.shields.io/pypi/v/fosslight_yocto" alt="Current python package version." /> <img src="https://img.shields.io/pypi/pyversions/fosslight_yocto" /> [![REUSE status](https://api.reuse.software/badge/github.com/fosslight/fosslight_yocto_scanner)](https://api.reuse.software/info/github.com/fosslight/fosslight_yocto_scanner)

[**FOSSLight Yocto Scanner**](https://github.com/fosslight/fosslight_yocto_scanner)는 Yocto Project에 기반하여 build 시, rootfs 이미지에 포함되는 Package에 대한 OSS 정보를 FOSS Report형식으로 출력해주는 Python Script입니다.

- Package별 OSS 정보 출력 방법 : Recipe에 정의된 OSS 정보(OSS Name, OSS Version, LICENSE, Download Location)를 출력합니다.
이 때, OSS Name은 Recipe name으로 출력합니다.
- ⚠️**rootfs 이미지 외 target에 탑재되는 이미지 (ex- 커널, 부트로더)에 대해서는 Script가 OSS 정보를 출력해주지 않습니다.** 이에 대해서는 사용자가 직접 OSS Report에 OSS 정보를 추가해야 합니다.   
   
**Github Repository** : [https://github.com/fosslight/fosslight_yocto_scanner](https://github.com/fosslight/fosslight_yocto_scanner)  
**License** : [Apache-2.0](https://github.com/fosslight/fosslight_yocto_scanner/blob/main/LICENSE)

## 목차
- [필요 조건](#-필요-조건)
- [설치 방법](#-설치-방법)
- [실행 방법](#-실행-방법)
- [결과](#-결과)
- [동작 방식](#-동작-방식)


## 📋 필요 조건
OSS 정보(OSS Name, OSS Version, License)를 Binary DB로부터 추출하는 기능을 사용하려면 [DB 세팅 가이드](etc/binary_db.md)를 참고하세요.    


## 🎉 설치 방법    
0. (windows의 경우) https://visualstudio.microsoft.com/ko/vs/older-downloads/ > 재배포 가능 패키지 및 빌드 도구에서 Microsoft Build Tools 설치
1. [python virtualenv](etc/guide_virtualenv.md) 환경 세팅
2. Python package인 fosslight_yocto 설치
    ```
    $ pip3 install fosslight_yocto
    ```

## 🚀 실행 방법
### 방법 1. bom.bbclass를 이용하는 방법

---
[bom.bbclass](https://github.com/fosslight/fosslight_yocto_scanner/blob/main/files_for_preparation/bom.bbclass) 를 이용하여 추출한 결과를 FOSSLight Yocto를 이용하여 OSS Report형태로 변환합니다. 
- Sheet 별 출력 사항:
    - SRC Sheet : Installed package 목록을 추출하고 OSS 정보를 출력합니다.
    - BIN Sheet : rootfs image를 압축 해제한 폴더에서 binary를 추출한 후 binary별 OSS 정보를 출력합니다. 

---

#### 준비 사항
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

#### 실행
fosslight_yocto 명령어를 실행합니다.
```
$ fosslight_yocto -i [installed-package-names.txt] -b [bom.json] -p [buildhistory/packages] -a [path_to_binary_analysis]
```

- Options
    ```
    Mandatory
        -p <path>                      Path of buildhistory/package
        -b <file_with_path>            bom.json
        -i <file_with_path>            installed-package-names.txt
        -ip <file_with_path>           installed-packages.txt

    Optional
        -h                             Print help message
        -v                             Print FOSSLight yocto version
        -y <file_with_path>            oss-pkg-info.yaml
        -a <path>                      Path to analyze the binaries
        -n                             Print result in BIN(Android) format        
        -s                             Analyze source code for New Open Source
        -c                             Analyze all the source code
        -e <path>                      Top build output path with bom.json to compress all the source code (ex. /data001/projectA/build)
        -o <path>                      Output Path
        -f <format>                    Output file format (excel, csv, opossum)
        -pr                            Print all data of bom.json
    ``` 

### 방법 2. meta-doubleopen을 이용하는 방법
---
[meta-doubleopen](https://github.com/doubleopen-project/meta-doubleopen)를 이용하여 spdx.json으로 추출하고 FOSSLight Yocto를 이용하여 FOSS Report 형식으로 변환합니다.
- Sheet 별 출력 사항:
    - SRC_distributed: rootfs 이미지에 포함되는 Package
    - SRC_recipe: build에 포함되는 Recipe
    - SRC_not_distributed: rootfs 이미지에 포함되지 않는 Package

- Package별 OSS 정보 출력 방법 : Recipe에 정의된 OSS 정보(OSS Name, OSS Version, LICENSE, Download Location, Homepage)를 출력합니다. 이 때, OSS Name은 Recipe name으로 출력합니다.

---

#### 준비 사항
[meta-doubleopen](https://github.com/doubleopen-project/meta-doubleopen)을 이용하여 이미지에 대한 spdx.json 파일을 생성합니다.

#### 실행
fosslight_doubleopen 명령어를 실행합니다.
```
$ source venv/bin/activate
(.venv) $  fosslight_doubleopen -f core-image-minimal.spdx.json
```
- Option f {[image].spdx.json} : meta-doubleopen 실행 결과 생성되는 spdx.json 파일

## 📁 결과

```
$ tree
.
├── fosslight_log_220904_0912.txt
├── fosslight_report_220904_0912.xlsx
└── fosslight_opossum_220904_0912.json

```
- fosslight_log_[datetime].txt : 실행 log
- fosslight_report_[datetime].xlsx : FOSSLight Yocto의 결과 (FOSSLight Report 형태)    
   - Binary별 checksum, tlsh 값은 report에 기본적으로 숨김 처리 되어 있음.
- fosslight_opossum_[datetime].json : [OpossumUI](https://github.com/opossum-tool/OpossumUI)에서 활용 가능한 Binary 분석 결과     
