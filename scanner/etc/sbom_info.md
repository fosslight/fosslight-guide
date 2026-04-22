---
published: true
---

# 스캔 결과 정정/제외 설정 방법 (sbom-info.yaml)

스캔 결과 중 정정(Correction)이 필요하거나 특정 파일/디렉토리를 결과에서 제외(Exclude)하고 싶은 경우, `sbom-info.yaml` 파일을 작성하여 활용할 수 있습니다.

> ℹ️ **지원 Scanner:** FOSSLight Scanner, FOSSLight Source Scanner, FOSSLight Binary Scanner
> (FOSSLight Dependency Scanner에는 적용되지 않습니다.)

## 동작 방식
{: .left-bar-title}
스캐너는 스캔 결과의 각 항목에 대해 **source path**를 기준으로 `sbom-info.yaml`에 동일한 `source name or path`가 작성된 항목이 있는지 확인합니다.  
일치하는 항목이 존재할 경우, 스캔 결과 대신 **`sbom-info.yaml`에 작성된 OSS 정보(name, version, license 등)를 우선 적용**합니다.

## 사용 방법
{: .left-bar-title}
1. 검사하려는 프로젝트의 최상위(Top) 디렉토리에 `sbom-info.yaml` 파일을 생성합니다.
2. 아래의 양식과 필드 설명을 참고하여, 정정 또는 제외하고자 하는 오픈소스 및 파일 정보를 작성합니다.
3. 스캐너 실행 시, 디폴트로 `sbom-info.yaml`의 내용이 스캔 결과에 자동으로 반영됩니다.

### 옵션 설명
{: .specific-title}

- `--no_correction` : 이 옵션을 추가하여 실행하면, 작업 디렉토리에 `sbom-info.yaml` 파일이 존재하더라도 **정정된 내용을 스캔 결과에 반영하지 않습니다.**
- `--correct_fpath [PATH]` : 기본 스캔 위치의 최상위(Top) 디렉토리가 아닌 다른 위치의 설정 파일(예: `[PATH]/sbom-info.yaml`)을 사용하여 스캔 결과를 정정하고자 할 때 파일이 위치한 경로를 명시합니다.

<br><br>

## sbom-info.yaml 작성 예시
{: .left-bar-title}

```yaml
libidn: # 오픈소스인 경우
- version: "1.5"
  source name or path:
  - "src/libidn/*"
  - "b.c"
  license:
  - "GPL-3.0"
  - "LGPL-2.1"
  download location: "http://ftp.gnu.org/gnu/libidn"
  homepage: "https://www.gnu.org/software/libidn"
  copyright text: "Copyright 2002-2007, Simon Josefsson"

rsync: # 동일한 패키지의 여러 버전을 사용하는 경우
- version: "2.6.9"
  source name or path: "test/tool"
  license: "GPL-2.0"
  download location: "https://download.samba.org/pub/rsync/src"
  homepage: "http://rsync.samba.org"
- version: "3.1.2"
  source name or path: "test/tool_new"
  license: "GPL-3.0"
  download location: "https://download.samba.org/pub/rsync/src"
  homepage: "http://rsync.samba.org"
  copyright text:
  - "Copyright 1996 Andrew Tridgell"
  - "Copyright 1996 Paul Mackerras"
  - "Copyright 2003-2015 Wayne Davison"

'-': # 디렉토리의 모든 파일을 LG전자에서 자체 개발한 경우
- version: ''
  license: LicenseRef-LGE-Proprietary
  copyright text: Copyright 2026 LG Electronics Inc.
  source name or path:
  - "src/lge/*"

'-': # 특정 경로를 스캔 결과에서 제외하는 경우
- version: ''
  exclude: True
  source name or path:
  - "build/*"
  - "test/*"
```

<br><br>

## sbom-info.yaml 필드 상세 설명
{: .left-bar-title}

`sbom-info.yaml` 작성 시 사용 가능한 필드는 다음과 같습니다.

### 1. Package Name (Header paragraph)
{: .specific-title}
- **Required (필수):** Package Name(OSS Name)을 키(Key) 값으로 기입합니다.
- 패키지 명이 없는 자체 개발 코드 같은 경우 `'-'` 로 기입합니다.

### 2. Version paragraph
{: .specific-title}

| Field | Required / Optional | Value Type | Description / Example |
|---|---|---|---|
| **version** | Required | String | 패키지 버전. 버전이 없는 경우 빈 문자열(`''`)을 기입합니다.<br>ex) `version: "2.8"` |
| **source name or path** | Optional | String \| Array of String | 정정 또는 제외할 대상 파일이나 경로를 명시합니다.<br>ex) `source name or path: "src/*"`<br>ex) `- "main.c"`<br>&nbsp;&nbsp;&nbsp;&nbsp;`- "main.h"` |
| **license** | Optional | String \| Array of String | 적용 라이선스를 기입합니다.<br>ex) `license: "Apache-2.0"`<br>ex) `- "GPL-2.0"`<br>&nbsp;&nbsp;&nbsp;&nbsp;`- "LGPL-2.1"` |
| **download location** | Optional | String | 해당 오픈소스를 다운로드할 수 있는 URL을 기입합니다.<br>ex) `download location: "https://ftp.gnu.org/gnu/glibc"` |
| **homepage** | Optional | String | 오픈소스 프로젝트의 홈페이지 URL을 기입합니다.<br>ex) `homepage: "http://google.com"` |
| **copyright text** | Optional | String | 패키지와 연관된 저작권 문구를 기입합니다.<br>ex) `copyright text: "Copyright 2020 Test"`<br>여러 줄 입력 시 YAML의 `\|` 문법을 사용할 수 있습니다. |
| **exclude** | Optional | Boolean | 스캔 결과 산출물 포함 여부 (ex. 빌드 스크립트 등 출력물에서 제외 시 `True`).<br>ex) `exclude: True` |
| **comment** | Optional | String | 부가적인 코멘트를 기입합니다.<br>ex) `comment: "This is the build tool"` |
