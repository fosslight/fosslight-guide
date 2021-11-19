# Contribution

## Report an issue
개선 사항이나 버그는 Git Repository에 이슈를 생성하여 리포트해주시기 바랍니다. 이슈 리포트는 FOSSLight 업그레이드에 많은 도움이 됩니다.
- [FOSSLight Hub](https://github.com/fosslight/fosslight/issues)
- [FOSSLight Dependency Scanner](https://github.com/fosslight/fosslight_dependency_scanner/issues)
- [FOSSLight Source Scanner](https://github.com/fosslight/fosslight_source_scanner/issues)
- [FOSSLight Reuse](https://github.com/fosslight/fosslight_reuse/issues)
- [FOSSLight Guide](https://github.com/fosslight/fosslight-guide/issues)

## Contributing

당신의 기여는 이 프로젝트를 훌륭하게 유지하는 데 필수적입니다.

### Pull request 생성하기

1. Repository를 Fork 하고 clone 합니다.
2. branch를 생성합니다. : `git checkout -b my-branch-name`
3. 당신의 수정사항을 반영하고 동작성을 확인하십시오.
4. 당신의 수정사항을 Commit하고, [Developer Certificate of Origin](#Developer-Certificate-of-Origin)에 대한 서명을 합니다.
5. fork한 repository에 push하고 pull request를 요청합니다.
6. PR Template를 작성할 때, 수정사항의 type에 따라 label를 추가합니다.
   - ![bug fix](https://img.shields.io/badge/-bug%20fix-B60205) : Bug 수정
   - ![enhancement](https://img.shields.io/badge/-enhancement-1D76DB) : 신규 기능
   - ![documentation](https://img.shields.io/badge/-documentation-0E8A16) : 문서 업데이트
   - ![chore](https://img.shields.io/badge/-chore-0E8A16) : Refactoring, 유지 보수
7. pull request가 검토되고 merge 될 때까지 기다리십시오.    
    
다음은 pull 요청이 수락 될 가능성을 높이기 위해 수행 할 수있는 몇 가지 작업입니다.    

- 가능한 한 집중적으로 변경하십시오. 서로 의존하지 않는 변경 사항이 여러 개있는 경우 별도의 풀 요청으로 제출하는 것이 좋습니다.
- [good commit message](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)를 작성합니다.

Work in Progress pull request는 초기에 피드백을 받거나 차단된 것이 있는 경우 환영합니다.

### Developer Certificate of Origin

이 프로젝트에 기여하려면 커밋 할 때마다 개발자 원본 인증서 (DCO)에 동의해야합니다.    

동의해야하는 내용과 작동 방식에 대한 전문은 [DCO](https://developercertificate.org/)를 참조하십시오.     

기여에 대한 DCO에 동의를 나타내려면 각 git commit 메시지에 다음 줄을 추가하면됩니다.

```
Signed-off-by: Your name <your-id@your-email-domain.com>
```

#### DCO Sign-Off Methods

git commit에 -s 또는 --signoff 플래그를 사용하여 sign-off를 커밋에 자동으로 추가 할 수 있습니다. 이름과 연락 가능한 이메일 주소를 사용해야합니다.

```
git commit -s -m "Write the commit message"
```

### Resources

- [How to Contribute to Open Source](https://opensource.guide/how-to-contribute/)
- [Using Pull Requests](https://help.github.com/articles/about-pull-requests/)
- [GitHub Help](https://help.github.com)

### FOSSLight Guide의 License
[FOSSLight Guide 페이지](https://fosslight.org/fosslight-guide)에 작성된 모든 콘텐츠는 [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0) License하에 이용하실 수 있습니다.
