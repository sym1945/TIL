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

## Reference
https://www.baeldung.com/java-8-streams</br>
https://futurecreator.github.io/2018/08/26/java-8-streams/
