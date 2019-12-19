## 자동 설정 ##
- ```@EnableAutoConfiguration```은 spring.factories(org.springframework.boot.autoconfigure.EnableAutoConfiguration)내의 설정 파일을 bean으로 등록함
- 일부 조건에 따라 bean으로 등록하지 않는 것도 존재함. (@ContionalOnXXX)
- Bean 등록 순서는 ```@ComponentScan``` -> ```@EnableAutoConfigration``` 순서

## 출처 ##
백기선의 스프링 
