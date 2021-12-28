## IIS 원격 웹 배포 체크리스트 ##
- ```IIS 관리 서비스``` 기능 활성화 (IIS Management Service)
  * ```원격 연결 사용``` 활성화
  * ```Windows 자격 증명 또는 IIS 관리자 자격 증명``` 설정
- ```Web Deploy``` 설치 (VS2017 기준 Web Deploy 3.6 버전에서 작동 확인)
- ```IIS 관리자 사용자``` 등록
- IIS 사이트 개설
- 개설한 사이트에 ```IIS 관리자 권한``` 등록


## IIS 에서 ASP.NET Core App 구동 체크리스트 ##
- ```ASP.NET core Runtime - Windows Hosting Bundle``` 설치
- IIS 사이트 개설


## Reference ##
https://sorceryforce.net/ko/tips/asp-net-core-web-deploy
