---
sort: 5
published: true
title: 🔎 FOSSLight Scanner
---
# FOSSLight Scanner


## Introduction

FOSSLight Scanner는 Open Source Compliance를 위한 분석 과정을 한번에 수행 가능한 툴입니다. 소스코드, 바이너리, 디펜던시에 대한 Open Source 분석을 수행하고, 저작권/License 표기 규칙 준수 여부를 체크할 수 있습니다.

## Features

<div class="flex-container">
  <div class="flex-contents">
    <div>
      <div id="feature_title">
        Inclusive Scanning
      </div>
      <div id="feature_img">
        <img src="https://img.icons8.com/dotty/80/000000/check-all.png"/>
      </div>
      <div id="feature_content">
        소스코드, 바이너리<br>그리고 디펜던시 분석까지<br>수행할 수 있습니다.
      </div>
    </div>
  </div>

  <div class="flex-contents">
    <div>
      <div id="feature_title">
        Integrated One
      </div>
      <div id="feature_img">
        <img src="https://img.icons8.com/wired/64/000000/workspace-one.png"/>
      </div>
      <div id="feature_content">
        하나로 통합된 패키지로<br>단 한줄의 명령어로<br>실행 가능합니다.
      </div>
    </div>
  </div>

  <div class="flex-contents">
    <div>
      <div id="feature_title">
        Independent Module
      </div>
      <div id="feature_img">
        <img src="https://img.icons8.com/dotty/80/000000/module.png"/>
      </div>
      <div id="feature_content">
        스캐너 모듈은 독립적으로,<br>가볍게 사용할 수 있습니다.
      </div>
    </div>
  </div>
</div>

## Scanner Projects

#### 1. [**FOSSLight Reuse**](1_reuse.md)
- FOSSLight Reuse는 reuse-tool을 이용하여 소스 코드의 저작권 및 License 표기 규칙을 준수하는지 확인하고 보완하기 위해 사용할 수 있는 도구입니다.
- Reuse 준수 여부 체크시 **[reuse-tool](https://github.com/fsfe/reuse-tool)** 오픈 소스를 이용합니다.

#### 2. [**FOSSLight Source Scanner**](2_source.md)
- FOSSLight Source Scanner는 소스 코드 스캐너인 ScanCode를 이용하여, 파일 안에 포함된 Copyright과 License 문구를 검출합니다. 
- 소스코드 스캔 작업을 위해 **[scancode-toolkit](https://github.com/nexB/scancode-toolkit)** 오픈 소스를 이용합니다.

#### 3. [**FOSSLight Dependency Scanner**](3_dependency.md)
- FOSSLight Dependency Scanner는 여러 패키지 매니저에 대한 Dependency 분석을 지원하는 도구로써, 사용된 Dependency들의 License를 포함한 OSS 정보를 자동으로 출력합니다.
- Package manager에 따라 다음 오픈소스를 이용하여 디펜던시 분석을 수행합니다.
  - NPM : **[NPM License Checker](https://github.com/davglass/license-checker)**
  - Pypi : **[pip-licenses](https://github.com/raimon49/pip-licenses)**
  - Gradle : **[License Gradle Plugin](https://github.com/hierynomus/license-gradle-plugin)**
  - Maven : **[license-maven-plugin](https://github.com/mojohaus/license-maven-plugin)**
  - Pub : **[flutter_oss_licenses](https://github.com/espresso3389/flutter_oss_licenses)**

#### 4. [**FOSSLight Binary Scanner**](4_binary.md)
- FOSSLight Binary Scanner는 Binary를 찾아 출력하고 Binary DB에 동일하거나 비슷한 Binary가 있으면 해당 OSS 정보를 출력합니다.
- jar 파일에 대한 오픈 소스 분석 시, **[Dependency-check-py](https://github.com/jhermann/dependency-check-py)** 오픈 소스를 이용합니다.

#### 5. [**FOSSLight Scanner**](https://github.com/fosslight/fosslight_scanner)
- FOSSLight Scanner는 로컬 소스코드 또는 소스를 다운로드 받은 후 오픈 소스 분석을 수행합니다.
- 오픈 소스 분석 수행 시, FOSSLight Source Scanner, FOSSLight Dependency Scanner, FOSSLight Binary Scanner를 이용합니다.


      
<div class="right"><a href="https://icons8.com/icon">&lt;Icons by Icons8&gt;</a></div>
