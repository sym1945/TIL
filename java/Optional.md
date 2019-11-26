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

## 10. Conditional Return with  _filter()_
```java
@Test
public void whenOptionalFilterWorks_thenCorrect() {
    Integer year = 2016;
    Optional<Integer> yearOptional = Optional.of(year);
    boolean is2016 = yearOptional.filter(y -> y == 2016).isPresent();
    assertTrue(is2016);
    boolean is2017 = yearOptional.filter(y -> y == 2017).isPresent();
    assertFalse(is2017);
}
```
- Optional value가 **filter**의 조건을 만족하면 해당 값을 담은 Optional을 리턴하고, 조건을 만족하지 않으면 _null_ 을 담은 Optional을 리턴한다.
- **filter**를 기반으로 값을 _거부_ 하거나 _수락_ 할 수 있다.
- **filter**를 이용하면 null 체크 및 조건 체크를 좀 더 우아하게 할 수 있다.
```java
public class Modem {
    private Double price;
 
    public Modem(Double price) {
        this.price = price;
    }
    // standard getters and setters
}
```
위와 같은 Modem class가 있고 Range를 체크하는 method가 아래와 같이 있다면
```java
public boolean priceIsInRange1(Modem modem) {
    boolean isInRange = false;
 
    if (modem != null && modem.getPrice() != null
      && (modem.getPrice() >= 10
        && modem.getPrice() <= 15)) {
 
        isInRange = true;
    }
    return isInRange;
}
```
Optional을 사용하면 아래와 같이 심플하게 작성 가능하다
```java
public boolean priceIsInRange2(Modem modem2) {
     return Optional.ofNullable(modem2)
       .map(Modem::getPrice)
       .filter(p -> p >= 10)
       .filter(p -> p <= 15)
       .isPresent();
 }
```

## 11. Transforming Value with  _map()_
```java
@Test
public void givenOptional_whenMapWorksWithFilter_thenCorrect() {
    String password = " password ";
    Optional<String> passOpt = Optional.of(password);
    boolean correctPassword = passOpt.filter(
      pass -> pass.equals("password")).isPresent();
    assertFalse(correctPassword);
 
    correctPassword = passOpt
      .map(String::trim)
      .filter(pass -> pass.equals("password"))
      .isPresent();
    assertTrue(correctPassword);
}
```
- **map**은 Optional value를 변화시키고 이를 담은 Optional을 리턴한다.
- 위 예제와 같이 **filter**와 조합하여 사용하면 효과적이다.

## 12. Transforming Value with  _flatMap()_
- **map**과 마찬가지로 값을 변화시킨다.
- **map**과의 차이점은 **map**은 변환되는 _value_ 를 Optional로 래핑하여 리턴하는 반면, **flatMap**은 변환된 _value가 래핑된 Optional_ 이 필요하며 최종적으로 value를 Optional로 래핑하여 리턴한다.
- 요약하면 **map**은 value를 받아다가 Optional\<value\>를 리턴하고, **flatMap**은 Optional\<value\>를 받아다가 Optional\<value\>로 리턴한다.
```java
public class Person {
    private String name;
    private int age;
    private String password;
 
    public Optional<String> getName() {
        return Optional.ofNullable(name);
    }
 
    public Optional<Integer> getAge() {
        return Optional.ofNullable(age);
    }
 
    public Optional<String> getPassword() {
        return Optional.ofNullable(password);
    }
 
    // normal constructors and setters
}
```
```java
Person person = new Person("john", 26);
Optional<Person> personOptional = Optional.of(person);
```
- 위와 같이 Optional을 property로 갖는 **Person** 클래스가 있고 이 객체를 Optional로 래핑했다.
- Optional 래핑 중첩으로 인해 _name_ 을 검사하는 테스트 method를 만든다고 하면 **map**만 사용할 시 굉장히 복잡해진다.
```java
@Test
public void givenOptional_whenMapWorks_thenCorrect() {
    Person person = new Person("john", 26);
    Optional<Person> personOptional = Optional.of(person);
 
    Optional<Optional<String>> nameOptionalWrapper  
      = personOptional.map(Person::getName); 
      // Person::getName의 value는 Optional<String>으로 이를 Optional로 래핑하여 반환한다. (Optional 중첩)
    Optional<String> nameOptional  
      = nameOptionalWrapper.orElseThrow(IllegalArgumentException::new);
    String name1 = nameOptional.orElse(""); // 중첩된 Optional에서 실제 value만 갖고오기 무지 까다롭다.
    assertEquals("john", name1);
}
```
**flatMap**을 사용하면 아래와 같이 가능하다.
```java
@Test
public void givenOptional_whenFlatMapWorks_thenCorrect() {
	Person person = new Person("john", 26);
    Optional<Person> personOptional = Optional.of(person);
    
    String name = personOptional
      .flatMap(Person::getName) // Person::getName은 Optional<String>으로 이를 그대로 리턴함.
      .orElse("");
    assertEquals("john", name);
}
```

## 13. Misusage of Optionals
```java
public List<Person> search(List<Person> people, String name, Optional<Integer> age) {
    // Null checks for people and name
    people.stream()
      .filter(p -> p.getName().equals(name))
      .filter(p -> p.getAge() >= age.orElse(0))
      .collect(Collectors.toList());
}
```
위와같이 **Optional**을 매개변수로 사용하는 것은 **지양**해야 한다.
```java
someObject.search(people, "Peter", null);
```
위와 같이 호출할 경우 _NullPointerException_ 이 발생하며 이는 **Optional**의 본래 목적에 어긋나기 때문이다. (null에서 자유로워지고 싶었는데 Optional 자체를 null체크하라고?)

코드가 길어져도 아래처럼 평범한 **Integer** 매개변수로 받아서 한번만 null 체크를 하던가, method를 오버로드 해서 사용하는게 더 옳다고 한다.
```java
public List<Person> search(List<Person> people, String name, Integer age) {
    // Null checks for people and name
 
    age = age != null ? age : 0;
    people.stream()
      .filter(p -> p.getName().equals(name))
      .filter(p -> p.getAge() >= age)      
      .collect(Collectors.toList());
}
```
```java
public List<Person> search(List<Person> people, String name) {
    return doSearch(people, name, 0);
}
 
public List<Person> search(List<Person> people, String name, int age) {
    return doSearch(people, name, age);
}
 
private List<Person> doSearch(List<Person> people, String name, int age) {
    // Null checks for people and name
    return people.stream()
      .filter(p -> p.getName().equals(name))
      .filter(p -> p.getAge() >= age)
      .collect(Collectors.toList());
}
```
**Option**을 매개변수로 사용하는 건 [code inpector들도 권장하지 않는다고 한다.](https://rules.sonarsource.com/java/RSPEC-3553)

## Reference
[Guide To Java 8 Optional | Baeldung](https://www.baeldung.com/java-optional)<br/>
[자바8 Optional 1부: 빠져나올 수 없는 null 처리의 늪 | DaleSeo](https://www.daleseo.com/java8-optional-before/)
