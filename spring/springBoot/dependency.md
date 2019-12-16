## 정리 ##
- 실제 ```pom.xml```에 추가해야 하는 의존성은 굉장히 적음 (spring-boot-starter-web, spring-boot-starter-test 등)
- 해당 의존성은 따로 버전을 명시하지 않아도 됨
- 그 이유는 parent 프로젝트로 설정된  **spring-boot-starter-parent** 안에 모든게 정의되어있기 때문
- **spring-boot-starter-parent**는 parent로 **spring-boot-dependecies**를 참조함
- **spring-boot-dependecies**는 dependencyManagement로 필요한 의존성들의 버전을 관리함
- 때문에 개발자는 별도로 의존성들의 버전을 관리할 필요가 없어짐(할일이 줄어듬, 대신 관리해줘서 매우 편함)
- 만약 특정 의존성 파일의 버전을 다르게 설정하고 싶으면 ```pom.xml``` 파일에서 바꿀 수 있음(overriding)
- 이 모든건 parent로 **spring-boot-starter-parent**를 참조해야 가능
- 만약 독자적인 프로젝트 구조로 parent로 **spring-boot-starter-parent**를 참조하지 못할 경우 dependencyManagement로 직접 **spring-boot-dependencies**를 참조하는 방법도 있음.
- 하지만 이 경우 **spring-boot-starter-parent** 안에 정의된 별도의 properties, 기타 설정을 활용할 수 없으므로 비추
- 따라서 최상위 프로젝트의 parent로 **spring-boot-starter-parent**를 참조하고 이 프로젝트를 상속받는 구조로 관리하는 걸 추천

## 참조 ##
백기선의 스프링 부트 강좌
