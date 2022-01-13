---
layout: post
title:  " val vs const val"
categories: Kotlin
author: bn-tw2020
---
* content
{:toc}


## const val vs val

> val와 const val 모두다 값을 변경할 수 없는 불변성을 가지고 있는데 다른 이유가 도저히 궁금해서 정리하게 되었다.





### 코틀린 변수

- 코틀린에서 변수는 2가지로 `var` 과 `val` 이 존재한다.

  언제든 값을 변경할 수 있는 `var` / 한번 초기화 하면 값을 변경할 수 없는 `val`

  상수는 선언하기 위해서 `const val` 이 존재한다.

  `val` `const val` 둘다 상수로 불변한 값을 가지고 있는데 왜 2가지가 존재하는지 의문이 들었다.

- 상수는 한 번 초기화하면 내부의 값을 사용할 수 있지만, 변경할 수 없는 값이다.

  `val` `const val` 둘다 속하는 것으로 느껴지고 정리하게 되었다.

### val

- `val` 은 **런타임** 시 결정되는 상수로 독립적인 동일한 프로그램 수행 중에 프로그램 수행에 따라 값이 바뀔 수 있다.

```kotlin
fun main() {
    val num = sumNumber(40, 20)
    print(num)
}

fun sumNumber(a: Int, b: Int): Int = a + b
```

  두 수의 합이 num 변수에 저장하지만, 여기에 저장되는 값은 sumNumber(a, b) 함수의 파라미터에 따라 값이 변한다.
    
  `val` 을 사용한 상수는 불완전한 불변성을 보유하고 있다.

### const val

- `const val` 는 **컴파일** 시 결정되는 상수로, val과 다른 불변성을 가지고 있다.

  const val는 클래스의 생성자에 할당되지 못하며, String을 포함한 기본 자료형으로만 선언이 가능하다.

  즉, 런타임 시 생성되는 다른 클래스의 객체는 const val는 불가하다.

- const val는 함수 내 지역변수, 클래스의 속성으로 사용이 불가능하다.

  함수나 클래스 내에서 사용하려면 companion object 내에서 선언해야 한다.

- const val는 함수, 클래스의 상태에 상관없이 동일한 값을 가지고 있다.

  완전한 고정된 값을 사용하고 싶을 때, `const val` 을 이용하면 된다.

  const val의 코딩 컨벤션은 대문자와 _을 이용을 권장한다.
  
  ```kotlin
  fun main() {
  	print(Test.CONST_VAL)
  }

  class Test {
  	companion object {
		const val CONST_VAL = 5
  	}
  }
  ```

- const val는 컴파일 시에 데이터가 메모리에 존재한다. 그래서 인스턴스 생성 후에 접근하지 않고 클래스명.상수명을 통해 접근이 가능하다.
