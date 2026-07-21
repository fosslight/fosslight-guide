---
published: true
---

# FOSSLight Database 연동 가이드
FOSSLight Scanner와 FOSSLight Source Scanner에서 FOSSLight Database에 접속하는 방법을 안내합니다.

## Scanner Database 연결
OSS Information (OSS Name, OSS Version, License)를 DB로부터 출력하려면 DB 접속 정보가 필요합니다.

### Linux / macOS
현재 터미널 세션에만 적용:
````
export KB_URL="https://kb.example.org/"
export KB_TOKEN="example-token-1234567890abcdef"
````

쉘 시작 파일에 추가하여 계속 사용:
````
echo 'export KB_URL="https://kb.example.org/"' >> ~/.bashrc
echo 'export KB_TOKEN="example-token-1234567890abcdef"' >> ~/.bashrc
source ~/.bashrc
````

zsh 사용자는 `~/.zshrc`에 추가합니다.

### Windows Command Prompt
현재 세션에만 적용:
````
set KB_URL=https://kb.example.org/
set KB_TOKEN=example-token-1234567890abcdef
````

사용자 환경변수로 저장:
````
setx KB_URL "https://kb.example.org/"
setx KB_TOKEN "example-token-1234567890abcdef"
````

### Windows PowerShell
현재 세션에만 적용:
````
$env:KB_URL = "https://kb.example.org/"
$env:KB_TOKEN = "example-token-1234567890abcdef"
````

사용자 환경변수로 저장:
````
[System.Environment]::SetEnvironmentVariable("KB_URL", "https://kb.example.org/", "User")
[System.Environment]::SetEnvironmentVariable("KB_TOKEN", "example-token-1234567890abcdef", "User")
````
