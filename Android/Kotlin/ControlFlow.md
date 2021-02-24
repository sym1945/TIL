## if else 문
- Java와 거의 유사함
```
// Traditional usage
var max = a
if (a < b) max = b
```
```
var max: Int
if (a > b) {
  max = a
} else {
  max = b
}
```
- if문이 식으로 사용되는 경우 값은 반환함
- if식의 경우 반드시 else를 동반해야 함
```
val max = if (a > b) a else b
```
- if식의 branches들이 블록을 가질 수 있음 {...}
- 블록의 마지막 구문이 반환 값이 됨
```
val max = if (a > b) {
  print("Choose a")
  a
} else {
  print("Choose b")
  b
}
```
- 삼항연산자(ternary)가 없음
  * if문이 삼항연산자 역할을 잘 해내기 때문에
```
// Java
int max = (a > b) ? a : b;
```
```
// Kotlin
val max = if (a > b) a else b
```

## when
- when문은 C계열 언어의 switch문을 대체함
- when문은 각각의 branches의 조건문이 만족할 때까지 위에서부터 순차적으로 인자를 비교함
``` 
when (x) {
  1 -> print("x == 1")
  2 -> print("x == 2")
  else -> { // Note the block
    print("x is neither 1 nor 2")
  }
}
```
- when문이 식으로 사용된 경우에는 조건을 만족하는 branch의 값이 전체 식의 결과 값이 됨
- else의 경우 다른 branch들의 조건이 만족되지 않을 때 수행됨
- when이 식으로 사용된 경우 else문이 필수임
- when이 식으로 사용된 경우 컴파일러가 else문이 없어도 된다는 것을 입증할 수 있는 경우에는 else를 생략가능
```
var res = when (x) {
  100 -> "A"
  90 -> "B"
  80 -> "C"
  else -> "F"
}
```
```
var res = when (x) {
  true -> "맞다"
  false -> "틀리다"
}
```
- 여러 조건들이 같은 방식으로 처리될 수 있는 경우, branch의 조건문에 콤마를 (,) 사용하여 표기하면 됨
```
when (x) {
  0, 1 -> print("x == 0 or x == 1")
  else -> print("otherwise")
}
```
- Branch의 조건문에서 함수나 식을 사용할 수 있음
```
when (x) {
  parseInt(x) -> print("s encodes x")
  1 + 3 -> print("4")
  else -> print("s does not encode x")
}
```
- range나 collection에 in이나 !in으로 범위 등을 검사할 수 있음
```
val validNubmers = listOf(3, 6, 9)
when (x) {
  in validNubmers -> print("x is valid")
  in 1..10 -> print("x is the range")
  !in 10..20 -> print("x is outside the range")
  else -> print("none of the above")
}
```
- is나 !is를 이용하여 타입도 검사할 수 있음
  * 이 때 스마트 캐스트가 적용됨
```
fun hasPrefix(x: Any) = when(x) {
  is String -> x.startsWith("prefix")
  else -> false
}
```
- when은 if-else if 체인을 대체할 수 있음
- when에 인자를 입력하지 않으면, 논리연산으로 처리됨
```
when {
  x.isOdd() -> print("x is odd")
  x.isEven() -> print("x is even")
  else -> print("x is funny")
}
```

## for loops
- for문은 iterator를 제공하는 모든 것을 반복할 수 있음
```
for (item in collection)
  print(item)
```
- for문의 Body가 블록이 올 수도 있음
```
for (item in collection) {
  print(item.id)
  print(item.name)
}
```
- for문을 지원하는 iterator의 조건
  * 멤버함수나 확장함수 중에
    - iterator()를 반환하는 것이 있는 경우
      >next()를 가지는 경우
      
      >hasNext(): Boolean을 가지는 경우
  * 위의 3함수는 operator로 표기되어야함
```
val myData = MyData()
for (item in myData) {
  print(item)
}
```
```
class MyData {
  operator fun iterator(): MyIterator {
    return MyIterator()
  }
}
```
```
class MyIterator {
  val data = listOf(1, 2, 3, 4, 5)
  var idx = 0
  operator fun hasNext(): Boolean {
    return data.size > idx
  }
  
  operator fun next(): Int {
    return data[idx++]
  }
}
```
- 배열이나 리스트를 반복할 때, index를 이용하고 싶다면 indices를 이용하면 됨
```
val array = arrayOf("가", "나", "다")
for (i in array.indicies) {
  println("$i: ${array[i]}")
}
```
- Index를 이용하고 싶을 때, withIndex()를 이용할 수도 있음
```
val array = arrayOf("가", "나", "다")
for ((index, value) in array.withIndex()) {
  println("$index: ${value}")
}
```

## while loops
- while, do-while문은 java와 거의 같음
- do-while문에서 body의 지역변수를 do-while문의 조건문이 참조 할 수 있음
```
while (x > 0) {
  x--
}
```
```
do {
  val y = retrieveData()
} while (y != null) // y is visible here!
```
