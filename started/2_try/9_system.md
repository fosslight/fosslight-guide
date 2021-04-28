---
sort: 9
published: true
---
# System
```note
(Admin Only) FOSSLight 운영 Log를 확인하거나 운영 Data를 변경합니다.
```

## Code Management
![config](../images/9_system_code.png)
- 시스템 동작시 읽을 세팅 값을 설정합니다.

## User Management
![config](../images/9_system_user.png)
등록된 계정 목록을 확인하고 정보를 수정합니다. 
- Create 버튼: [Rest API](../../features/1_rest_api.md)에서 사용할 Token을 생성합니다. 
- reset 버튼 : 비밀번호를 ID와 동일하게 초기화합니다. 
- Use YN : 휴면 계정을 설정합니다. 
- Admin : Admin 권한을 부여합니다.

## History List
![config](../images/9_system_history.png)
DB의 Data 변경 사항을 확인합니다. 

## Notification
시스템 접속시 띄울 공지 팝업을 관리합니다.
### ![config](../images/9_system_noti_list.png)
등록되었던 공지 목록을 확인, 수정합니다. 

### ![config](../images/9_system_noti_add.png)
List 왼쪽 하단의 + 버튼을 클릭하여 공지를 추가합니다.
- Start Date : 공지 시작일
- End Date : 공지 종료일
- Publish : 체크된 경우, 공지 팝업을 띄웁니다.

## Sent Mail List
![config](../images/9_system_mail.png)
메일 발송 내역을 확인합니다.

## Vulnerability Log
![config](../images/9_system_vul.png)
Vulnerability Data 변경 사항을 확인합니다.

