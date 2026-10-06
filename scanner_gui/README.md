---
sort: 3
published: true
title: 🚩FOSSLight Scanner GUI
---

# FOSSLight Scanner GUI

[**FOSSLight Scanner GUI**](https://github.com/fosslight/fosslight_scanner_gui)는 [FOSSLight Scanner](../scanner/README.md)를 Windows에서 실행하는 데스크톱 앱입니다. 설치 파일 하나에 분석 엔진이 포함되어 있어, Python이나 스캐너를 따로 설치하지 않고 Source, Dependency, Binary 분석을 실행하고 결과를 화면에서 확인할 수 있습니다.

분석 결과는 [FOSSLight Report](https://fosslight.org/hub-guide/learn/2_fosslight_report.html) 형식의 엑셀 파일로도 함께 저장됩니다.

## 개요
{: .left-bar-title}

- 저장소 : [fosslight/fosslight_scanner_gui](https://github.com/fosslight/fosslight_scanner_gui)
- 설치 파일 : [Releases](https://github.com/fosslight/fosslight_scanner_gui/releases)의 `fosslight-scanner-gui-<버전>-setup.exe`
- 지원하는 분석
    - [FOSSLight Source Scanner](../scanner/2_source.md)
    - [FOSSLight Dependency Scanner](../scanner/1_dependency.md)
    - [FOSSLight Binary Scanner](../scanner/3_binary.md)
- 분석 대상
    - 로컬 폴더
    - 압축 파일 (zip, tar, tar.gz, tgz, tar.bz2, tar.xz, bz2, jar, whl, rpm, src.rpm)
    - URL (git 저장소 clone, 또는 압축 파일 다운로드 주소)
- 이 앱에서 제공하지 않는 항목
    - [FOSSLight Android Scanner](../android/README.md)
    - [FOSSLight Yocto Scanner](../yocto/README.md)
    - [FOSSLight Prechecker](../prechecker/README.md)
    - CLI의 compare 모드

## 지원 환경
{: .left-bar-title}

- Windows 10 / 11 (64비트)
- 설치 프로그램은 분석 엔진을 포함하고 있어, 설치 중에 추가 파일을 내려받지 않습니다. 오프라인 PC에서도 설치할 수 있습니다.
- 설치 파일은 약 259MB이고, 설치 후 용량은 약 1GB입니다. Source 분석에 쓰는 라이선스 데이터가 대부분을 차지합니다.
- 기본 설치는 사용자 단위이며 관리자 권한이 필요하지 않습니다. 서비스 등록이나 시스템 PATH 변경은 하지 않습니다.

## 설치
{: .left-bar-title}

1. [Releases](https://github.com/fosslight/fosslight_scanner_gui/releases)에서 `fosslight-scanner-gui-<버전>-setup.exe`를 받습니다.
2. 설치 파일을 실행합니다. **Windows의 PC 보호(SmartScreen)** 경고가 나오면 **추가 정보**를 누른 뒤 **실행**을 누릅니다. 코드 서명이 없는 프로그램에서 표시되는 경고입니다.
3. 설치 마법사에서 설치 위치를 확인하고 **설치**를 누릅니다. 기본 위치는 `%LOCALAPPDATA%\Programs\fosslight-scanner-gui\`이며, 설치 화면에서 바꿀 수 있습니다.
4. 설치가 끝나면 바탕화면 또는 시작 메뉴의 **FOSSLight Scanner** 아이콘으로 실행합니다.

앱을 실행하면 GitHub Release를 확인해 새 버전이 있으면 다운로드 여부를 묻습니다. **다운로드**를 선택하면 받은 뒤 재시작으로 설치 마법사가 열립니다. 다운로드 중에는 앱을 사용할 수 없고, 재시작 시 진행 중인 스캔은 중단됩니다. 업데이트 확인에 실패하면(오프라인 등) 안내 없이 현재 버전으로 계속 사용할 수 있습니다.

## 화면 구성
{: .left-bar-title}

왼쪽 메뉴는 아래와 같습니다.

| 구역 | 메뉴 | 내용 |
|:-----|:-----|:-----|
| 메뉴 | New Scan | 새 분석을 실행합니다. |
| Scan Results | Overview | 검출 건수, 라이선스 위험도, 스캐너 정보를 요약합니다. |
| Scan Results | Source / Dependency / Binary | 스캐너별 검출 항목을 봅니다. 숫자는 검출 건수입니다. |
| Compliance | License | 검출된 라이선스의 위험도와 주요 의무사항을 봅니다. |
| 하단 | Open Source Notice | 이 앱에 포함된 오픈소스 고지문을 엽니다. |
| 하단 | 버전 | 마우스를 올리면 GUI와 포함된 스캐너 버전을 볼 수 있습니다. |
| 하단 | GitHub 아이콘 | [이슈](https://github.com/fosslight/fosslight_scanner_gui/issues) 페이지를 엽니다. |

스캔이 진행 중이면 메뉴 아래에 **스캔 진행 중...**이 표시됩니다. 앱을 다시 실행하면 마지막으로 성공한 스캔 결과가 자동으로 열립니다.

## 스캔 실행
{: .left-bar-title}

**New Scan**에서 아래 항목을 입력한 뒤 **스캔 시작**을 누릅니다.

### 분석 대상
{: .specific-title}

**폴더**, **압축파일**, **URL** 중 하나를 선택합니다.

- **폴더** : **폴더 선택**으로 분석할 프로젝트 폴더를 지정합니다. 리포트 저장 위치는 그 폴더로 채워집니다.
- **압축파일** : zip, tar.gz, jar, whl, rpm 등 지원 확장자의 파일을 선택합니다. 앱이 압축을 푼 뒤 분석합니다. 리포트 저장 위치는 그 파일이 있는 폴더로 채워집니다.
- **URL** : `https://` 또는 `git@`로 시작하는 주소를 입력합니다.
    - git 저장소 주소는 clone한 뒤 분석합니다. PC에 [git](https://git-scm.com/download/win)이 설치되어 있어야 합니다.
    - 주소가 압축 파일 확장자로 끝나면 파일을 내려받아 푼 뒤 분석합니다. git은 필요하지 않습니다.
    - git 저장소인 경우 **Branch 또는 Tag**를 입력할 수 있습니다. 비워 두면 기본 브랜치를 사용합니다. 입력 칸에서 포커스가 빠지면 저장소에 해당 브랜치 또는 태그가 있는지 확인하며, 유효하지 않으면 스캔을 시작하지 않습니다. 비슷한 이름이 있으면 후보를 보여 줍니다.

예시

- git clone : `https://github.com/fosslight/fosslight_scanner`
- 특정 태그 : URL은 위와 같이 두고 Branch 또는 Tag에 `v2.1.25`
- 압축 파일 : `https://github.com/fosslight/fosslight_scanner/archive/refs/tags/v2.1.25.zip`

### 분석 유형
{: .specific-title}

**Source Code**, **Dependency**, **Binary** 중 실행할 항목을 선택합니다. 기본값은 세 가지 모두이며, 최소 한 가지는 선택되어 있어야 합니다.

### Source 분석 옵션
{: .specific-title}

Source Code를 선택한 경우에만 표시됩니다. 둘 다 선택 사항입니다.

- **KB URL** : Source 분석에 사용할 KB 서버 주소
- **KB Token** : KB 인증 토큰

비워 두면 KB 없이 Source 분석을 수행합니다.

### 제외 경로
{: .specific-title}

분석에서 빼려는 경로를 입력하고 **추가**를 누릅니다. Enter 키로도 추가할 수 있습니다. 예: `node_modules`

### 리포트 저장 위치
{: .specific-title}

엑셀 리포트와 로그를 저장할 폴더입니다. 필수 항목입니다. 폴더나 압축파일을 고르면 기본값이 채워지고, URL 분석은 **폴더 선택**으로 직접 지정합니다.

### 진행과 취소
{: .specific-title}

스캔이 시작되면 단계가 **다운로드 → 도구 설치 → 준비 → 분석 → 결과 정리** 순서로 표시됩니다. 다운로드와 도구 설치는 필요할 때만 진행됩니다. 화면의 로그는 저장 위치의 `fosslight_gui_<시각>.log`에도 같은 내용으로 기록됩니다.

대상 크기에 따라 수 분에서 수십 분이 걸릴 수 있습니다. **취소**를 누르면 확인 후 진행 중인 작업이 중단되고, **스캔이 취소되었습니다.**가 표시됩니다.

- 오류는 빨간색 배너로 표시됩니다.
- 분석은 끝났지만 일부 도구가 없거나 설치에 실패한 경우는 노란색 경고로 표시됩니다. 경고만 있고 완료 메시지가 있으며 리포트 파일이 생성되었다면 해당 분석은 완료된 것입니다.

## 결과 확인
{: .left-bar-title}

### Overview
{: .specific-title}

- **Open Source 검출** : Source, Dependency, Binary 건수입니다. 카드를 누르면 해당 결과 화면으로 이동합니다.
- **License 정보** : 고유 라이선스 수, 위험도 분포, 항목 수가 많은 라이선스입니다. Strong Copyleft 또는 Restricted 라이선스가 있으면 경고가 표시되고, 누르면 License 화면으로 이동합니다.
- **Result file** : 이번 스캔의 리포트 저장 폴더를 탐색기에서 엽니다.
- **스캐너 정보** : 실행된 스캐너와 분석 환경 정보입니다.

Exclude로 표시된 항목은 Overview 통계에서 빠집니다.

### Source / Dependency / Binary
{: .specific-title}

검출 항목을 표로 보여 줍니다. 경로, OSS 이름, 라이선스로 검색할 수 있고, 열 제목을 눌러 정렬할 수 있습니다. 한 페이지에 50건씩 표시됩니다.

행을 누르면 Download Location, Homepage, Copyright, Comment가 펼쳐집니다. Exclude인 행은 흐리게 표시됩니다.

- Source의 경로 열은 **Source Path**
- Dependency의 경로 열은 **Package URL**
- Binary의 경로 열은 **Binary Path**

### License
{: .specific-title}

검출된 라이선스를 위험도가 높은 순으로 모읍니다. 분류와 화면의 위험도 표시는 아래와 같습니다.

| 분류 | 위험도 |
|:-----|:-------|
| Restricted | 높음 |
| Strong Copyleft | 높음 |
| Weak Copyleft | 중간 |
| Permissive | 낮음 |
| 미분류 | 확인 필요 |

각 라이선스의 주요 의무사항이 함께 표시됩니다. 행을 누르면 그 라이선스가 검출된 항목 목록이 펼쳐집니다. 분류되지 않은 라이선스는 숨기지 않고, 화면 상단에 수동 검토가 필요하다고 표시됩니다.

## 생성되는 파일
{: .left-bar-title}

리포트 저장 위치에 아래 파일이 생성됩니다.

| 파일 | 내용 |
|:-----|:-----|
| `fosslight_report_*.xlsx` | Source, Dependency, Binary 결과가 담긴 FOSSLight Report입니다. FOSSLight Hub에 업로드할 수 있습니다. |
| `fosslight_report_*.yaml` | 같은 분석 결과의 YAML 파일입니다. |
| `fosslight_gui_<시각>.log` | 스캔 화면에 표시된 로그입니다. |
| `gui_result.json` | 앱이 결과를 다시 열 때 읽는 파일입니다. |

앱을 다시 실행했을 때 마지막 결과를 여는 데 쓰는 목록은 `%APPDATA%\fosslight-scanner-gui\`에 있습니다.

## 의존성 분석에 필요한 도구
{: .left-bar-title}

Dependency를 포함한 스캔에서, 프로젝트의 manifest에 맞는 도구가 없으면 앱이 분석을 시작하기 전에 준비합니다. URL과 압축 파일은 받은 뒤 같은 방식으로 확인합니다. 설치에 실패해도 스캔 전체를 멈추지는 않으며, 해당 패키지 매니저 분석만 실패할 수 있습니다. 이 경우는 노란색 경고로 안내합니다.

| 감지 파일 | 필요한 도구 | 앱의 동작 |
|:----------|:------------|:----------|
| package.json | Node.js (npm) | 없으면 설치합니다. winget을 쓸 수 없으면 공식 zip으로 받아 이번 분석에만 사용합니다. |
| pom.xml | Apache Maven, Java | Maven Wrapper(`mvnw`) 또는 이미 설치된 `mvn`이 없으면 Maven을 준비합니다. Java도 없으면 준비합니다. |
| build.gradle, build.gradle.kts | Java | Gradle은 설치하지 않고 프로젝트의 Gradle Wrapper를 사용합니다. Java가 없으면 준비합니다. |
| requirements.txt, setup.py, setup.cfg, pyproject.toml, Pipfile | Python | 앱에 포함된 Python으로 분석합니다. 별도 설치가 필요하지 않습니다. |
| go.mod | Go | 없으면 winget으로 설치합니다. |
| Chart.yaml | Helm | 없으면 winget으로 설치합니다. |
| Cargo.toml | Rust (cargo) | 없으면 winget으로 설치합니다. |
| Gemfile | Ruby | 없으면 winget으로 설치합니다. |
| pubspec.yaml | Flutter | 자동 설치하지 않습니다. [Flutter Windows 설치 안내](https://docs.flutter.dev/get-started/install/windows)에 따라 직접 설치한 뒤 다시 스캔합니다. |
| Podfile, Podfile.lock | CocoaPods | Windows에서는 지원하지 않습니다. macOS에서 분석해야 합니다. |

winget으로 설치하는 도중 Windows 권한 확인 창이 나오면 허용합니다. winget을 사용할 수 없으면 로그에 직접 설치가 필요하다는 안내가 남고, Node.js만 앱이 따로 내려받을 수 있습니다.

## 제거
{: .left-bar-title}

Windows **설정 > 앱 > 설치된 앱**에서 **FOSSLight Scanner**를 제거하거나, 설치 폴더의 `Uninstall FOSSLight Scanner.exe`를 실행합니다.

## 자주 묻는 질문
{: .left-bar-title}

**Q. 설치할 때 SmartScreen 경고가 나옵니다.**  
코드 서명이 없는 설치 파일에서 표시되는 경고입니다. **추가 정보 → 실행**으로 설치를 계속할 수 있습니다.

**Q. git 저장소 주소로 분석이 되지 않습니다.**  
git 저장소 URL은 PC에 git이 있어야 합니다. 압축 파일 다운로드 주소는 git 없이 동작합니다.

**Q. 로그에 WARNING이 있습니다. 실패한 것인가요?**  
아닙니다. 오류는 빨간색 배너로 따로 표시됩니다. 리포트 파일이 생성되었다면 분석은 완료된 것이고, WARNING은 일부 도구 부재처럼 확인이 필요한 안내입니다.

**Q. 의존성 결과가 비어 있습니다.**  
해당 패키지 매니저가 설치되지 않았거나, Flutter·CocoaPods처럼 자동 설치가 되지 않는 도구일 수 있습니다. 스캔 로그의 경고를 확인한 뒤 도구를 설치하고 다시 스캔합니다.

**Q. 설치 용량이 큽니다.**  
Source 분석용 라이선스 데이터가 설치 용량의 큰 부분을 차지합니다. 설치 프로그램은 이 데이터를 압축한 상태이고, 설치는 압축을 푸는 작업입니다.

문제가 있으면 앱 왼쪽 아래 GitHub 아이콘을 누르거나 [이슈](https://github.com/fosslight/fosslight_scanner_gui/issues)로 남겨 주세요.
