---
layout: post
title:  " 고차 함수"
categories: Kotlin
author: bn-tw2020
---
* content
{:toc}


## Intro

클로저가 나오면 고차함수가 끊임 없이 나오기에 스스로 정리해 이해하기 위해 작성하게 되었다.


## 고차 함수

고차 함수는 매개변수로 함수를 전달 or 함수를 반환하는 함수이다.

```kotlin
fun highFunction(a: Int, argFunction: (Int) -> Int) {
    val result = argFunction(10)
    println("a: $a, highFunction : $result")
}

highFunction(10, {x -> x + x})
```

- (Int) -> Int 함수를 argFunction으로 매개변수로 이용되었다.





### 함수 타입의 매개변수 대입

```kotlin
fun highFunction(argFunction: (Int) -> Int) {
    val result = argFunction(10)
    println("result : $result")
}

highFunction({x -> x + x})
highFunction {x -> x + x}
```

- 함수를 호출할 때 함수명 뒤에 ()를 붙이고 ()안에 인수를 작성한다.

  고차 함수의 매개변수가 함수 타입이면 함수 호출 시 () 생략이 가능하다.

    ```kotlin
    public inline fun <T> Iterable<T>.filter(predicate: (T) -> Boolean): List<T> {
        return filterTo(ArrayList<T>(), predicate)
    }
    ```

    - 실제로 filter 함수도 매개변수가 함수 타입이기 때문에 `filter { }` 형태로 사용하고 있다.
    

### 함수 타입 기본값

```kotlin
fun highFunction(argFunction: (Int) -> Int, argFunction2: (Int) -> Boolean = {x: Int -> x > 2}) {
    val result = argFunction(10)
    println("result : $result")
}

highFunction({x -> x + x}, {x: Int -> x > 10})
highFunction({x -> x + x})
```

- 함수를 선언할 때 매개변수에 기본 값을 지정하는 것처럼 고차 함수에서도 가능하다.


### 고차함수와 함수 반환

```kotlin
fun highFunction(str: String): (a: Int, b: Int) -> Int {
    return when(str) {
        "+" -> { a: Int, b:Int -> a + b }
        else -> { a: Int, b:Int -> a - b }
    }
}

val result = highFunction("+")
println("result : ${result(1, 2)}")
```

- 매개변수로 함수를 받는 것처럼 반환도 함수로 할 수 있다.

### 익명 함수를 이용한 함수 전달

```kotlin
fun highFunction(argFunction: (Int) -> Int) {
  println("${argFunction(10)}")
}

highFunction(fun(x: Int): Int = x + 2)
```

- 람다 함수는 `return` 예약어 사용이 불가능하다.

  위와 같이 익명함수를 매개 변수로 전달 할 수 있다.
