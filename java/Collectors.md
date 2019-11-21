## **1. Overview**
Java 8 기준 Collectors 정리

## **2. The  _Stream.collect()_  Method**
- Stream 요소를 수집하는 최종 연산
- Stream.collect()가 Stream의 요소를 수집하기 위한 수집 방법이 정의된 것이 **Collectors**.
- **Collectors**는 _Collector_ interface를 구현한 것. 

## **3.  _Collectors_**
예제에 도움을 줄 테스트용 리스트
```java
List<String> givenList = Arrays.asList("a", "bb", "ccc", "dd");
```

### **3.1.  _Collectors.toList()_**
```java
List<String> result = givenList.stream()
	.collect(Collectors.toList());
```

### **3.2.  _Collectors.toSet()_**
```java
Set<String> result = givenList.stream()
	.collect(Collectors.toSet());
```
- Set은 중복되는 요소를 가질 수 없다. 
- 원본 collection이 중복된 요소를 가지고 있을 경우, toSet으로 collect시 중복 요소는 하나만 포함 한다.

### **3.3.  _Collectors.toCollection()_**
```java
List<String> result = givenList.stream()
	.collect(Collectors.toCollection(LinkedList::new));
```
- List와 Set 외 원하는 타입의 컬렉션으로 지정할 때 사용한다.
- immutable collection은 지정할 수 없으며, 이 경우엔 커스텀 Collector를 구현하거나 **collectionAndThen**을 사용하면 된다.

### **3.4.  _Collectors_._toMap()_**
```java
Map<String, Integer> result = givenList.stream()
	.collect(Collectors.toMap(
			Function.identity(), 
			String::length
		)
	);
```
- **toMap**은  _keyMapper_ 와  _valueMapper_ 를 지정해줘야 한다.<br/> >> **toMap**(_keyMapper_, _valueMapper_)
- **Function**._identity()_ 는 요소를 그대로 가져오는 함수다. 
- **toSet**과 다르게 중복 요소가 있을 경우 **IllegalStateException**을 발생시킨다. (key가 중복되면 안되기 때문)
- 따라서 세번째 인자로 **BinaryOperator**를 지정해 중복되는 key의  _현재 value_ 를 사용할 것인지 _새로운 value_ 를 사용할 것인지 선택할 수 있다.
```java
Map<String, Integer> result = givenList.stream()
	.collect(Collectors.toMap(
			Function.identity(),
			String::length, 
			(existValue, newValue) -> existValue
		)
	);
```

### **3.5.  _Collectors.collectingAndThen()_**
```java
List<String> result = givenList.stream()
	.collect(Collectors.collectingAndThen(
			Collectors.toList(), 
			Collections::unmodifiableList
		)
	);
```
- 컬렉팅 후 두번째 인자로 지정한 컬렉션 타입으로 반환한다.

### **3.6.  _Collectors.joining()_**
```java
String result = givenList.stream()
	.collect(Collectors.joining());
```
```java
result="abbcccdd"
```
```java
String result = givenList.stream()
	.collect(Collectors.joining(" "));
```
```java
result="a bb ccc dd"
```
```java
String result = givenList.stream()
	.collect(Collectors.joining(" ", "PRE-", "-POST"));
```
```java
result="PRE-a bb ccc dd-POST"
```

### **3.7. _Collectors.counting()_**
```java
Long result = givenList.stream()
	.collect(Collectors.counting());
```
- Stream 요소들의 개수를 반환한다.

### **3.8. _Collectors.summarizingDouble/Long/Int()_**
```java
DoubleSummaryStatistics result = givenList.stream()
	.collect(Collectors.summarizingDouble(String::length));
```
- 추출 된 요소의 Stream에서 숫자 데이터에 대한 통계 정보를 포함하는 특수 클래스를 리턴한다.
```java
assertThat(result.getAverage()).isEqualTo(2);
assertThat(result.getCount()).isEqualTo(4);
assertThat(result.getMax()).isEqualTo(3);
assertThat(result.getMin()).isEqualTo(1);
assertThat(result.getSum()).isEqualTo(8);
```

### **3.9. _Collectors.averagingDouble/Long/Int()_**
```java
Double result = givenList.stream()
	.collect(Collectors.averagingDouble(String::length));
```
- 추출 된 요소의 평균을 반환한다.

### **3.10. _Collectors.summingDouble/Long/Int()_**
```java
Double result = givenList.stream()
	.collect(Collectors.summingDouble(String::length));
```
- 추출 된 요소의 합을 반환한다.

### **3.11 _Collectors.maxBy()/minBy()_**
```java
Optional<String> result = givenList.stream()
	.collect(Collectors.maxBy(Comparator.naturalOrder()));
```
- 제공된 **Comparator** 인스턴스에 따라 Stream 요소 중 가장 큰/작은 값을 반환한다. 
- 반환값은 **Optional** 인스턴스이다.

### **3.12.  _Collectors.groupingBy()_**
```java
Map<Integer, Set<String>> result = givenList.stream()
	.collect(Collectors.groupingBy(String::length, Collectors.toSet()));
```
- **groupingBy** 메서드의 첫번째 인자를 키로 콜렉션을 Grouping하고 Map객체로 저장한다.
- **groupingBy** 메서드의 두번째 인자의 _Downstream Collector_ 로 원하는 결과로 Grouping 할 수 있다. (toSet, toList, maxBy, counting, summingInt 등...)
```java
assertThat(result)
	.containsEntry(1, newHashSet("a"))
	.containsEntry(2, newHashSet("bb", "dd"))
	.containsEntry(3, newHashSet("ccc"));
```

### **3.13.  _Collectors.partitioningBy()_**
```java
Map<Boolean, List<String>> result = givenList.stream()
	.collect(Collectors.partitioningBy(s -> s.length() > 2));
```
```java
{false=["a", "bb", "dd"], true=["ccc"]}
```
- Grouping 방식 중 하나로 _Boolean_ 타입을 키로 하여 두 개의 그룹으로 분류할 때 사용한다.



## **Reference**
[Guide to Java 8 Collectors | Baeldung](https://www.baeldung.com/java-8-collectors)<br/>
[[JAVA] 스트림 API](http://iloveulhj.github.io/posts/java/java-stream-api.html)<br/>
[자바의 정석 - 스트림(Stream) | Integerous DevLog](https://ryan-han.com/post/java/java-stream/)<br/>
