---
published: true
---

# FOSSLight Scanner Database 연동 가이드
`fosslight` 또는 `fosslight_source` 명령어 실행 시 `--kb_url`, `--kb_token` 옵션을 지정하면 FOSSLight Scanner Database의 OSS Information(OSS Name, OSS Version, Download location)을 추가로 조회할 수 있습니다.

예시 (fosslight, 현재 directory 분석하는 경우):
````
fosslight --kb_url "https://kb.example.org/" --kb_token "example-token-1234567890abcdef" -p .
````

예시 (fosslight_source, 현재 directory 분석하는 경우):
````
fosslight_source --kb_url "https://kb.example.org/" --kb_token "example-token-1234567890abcdef" -p .
````


## Scanner Database 접속 정보 저장
아래와 같이 FOSSLight Database 접속 정보를 저장하면, 명령어 실행 시 `--kb_url`, `--kb_token`을 매번 입력하지 않아도 저장된 값이 자동으로 적용됩니다.

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
