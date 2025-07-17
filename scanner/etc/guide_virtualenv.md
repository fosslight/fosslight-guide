---
published: true
---
# Virtualenv 세팅 가이드

Python package를 설치 및 실행하기 위한 virtualenv 환경 세팅하는 가이드입니다.

## Contents
- [추가 Package 설치](#pre)
- [Python, python-dev 설치](#python)
- [virtualenv 세팅하는 법](#virtualenv)
- [virtualenv 명령어](#command)

## 📋 <a name="pre"></a>Prerequisite
macOS의 경우, 하기 package를 추가로 설치합니다.
```
brew install openssl
brew install libmagic
brew install postgresql
```

## 💻 <a name="python"></a>Python, python-dev 설치

- Python 설치 방법은 [설치 가이드][install] 링크를 참조하세요.
- 사용하는 python 버전에 맞게 python-dev, python-distutils를 설치합니다.
  ```
  $ sudo apt-get install python3.10 python3-pip python3.10-dev python3.10-distutils
  ```

[install]: https://realpython.com/installing-python

## 📋 <a name="virtualenv"></a>virtualenv 생성하고 활성화하는 법

```
$ pip3 install virtualenv
$ virtualenv -p /usr/bin/python3.10 venv
$ source venv/bin/activate
```
자세한 virtualenv 설명: [Python virtualenv page][venv]

[venv]: https://docs.python.org/3.10/library/venv.html

## ⌨️ <a name="command"></a>virtualenv 명령어

| Command description  | command |
| ------------- | ------------- |
| 가상환경 생성 | virtualenv -p [python_version] [env_name] |
| 가상환경 활성화 | source [env_name]/bin/activate |
| 가상환경 비활성화 | deactivate |
