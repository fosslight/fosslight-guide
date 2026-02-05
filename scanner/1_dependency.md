---
published: true
title: "  ㄴ FOSSLight Dependency Scanner"
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
- [PNPM](https://pnpm.io/) (Node.js)
- [Yarn](https://yarnpkg.com/) (Node.js)
- [PyPi](https://pip.pypa.io/) (Python)
- [Pub](https://pub.dev/) (Dart with flutter)
- [Cocoapods](https://cocoapods.org/) (Swift/Obj-C)
- [Swift](https://swift.org/package-manager/) (Swift)
- [Carthage](https://github.com/Carthage/Carthage) (Carthage)
- [Go](https://pkg.go.dev/) (Go)
- [Nuget](https://www.nuget.org/) (.NET)
- [Helm](https://helm.sh/) (Kubernetes)
- [Unity](https://unity.com/) (Unity)
- [Cargo](https://crates.io/) (Rust)
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
  - [패키지별 지원 레벨](#-패키지별-지원-레벨)


## 📋 필요 조건
각 패키지 매니저마다 다른 오픈소스 소프트웨어를 이용하여 Dependency 분석을 수행하고 있습니다. 이에 분석하고자 하는 패키지 매니저에 따라 각각의 Prerequisite 단계를 수행하시기 바랍니다.

{::options parse_block_html="true" /}
<details>
<summary markdown="span">**Prerequisite for Npm or Yarn**</summary>
1. Dependency 분석을 수행하기 위해 NPM License Checker를 설치합니다.
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
<summary markdown="span">**Prerequisite for Pnpm**</summary>
```tip
FOSSLight Dependency Scanner 내부에서 'pnpm install' 및 'pnpm ls' 명령어를 통해 패키지 목록 및 license, repository와 같은 오픈소스 정보를 취합하고 있습니다. 이에 별도의 prerequisite단계없이, 바로 fosslight_dependency 명령어 실행하여 이용하실 수 있습니다.
```
</details>

<details>
<summary markdown="span">**Prerequisite for Gradle**</summary>
1. 'build.gradle' 파일에 License Gradle Plugin을 추가합니다.
```
plugins {
    id 'com.github.hierynomus.license' version '0.16.1' // gradle 버전이 6.x 이하인 경우에는 version '0.15.0'을 이용해야 합니다.
}
downloadLicenses {
    includeProjectDependencies = true
    dependencyConfiguration = 'runtimeClasspath' // gradle 버전이 4.6 이하인 경우에는 'runtimeClasspath' 대신 'runtime'으로 추가합니다.
}
```

2. 'downloadLicenses' task를 실행합니다.
```
$ gradlew downloadLicenses
```

</details>

<details>
<summary markdown="span">**Prerequisite for Android (gradle)**</summary>
```tip
Android (gradle)의 경우, input directory에 gradlew 실행 파일 및 build.gradle 파일이 존재하는 경우, plugin 추가 및 실행을 FOSSLight Dependency Scanner 내부에서 자동으로 수행하므로 다음은 skip하셔도 됩니다.
```
##### java/groovy project
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

2. 플러그인이 적용되는 app 디렉토리 내에 위치한 build.gradle 파일 내에 다음과 같이 추가합니다. 이때 plugins 블록 (또는 apply plugin:'com.android.application') 하위 라인에 추가되어야 에러가 발생하지 않습니다.
```
apply plugin: 'org.fosslight'
```

3. 'generateLicenseTxt' task를 실행합니다.
```
$ gradlew generateLicenseTxt
```

##### kotlin project
1. 'build.gradle.kts' 파일에 android-dependency-scanning plugin을 추가합니다.
```
buildscript {
    dependencies {
        classpath("org.fosslight:android-dependency-scanning:1.0.0")
    }
}
```

2. settings.gradle.kts 파일에 pluginManagement > repositories 내 mavenCentral을 추가합니다.
```
pluginManagement {
    repositories {
        mavenCentral()
    }
}
```

3. 플러그인이 적용되는 app 디렉토리 내에 위치한 build.gradle.kts 파일 내에 아래와 같이 추가합니다.
```
plugins {
    id("org.fosslight")
}
```

4. 'generateLicenseTxt' task를 실행합니다.
```
$ gradlew generateLicenseTxt
```
</details>

<details>
<summary markdown="span">**Prerequisite for Pypi**</summary>
```tip
- 시스템 내 전역으로 설치된 파이썬 dependency로부터 분석하고자 하는 프로젝트 dependency를 분리하기 위해 가상환경을 설정하여 이용하기를 권장합니다.
- 만약 input path내 requirements.txt가 존재한다면, FOSSLight Dependency Scanner가 자동으로 dependency 설치하여 분석 실행 가능하므로, 2번 단계부터는 skip합니다.
```

1. python3-venv를 설치합니다.
```
$ sudo apt-get install python3-venv
```
2. 가상환경을 생성하고 활성화합니다.
```
// virtualenv example
$ virtualenv -p /usr/bin/python3.10 venv
$ source venv/bin/activate
// conda example
$ conda create --name {venv name}
$ conda activate {venv name}
```
3. 가상환경 내 분석하고자 하는 프로젝트에서 사용된 패키지를 설치합니다.
4. FOSSLight Dependency Scanner 실행 시, '-a', '-d' 옵션을 이용하여 해당 가상환경 activate, deactivate 명령어를 추가합니다.
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
> FOSSLight Dependency Scanner 실행하는 환경에서 flutter pub 명령어 사용 가능하지 않은 경우,  flutter pub 사용 가능한 환경에서 미리 아래 과정을 수행하시기 바랍니다.
1. 다음 명령어를 통해 flutter_oss_licenses를 실행합니다. (optional)
```
$ flutter pub add dev:flutter_oss_licenses:'^2.0.1'
$ flutter pub get
$ flutter pub deps --json > tmp_deps.json
$ flutter pub deps --no-dev -s compact > tmp_no_dev_deps.txt
$ flutter pub run flutter_oss_licenses:generate.dart -o tmp_flutter_oss_licenses.json --json
```
2. 1번 수행 결과에서 생성된 파일이 존재하는 path에서 FOSSLight Dependency Scanner를 실행합니다.
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
Token생성 방법은 [Github docs 가이드](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and/data-secure/creating-a-personal-access-token)를 참조하시기 바랍니다.
</details>

<details>
<summary markdown="span">**Prerequisite for Go**</summary>
```tip
Go의 경우, go module에 한해 dependency 분석을 지원합니다. FOSSLight Dependency Scanner 내부에서 자동으로 'go list -m all' 명령어를 수행하여 dependency 목록을 얻은 뒤, license, repository와 같은 오픈소스 정보를 취합하고 있습니다. 이에 별도의 prerequisite단계없이, 바로 fosslight_dependency 명령어 실행하여 이용하실 수 있습니다.
```
</details>

<details>
<summary markdown="span">**Prerequisite for Nuget**</summary>
```tip
FOSSLight Dependency Scanner 내부에서 packages.config 파일 또는 PackageReference형태로 이용하는 경우 obj/project.assets.json 파일을 통해 패키지 목록을 확인하고, nuget api를 통해 license, repository와 같은 오픈소스 정보를 취합하고 있습니다. 이에 별도의 prerequisite단계없이, 바로 fosslight_dependency 명령어 실행하여 이용하실 수 있습니다.
CPM 프로젝트인 경우, Directory.Packages.props가 존재하는 path에서 실행하셔야 dependency 분석 가능합니다. 이때 obj/project.assets.json 파일이 존재하지 않는 경우, 실행 path 하위 내 .csproj 또는 .sln이 존재하는 path에서 자동으로 'dotnet restore' 명령어 수행하여 obj/project.assets.json 파일 생성한 뒤 Dependency 분석 수행합니다.
```
</details>

<details>
<summary markdown="span">**Prerequisite for Helm**</summary>
```tip
FOSSLight Dependency Scanner 내부에서 Chart.yaml 파일과 helm dependency build 명령어를 통해 패키지 목록 및 license, repository와 같은 오픈소스 정보를 취합하고 있습니다. 이에 별도의 prerequisite단계없이, 바로 fosslight_dependency 명령어 실행하여 이용하실 수 있습니다.
```
</details>

<details>
<summary markdown="span">**Prerequisite for Unity**</summary>
```tip
FOSSLight Dependency Scanner 내부에서 Library/PackageManager/ProjectCache 파일과 Library/PackageCache 디렉토리 내 각 패키지 디렉토리에서 패키지 목록 및 license, repository와 같은 오픈소스 정보를 취합하고 있습니다. 이에 해당 파일들이 존재하는 환경에서 fosslight_dependency 명령어 실행하여 이용하실 수 있습니다.
```
</details>

<details>
<summary markdown="span">**Prerequisite for Cargo**</summary>
```tip
FOSSLight Dependency Scanner 내부에서 Cargo.toml 파일과 'cargo metadata' 명령어를 통해 패키지 목록 및 license, repository와 같은 오픈소스 정보를 취합하고 있습니다. 이에 별도의 prerequisite단계없이, 바로 fosslight_dependency 명령어 실행하여 이용하실 수 있습니다.
```
</details>
{::options parse_block_html="false" /}

## 🎉 설치 방법

FOSSLight Dependency Scanner는 pip3를 이용하여 설치할 수 있습니다.     
[python 3.10 + virtualenv](etc/guide_virtualenv.md) 환경에서 설치할 것을 권장합니다.

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
            -v                              Print the version of the script.
            -m <package_manager>            Enter the package manager.
                                                (npm, maven, gradle, pypi, pub, cocoapods, android, swift, carthage, go, nuget, helm, unity, cargo, pnpm, yarn)
            -p <input_path>                 Enter the path where the script will be run.
            -e <exclude_path>               Enter the path where the analysis will not be performed.(Files and directories, pattern matching is available)
                                             * IMPORTANT: Always wrap patterns in quotes("") to avoid shell expansion.
                                               Example) fosslight_dependency -e "dev/" "tests/
            -o <output_path>                Output path
                                                (If you want to generate the specific file name, add the output path with file name.)
            -f <format> [<format> ...]      Output formats
                                               (excel, csv, opossum, yaml, spdx-yaml, spdx-json, spdx-xml, spdx-tag, cyclonedx-json, cyclonedx-xml)
                                            Multiple formats can be specified separated by space.
            --graph-path <save_path>        Enter the path where the graph image will be saved
                                                (ex. /your/directory/path/filename.{pdf, jpg, png}) (recommend pdf extension)
            --graph-size <width> <height>   Enter the size of the graph image (The size unit is pixels)
                                                --graph-path option is required
            --direct                        Print the direct/transitive dependency type in comment.
                                                Choice 'True' or 'False'. (default:True)
            -r                              Recursive mode. Scan all subdirectories for manifest files.
            --notice                        Print the open source license notice text.

        Required only for swift, carthage
            -t <token>                      Enter the github personal access token.

        Optional only for pypi
            -a <activate_cmd>               Virtual environment activate command(ex, 'conda activate (venv name)')
            -d <deactivate_cmd>             Virtual environment deactivate command(ex, 'conda deactivate')

        Optional only for gradle, maven
            -c <dir_name>                   Enter the customized build output directory name
                                                -Default name : 'build' for gradle, 'target' for maven

        Optional only for android
            -n <app_name>                   Enter the application directory name where the plugin output file is located(default: app)

```
- -e 옵션 관련 [Pattern 매칭 가이드](https://scancode-toolkit.readthedocs.io/en/stable/cli-reference/scan-options-pre.html?highlight=ignore#glob-pattern-matching)
   - ⚠️ 사용 시 반드시 쌍 따옴표("")를 이용하여 입력하시기 바랍니다.
       - 예시) fosslight_dependency -e "dev/" "tests/"
   - ⚠️ 입력 시 파일명과 확장자는 대소문자를 정확히 구분해야 합니다.

### Tips to run
FOSSLight Dependency Scanner 실행 시, 기본적으로 input path('-p' 옵션)부터 순차적으로 패키지 매니저의 manifest 파일을 감지하고, 만약 manifest 파일이 감지된다면 더 이상 하위 path에 대해 manifest 파일 감지를 중지하고, dependency 분석을 수행합니다.
(만약 전체 input path에 대해 존재하는 manifest 파일에 대해 dependency 분석 수행을 원하시는 경우, '-r' 옵션을 추가하여 실행하시기 바랍니다.)
각 패키지 매니저별 manifest 파일은 다음과 같습니다.
```
  - Npm : package.json
  - Pnpm : pnpm-lock.yaml
  - Yarn : package.json
  - Pypi : requirements.txt / setup.py / pyproject.toml
  - Maven : pom.xml
  - Gradle (Android) : build.gradle
  - Pub : pubspec.yaml
  - Cocoapods : Podfile
  - Swift : Package.resolved
  - Carthage : Cartfile.resolved
  - Go : go.mod
  - Nuget : packages.config / {project name}.csproj / Directory.Packages.props
  - Helm : Chart.yaml
  - Unity : Library/PackageManager/ProjectCache
  - Cargo : Cargo.toml
```

- Android (gradle)
  - module name이 default인 app이 아닌 경우, module name을 '-n' 옵션으로 지정하여 실행하셔야 합니다. (fosslight_dependency -n {module_name})
- Swift package manager
  - 예외적으로 Swift package manager는 {프로젝트명}.xcodeproj 파일이 위치한 path에서 "fosslight_dependency -m swift -t {token}" 명령어를 실행하실 수 있습니다.
  - 이 경우에는 {프로젝트명}.xcodeproj/project.xcworkspace/xcshareddata/swiftpm path에서 'Package.resolved' 파일을 자동으로 찾고 프로그램이 실행됩니다.
- Unity
  - Library 폴더 존재하는 directory에서 "fosslight_dependency -m unity" 명령어를 실행하시면 됩니다.


## 📁 결과
```
$ tree
.
├── fosslight_report_dep_210503_0039.xlsx
├── fosslight_log_210503_0039.txt
└── fosslight_opossum_210503_0039.json
```
- fosslight_report_dep_[datetime].xlsx : FOSSLight Report 형태의 Dependency 분석 결과
- fosslight_log_dep_[datetime].txt: 실행 로그가 저장된 파일
- fosslight_opossum_dep_[datetime].json : [OpossumUI](https://github.com/opossum-tool/OpossumUI)에서 활용 가능한 Dependency 분석 결과 (-f opossum 결과)
- third_party_notice.txt : Unity로 실행한 경우에만 생성되는 파일로써, 각 패키지의 third party notice를 모아서 출력함

### Graph Network 생성 결과
``` bash
# $ fosslight_dependency -p /project/path --graph-path ~/temp/graph.png --graph-size 1000 1000
$ cd ~/temp
$ tree
.
└── graph.png
```

<img src="images/fosslight_depenency_graph.png" width="600">
- fosslight_report_dep_[datetime].xlsx 파일의 결과 중 Depends On 부분을 이용하여 각 Dependency 간의 의존 관계 그래프 이미지 저장

### 결과 파일 내용
FOSSLight Report 결과 파일에는 transitive dependency들을 포함한 모든 분석된 dependency들의 manifest 파일을 기반으로 OSS 정보가 기록됩니다.
이때, 고유한 OSS명을 작성하기 위해, OSS명은 (패키지 매니저):(OSS명) 또는 (group id):(artifact id) 양식으로 기록됩니다.

| Package manager                | OSS Name                 | Download Location                                                                                  | Homepage                                            |
| ------------------------------ | ------------------------ | -------------------------------------------------------------------------------------------------- | --------------------------------------------------- |
| Npm, Pnpm, Yarn                | npm:(oss name)           | npmjs.com/package/(oss name)/v/(oss version)                                                       | 우선순위1. repository in package.json <br> 우선순위2. npmjs.com/package/(oss name)  |
| Pypi                           | pypi:(oss name)          | pypi.org/project/(oss name)/(version)                                                              | homepage in (pip show) information                  |
| Maven<br>& Gradle<br>& Android | (group_id):(artifact_id) | mvnrepository.com/artifact/(group id)/(artifact id)/(version)                                      | mvnrepository.com/artifact/(group id)/(artifact id) |
| Pub                            | pub:(oss name)           | pub.dev/packages/(oss name)/versions/(version)                                                     | homepage in (pub information)                       |
| Cocoapods                      | cocoapods:(oss name)     | source in (pod spec information)                                                                   | cocoapods.org/pods/(oss name)                       |
| Swift                          | swift:(oss name)         | repositoryURL in Package.resolved                                                                  | repositoryURL in Package.resolved                   |
| Carthage                       | carthage:(oss name)      | github repository in Cartfile.resolved                                                             | github repository in Cartfile.resolved              |
| Go                             | go:(oss name)            | pkg.go.dev/(oss name)@(oss version)                                                                | repository in pkg.go.dev/(oss name)@(oss version)   |
| Nuget                          | nuget:(oss name)         | 우선순위1. repository in nuget.org/packages/(oss name)/(oss version) <br> 우선순위2. projectUrl in nuget.org/packages/(oss name)/(oss version) <br> 우선순위3. nuget.org/packages/(oss name)/(oss version)  | nuget.org/packages/(oss name) |
| Helm                           | helm:(oss name)          | first url of sources in (Chart.yaml)                                                               | home in (Chart.yaml)                                |
| Unity                          | (oss name)               | url in repository in ProjectCache                                                                  | url in repository in ProjectCache                   |
| Cargo                          | cargo:(oss name)         | repository of the package in the result file for 'cargo metadata'                                  | crates.io/crates/(oss name)                        |


```warning
- Npm, Maven, gradle의 결과 파일 내용 중, Local path나 local repository를 통해 설치된(npmjs.com / mvnrepository에 배포되지 않은) 패키지의 경우, download location이 실제와 다를 수 있습니다.
- Helm은 root 프로젝트의 Chart.yaml파일에 작성된 dependencies 항목에 대해서만 출력 가능하며, 각 dependency의 dependency 항목 출력은 현재 지원하지 않고 있습니다. 또한, 'helm dependency build' 명령어 수행 후 charts/ 디렉토리 내 다운로드된 .tgz 파일 내 Chart.yaml 파일 정보에서 각 dependency의 OSS 정보를 얻어오고 있습니다.
따라서 Chart.yaml에 License 또는 Homepage와 같은 정보가 누락된 경우, 해당 정보 얻어올 수 없기에 사용자가 수기로 확인 및 보완하는 작업이 필요합니다.
```

## 🧐 동작 방식
FOSSLight Dependency Scanner는 패키지 매니저에 따른 dependency를 분석하기 위해 오픈 소스 소프트웨어를 활용합니다. 이때 활용되는 오픈 소스 소프트웨어는 direct dependency뿐만 아니라 transitive dependency까지 추출 가능하며, 오픈소스명, 버전, 라이선스명을 추출 가능합니다.

각 패키지 매니저별 사용하는 소프트웨어는 다음과 같습니다:

- NPM : [NPM License Checker](https://www.npmjs.com/package/license-checker)
- Gradle : [License Gradle Plugin](https://github.com/hierynomus/license-gradle-plugin)
- Maven : [license-maven-plugin](https://github.com/mojohaus/license-maven-plugin)
- Pub : [flutter_oss_licenses](https://pub.dev/packages/flutter_oss_licenses)
- Android(gradle) : [android-dependency-scanning](https://github.com/fosslight/android-dependency-scanning)
- Pypi : [pipdeptree](https://pypi.org/project/pipdeptree/)

이에 패키지 매니저마다 각기 다른 오픈 소스 소프트웨어를 활용함으로써, FOSSLight Dependency Scanner를 실행하기 위해 패키지 매니저별 **Prerequisite** 단계를 먼저 수행해야 합니다.

## 👀 패키지별 지원 레벨
<table>
<thead>
  <tr>
    <th>Language/<br>Project</th>
    <th>Package Manager</th>
    <th>Manifest file</th>
    <th>Direct dependencies</th>
    <th>Transitive dependencies</th>
    <th>Relationship of dependencies<br>(Dependencies of each dependency)</th>
    <th>Internet Access<br>Required</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td rowspan="3">Javascript</td>
    <td>Npm</td>
    <td>package.json</td>
    <td>O</td>
    <td>O</td>
    <td>O</td>
    <td>X</td>
  </tr>
  <tr>
    <td>Pnpm</td>
    <td>pnpm-lock.yaml</td>
    <td>O</td>
    <td>O</td>
    <td>O</td>
    <td>X</td>
  </tr>
  <tr>
    <td>Yarn</td>
    <td>package.json</td>
    <td>O</td>
    <td>O</td>
    <td>O</td>
    <td>X</td>
  </tr>
  <tr>
    <td rowspan="2">Java</td>
    <td>Gradle</td>
    <td>build.gradle</td>
    <td>O</td>
    <td>O</td>
    <td>O</td>
    <td>X</td>
  </tr>
  <tr>
    <td>Maven</td>
    <td>pom.xml</td>
    <td>O</td>
    <td>O</td>
    <td>O</td>
    <td>X</td>
  </tr>
  <tr>
    <td>Java (Android)</td>
    <td>Gradle</td>
    <td>build.gradle</td>
    <td>O</td>
    <td>O</td>
    <td>O</td>
    <td>X</td>
  </tr>
  <tr>
    <td rowspan="2">ObjC, Swift (iOS)</td>
    <td>Cocoapods</td>
    <td>Podfile.lock</td>
    <td>O</td>
    <td>O</td>
    <td>O</td>
    <td>X</td>
  </tr>
  <tr>
    <td>Carthage</td>
    <td>Cartfile.resolved</td>
    <td>O</td>
    <td>O</td>
    <td>X</td>
    <td>O</td>
  </tr>
  <tr>
    <td>Swift (iOS)</td>
    <td>Swift</td>
    <td>Package.resolved</td>
    <td>O</td>
    <td>O</td>
    <td>O</td>
    <td>O</td>
  </tr>
  <tr>
    <td>Dart, Flutter</td>
    <td>Pub</td>
    <td>pubspec.yaml</td>
    <td>O</td>
    <td>O</td>
    <td>O</td>
    <td>X</td>
  </tr>
  <tr>
    <td>Go</td>
    <td>Go</td>
    <td>go.mod</td>
    <td>O</td>
    <td>O</td>
    <td>O</td>
    <td>O</td>
  </tr>
  <tr>
    <td>Python</td>
    <td>Pypi</td>
    <td>requirements.txt,<br>setup.py,<br>pyproject.toml</td>
    <td>O</td>
    <td>O</td>
    <td>O</td>
    <td>X</td>
  </tr>
  <tr>
    <td>.NET</td>
    <td>Nuget</td>
    <td>packages.config,<br>obj/project.assets.json</td>
    <td>O</td>
    <td>O</td>
    <td>O</td>
    <td>O</td>
  </tr>
  <tr>
    <td>Kubernetes</td>
    <td>Helm</td>
    <td>Chart.yaml</td>
    <td>O</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
  </tr>
  <tr>
    <td>Unity</td>
    <td>Unity</td>
    <td>Library/PackageManager/ProjectCache</td>
    <td>O</td>
    <td>O</td>
    <td>X</td>
    <td>X</td>
  </tr>
  <tr>
    <td>Rust</td>
    <td>Cargo</td>
    <td>Cargo.toml</td>
    <td>O</td>
    <td>O</td>
    <td>O</td>
    <td>X</td>
  </tr>
</tbody>
</table>

```tip
**인터넷 접근 필요 기준**: 로컬 manifest/lock/캐시/플러그인 출력만으로 라이선스·홈페이지 등 OSS 정보를 해석할 수 없으면 인터넷 접근이 필요합니다.
```
