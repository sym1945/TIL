## Class

### 1. ReloadableResourceBundleMessageSource 

> **Method**

- **public void setBasename(String basename)**
  * message 파일 경로 및 기본 이름 설정.<br/><br/>
`baseName`:  "path/basename" 으로 설정한 경우 path 폴더 내 디폴트 파일인 basename.properties 와 basename_ko.properties, basename_en.properties 와 같이 로케일이 붙은 모든 파일을 읽는다. xml 속성 파일도 지원된다.
<br/><br/>
- **public void setDefaultEncoding(@Nullable String defaultEncoding)**
  * properties 파일을 분석하는데 사용할 encoding 설정. 
  * 기본값은 `java.util.Properties`의 기본 encoding(ISO-8859-1). xml 파일은 해당 없음.
<br/><br/>
- **public void setFallbackToSystemLocale(boolean fallbackToSystemLocale)**
  * 특정 locale 파일이 없을경우 시스템 locale로 fallback 여부 설정. 
  * 기본값은 `true`.
  * `false`로 설정할 경우 "message.properties"와 같은 (locale이 지정되지 않은)디폴트 파일로 대체한다.
<br/><br/>
- **public void setUseCodeAsDefaultMessage(boolean useCodeAsDefaultMessage)**
  * 메세지 코드가 존재하지 않을 경우`NoSuchMessageException`를 발생시키는 대신 메세지 코드 자체를 출력할지 여부 설정. 
  * 기본값은 `false`.
<br/><br/>
- **public void setCacheMillis(long cacheMillis)**
  * 로드 된 properties 파일을 캐시할 시간(msec)을 설정. 
  * 기본값은 `-1`로 캐시하지 않음.
  * `양수`면 설정된 msec 간격으로 properties 파일을 reload함. reload 시도 전 파일 수정 시간부터 확인하므로 파일이 변경되지 않으면 reload하지 않음.
  * `0`으로 설정하면 모든 메세지 엑세스에서 파일을 reload함. **프로덕션 환경에선 사용하지 말 것**

## Local Resolver
다국어 처리 방법
1. ### AceeptHeaderLocaleResolver
- 기본 resolver로 `SessionLocaleResolver`와 `CookieLocaleResolver`를 별도로 구현하지 않으면 `AceeptHeaderLocaleResolver`로 동작한다.
- 브라우저에 설정된 locale 정보로 처리한다.
2. ### SessionLocaleResolver
- request session에 저장된 locale 정보로 처리한다.
- session에 locale 정보가 없다면 브라우저 locale에 따르고, 따로 `defaultLocale`을 설정했다면  해당 locale을 따른다.
3. ### CookieLocaleResolver
- cookie에 저장된 locale 정보로 처리한다.
- cookie에 locale 정보가 없다면 브라우저 locale에 따르고, 따로 `defaultLocale`을 설정했다면  해당 locale을 따른다.

## Local Change Interceptor
- request 파라미터에 `locale`에 매개변수가 존재할 경우 해당 값으로 현재 locale을 변경해주는 Interceptor.
- 파라미터 이름 기본값은 `locale`.
- **setParamName** 메서드를 통해 파라미터 이름 변경할 수 있음.

## Config
**ReloadableResourceBundleMessageSource**
``` java
@Configuration
public class LocaleConfig {
	@Bean
	public ReloadableResourceBundleMessageSource messageSource() {
		ReloadableResourceBundleMessageSource source = new ReloadableResourceBundleMessageSource();
		source.setBasename("messages/message");
		source.setDefaultEncoding("UTF-8");
		source.setFallbackToSystemLocale(false);
		source.setUseCodeAsDefaultMessage(true);

		return source;
	}
}
```

<br/>**SessionLocaleResolver**
```java
@Bean
public SessionLocaleResolver localeResolver() {
	SessionLocaleResolver resolver = new SessionLocaleResolver();
	resolver.setDefaultLocale(Locale.KOREA); // defaulLocale 설정
	return resolver;
}
```

<br/>**LocaleChangeInterceptor**
```java
@Bean
public LocaleChangeInterceptor localeChangeInterceptor() {
	LocaleChangeInterceptor interceptor = new LocaleChangeInterceptor();
	interceptor.setParamName("lang");	// url/?lang=ko처럼 lang으로 들어오는 파라미터 감지
	return interceptor;
}
```

## Use
- properties
``` java
site.count=사이트 개수는{0}개
```
- in code
``` java
@Autowired
private MessageSource messageSource;
  
@GetMapping(path = "/site")
public String siteCount(Locale locale, HttpServletRequest request, Model model) {
	Integer[] arg = new Integer[] { 1 };
	messageSource.getMessage("site.count", arg, locale));
    return "site";
}
```
- in jsp
``` java
<%@ taglib prefix="spring" uri="http://www.springframework.org/tags" %>

<spring:message code="site.count" arguments="${siteCount}" text="default text" />
```

## Reference
- [ReloadableResourceBundleMessageSource (Spring Framework 5.2.1.RELEASE API)](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/support/ReloadableResourceBundleMessageSource.html)
- [Java Config로 메세지소스(MessageSource) 설정하기(다국어 지원)](https://offbyone.tistory.com/382)
- [Spring에서 다국어 적용하기](https://yookeun.github.io/java/2015/08/12/spring-i18n/)
