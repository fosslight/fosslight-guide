---
published: true
---
# FOSSLight Android Scanner

<img src="https://img.shields.io/pypi/l/fosslight_android" alt="FOSSLight Android is released under the Apache-2.0." /> <img src="https://img.shields.io/pypi/v/fosslight_yocto" alt="Current python package version." /> <img src="https://img.shields.io/pypi/pyversions/fosslight_yocto" /> [![REUSE status](https://api.reuse.software/badge/github.com/fosslight/fosslight_android_scanner)](https://api.reuse.software/info/github.com/fosslight/fosslight_android_scanner)

[**FOSSLight Android Scanner**](https://github.com/fosslight/fosslight_android_scanner)는 Android 모델에 탑재되는 Binary를 모두 나열하여 각 Binary별로 Open Source가 사용되었는지 확인하고, 고지해야 할 사항이 OSS 고지문(ex. NOTICE.html)에 적절하게 포함되었는지 확인하기 위해 수행합니다.

## 목차
- [필요 조건](#-필요-조건)
- [설치 방법](#-설치-방법)
- [실행 방법](#-실행-방법)
- [결과](#-결과)
- [추가 기능](#-추가-기능)


## 📋 필요 조건
[**FOSSLight Android Scanner**](https://github.com/fosslight/fosslight_android_scanner)는 Python 3.7+ 기반에서 동작합니다.  
OSS 정보(OSS Name, OSS Version, License)를 Binary DB로부터 추출하는 기능을 사용하려면 [DB 세팅 가이드](etc/binary_db.md)를 참고하세요.    

## 🎉 설치 방법
1. [python 3.7 + virtualenv](etc/guide_virtualenv.md) 환경 세팅
2. Python package인 fosslight_android 설치
    ```
    $ pip3 install fosslight_android
    ```

## 🚀 실행 방법
FOSSLight Android를 실행합니다. (이때, build 산출물 (/out directory) 및 build log file (android.log) 가 android source path 에 존재해야 합니다.)

#### 준비 사항
Android Build
Android 모델을 build clean 상태에서 build하여 산출물(/out directory) 및 build log(android.log)를 확보합니다. 
```
(Android native source build 예)
$ source ./build/envsetup.sh
$ make clean
$ lunch aosp_hammerhead-user
$ make -j4 2>&1 | tee android.log
```

{::options parse_block_html="true" /}
<details>
    <summary markdown="span" style="font-weight:bold">Android 7.0 이전 version의 모델</summary>
Android 7.0 이전 version의 모델일 경우, 먼저 module-info.mk 파일을 build/core/tasks/하위에 위치시킨 후 build합니다. (build시 module-info.json 파일을 생성하게 하기 위함)

```
$ wget https://raw.githubusercontent.com/aosp-mirror/platform_build/android-cts-7.0_r33/core/tasks/module-info.mk
$ mv ./module-info.mk ./build/core/tasks
```

</details>   

#### 실행
fosslight_android 명령어를 실행합니다.
이때, build 산출물 (/out directory) 및 build log file (android.log) 가 android source path 에 존재해야 합니다.

```
(venv)$ fosslight_android  -s [android source path] -a [build log file name]
```

- Options
    ```
    Options:
        Mandatory
            -s <android_source_path>       Path to analyze
            -a <build_log_file_name>       The file must be located in the android source path.

        Optional
            -h                             Print help message
            -m                             Analyze the source code for the path where the license could not be found.
            -p                             Check files that should not be included in the Packaging file.
            -f                             Print result of Find Command for binary that can not find Source Code Path.
            -t                             Collect NOTICE for binaries that are not added to NOTICE.html.
            -d                             Divide needtoadd-notice.html by binary.
            -i                             Disable the function to automatically convert OSS names based on AOSP.
            -r <result.txt>                result.txt file with a list of binaries to remove.
    ``` 

## 📁 결과
- fosslight_report_[datetime].xlsx : FOSSLight Android 분석 결과 (FOSSLight Report 형태)    
   - jar 파일 분석 시, Vulnerability Link Column이 FOSSLight-Report_[datetime].xlsx에 추가 됨.    
- fosslight_binary_android_[datetime].txt : Binary별 checksum, tlsh 값이 출력된 결과
- fosslight_log_[datetime].txt : 실행 log    
- REMOVED_BIN_BY_DUPLICATION_[datetime].txt : output path내 binary name과 checksum이 동일한 파일이 2개 이상 존재하여 FOSSLight Report에서 중복 제거된 목록입니다.
더불어 -r 옵션으로 추가로 제거된 목록도 출력됩니다.

| Column           | 내용                                                                                            |
|:-----------------|:----------------------------------------------------------------------------------------------|
| Binary Name      | out directory 내 존재하는 Binary 목록 (binary, library, APK, font 등 )                                |  
| Source Code Path | Binary를 구성하는 Source Code의 Path 정보 (LOCAL_PATH)                                                |  
| NOTICE.html      | NOTICE 파일에 Binary 정보가 표시되었는지 여부를 표시합니다. Open Source가 사용된 Binary라면, ok여야 합니다.                  |
| OSS Name         | Binary DB 에서 load한 OSS 정보 또는 Android Open Source Project인 경우 해당 Repository 기반의 이름을 출력합니다.     |
| OSS Version      | Open Source의 Version. Binary DB에서 매칭하는 Binary의 정보를 가져와서 보여줍니다.                                |
| License          | 1. Binary DB 2. Source Path의 "MODULE_LICENSE_xxxxxx" 3.{MODULE_NAME}.meta_lic에서 찾은 정보를 출력합니다. |
| Need Check       | 'O'인 경우, 검토가 필요합니다.                                                                           |
| Comment          | 검토가 필요한 사항을 출력합니다.                                                                            |

## 🚗 추가 기능
---
하기 옵션을 통해 부가 기능을 활용할 수 있습니다.
- Option: -b, -n, -c : NOTICE.html에 Binary 이름 포함 여부 확인
- Option: -p : Packaging 파일에 포함되지 않아야 하는 파일 확인
- Option: -f : Source Code Path를 찾지 못하는 binary에 대하여 Find Command 실행 결과 출력
- Option: -i : Android reference 의 repository기준으로 OSS Name 자동 출력 끄기
- Option: -r : 특정 binary를 FOSSLight Report에서 중복 제거. Android native와 vendor가 분리되어 build되는 구조에서 사용하는 옵션으로 중복으로 포함되는 Binary를 제거합니다. vendor에 대한 FOSSLight Android 실행시 -r 옵션으로 android native 결과 생성되는 result_*.txt 파일을 parameter로 추가합니다.
- Option: -m : License가 빈칸인 부분에 대해 자동으로 Source path 내 Source code 분석(소스 파일 내 License text 기반 License 검출)을 실행하여 License 값을 채워줍니다. (그러나 분석에 시간이 오래 걸립니다. Android native에서 44개 Path기준 약 35분 소요)

---

### -b, -n, -c : NOTICE.html에 Binary 이름 포함 여부 확인 기능
- **OSS가 사용 된 경우 ( NOTICE.html이 ok 또는 ok(NA)인 경우 )** FOSSLight Report의 BIN(Android) sheet내 NOTICE.html column의 값이 ok 또는 ok(NA)라면, 그 Binary 이름은 NOTICE.html에 포함되어 있어야 합니다.      
Binary 이름이 NOTICE.html에 포함되었는지 확인하는 방법은 다음과 같습니다.
    1. NOTICE.html 의 값을 ok, ok(NA)만 조회합니다. 
    2. Binary Name column에서 모든 항목을 선택하여 Copy합니다.
    3. binary.txt 라는 파일을 생성하고 2에서 Copy한 내용을 저장합니다.
    4. NOTICE.html을 binary.txt와 같은 directory에 위치시킵니다.
    5. "-c" option을 사용하여 결과를 추출합니다. 이때, -c option parameter로 ok 항목에 대한 체크사항이므로 ok라고 입력합니다.
       ```
       (command)
       (venv)$ fosslight_android -b [binary.txt] -n [NOTICE.html] -c [ok|nok]
       
       (ex)
       (venv)$ fosslight_android -b binary.txt -n NOTICE.xml,NOTICE_VENDOR.xml,NOTICE_PRODUCT.xml -c ok
       ```
    6. result.txt 파일을 열면, nok 항목인 binary 목록이 출력됩니다.

### -p: Packaging 파일에 포함되지 않아야 하는 파일 확인
공개할 Source Code 취합시, 포함되지 말아야 하는 파일 이름, 확장자, 디렉토리를 체크합니다.      

사전 준비      
- Packaging Config File : 체크할 항목을 json 형식의 pkgConfig.json 파일 이름으로 생성합니다.

Example : pkgConfig.json
    ```
    {
       "Prohibited_File_Names":[
          "key_file",
          "confidential_key"
       ],
       "Prohibited_File_Extensions":[
          "exe",
          "jar"
       ],
       "Prohibited_Path":[
          "confidential",
          ".git"
       ]
    }
    ```

    - Prohibited_File_Names : 검출하려는 파일 이름 
    - Prohibited_File_Extensions : 검출하려는 파일 확장자 
    - Prohibited_Path : 검출할 파일 디렉토리
    - 공개할 소스 코드를 취합한 디렉토리 위치 혹은 압축 파일 확인 
      - 공개할 소스 코드 취합한 디렉토리나 압축 파일 내 압축된 파일이 있을 경우, 압축을 해제하여 검색합니다. 
      - 압축 해제 지원 확장자 : tar, tar.gz, zip

**실행 방법**
1. Packaging Config File을 pkgConfig.json 파일명(json 형식)으로 준비합니다.
2. -p 옵션을 추가하여 실행합니다. (-p : 공개할 소스 코드를 취합한 Path 혹은 압축 파일)
    ```
    (venv)$ fosslight_android -p [A path or compressed file containing the source code to be disclosed]
     
    ex
    (venv)$ fosslight_android -p /home/test/sourceCodeToBeDisclosed.tar.gz
    ```

3. 결과 확인 
검출된 항목별로 추출된 목록을 보여줍니다.        
       
결과 example :       

    ```
    (venv)$ fosslight_android  -p /home/test/sourceCodeToBeDisclosed.tar.gz
    1. Prohibited file names : 1
    sourceCode/executable/LgeOscClient/confidential_key
    2. Prohibited file extension : 4
    sourceCode/executable/Report_Jenkins_ubuntu.exe
    sourceCode/executable/ReportTool_v3.03_181128U.jar
    sourceCode/executable/Protex_Create_Upload_Analyze_v3.03_181128U.jar
    sourceCode/executable/ReportTool_CLI_v3.03_181128U.jar
    3. Prohibited Path : 2
    sourceCode/.git
    sourceCode/executable/LgeOscClient/confidential
    4. Fail to read : 0
    ```
   
   - Prohibited file names : 공개할 소스 코드 중 파일명에 pkgConfig.json의 Prohibited_File_Names 값을 포함하는 경우 출력합니다.
   - Prohibited file extension : 공개할 소스 코드 중 파일 확장자가 pkgConfig.json의 Prohibited_File_Extensions 값인 경우 출력합니다.
   - Prohibited Path : 공개할 소스 코드 중 파일 Path 중 pkgConfig.json의 Prohibited_Path 값을 포함하는 경우 출력합니다.
   - Fail to read : 압축 해제에 실패한 파일 목록을 출력합니다.

### -f: Source Code Path를 찾지 못하는 binary에 대하여 Find Command 실행 결과 출력
Source Code Path를 찾지 못하는 Binary에 대하여 Android의 Source Path내 폴더 (out directory, .으로 시작하는 숨김 directory 제외)별로 Find Command 실행 결과를 출력합니다.     

1. -f 옵션을 추가하여 실행합니다.
    ```commandline
    (venv)$ fosslight_android  -s [android source path] -a [build log file name] -f
     
    ex
    (venv)$ fosslight_android  -s /home/soim/android/source -a android.log -f
    ```

2. 결과 확인        
Source Code Path를 찾지 못하는 Binary별 Find command 실행 결과는 'FIND_RESULT_OF_BINARIES.txt' 파일로 생성됩니다.
단, Source Code Path를 찾지 못하는 Binary가 없을 경우 해당 파일은 생성되지 않습니다.

### -i: OSS Name 자동 완성 기능 끄기
FOSSLight Android는 Binary DB에서 OSS 정보를 찾을 수 없는 경우이거나 OSS Name이 "Android Open Source Project"인 경우, Source Code Path를 기준으로 [Android Native](https://android.googlesource.com/platform)에 있는 저장소라면 OSS Name을 자동으로 출력해줍니다.      
OSS Name 자동 완성 기능을 끄고자 할 경우 선택합니다.      

### -r: 특정 binary를 FOSSLight Report에서 중복 제거
하나의 Model에 탑재하는 Android native와 vendor가 분리된 output으로 생성되는 경우에 한하여 활용합니다.        
vendor에 대한 FOSSLight Android 실행시 -r 옵션을 이용하여 Android native에도 포함되는 binary를 중복 제거합니다.       

중복 제거 조건 : Binary name이 같고 checksum이 같거나, Binary name이 같고 TLSH 값 차이가 120이하인 경우
중복 제거된 binary는 REMOVED_BIN_BY_DUPLICATION.txt에 출력됩니다.

1. -r 옵션을 추가하여 실행합니다. 
    ```commandline
    (venv)$ fosslight_android -s [vendor_source_path] -a [android_build_log_file] -r [android_native_result.txt]
     
    ex
    (venv)$ fosslight_android -s [vendor_source_path] -a android.log -r android_native_result.txt
    ```

2. 결과 확인         
android_native_result.txt와 중복된 binary는 FOSSLight-Report.xlsx에서 제거되고, REMOVED_BIN_BY_DUPLICATION.txt에 출력됩니다.


### -m: 소스 코드 분석하여 License 출력
License 정보를 못 찾은 경우에 한하여 FOSSLight Source를 이용하여 Source code를 분석한 결과를 License란에 출력합니다.        

1. -m 옵션을 추가합니다.
    ```commandline
    (venv)$ fosslight_android -s [vendor_source_path] -a [android_build_log_file] -m
     
    ex
    (venv)$ fosslight_android -s [vendor_source_path] -a android.log -m
    ```

2. 결과 확인         
FOSSLight Report의 License column에 분석한 결과가 채워집니다.              
추가로 source_analyzed_[datetime] 폴더에 소스 코드별 분석한 결과가 생성됩니다.       