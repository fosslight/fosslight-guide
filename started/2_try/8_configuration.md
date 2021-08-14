---
sort: 8
published: true
---
# Configuration
```note
(Admin Only) FOSSLight 운영을 위한 세팅 값을 변경합니다. 
```

![config](../images/8-3_configuration.png)

## Authentication using LDAP
FOSSLight는 JNDI를 사용하여 Active Directory 등 LDAP을 사용할 수 있는 환경에서는 LDAP을 이용한 사용자 패스워드 인증 처리를 지원합니다.
- Provider Url: LDAP 서버 정보를 ldap://&lt;AD_SERVER_IP&gt;:&lt;LDAP_PORT&gt; 형식으로 설정합니다. (javax.naming.Context.PROVIDER_URL)

## SMTP Setting

- Mail Server: SMTP Host (예 smtp.gmail.com )
- Email Address: 발송자 이메일 주소( 예 no-reply@fosslight.org )
- Port: SMTP Port 번호 (예 25 또는 587)
- Encoding: Default UTF-8 (필요한 경우만 변경)
- Username: SMTP 사용자명 (일반적으로는 발송자 이메일 주소와 동일)
- Password: SMTP 사용자 패스워드 (패스워드는 암호화되어 저장되며, 공백인 경우 기존 패스워드를 변경하지 않습니다.)

## Workspace Path Setting
- Root Path: 업/다운로드 파일 저장소의 최상위 work space 경로

## Notice Setting
- Notice Type: 발급 가능한 OSS 고지문 형식을 설정합니다.
