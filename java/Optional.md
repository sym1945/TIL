## 1. Overview
- **Optional** 클래스는 _null_ 이 될 수 있는 객체를 감싸고 있는 일종의 래퍼 클래스다.
- 명시적으로 해당 변수가 _null_ 일 수도 있다는 가능성을 표현하여 NPE를 유발할 수 있는 _null_ 을 직접 다루지 않아도 된다.
- 이하 Java8 기준 **Optional** 정리.

## 2. Creating _Optional_ Objects
```java
Optional<String> empty = Optional.empty();
```
- static method **empty()**는 value가 _null_ 인 Optional 객체를 생성함.

```java
String name = "baeldung";
Optional<String> opt = Optional.of(name);
```
- static method **of(Object)**는 Object를 value로 갖는 Optional 객체를 생성함.
- Object가 _null_ 일 경우 **NullPointerException** 발생함.

```java
String name = "baeldung";
Optional<String> opt = Optional.ofNullable(name);
```
- static method **ofNullable(Object)**는 of(Object)와 마찬가지로 Object를 value로 갖는 Optional 객체를 생성함.
- 대신 Object가 _null_ 이면 이를 value로 갖는 Optional 객체를 생성함.

## 3. Checking Value Presence: _isPresent()_ 
```java
Optional<String> opt = Optional.of("Baeldung");
assertTrue(opt.isPresent());
```
- **isPresent()** method 는 Optional의 value가 _null이 아니면 true_ , _null이면 false_ 를 반환함.

## 4. Conditional Action With  _ifPresent()_
```java
public void printName(String value) {
	Optional<String> opt = Optional.ofNullable(value);
	opt.ifPresent(name -> System.out.println(name.length()));
}
```
- Optional의 값이 _null_ 이 아닐 경우 **ifPresent()** method 안의 logic을 실행한다.
- 아래 null 체크 방식의 방어 코드라 할 수 있다.
```java
public void printName(String value) {
	if (value != null) {
		System.out.println(value.length());
	}
}
```

## 5. Default Value With  _orElse()_
```java
String name = Optional.ofNullable(nullName).orElse("john");
```
- **ofNullable(Object)** method 안의 Object가 null인 경우 **orElse(value)** 안의 value를 리턴한다.
- 아래의 null 체크 방식의 방어 코드라 할 수 있다.
```java
String name = null;
if (nullName == null) {
	name = "john";
} else {
	name = nullName;
}
```

## 6. Default Value With  _orElseGet()_
```java
String name = Optional.ofNullable(nullName).orElseGet(() ->"john");
```
- **orElse**는 null일 경우 반환해야하는 default value(T)를 직접 지정해줘야 하는 반면, **orElseGet**은 null일 경우 value(T)를 반환하는 function을 지정해줘야 한다.

## 7. Difference Between  _orElse_  and  _orElseGet()_
```java
public String getMyDefault() {
	System.out.println("Getting Default Value");
	return "Default Value";
}
```
```java
@Test
public void whenOrElseGetAndOrElseDiffer_thenCorrect() {
    String text = "Text present";
 
    System.out.println("Using orElseGet:");
    String defaultText 
      = Optional.ofNullable(text).orElseGet(this::getMyDefault);
    assertEquals("Text present", defaultText);
 
    System.out.println("Using orElse:");
    defaultText = Optional.ofNullable(text).orElse(getMyDefault());
    assertEquals("Text present", defaultText);
}
```
```
Using orElseGet:
Using orElse:
Getting default value...
```
- **orElse**안에서 value를 반환하는 method를 직접 호출하는 경우 Optional value가 null이 아니어도 해당 method는 호출된다.
- orElse 안에 DB에 접근하는 query나 web service를 호출하는 등의 무거운 동작을 하는 method를 넣으면 당연히 안되겠다.

## 8. Exceptions with  _orElseThrow()_
```java
String name = Optional.ofNullable(nullName)
	.orElseThrow(IllegalArgumentException::new);
```
- Optional value가 _null_ 이면 **orElseThrow** 안의 Exception을 던진다.

## 9. **Returning Value with  _get()_**
```java
Optional<String> opt = Optional.of("baeldung");
String name = opt.get();
```
- **get()** Optional 의 value를 반환한다.
- Optional의 value가 null일때 **get()** 을 호출하면 **NoSuchElementException**이 발생한다.



## Reference
[Guide To Java 8 Optional | Baeldung](https://www.baeldung.com/java-optional)<br/>
[자바8 Optional 1부: 빠져나올 수 없는 null 처리의 늪 | DaleSeo](https://www.daleseo.com/java8-optional-before/)
