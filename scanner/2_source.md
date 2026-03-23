---
published: true
title: "  ㄴ FOSSLight Source Scanner"
---
# FOSSLight Source Scanner

<img src="https://img.shields.io/pypi/l/fosslight_source" alt="FOSSLight Source is released under the Apache-2.0 License." /> <img src="https://img.shields.io/pypi/v/fosslight_source" alt="Current python package version." /> <img src="https://img.shields.io/pypi/pyversions/fosslight_source" /><a href="https://github.com/fosslight/fosslight_source_scanner"><img src="https://img.shields.io/badge/GitHub-Repository-purple?logo=github" alt="GitHub Repository" /></a> [![REUSE status](https://api.reuse.software/badge/github.com/fosslight/fosslight_source_scanner)](https://api.reuse.software/info/github.com/fosslight/fosslight_source_scanner)

[**FOSSLight Source Scanner**](https://github.com/fosslight/fosslight_source_scanner)는 [ScanCode][sc], [SCANOSS][scanoss]와 KB(LGE Only) mode로 동작합니다.  
  - [ScanCode][sc] : 파일 안에 포함된 Copyright과 License 문구를 검출합니다.  
  - [SCANOSS][scanoss] : OSS Name, OSS Version, Download Location, Copyright, License 정보를 [OSSKB][osskb]에서 검색합니다. 
  - KB(LGE Only) : LG전자에서 구축한 Knowledge Database 서버로부터 해당 파일의 출처를 조회하여 OSS Name, OSS Version, Download Location 정보를 출력합니다.    

> Build Script, Binary, Directory, 특정 Directory (ex-test), 숨김 폴더 안의 파일은 제외됩니다.

[sc]: https://github.com/nexB/scancode-toolkit
[scanoss]: https://github.com/scanoss/scanoss.py
[osskb]: https://osskb.org/


## 필요 조건
{: .left-bar-title} 
[**FOSSLight Source Scanner**](https://github.com/fosslight/fosslight_source_scanner)는 Python 3.10+ 기반에서 동작합니다.     
 <br><br> 

## 설치 방법
{: .left-bar-title} 
FOSSLight Source Scanner는 pip3를 이용하여 설치할 수 있습니다.     
[python 3.10 + virtualenv](etc/guide_virtualenv.md) 환경에서 설치할 것을 권장합니다.

```
$ pip3 install fosslight_source
```
<br><br>

## 실행 방법  
{: .left-bar-title} 

Source Code 분석을 실행한 후 FOSSLight Report 형식으로 출력합니다.
````
$ fosslight_source [option] <arg>
````  
### Options
{: .specific-title}

```
📖 Usage
    ────────────────────────────────────────────────────────────────────
    fosslight_source [options] <arguments>

    📝 Description
    ────────────────────────────────────────────────────────────────────
    FOSSLight Source Scanner analyzes source code to detect copyright and
    license information using several modes.

    Note: Build scripts, binary files, and test directories are automatically
          excluded from analysis.

    📚 Guide: https://fosslight.org/fosslight-guide/scanner/2_source.html

    ⚙️  General Options
    ────────────────────────────────────────────────────────────────────
    -p <path>              Source path to analyze (default: current directory)
    -o <path>              Output file path or directory
    -f <format>            Output formats: excel, csv, opossum, yaml, spdx-yaml, spdx-json, spdx-xml, spdx-tag, cyclonedx-json, cyclonedx-xml
                           (multiple formats can be specified, separated by space)
    -e <pattern>           Exclude paths from analysis (files and directories)
                           ⚠️  IMPORTANT: Always wrap in quotes to avoid shell expansion
                           Example: fosslight_source -e "dev/" "tests/" "*.jar"
    -m                     Generate detailed scan results on separate sheets
    -h                     Show this help message
    -v                     Show version information

    🔍 Scanner-Specific Options
    ────────────────────────────────────────────────────────────────────
    -s <mode>              Choose mode: scancode, scanoss, kb, or all(default)
    -c <number>            Number of CPU cores/threads to use for scanning
    -t <seconds>           Timeout in seconds for ScanCode scanning
    -j                     Generate raw scanner results in JSON format
    --no_correction        Skip OSS information correction with sbom-info.yaml
    --correct_fpath <path> Path to custom sbom-info.yaml file

    💡 Examples
    ────────────────────────────────────────────────────────────────────
    # Scan current directory
    fosslight_source

    # Scan specific path with exclusions
    fosslight_source -p /path/to/source -e "test/" "node_modules/"

    # Generate output in specific format
    fosslight_source -f excel -o results/

    # Generate raw scanner results in JSON format
    fosslight_source -p /path/to/source -j
```
- -s 옵션이 추가되지 않을 경우 all 모드(ScanCode, SCANOSS, KB)가 동작한 결과가 취합됩니다.
- -e 옵션 관련 [Pattern 매칭 가이드](https://scancode-toolkit.readthedocs.io/en/stable/reference/scancode-cli/cli-pre-scan-options.html#glob-pattern-matching)
   - ⚠️ 사용 시 반드시 쌍 따옴표("")를 이용하여 입력하시기 바랍니다.
       - 예시) fosslight_source -e "dev/" "tests/"
   - ⚠️ 입력 시 파일명과 확장자는 대소문자를 정확히 구분해야 합니다.

### Example
{: .specific-title}
Source Code 분석
```
$ fosslight_source -p /home/source_path 
```

## 결과
{: .left-bar-title} 
```
$ tree
.
├── fosslight_log_src_260311_1503.txt   
└── fosslight_report_src_260311_1544.xlsx  
```

  - fosslight_log_src_[datetime].txt : 실행 로그가 저장된 파일  
  - fosslight_report_src_[datetime].xlsx : FOSSLight Report 형태의 Source Code 분석 결과  
  - fosslight_opossum_src_[datetime].json : [OpossumUI](https://github.com/opossum-tool/OpossumUI)에서 활용 가능한 Source Code 분석 결과 ( -f opossum 옵션)
  - fosslight_report_src_[datetime].csv : FOSSLight Report를 csv로 출력한 결과  (  -f csv 옵션)
  - scancode_raw_result.json : ScanCode 실행 결과 ( -j 옵션 )
  - scanoss_raw_result.json : SCANOSS 실행 결과 ( -j 옵션  )
  - scanner_output.wfp : SCANOSS 실행 시 생성된 Finger Print ( -j 옵션 )
