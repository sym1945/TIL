# Data Classes
- 용도: 데이터는 보유하지만 아무것도 하지 않는 클래스
- 코틀린에서는 data class 제공
```kotlin
data class User(val name: String, val age: Int)
```
- 기본 생성자에서 선언된 속성을 통해, 아래의 기능들을 컴파일러가 자동으로 생성해 줌
  * equals()
  * hashCode()
  * copy()
  * toString() of the form "User(name=John, age=42)",
  * compoenetN() functions
- 명시적으로 정의해주는 경우에는, 컴파일러가 자동으로 생성해 주지 않음

## 의미 있는 Data 클래스의 조건
- 기본생성자에 1개 이상의 파라미터
- 기본생성자의 파라미터가 var, val로 선언
- Data 클래스는 abstract, open, sealed, inner가 안됨

## 1.1 이후에 바뀐 점
- Data 클래스 interface 구현 가능
- Sealed class 상속 가능

## 기본 값
- JVM에서 파라미터가 없는 생성자가 필요한 경우,
- 모든 프로퍼티에 기본 값을 설정해 주면 됨
```Kotlin
data class User(val name: String = "", val age: Int = 0)
```
```Kotlin
val exam_0 = User()
val exam_1 = User("Kotlin")
val exam_2 = User("Kotlin", 113)
val exam_3 = User(age=113)
val exam_4 = User(name="Kotlin", age=113)
```

## 복사
- 객체의 기존 값들을 유지하고, 일부 값만 고쳐서 새로운 객체를 만들고 싶은 경우
- Data 클래스에 컴파일러가 copy를 만들어 주기 때문에 바로 copy를 호출해서 사용하면 됨
```Kotlin
fun copy(name: String = this.name, age: Int = this.age) = User(name, age)
```
```Kotlin
val jack = User(name="Jack", age=1)
val oldJack = jack.copy(age=2)
```

## Destructuring Declarations
- data 클래스는 Destructuring Declarations을 사용 가능 함
- 컴파일러가 componentN 함수를 자동으로 만들어 주기 때문
```Kotlin
val jane = User("Jane", 35)
val (name, age) = jane
println("$name, $age years of age")
// prints "Jane, 35 years of age"
```

## Standard Data Classes
- 스텐다드 라이브러리에서 제공하는 data 클래스도 있음
  * Pair
  * Triple
- 물론 가독성을 위해서는 프로퍼티에 의미 있는 이름을 제공하는 클래스가 최고임
```Kotlin
val jane = User("Jane", 35)
println(jane) // User(name=Jane, age=35)

val pair = Pair("Jane", 35)
println(pair) // (Jane, 35)
```

## 중첩 클래스 (Nested classes)
- 클래스는 다른 클래스에 중첩될 수 있음
```Kotlin
class Outer {
  private val bar: Int = 1
  
  class Nested {
    fun foo() = 2
  }
}

val demo = Outer.Nested().foo() // == 2
```

## 내부 클래스 (Ineer class)
- 클래스에 inner를 표기하면 바깥쪽 클래스의 멤버에 접근 할 수 있음
```Kotlin
class Outer {
  private val bar: Int = 1
  
  inner class Inner {
    fun foo() = bar
  }
}

val demo = Outer().Inner().foo() // == 1
```

## 익명 내부 클래스 (Anonymous inner classes)
- 객체 표현식(object expression)을 이용해서 익명 내부 클래스의 인스털스를 생성할 수 있음
```Kotlin
mSearchEditText.setOnClickListener(object: View.OnClickListener {
  override fun onClick(v: View?) {  // ...
  }
})
```
- Functional java interface인 경우에는 접두어에 인터페이스 이름을 사용해서 람다식으로 표현할 수도 있음
```Kotlin
mSearchEditText.setOnClickListener(View.OnClickListener {
  v -> // ...
})
```


