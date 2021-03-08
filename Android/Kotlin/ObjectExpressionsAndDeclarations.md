# Object Expressions and Declarations
## Object 용도
- 어떤 class에서 조금 변경된 객체를 생성할 때
- 새로운 subclass의 명시적인 선언 없이 객체 생성

## Object Expressions
- Java 익명 객체

## Object Declarations
- 싱글턴

## Companion Object
- 싱글턴 + class method (static)

## 객체 표현식
- Java에서는 익명 내부 클래스를 사용해서 처리했음
- Kotlin에서는 object expressions를 이용
```Kotlin
btn.setOnClickListener(new OnClickListener() {
  public void onClick(View v) {
  }
});
```

# Object expressions
## 객체 표현식 문법
- 어떤 클래스를 상속 받은 익명 객체를 생성
```kotlin
window.addMouseListener(object : MouseAdapter() {
  override fun mouseClicked(e: MouseEvent) {}
  override fun mouseEntered(e: MouseEvent) {}
})
```

## 객체 표현식 상속
- 슈퍼타입의 생성자가 있는 경우, 반드시 값을 전달 해 주어야 함
- 슈터타입이 여러 개인 경우 ':'콜론 뒤에 ','콤마로 구분해서 명시해주면 됨
```kotlin
open class A(x: Int) {
  public open val y: Int = x
}

interface B {...}

val ab: A = object: A(1), B {
  override val y = 15
}
```

## 객체 표현식 상속 없는 경우
- 특별히 상속 받을 supertypes가 없는 경우, 간단하게 작성 가능
```kotlin
val adHoc = object {
  var x: Int = 0
  var y: Int = 0
}
print(adHoc.x + adHoc.y)
```

## 객체 표현식 제약 사항
- 익명 객체가 loacal 이나 private으로 선언될 때만 type으로 사용될 수 있음
- 익명 객체가 public function이나 public property 에서 리턴되는 경우, 익명 객체의 슈퍼타입으로 동작됨. 이런 경우 익명 객체에 추가된 멤버에 접근이 불가능함
```kotlin
class C {
  private fun foo() = object { val x: String = "x" }
  fun publicFoo() = object { val x: String = "x" }
  
  fun bar() {
    val x1 = foo().x        // Works
    val x2 = publicFoo().x  // ERROR
  }
}
```

## 객체 표현식 특징
- 익명 객체의 코드는 enclosing scope의 변수를 접근 할 수 있음
- Java와는 다르게 final variables 제약 조건이 없음
```kotlin
fun countClicks(window: JComponent) {
  var clickCount = 0
  var enterCount = 0
  window.addMouseListener(object : MouseAdapter() {
    override fun mouseClicked(e: MouseEvent) {
      clickCount++
    }
    override fun mouseEntered(e: MouseEvent) {
      enterCount++
    }
  })
}
```

# Object declarations
## 객체 선언 용도
- 매우 유용한 Sigleton 패턴을 Kotlin에서는 object declarations을 이용해서 만들 수 있음
```kotlin
object DataProviderManager {
  fun registerDataProvider(provider: DataProvider) {
    // ...
  }
  
  var allDataProviders: Collection<DataProvider>
    get() = // ...
}
```

## 객체 선언 문법
- object 키워드 뒤에 항상 이름이 있어야함
- object declaration은 object expression이 아님
- 그래서 할당 구문의 우측에 사용 될 수가 없음
```kotlin
object DataProviderManager {
  fun registerDataProvider(provider: DataProvider) {}
  val allDataProviders: Collection<DataProvider>
}
```
- object declaration의 객체를 참조 하려면, 해당 이름으로 직접 접근하면 됨
```kotlin
DataProviderManager.registerDataProvider(...)
```

## 객체 선언 문법
- 슈퍼타입을 가질 수 있음 (상속 가능)
```kotlin
object DefaultListener : MouseAdapter() {
  override fun mouseClicked(e: MouseEvent) {}
  override fun mouseEntered(e: MouseEvent) {}
}
```

# Companion Object
## 동반자 객체
- 클래스 내부의 object declaration은 companion 키워드를 붙일 수 있음
```kotlin
class MyClass {
  companion object Factory {
    fun create(): MyClass = MyClass()
  }
}
```
- companion object의 멤버는 클래스 이름을 통해서 호출 할 수 있음
```kotlin
val instance = MyClass.create()
```
- Companion object의 이름은 생략될 수 있음
- 이런 경우 [class name].Companion 형태로 객체에 접근 가능
```kotlin
class MyClass {
  companion object {
  }
}

val x = MyClass.Companion
```
- companion object의 멤버가 다른 언어의 static 멤버 처럼 보일 수 있지만 아님
- companion object의 멤버는 실제 객체의 멤버임
- 슈퍼클래스도 가질 수 있는 클래스의 객체임
```kotlin
interface Factory<T> {
  fun create() : T
}

class MyClass {
  companion object : Factory<MyClass> {
    override fun create() : MyClass = MyClass()
  }
}
```

# Semantic difference between object expressions and declarations
## Object expressions vs Object declaration
- object expressions는 즉시 초기화 되고 실행 된다.
- object declarations는 나중에 초기화 된다. (최초 접근 시)
- companion object는 클래스가 로드 될 때 초기화 됨. java static initializer와 매칭됨
