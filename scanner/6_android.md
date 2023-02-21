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
- [동작 방식](#-동작-방식)


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

| Column           | 내용                                                                                                                                               |
|:-----------------|:-------------------------------------------------------------------------------------------------------------------------------------------------|
| Binary Name      | out directory 내 존재하는 Binary 목록 (binary, library, APK, font 등 )                                                                                   |  
| Source Code Path | Binary를 구성하는 Source Code의 Path 정보 (LOCAL_PATH)                                                                                                   |  
| Notice           | NOTICE 파일에 Binary 정보가 표시되었는지 여부       Open Source가 사용된 Binary라면, NOTICE.html 값이 ok여야 함.ok : LOCAL_PATH에 NOTICE 파일이 있고, NOTICE.html에 Binary 정보 포함 |
| OSS Name           | Binary DB 에서 load한 OSS 정보 또는 Android Open Source Project인 경우 해당 Repository 기반의 이름을 출력합니다.                                                        |



