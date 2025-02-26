---
published: true
---
# FOSSLight Android Scanner

<img src="https://img.shields.io/pypi/l/fosslight_android" alt="FOSSLight Android is released under the Apache-2.0." /> <img src="https://img.shields.io/pypi/v/fosslight_yocto" alt="Current python package version." /> <img src="https://img.shields.io/pypi/pyversions/fosslight_yocto" /> [![REUSE status](https://api.reuse.software/badge/github.com/fosslight/fosslight_android_scanner)](https://api.reuse.software/info/github.com/fosslight/fosslight_android_scanner)

[**FOSSLight Android Scanner**](https://github.com/fosslight/fosslight_android_scanner)는 Android 모델에 탑재되는 Binary를 모두 나열하여 각 Binary별로 Open Source가 사용되었는지 확인하고, 고지해야 할 사항이 OSS 고지문(ex. NOTICE.html)에 적절하게 포함되었는지 확인하기 위해 수행합니다.

**Github Repository** : [https://github.com/fosslight/fosslight_android_scanner](https://github.com/fosslight/fosslight_android_scanner)    
**License** : [Apache-2.0](https://github.com/fosslight/fosslight_android_scanner/blob/main/LICENSE)

## 목차
- [필요 조건](#-필요-조건)
- [설치 방법](#-설치-방법)
- [실행 방법](#-실행-방법)
- [결과](#-결과)
- [추가 기능](#-추가-기능)


## 📋 필요 조건
[**FOSSLight Android Scanner**](https://github.com/fosslight/fosslight_android_scanner)는 Python 3.8+ 기반에서 동작합니다.  
OSS 정보(OSS Name, OSS Version, License)를 Binary DB로부터 추출하는 기능을 사용하려면 [DB 세팅 가이드](etc/binary_db.md)를 참고하세요.    

## 🎉 설치 방법
1. [python 3.8 + virtualenv](etc/guide_virtualenv.md) 환경 세팅
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
<summary markdown="span">**Android 7.0 이전 version의 모델**</summary>

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
            -e <path1> <path2..>           Path to exclude from source analysis.
            -p                             Check files that should not be included in the Packaging file.
            -f                             Print result of Find Command for binary that can not find Source Code Path.
            -t                             Collect NOTICE for binaries that are not added to NOTICE.html.
            -d                             Divide needtoadd-notice.html by binary.
            -i                             Disable the function to automatically convert OSS names based on AOSP.
            -r <result.txt>                result.txt file with a list of binaries to remove.
    ``` 
    

## 📁 결과
- fosslight_report_[datetime].xlsx : FOSSLight Android 분석 결과 (FOSSLight Report 형태)    
   - Binary별 checksum, tlsh 값은 report에 기본적으로 숨김 처리 되어 있음.    
- fosslight_log_[datetime].txt : 실행 log
- notice file
   - notice_to_fosslight_hub_{datetime}.zip : notice file이 1개 이상인 경우 .zip으로 압축됨
   - notice_to_fosslight_hub_{datetime}.{extension} : notice file이 1개인 경우(ex. NOTICE.html) 
- REMOVED_BIN_BY_DUPLICATION_[datetime].txt : output path내 binary name과 checksum이 동일한 파일이 2개 이상 존재하여 FOSSLight Report에서 중복 제거된 목록입니다.
더불어 -r 옵션으로 추가로 제거된 목록도 출력됩니다.



| Column           | 내용                                                                                            |
|:-----------------|:----------------------------------------------------------------------------------------------|
| Binary Name      | out directory 내 존재하는 Binary 목록 (binary, library, APK, font 등 )                                |  
| Source Code Path | Binary를 구성하는 Source Code의 Path 정보 (LOCAL_PATH)                                                |  
| Notice           | NOTICE 파일에 Binary 정보가 표시되었는지 여부를 표시합니다. Open Source가 사용된 Binary라면, ok여야 합니다.<br>&ensp;&ensp;- ok : Source Path에 NOTICE 파일이 있고, 최종 output NOTICE (ex. NOTICE.html)에 Binary 포함<br>&ensp;&ensp;- ok(NA) :  Source Path에 NOTICE 파일이 없으나, 최종 output NOTICE (ex. NOTICE.html)에 Binary 포함<br>&ensp;&ensp;- nok :  Source Path에 NOTICE 파일이 없고, 최종 output NOTICE (ex. NOTICE.html)에 Binary가 포함되어 있지 않음<br>&ensp;&ensp;- nok(NA) :  Source Path에 NOTICE 파일이 있음에도, 최종 output NOTICE (ex. NOTICE.html)에 Binary가 포함되어 있지 않음<br>&ensp;&ensp;- CANNOT_FIND_NOTICE_HTML : NOTICE.html 파일을 찾을 수 없음. (이 경우, Script 실행 시, -n [NOTICE.html_path]를 주어 NOTICE.html 파일 위치를 Parameter로 줘야 함)     |
| OSS Name         | LGE Binary DB에서 매칭하는 Binary의 정보를 가져와서 보여줍니다.     |
| OSS Version      | LGE Binary DB에서 매칭하는 Binary의 정보를 가져와서 보여줍니다.                               |
| License          | 하기 정보로 부터 추출한 Open Source License 를 보여줍니다.<br>&ensp;&ensp;- LGE Binary DB에서 매칭되는 Binary의 정보<br>&ensp;&ensp;- Source Code Path 내 "MODULE_LICENSE_xxxxxx"와 같이 License를 명시한 file을 읽어서 표시<br>&ensp;&ensp;- output의 {MODULE_NAME}.meta_lic에서 찾은 정보    |
| Need Check       | 'O'인 경우, 검토가 필요합니다.                                                                           |
| Comment          | 검토가 필요한 사항을 출력합니다.<br>&ensp;&ensp;- Fill in [Column명] : 기입이 필요한 Column을 표시.<br>&ensp;&ensp;ex) Fill in OSS Name : 'OSS Name' Column에 사용한 OSS의 이름을 기입해야 함.<br>&ensp;&ensp;- Add NOTICE to path : Source Code Path에 NOTICE 파일이 없으므로, NOTICE 파일을 해당 binary의 Source Code Path에 추가해야함.<br>&ensp;&ensp;단, NOTICE 파일을 Source code path에 추가하기 어렵거나 NOTICE파일을 추가해도 최종 target에 탑재되는 NOTICE에 포함되지 않는 경우 FOSSLight Hub를 통해 Project를 리뷰 받은 후 Supplement NOTICE.html 기능을 통해 추가되어야하는 NOTICE를 다운로드 받은 후 Android 모델 OSS 고지문 > '별도 생성한 NOTICE를 OSS 고지문에 추가' 방법을 통해 보완이 필요합니다.|
| (TLSH)           | Binary의 TLSH 값을 출력합니다.                                                            |
| (SHA1)           | Binary의 Checksum 값을 출력합니다.                                                            |



## 🚗 추가 기능
---
하기 옵션을 통해 부가 기능을 활용할 수 있습니다.
- Option: -b, -n, -c : NOTICE.html에 Binary 이름이 포함되었는지 여부를 확인합니다.
- Option: -p : Packaging 파일에 포함되지 않아야 하는 파일을 확인합니다.
- Option: -f : Source Code Path를 찾지 못하는 binary에 대하여 Find Command 실행 결과를 출력해줍니다.
- Option: -t : NOTICE가 Source Code Path에 있음에도 NOTICE.html에 추가되지 않는 binary에 대하여 NOTICE를 취합합니다.
- Option: -i : Android reference 의 repository기준으로 OSS Name의 자동 출력을 비활성화합니다.
- Option: -d : needtoadd-notice.html파일을 Binary별로 분할합니다.
- Option: -r : 특정 binary가 FOSSLight Report에서 중복되는것을 제거합니다. Android native와 vendor가 분리되어 build되는 구조에서 사용하는 옵션으로 중복으로 포함되는 Binary를 제거합니다. vendor에 대한 FOSSLight Android 실행시 -r 옵션으로 android native 결과 생성되는 result_*.txt 파일을 parameter로 추가합니다.
- Option: -m : License가 빈칸인 부분에 대해 자동으로 Source path 내 Source code 분석(소스 파일 내 License text 기반 License 검출)을 실행하여 License 값을 채워줍니다. (그러나 분석에 시간이 오래 걸립니다. Android native에서 44개 Path기준 약 35분 소요)


---

### -b, -n, -c: NOTICE.html에 Binary 이름 포함 여부 확인
#### (1) OSS가 사용 된 경우 ( NOTICE.html이 ok 또는 ok(NA)인 경우)

OSS 보고서 BIN(Android) sheet내 NOTICE.html column의 값이 ok 또는 ok(NA)라면, 그 Binary 이름은 NOTICE.html에 포함되어 있어야 합니다.

#### 실행방법

Binary 이름이 NOTICE.html에 포함되었는지 확인하는 방법은 다음과 같습니다.
1. NOTICE.html 의 값을 ok, ok(NA)만 조회합니다.
2. 이제 Binary Name column에서 모든 항목을 선택하여 Copy합니다.
3. Linux 환경에서 binary.txt 라는 파일을 vi editor를 이용하여 생성합니다.
    ```
    (venv)$ vi binary.txt
    ```
4. 그리고, 2에서 copy한 내용을 binary.txt에 붙여넣고 저장합니다.
5. NOTICE.html을 binary.txt와 같은 directory에 위치시킵니다.
    ```
    (venv)$ ls
    binary.txt  NOTICE.html
    ```

6. "-c" option을 사용하여 결과를 추출합니다. 이때, -c option parameter로 ok 항목에 대한 체크사항이므로 ok라고 입력합니다.
    ```
    (command)
    (venv)$ fosslight_android -b [binary.txt] -n [NOTICE.html] -c [ok|nok]
    
    (ex)
    (venv)$ fosslight_android -b binary.txt -n NOTICE.html -c ok
    ```

7. NOTICE 파일이 여러개일 경우 ,로 구분하여 작성합니다.

   ex) NOTICE파일이 NOTICE.xml,NOTICE_VENDOR.xml,NOTICE_PRODUCT.xml,NOTICE_PRODUCT_SERVICES.xml 인 경우:
   ```
   (venv)$ fosslight_android -b binary.txt -n NOTICE.xml,NOTICE_VENDOR.xml,NOTICE_PRODUCT.xml,NOTICE_PRODUCT_SERVICES.xml  -c ok
   ```

8. 어떤 binary가 NOTICE.html내 포함되어 있지 않은지 확인합니다.
 
   - result.txt 파일을 열면, nok 항목인 binary 목록이 출력됩니다.
   - 해당 binary는 OSS 보고서 내 NOTICE.html column에는 ok 또는 ok(NA)로 되어 있음에도 NOTICE.html에 포함되어 있지 않은 것을 확인할 수 있습니다.
   - (warning) NOTICE.html이 ok 또는 ok(NA)인 binary의 경우, 모두 NOTICE.html에 표시되어야 하므로 위 result.txt 파일 내 nok로 표기되는 binary가 없어야 합니다.  
<br>  

#### (2) OSS 사용되지 않은 경우 ( NOTICE.html이 nok 또는 nok(NA)인 경우 )  

OSS가 사용되지 않았다고 명시한 binary의 경우, 그 이름이 NOTICE.html에 포함되서는 안됩니다.
Binary 이름이 NOTICE.html에 포함되지 않았는지 확인 방법은 다음과 같습니다.

1. NOTICE.html column에서 nok, nok(NA)만 조회 합니다.
2. 위의 (1)번 내 2부터 5까지를 참고하여 실행합니다. 이때, -c option parameter로 nok 항목에 대한 체크사항이므로 nok라고 입력합니다.
   ```
   (command)
   (venv)$ fosslight_android -b [binary.txt] -n [NOTICE.html] -c [ok|nok]
  
   (ex)
   (venv)$ fosslight_android -b binary.txt -n NOTICE.html -c nok
   ```

3. 추출한 결과에서 어떤 binary가 NOTICE.html에 포함되어 있는지 확인합니다.
   result.txt 파일을 열면, OSS 보고서에 OSS가 사용되지 않은 것으로 명시되었음에도 NOTICE.html에 포함된 binary를 확인할 수 있습니다.
   (warning) NOTICE.html이 nok 또는 nok(NA)인 경우, NOTICE.html에 포함되면 안 되므로, 위 result.txt 에 ok로 출력되는 binary가 있어서는 안 됩니다
<br>
<br>

### -p: Packaging 파일에 포함되지 않아야 하는 파일 확인
공개할 Source Code 취합시, 포함되지 말아야 하는 파일 이름, 확장자, 디렉토리를 체크합니다.  


#### 사전 준비

1. Packaging Config File : 체크할 항목을 json 형식의 pkgConfig.json 파일 이름으로 생성합니다.
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

2. 항목 별 설명 : 항목 별 작성할 사항이 1개 이상인 경우 "," 로 구분하여 작성합니다.
    - Prohibited_File_Names : 검출하려는 파일 이름 
    - Prohibited_File_Extensions : 검출하려는 파일 확장자 
    - Prohibited_Path : 검출할 파일 디렉토리

3. 공개할 소스 코드를 취합한 디렉토리 위치 혹은 압축 파일 확인
    - 공개할 소스 코드 취합한 디렉토리나 압축 파일 내 압축된 파일이 있을 경우, 압축을 해제하여 검색합니다.
    - 압축 해제 지원 확장자 : tar, tar.gz, zip
        - tar, tar.gz, zip 외의 압축 파일이 있다면, 압축 해제를 미리 수동으로 해야합니다.
    
  


#### 실행방법
1. Packaging Config File을 pkgConfig.json 파일명(json 형식)으로 준비합니다.
2. -p 옵션을 추가하여 실행합니다. (-p : 공개할 소스 코드를 취합한 Path 혹은 압축 파일)
    ```
    (venv)$ fosslight_android -p [A path or compressed file containing the source code to be disclosed]
     
    ex
    (venv)$ fosslight_android -p /home/test/sourceCodeToBeDisclosed.tar.gz
    ```   

#### 결과 확인 
1. 검출된 항목별로 추출된 목록을 보여줍니다.
2. 결과 example :       

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
<br>
<br>




### -f: Source Code Path를 찾지 못하는 binary에 대하여 Find Command 실행 결과 출력
Source Code Path를 찾지 못하는 Binary에 대하여 Android의 Source Path내 폴더 (out directory, .으로 시작하는 숨김 directory 제외)별로 Find Command 실행 결과를 출력합니다.     

#### 실행방법
1. -f 옵션을 추가하여 실행합니다.
    ```commandline
    (venv)$ fosslight_android  -s [android source path] -a [build log file name] -f
     
    ex
    (venv)$ fosslight_android  -s /home/soim/android/source -a android.log -f
    ```

#### 결과 확인        
1. Source Code Path를 찾지 못하는 Binary별 Find command 실행 결과는 'FIND_RESULT_OF_BINARIES.txt' 파일로 생성됩니다.
2. 단, Source Code Path를 찾지 못하는 Binary가 없을 경우 해당 파일은 생성되지 않습니다.
<br>
<br>

### -t: NOTICE.html에 추가되지 않는 binary에 대하여 NOTICE를 취합
Source Code Path에 NOTICE가 있음에도 최종 제품에 탑재되는 NOTICE(NOTICE.html)에 포함되지 않는 Binary에 대하여 NOTICE를 취합한 파일을 생성합니다.

#### 실행방법
1. -t 옵션을 추가하여 실행합니다.
    ```commandline
    (venv)$ fosslight_android -s [android source path] -a [build log file name] -t
 
    ex
    (venv)$ fosslight_android -s /home/soim/android/source -a android.log -t
    ```

#### 결과 확인        
1. Source Code Path에 NOTICE가 있음에도 최종 제품에 탑재되는 NOTICE(NOTICE.html)에 포함되지 않는 Binary의 NOTICE를 취합하여 'needtoadd-notice.html'이라는 파일로 생성됩니다.
2. 단, 위 사항에 해당하는 Binary가 없을 경우 해당 파일은 생성되지 않습니다.
3. 생성된 needtoadd-notice.html는 별도 생성한 OSS 고지문의 NOTICE.html 삽입 방법을 통하여 최종 제품에 탑재되는 NOTICE.html에 포함시킵니다.
<br>
<br>


### -d: -t 옵션으로 생성한 needtoadd-notice.html파일을 Binary별 분할
needtoadd-notice.html 파일을 읽어 Binary 별 license text 파일을 생성합니다. 이 기능은 xml/html 포맷 무관하게 Android의 OSS Notice 파일에 needtoadd-notice.html 파일을 추가할 때 활용합니다. 하기와 같이 폴더와 파일을 생성하고 License text를 해당 파일에 출력합니다.

1. Binary name: system/core/local_bin.jar 인 경우, 
    - NOTICE_FILES
    - ---system
    - -----------core
    - ------------------local_bin.jar.txt
<br>
<br>



### -i: OSS Name 자동 완성 기능 끄기
FOSSLight Android는 Binary DB에서 OSS 정보를 찾을 수 없는 경우이거나 OSS Name이 "Android Open Source Project"인 경우, Source Code Path를 기준으로 [Android Native](https://android.googlesource.com/platform)에 있는 저장소라면 OSS Name을 자동으로 출력해줍니다.      
OSS Name 자동 완성 기능을 끄고자 할 경우 선택합니다.      

#### 실행방법
1. -i 옵션을 추가하여 실행합니다.
    ```commandline
    (venv)$ fosslight_android -s [android source path] -a [build log file name] -i
 
    ex
    (venv)$ fosslight_android -s /home/soim/android/source -a android.log -i
    ```

#### 결과 확인        
1. 생성 파일명 : RESULT_COMPARE_I_OPTION.xlsx
    - Sheet "Ref_with_i_option" : i 옵션으로 android reference 소스에서 FOSSLight Android 분석 결과
    - Sheet "Ref_without_i_option" : android reference 소스에서 FOSSLight Android 분석 결과
<br>
<br>



### -r: 특정 binary를 FOSSLight Report에서 중복 제거
하나의 Model에 탑재하는 Android native와 vendor가 분리된 output으로 생성되는 경우에 한하여 활용합니다. vendor에 대한 FOSSLight Android 실행시 -r 옵션을 이용하여 Android native에도 포함되는 binary를 중복 제거합니다.
    - 중복 제거 조건 : Binary name이 같고 checksum이 같거나, Binary name이 같고 TLSH 값 차이가 120이하인 경우
    - 중복 제거된 binary는 REMOVED_BIN_BY_DUPLICATION.txt에 출력됩니다.
<br>
<br>


#### 실행방법
1. FOSSLight Android 분석 실행시 -r 옵션을 추가합니다.
    ```commandline
    (venv)$ fosslight_android -s [vendor_source_path] -a [android_build_log_file] -r [android_native_result.txt]
 
    ex
    (venv)$ fosslight_android -s [vendor_source_path] -a android.log -r android_native_result.txt
    ```

#### 결과 확인        
1. android_native_result.txt와 중복된 binary는 FOSSLight-Report.xlsx에서 제거되고, REMOVED_BIN_BY_DUPLICATION.txt에 출력됩니다.
<br>
<br>


### -m: 소스 코드 분석하여 License 출력
License 정보를 못 찾은 경우에 한하여 FOSSLight Source를 이용하여 Source code를 분석한 결과를 License란에 출력합니다.        

#### 실행방법
1. -m 옵션을 추가합니다.
    ```commandline
    (venv)$ fosslight_android -s [vendor_source_path] -a [android_build_log_file] -m
     
    ex
    (venv)$ fosslight_android -s [vendor_source_path] -a android.log -m
    ```

#### 결과 확인         
1. FOSSLight Report의 License column에 분석한 결과가 채워집니다.              
2. 추가로 source_analyzed_[datetime] 폴더에 소스 코드별 분석한 결과가 생성됩니다.       
