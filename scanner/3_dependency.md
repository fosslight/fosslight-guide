---
published: true
title: FOSSLight Dependency Scanner
---
# FOSSLight Dependency Scanner

<img src="https://img.shields.io/pypi/l/fosslight_dependency" alt="License" /> <img src="https://img.shields.io/pypi/v/fosslight_dependency" alt="Current python package version." /> <img src="https://img.shields.io/pypi/pyversions/fosslight_dependency" /> [![REUSE status](https://api.reuse.software/badge/github.com/fosslight/fosslight_dependency_scanner)](https://api.reuse.software/info/github.com/fosslight/fosslight_dependency_scanner)
    
[**FOSSLight Dependency Scanner**](https://github.com/fosslight/fosslight_dependency_scanner)는 여러 패키지 매니저에 대한 종속성 분석을 지원하는 도구입니다. 패키지 매니저의 Manifest 파일을 자동으로 감지하고 오픈 소스 도구를 사용하여 종속성을 분석합니다. 그 후 종속성의 OSS 정보가 포함된 보고서 파일을 생성합니다. 

{::options parse_block_html="true" /}
<details>
<summary markdown="span">지원하는 Package Manager</summary>
- [Gradle](https://gradle.org/) (Java/Android)
- [Maven](http://maven.apache.org/) (Java)
- [NPM](https://www.npmjs.com/) (Node.js)
- [PIP](https://pip.pypa.io/) (Python)
- [Pub](https://pub.dev/) (Dart with flutter)
- [Cocoapods](https://cocoapods.org/) (Swift/Obj-C)
- [Swift](https://swift.org/package-manager/) (Swift)
- [Carthage](https://github.com/Carthage/Carthage) (Carthage)
- [Go](https://pkg.go.dev/) (Go)
</details>
{::options parse_block_html="false" /}

**Github Repository** : [https://github.com/fosslight/fosslight_dependency_scanner](https://github.com/fosslight/fosslight_dependency_scanner)  
**License** : [Apache-2.0](https://github.com/fosslight/fosslight_dependency_scanner/blob/main/LICENSE)

## 목차
  - [필요 조건](#-필요-조건)
  - [설치 방법](#-설치-방법)
  - [실행 방법](#-실행-방법)
  - [결과](#-결과)
  - [동작 방식](#-동작-방식)


## 📋 필요 조건
각 패키지 매니저마다 다른 오픈소스 소프트웨어를 이용하여 Dependency 분석을 수행하고 있습니다. 이에 분석하고자 하는 패키지 매니저에 따라 각각의 Prerequisite 단계를 수행하시기 바랍니다.

{::options parse_block_html="true" /}
<details>
<summary markdown="span">**Prerequisite for NPM**</summary>
1. Npm dependency 분석을 수행하기 위해 NPM License Checker를 설치합니다.
```
$ npm install -g license-checker
```
 > license-checker를 전역 패키지로 설치하기 위해서는, 반드시 '-g' option을 추가해 주어야 합니다. 만약 'sudo' 권한이 없는 경우, 다음 명령어를 통해 전역 모듈이 설치되는 기본 path를 변경하여 이용하실 수 있습니다.
```
$ npm set prefix ~/.npm
$ PATH=~/.npm/bin:$PATH
```

2. dependency를 설치하기 위해 다음 명령어를 실행합니다. (optional)
```
$ npm install
```
 > 아래 케이스 중 해당하는 경우, 이 단계는 skip 가능합니다.
 > - package.json 파일이 input directory에 존재하는 경우 : FOSSLight Dependency Scanner에서 자동으로 패키지 설치하여 실행 가능합니다.
 > - 이미 dependency들이 설치된 node_modules 디렉토리가 존재하는 경우 : node_modules폴더가 존재하는 path를 input directory로 설정하여 실행 가능합니다.
</details>

<details>
<summary markdown="span">**Prerequisite for Gradle**</summary>
1. 'build.gradle' 파일에 License Gradle Plugin을 추가합니다.
```
plugins {
    id 'com.github.hierynomus.license' version '0.15.0'
}
downloadLicenses {
    includeProjectDependencies = true
    dependencyConfiguration = 'runtimeClasspath'
}
```
 > 사용하는 gradle 버전이 4.6 또는 더 낮은 버전인 경우에는, dependencyConfiguration에 'runtimeClasspath' 대신 'runtime'을 추가합니다.

2. 'downloadLicenses' task를 실행합니다.
```
$ gradlew downloadLicenses
```
</details>

<details>
<summary markdown="span">**Prerequisite for Android (gradle)**</summary>
1. 'build.gradle' 파일에 android-dependency-scanning Plugin을 추가합니다.
```
buildscript {
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath 'org.fosslight:android-dependency-scanning:1.0.0'
    }
}
```

2. 플러그인이 적용되는 app 디렉토리 내에 위치한 build.gradle 파일 내에 다음과 같이 추가합니다. 
```
apply plugin: 'org.fosslight'
```

3. 'generateLicenseTxt' task를 실행합니다.
```
$ gradlew generateLicenseTxt
```
</details>

<details>
<summary markdown="span">**Prerequisite for Pypi**</summary>
```tip
- 시스템 내 전역으로 설치된 파이썬 dependency로부터 분석하고자 하는 프로젝트 dependency를 분리하기 위해 가상환경을 설정하여 이용하기를 권장합니다.
- 만약 input path내 requirements.txt가 존재한다면, FOSSLight Dependency가 자동으로 dependency 설치하여 분석 실행 가능하므로, prerequisite단계 skip 가능합니다.
```

1. 가상환경을 생성하고 활성화합니다.
```
// virtualenv example
$ virtualenv -p /usr/bin/python3.6 venv
$ source venv/bin/activate
// conda example
$ conda create --name {venv name}
$ conda activate {venv name}
```

2. 가상환경 내 분석하고자 하는 프로젝트의 dependency를 설치합니다.
```
// If you install the dependencies with requirements.txt...
$ pip install -r requirements.txt
```
</details>

<details>
<summary markdown="span">**Prerequisite for Maven**</summary>
```tip
Maven의 경우, input directory에 pom.xml 파일이 존재하는 경우, plugin 추가 및 실행을 FOSSLight Dependency Scanner 내부에서 자동으로 수행하므로 다음은 skip하셔도 됩니다.
```
<ol>
<li>pom.xml 파일에 license-maven-plugin을 추가합니다.</li>
<pre>
&lt;project&gt;
  ...
  &lt;build&gt;
  ...
    &lt;plugins&gt;
    ...
      &lt;plugin&gt;
        &lt;groupId&gt;org.codehaus.mojo&lt;/groupId&gt;
        &lt;artifactId&gt;license-maven-plugin&lt;/artifactId&gt;
        &lt;version&gt;2.0.0&lt;/version&gt;
        &lt;executions&gt;
          &lt;execution&gt;
            &lt;id&gt;aggregate-download-licenses&lt;/id&gt;
            &lt;goals&gt;
              &lt;goal&gt;aggregate-download-licenses&lt;/goal&gt;
            &lt;/goals&gt;
          &lt;/execution&gt;
        &lt;/executions&gt;
      &lt;/plugin&gt;
    &lt;/plugins&gt;
    ...
  &lt;/build&gt;
  ...
&lt;/project&gt;
</pre>

<li>license-maven-plugin task를 실행합니다.</li>
<pre>
$ mvnw license:aggregate-download-licenses
</pre>
</ol>
</details>

<details>
<summary markdown="span">**Prerequisite for Pub**</summary>
1. 다음 명령어를 통해 flutter_oss_licenses를 실행합니다.
```
$ flutter pub get
$ flutter pub global activate flutter_oss_licenses
$ flutter pub global run flutter_oss_licenses:generate.dart
```
</details>

<details>
<summary markdown="span">**Prerequisite for Cocoapods**</summary>
1. Podfile을 통해 pod package를 설치합니다.
```
$ pod install
```
</details>

<details>
<summary markdown="span">**Prerequisite for Swift**</summary>
1. Github personal access token을 생성하여 FOSSLight Dependency Scanner 실행 시 '-t' 파라미터로 사용합니다. 이 토큰은 Github repository의 license정보를 가져오기 위해 Github API를 사용하기 위해 필요합니다.
Token생성 방법은 [Github docs 가이드](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token)를 참조하시기 바랍니다.
</details>

<details>
<summary markdown="span">**Prerequisite for Carthage**</summary>
1. 다음과 같이 패키지 설치 명령어를 수행하여 'Cartfile.resolved' 파일을 생성합니다.
```
$ carthage update
```
2. Github personal access token을 생성하여 FOSSLight Dependency Scanner 실행 시 '-t' 파라미터로 사용합니다. 이 토큰은 Github repository의 license정보를 가져오기 위해 Github API를 사용하기 위해 필요합니다.
Token생성 방법은 [Github docs 가이드](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token)를 참조하시기 바랍니다.
</details>

<details>
<summary markdown="span">**Prerequisite for Go**</summary>
```tip
Go의 경우, go module에 한해 dependency 분석을 지원합니다. FOSSLight Dependency Scanner 내부에서 자동으로 'go list -m all' 명령어를 수행하여 dependency 목록을 얻은 뒤, license, repository와 같은 오픈소스 정보를 취합하고 있습니다. 이에 별도의 prerequisite단계없이, 바로 fosslight_dependency 명령어 실행하여 이용하실 수 있습니다.
```
</details>

{::options parse_block_html="false" /}

## 🎉 설치 방법

FOSSLight Dependency Scanner는 pip3를 이용하여 설치할 수 있습니다.     
[python 3.6 + virtualenv](etc/guide_virtualenv.md) 환경에서 설치할 것을 권장합니다.

```
$ pip3 install fosslight_dependency
```


## 🚀 실행 방법

FOSSLight Dependency Scanner는 패키지 매니저에 따라 다음 option들을 이용하여 실행할 수 있습니다.

```
$ fosslight_dependency [option] <arg>
```
### Options
```
        Optional
            -h                              Print help message.
            -v                              Print the version of the fosslight_dependency.
            -m <package_manager>            Enter the package manager.
                                             (npm, maven, gradle, pip, pub, cocoapods, android, swift, carthage, go)
            -p <input_path>                 Enter the path where the script will be run.
            -o <output_path>                Output path
                                             (If you want to generate the specific file name, add the output path with file name.)
            -f <format>                     Output file format (excel, csv, opossum)

        Required only for pypi
            -a <activate_cmd>               Virtual environment activate command (ex, 'conda activate (venv name)')
            -d <deactivate_cmd>             Virtual environment deactivate command (ex, 'conda deactivate')

        Required only for swift, carthage
            -t <token>                      Enter the github personal access token.

        Optional only for gradle, maven
            -c <dir_name>                   Enter the customized build output directory name
                                              (default name : 'build' for gradle, 'target' for maven)

        Optional only for android
            -n <app_name>                   Enter the application directory name where the plugin output file is located
                                             (default: app)
```

### Tips to run
FOSSLight Dependency Scanner 실행 시, input path('-p' 옵션)는 dependency 분석을 수행하고자 하는 패키지 매니저의 manifest 파일이 존재하는 프로젝트의 top directory로 지정해 주어야 합니다.
각 패키지 매니저별 manifest 파일은 다음과 같습니다.
```
  - Npm : package.json
  - Pypi : requirements.txt
  - Maven : pom.xml
  - Gradle (Android) : build.gradle
  - Pub : pubspec.yaml
  - Cocoapods : Podfile
  - Swift : Package.resolved
  - Carthage : Cartfile.resolved
  - Go : go.mod
```

- Swift package manager
  - 예외적으로 Swift package manager는 {프로젝트명}.xcodeproj 파일이 위치한 path에서 "fosslight_dependency -m swift -t {token}" 명령어를 실행하실 수 있습니다.
  - 이 경우에는 {프로젝트명}.xcodeproj/project.xcworkspace/xcshareddata/swiftpm path에서 'Package.resolved' 파일을 자동으로 찾고 프로그램이 실행됩니다.

## 📁 결과
```
$ tree
.
├── FOSSLight-Report_2021-05-03_00-39-49_SRC.csv
├── FOSSLight-Report_2021-05-03_00-39-49.xlsx
├── fosslight_dependency_log_2021-05-03_00-39-49.txt
└── Opossum_input_2021-05-03_00-39-49.json
```
- FOSSLight-Report_[datetime].xlsx : FOSSLight Report 형태의 Dependency 분석 결과
- FOSSLight-Report_[datetime]_[sheet_name].csv : FOSSLight Report를 csv로 출력한 결과
- fosslight_dependency_log_[datetime].txt: 실행 로그가 저장된 파일
- Opossum_input_[datetime].json : [OpossumUI](https://github.com/opossum-tool/OpossumUI)에서 활용 가능한 Dependency 분석 결과

### 결과 파일 내용
FOSSLight Report 결과 파일에는 transitive dependency들을 포함한 모든 분석된 dependency들의 manifest 파일을 기반으로 OSS 정보가 기록됩니다.
이때, 고유한 OSS명을 작성하기 위해, OSS명은 (패키지 매니저):(OSS명) 또는 (group id):(artifact id) 양식으로 기록됩니다.

| Package manager                | OSS Name                 | Download Location                                                                                  | Homepage                                            |
| ------------------------------ | ------------------------ | -------------------------------------------------------------------------------------------------- | --------------------------------------------------- |
| Npm                            | npm:(oss name)           | 우선순위1. repository in package.json <br> 우선순위2. npmjs.com/package/(oss name)/v/(oss version) | npmjs.com/package/(oss name)                        |
| Pip                            | pypi:(oss name)          | pypi.org/project/(oss name)/(version)                                                              | homepage in (pip show) information                  |
| Maven<br>& Gradle<br>& Android | (group_id):(artifact_id) | mvnrepository.com/artifact/(group id)/(artifact id)/(version)                                      | mvnrepository.com/artifact/(group id)/(artifact id) |
| Pub                            | pub:(oss name)           | pub.dev/packages/(oss name)/versions/(version)                                                     | homepage in (pub information)                       |
| Cocoapods                      | cocoapods:(oss name)     | source in (pod spec information)                                                                   | cocoapods.org/pods/(oss name)                            |
| Swift                      | swift:(oss name)     | repositoryURL in Package.resolved                                                                   | repositoryURL in Package.resolved                            |
| Carthage                      | carthage:(oss name)     | github repository in Cartfile.resolved                                                                   | github repository in Cartfile.resolved                            |
| Go                      | go:(oss name)     | pkg.go.dev/(oss name)@(oss version)                                                                   | repository in pkg.go.dev/(oss name)@(oss version)                        |

## 🧐 동작 방식
FOSSLight Dependency Scanner는 패키지 매니저에 따른 dependency를 분석하기 위해 오픈 소스 소프트웨어를 활용합니다. 이때 활용되는 오픈 소스 소프트웨어는 direct dependency뿐만 아니라 transitive dependency까지 추출 가능하며, 오픈소스명, 버전, 라이선스명을 추출 가능합니다.

각 패키지 매니저별 사용하는 소프트웨어는 다음과 같습니다:

- NPM : [NPM License Checker](https://github.com/davglass/license-checker)
- Pypi : [pip-licenses](https://github.com/raimon49/pip-licenses)
- Gradle : [License Gradle Plugin](https://github.com/hierynomus/license-gradle-plugin)
- Maven : [license-maven-plugin](https://github.com/mojohaus/license-maven-plugin)
- Pub : [flutter_oss_licenses](https://github.com/espresso3389/flutter_oss_licenses)
- Android(gradle) : [android-dependency-scanning](https://github.com/fosslight/android-dependency-scanning)

이에 패키지 매니저마다 각기 다른 오픈 소스 소프트웨어를 활용함으로써, FOSSLight Dependency Scanner를 실행하기 위해 패키지 매니저별 **Prerequisite** 단계를 먼저 수행해야 합니다.
