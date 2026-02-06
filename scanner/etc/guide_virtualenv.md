---
published: true
---
# Virtualenv 세팅 가이드

python 패키지를 설치하고 실행하기 위한 virtualenv 환경 설정 가이드입니다.<br><br>  
## About Virtualenv  
{: .left-bar-title}  
- python을 새로 설치해서 내가 원하는 모듈만 운용하는 바구니와 같습니다.  
- Virtualenv는 시스템에 설치된 Python 환경에 영향을 주지 않도록 독립적인 가상 환경을 생성하는 도구입니다. 이 환경에서는 별도의 python 실행 파일과 라이브러리 경로가 제공되며, 패키지를 설치해도 시스템 전체의 python 설정에는 영향을 미치지 않습니다. 또한 여러 버전의 python을 설치해 두고, 그중 원하는 버전을 선택해 가상 환경을 구성할 수 있습니다. 
<br><br> 

## 필요 조건
{: .left-bar-title}  
### 1.Unix 계열 (Ubuntu, macOS)
{: .specific-title}
#### 1-1.Ubuntu
예) python 3.10 설치(권장 Python 버전 : 3.10 ~3.12)
  ```
  $ sudo apt-get update
  $ sudo apt-get install python3.10 python3-pip python3.10-dev python3.10-distutils
  ```  

#### 1-2.macOS
```
brew install openssl
brew install libmagic
brew install postgresql
brew install python3  
```    

### 2.Windows  
{: .specific-title}  

<details>
  <summary markdown="span">python 설치(권장 버전: 3.10 ~ 3.12)</summary>
    https://www.python.org/downloads/windows/에서 installer 선택하여 다운로드 및 실행합니다.<br> 
    1. Add Python.exe to PATH 체크 후 Install Now 실행합니다.<br>
    <img src="../images/fl_scanner_win_python_1.png" alt="python1"><br><br>
    2. Disable path length limit을 클릭합니다.<br>
    <img src="../images/fl_scanner_win_python_2.png" alt="python2"><br>
</details>  


<details>
  <summary markdown="span">OpenJDK 설치 (권장 버전: 11)</summary>
    https://learn.microsoft.com/ko-kr/java/openjdk/download에서 환경에 맞는 .msi 파일을 다운로드 및 실행합니다.<br>  
    1. .msi 파일 다운로드 및 실행합니다.<br>
    <img src="../images/fl_scanner_win_jdk_1.png" alt="jdk1"><br><br>
    2. 'Set or overide Jave_HOME...'을 선택합니다.<br>
    <img src="../images/fl_scanner_win_jdk_2.png" alt="jdk2"><br>
</details>   



<details>
  <summary markdown="span">Microsoft Visual C++ 설치 (권장 버전: 14.0 이상)</summary>
    https://visualstudio.microsoft.com/visual-cpp-build-tools/에서 build tool 다운로드 및 실행합니다.<br>
    1. C++를 사용한 데스크톱 개발 체크 후 설치합니다.<br>
    <img src="../images/fl_scanner_vs_2.png" alt="jdk1"><br><br>
</details>   

<details>
  <summary markdown="span"> Git 설치</summary>
    https://git-scm.com/download/win에서 환경에 맞는 Git 설치 파일(Installer)을 다운로드 합니다.<br> 
</details> 

<br><br>


## virtualenv 생성하고 활성화하는 법  
{: .left-bar-title} 
- 자세한 virtualenv 설명은 [Python virtualenv page](https://docs.python.org/3.10/library/venv.html)를 참고하시기 바랍니다.    

### 1.Ubuntu
{: .specific-title}
- virtualenv 설치 및 실행  
  ```
  $ pip3 install virtualenv
  $ virtualenv -p /usr/bin/python3.10 venv
  $ source venv/bin/activate
  ```
     
- virtualenv 명령어  

    | **Command Description** | **Command** |  
    |-------------------------|-------------|  
    | 가상환경 생성 | virtualenv -p [python_version] [env_name] |  
    | 가상환경 활성화 | source [env_name]/bin/activate |  
    | 가상환경 비활성화 | deactivate |    
    

### 2.MacOS
{: .specific-title}
- virtualenv 설치 및 실행  
```
# pip 존재하지 않는 경우
$ wget https://bootstrap.pypa.io/get-pip.py
$ python3 get-pip.py
$ pip 'install' virtualenv
$ virtualenv -p /usr/bin/python3.10 venv
$ source venv/bin/activate
```     

### 2.Windows
{: .specific-title}  
- virtualenvwrapper 설치   
```
$ pip install virtualenv
$ pip install virtualenvwrapper-win
$ mkvirtualenv venv
$ workon venv
``` 
  <details>
      <summary markdown="span">설치 시 발생할 수 있는 error</summary>
      <pre>  
        1.Building wheel for py-tlsh (setup.py) error 발생 <br>
          - Microsoft Visual C++ 14.0 이상 버전 다운로드 및 설치합니다.<br> 
        2.'LINK : fatal error LNK1158: 'rc.exe'을(를) 실행할 수 없습니다.' 라는 error 발생 <br>
        - C:\Program Files (x86)\Windows Kits\10\bin\10.xxx\x86 의 'rc.exe'와 'rcdll.dll' 파일을 C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\Tools 로 Copy합니다.<br>
      </pre>
  </details>    


- virtualenvwrapper-win 명령어    

    | **Command Description** | **Command** |  
    |-------------------------|-------------|  
    | 환경 비활성화 | deactivate |  
    | 가상 환경 생성 | mkvirtualenv [가상환경_이름] |  
    | 환경 활성화 | workon [가상환경_이름] |    