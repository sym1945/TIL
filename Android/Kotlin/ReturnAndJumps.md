# Return and jumps
## 3가지 Jump 표현식
- return: 함수나 익명 함수에서 반환
- break: 루프를 종료 시킴
- continue: 루프의 다음 단계로 진행
```kotlin
fun sum(a: Int, b: Int): Int {
  println("a: $a, b: $b")
  return a + b
}
```
```kotlin
for (x in 1..10) {
  if (x > 2) {
    break
  }
  println("x: $x")
}
```
```kotlin
for (x in 1..10) {
  if (x < 2) {
    continue
  }
  println("x: $x")
}
```
## Label로 Brean and Continue
- 레이블 표현: label@, abc@, fooBar@
  * 식별자 + @ 형태로 사용
```kotlin
labelDefinition
(used by prefixUaryOpertion, annotatedLambda)
: LabelName ++ "@"
;
```
```kotlin
loop@ for (i in 1..10) {
  println("--- i: $i ---")
  
  for (j in 1..10) {
    println("j: $j")
    if (i + j > 12) {
      break@loop
    }
  }
}
```
```kotlin
loop@ for (i in 1..10) {
  println("--- i: $i ---")
  
  for (j in 1..10) {
    if (j < 2) {
      continue@loop
    }
    println("j: $j")
  }
}
```

## Label로 Return
- 코틀린에서 중첩 될 수 있는 요소들
  * 함수 리터럴 (funtions literals)
  * 지역함수 (local function)
  * 객체 표현식 (object expression)
  * 함수 (functions)
```kotlin
fun foo() {
  var ints = listOf(0, 1, 2, 3)
  ints.forEach(fun(value: Int) {
    if (value == 1) return
    print(value)
  })
  print("End")
}
```
 
## 람다식에서 return 할 때 주의사항
- 람다식에서 return시 nearest enclosing 함수가 return됨
- 람다식에 대해서만 return 하려면 label을 이용해야 함
```kotlin
fun foo2() {
  var ints = listOf(0, 1, 2, 3)
  ints.forEach {
    if (it == 1) return
    print(it)
  }
  print("End")
}
```
```kotlin
fun foo3() {
  var ints = listOf(0, 1, 2, 3)
  ints.forEach label@ {
    if (it == 1) return@label
    print(it)
  }
  print("End")
}
```

## 암시적 레이블
- 람다식에서만 return 하는 경우 label을 이용해서 return 해야 함
- 직접 label을 사용하는 것 보다 암시적 레이블이 편리함
- 암시적 레이블은 람다가 사용된 함수의 이름과 동일함
```kotlin
fun foo4() {
  var ints = listOf(0, 1, 2, 3)
  ints.forEach {
    if (it == 1) return@forEach
    print(it)
  }
  print("End")
}
```

## 레이블 return시 값을 반환할 경우
- return@label 1 형태로 사용
- return + @label + 값
```kotlin
fun foo(): List<String> {
  var ints = listOf(0, 1, 2, 3)
  var result = ints.map {
    if (it == 0) {
      return@map "zero" // return at named label
    }
    "number $it"  // expression returned from lambda
  }
  return result
}
```

