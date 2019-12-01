## 1. Overview
- Java 8 기준 Stream 정리

## 2. Stream Creation
- Stream 생성 시 Source(원본)는 수정하지 않는다.

### 2.1 Empty Stream
```java
Stream<String> streamEmpty = Stream.empty();
```
- 요소가 없는 Stream에 대해 null을 반환하지 않도록 **empty()** 메서드를 사용하는 경우가 종종 있다.
```java
public Stream<String> streamOf(List<String> list) {
    return list == null || list.isEmpty() ? Stream.empty() : list.stream();
}
```

### 2.2 Stream of Collection
```java
Collection<String> collection = Arrays.asList("a", "b", "c");
Stream<String> streamOfCollection = collection.stream();
```

### 2.3 Stream of Array
```java
Stream<String> streamOfArray = Stream.of("a", "b", "c");
```
- 배열 전체 또는 일부를 Stream으로 만들 수 있다.
```java
String[] arr = new String[]{"a", "b", "c"};
Stream<String> streamOfArrayFull = Arrays.stream(arr);
Stream<String> streamOfArrayPart = Arrays.stream(arr, 1, 3);
```

### 2.4 Stream.builder()
```java
Stream<String> streamBuilder = 
	Stream.<String>builder().add("a").add("b").add("c").build();
```
- builder로 만들 시 type 지정해야 함. (지정 안하면 object Stream 생성)

### 2.5 Stream.generate()
```java
Stream<String> streamGenerated =
	Stream.generate(() -> "element").limit(10);
```
- **Supplier\<T\>** 의 요소를 생성한다.
- limit을 지정해줘야한다.
- limit을 지정해주지 않으면 메모리 제한에 도달할 때까지 **generate()** 메서드가 작동한다.

### 2.6 Stream.iterate()
```java
Stream<Integer> streamIterated = 
	Stream.iterate(40, n -> n + 2).limit(20);
```
- 무한 Stream을 만드는 또 다른 방법이다.
- 첫번째 매개변수로 초기값을 설정하고, 두번째 매개변수로 지정된 기능이 반복되어 적용된다.
- 위 예제는 40부터 2씩 커지는 요소를 20개 갖는 Stream을 만든다.

### 2.7 Stream of Primitives
- ```int```, ```long```, ```double``` primitive type에 대해선 전용 Stream을 제공한다.
- **IntStream**, **LongStream**, **DoubleStream**
```java
IntStream intStream = IntStream.range(1, 3);	// [1, 2]
LongStream longStream = LongStream.rangeClosed(1, 3);	// [1, 2, 3]
```
- **range(int startInclusive, int endExclusive)** 는 startInclusive부터 1씩 증가하여 endExclusive을 포함하지 않는 요소를 가진 stream을 생성한다.
- **rangeClosed(int startInclusive, int endExclusive)** startInclusive부터 1씩 증가하여 endExclusive을 포함하는 요소를 가진 stream을 생성한다.
- Java 8 이후 **Random** 클래스는 primitive Stream 생성을 위해 광범위한 메서드를 제공한다.
```java
Random random = new Random();
DoubleStream doubleStream = random.doubles(3);
```

### 2.8 Stream of String
- ```String```을 Stream을 만들기 위한 Source로 사용할 수 있다.
- ```String```의 **chars()** 메서드로 각 char(문자)를 IntStream으로 변환할 수 있다.
```java
IntStream streamOfChars = "abc".chars();
```
- ```String```을 RegEx로 분할하여 String stream으로 변환할 수 있다.
```java
Stream<String> streamOfString =
	Pattern.compile(", ").splitAsStream("a, b, c");
```

### 2.9 Stream of File
```java
Path path = Paths.get("C:\\file.txt");
Stream<String> streamOfStrings = Files.lines(path);
Stream<String> streamWithCharset = 
	Files.lines(path, Charset.forName("UTF-8"));
```
- Java NIO **Files** 클래스의 **line()** 메서드는 텍스트 파일의 String stream을 생성한다.
- 텍스트의 각 line은 stream의 각 요소가 된다.

## 3. Referencing a Stream
```java
Stream<String> stream = 
	Stream.of("a", "b", "c").filter(element -> element.contains("b"));
	
Optional<String> anyElement = stream.findAny();
```
- Stream 생성 후 터미널 작업을 통해 엑세스 가능한 참조를 가질 수 있음.
- 이후 같은 Stream을 액세스하면 **IllegalStateException** 발생함.
- **즉, Stream은 재사용할 수 없음.**
- 위 코드 실행 후 아래 코드 실행하면 **IllegalStateException** 발생함
```java
Optional<String> firstElement = stream.findFirst();
```

## 4. Stream Pipeline
- Stream을 통해 일련의 작업을 수행하고 결과를 집계하려면 **Source**, **intermediate operation**, **terminal operation** 이 필요함.
- **intermediate operation**(중간 작업)은 새로 수정된 Stream을 반환함.
- 아래는 기존 Stream에서 첫번째 요소를 제외한 Stream을 반환함.
```java
Stream<String> onceModifiedStream =
	Stream.of("abcd", "bbcd", "cbcd").skip(1);
```
- 둘 이상의 **intermediate operation**이 필요할 경우 이를 연결할 수 있음.
```java
Stream<String> twiceModifiedStream =
	stream.skip(1).map(element -> element.substring(0, 3));
```
- 마지막으로 결과를 집계하기 위해 **terminal operation**(터미널 작업)을 수행함.
- 아래는 중간 작업 결과에 대한 요소의 개수를 반환하는 예제.
```java
List<String> list = Arrays.asList("abc1", "abc2", "abc3");
long size = list.stream().skip(1)
	.map(element -> element.substring(0, 3)).sorted().count();
```

## 5. Lazy Invocation
- **intermediate operation**은 게을러서 **terminal operation**이 실행될 때 호출된다.
```java
private long counter;
  
private void wasCalled() {
    counter++;
}
```
```java
List<String> list = Arrays.asList(“abc1”, “abc2”, “abc3”);
counter = 0;
Stream<String> stream = list.stream().filter(element -> {
    wasCalled();
    return element.contains("2");
});
```
- 위 코드를 실행시키면 counter가 하나도 증가하지 않는다.
- 이유는 **terminal operation**을 실행하지 않았기 때문.
```java
Optional<String> stream = list.stream().filter(element -> {
    log.info("filter() was called");
    return element.contains("2");
}).map(element -> {
    log.info("map() was called");
    return element.toUpperCase();
}).findFirst();
```
- 위 코드를 실행시키면 ```filter()```는 2번 호출되고, ```map()```은 1번 호출된다.
- ```findFirst()```라는 **terminal operation**에 의해 ```filter()```는 조건을 만족하는 2번째 요소를 찾고 이를 새 Stream으로 반환, ```map()```은 ```filter()```가 반환한 Stream으로 작업하기 때문에 1번만 호출되는 것.

## 6. Order of Execution
- **intermediate operation**의 체인작업의 순서는 퍼포먼스 측면에서 매우 중요하다.
```java
long size = list.stream().map(element -> {
    wasCalled();
    return element.substring(0, 3);
}).skip(2).count();
```
- 위 코드를 실행시키면 ```wasCalled()```가 3번 호출된다.
- 그 후 ```skip(2)```에 의해 최종적으로 반환되는 값은 1개이다.
- 1개를 찾기 위해 ```map()```을 3번 호출하는 것은 굉장히 비효율적이다.

```java
long size = list.stream().skip(2).map(element -> {
    wasCalled();
    return element.substring(0, 3);
}).count();
```
- 따라서 위와같이 먼저 ```skip()```하여 Stream의 크기를 줄이고 ```map()```작업을 이어나가는 것이 효율적이다.
- 즉, Stream의 크기를 줄이는 ```skip()```, ```filter()```, ```distinct()``` 같은 작업을 체인의 가장 앞 부분에 배치하는게 좋다.

## 7. Stream Reduction
### 7.1 The reduce() Method
### 7.2 The collect() Method
- [**Collector.md** 정리 참조](https://github.com/sym1945/TIL/blob/master/java/Collectors.md)
## 8. Parallel Streams



## Reference
https://www.baeldung.com/java-8-streams</br>
https://futurecreator.github.io/2018/08/26/java-8-streams/
