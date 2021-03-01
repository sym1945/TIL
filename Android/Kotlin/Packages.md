# Package
- 소스 파일은 패키지 선언으로 시작 됨
- 모든 콘텐츠(클래스, 함수, ...)는 패키지에 포함 됨
- 패키지를 명세하지 않으면 이름이 없는 기본 패키지에 포함됨
```kotlin
package foo.bar

fun baz() {}

class Goo {}

fun main(args: Array<String>) {
  foo.bar.baz()
  foo.bar.Goo()
}
```

## 기본 패키지
- 기본으로 import되는 package가 있음
- 플랫폼 별로 import되는 package도 다른 부분도 있음
```kotlin
kotlin.*
kotlin.annotation.*
kotlin.collections.*
kotlin.comparisons.* (since 1.1)
kotlin.io.*
kotlin.ranges.*
kotlin.sequences.*
kotlin.text.*
```
```kotlin
JVM:
  java.lang.*
  kotlin.jvm.*
JS:
  kotlin.js.*
```

## Imports
- 기본으로 포함되는 패키지 외에도, 필요한 package 들을 직접 import 할 수 있음
```kotlin
// Bar 1개만 import함
import foo.bar

// 'foo' 패키지에 모든 것을 import함
import foo.*

// foo.Bar
// bar.Bar 이름이 충돌 나는 경우 'as' 키워드로 로컬 리네임 가능
import bar.Bar as bBar
```
