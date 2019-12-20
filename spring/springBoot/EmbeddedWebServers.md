## 정리 ##
- 스프링 부트의 기본 내장 웹서버는 톰캣			
- 내장된 톰캣이 서버로 실행될 뿐이지 스프링 부트 자체가 서버는 아님			
- ```pom.xml``` 설정에서 얼마든지 내장 웹 서버를 변경할 수 있음 (jetty, undertow 등)			
- 스프링 부트 자동설정 기능을 통해 내장 웹 서버 객체 생성, 포트 설정, 컨텍스트 추가, 서블릿 만들기, 서블릿 만들기, 서버 실행 및 대기의 일련의 과정이 자동으로 실행됨			
- 핵심과정
  - ```ServletWebServerFactoryAutoConfiguration``` : 서블릿 웹 서버 생성		
    - ```TomcatServletWebServerFactoryCustomizer``` : 서버 커스터마이징	
  - ```DispatcherServletAutoConfiguration```		
    - 서블릿 만들고 등록	
- ```application.properties``` 에 설정 추가로 웹 어플리케이션 타입 및 포트 등 수정 가능			
  - ```spring.main.web-application-type=none``` : 웹 서버 사용 안함		
	- ```server.port=7070``` : 포트 7070으로 변경		
	- ```server.port=0``` : 포트번호 랜덤 변경		
- ```ApplicationListner<ServletWebServerInitializedEvent>``` 를 구현하여 서버 실행 직후 수행할 작업을 구현할 수 있음			
- HTTPS를 사용하기 위해서 아래 작업이 필요함			
  - keystore 만들기		
  - ```application.properties``` 에 keystore 등록		
    - ```server.ssl.key-store=keysotre.p12```
    - ```server.ssl.key-store-password=123456```
    - ```server.ssl.keyStoreType=PKCS12```
    - ```server.ssl.keyAlias=spring```
- Spring Boot의 커넥터는 기본적으로 하나로 HTTPS로 설정하면 HTTP는 사용 불가능			
- 코드 추가하여 HTTP 커넥터를 추가할 수 있음			
  - https://github.com/spring-projects/spring-boot/tree/v2.0.3.RELEASE/spring-boot-samples/spring-boot-sample-tomcat-multi-connectors		
- HTTP2를 사용하기 위해서 아래 작업이 필요함			
  - HTTPS 설정 (SSL 인증서 필요)		
  - ```application.properties``` 에 설정 추가		
    - ```server.http2.enabled=true```
  - 위 설정은 서블릿 컨테이너마다 다를 수 있음		
  - 톰캣같은 경우 JDK 9, Tomcat 9 이상에서 설정 가능		

## 참조 ##
백기선의 스프링 부트
