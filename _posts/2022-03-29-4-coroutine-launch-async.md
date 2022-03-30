---
layout: post 
title:  "launch, async"
categories: Android 
author: bn-tw2020
---

* content {:toc}

## CoroutineBuilder

- 코루틴 스코프를 생성하는 코루틴 빌더에 대한 설명이다.

- [Scope](!https://bn-tw2020.github.io/2022/03/29/3-coroutine-scope/) 는 코루틴이 실행되는 범위 이다.

  CoroutineScope의 확장함수로, 다양한 요구사항에 맞게 코루틴을 만들 수 있다.

  launch, async, withContext, runBlocking, actor, produce이 존재한다.





## launch

- launch는 결과 반환을 하지 않고 반환 타입은 Job이다.

```kotlin
with(CoroutineScope(Dispatchers.Main)) {
    val job: Job = launch { println(1) }
}
```

## async

- async는 결과를 반환하며 결과값은 Deferred로 감싸서 반환된다.

  Deferred는 미래에 올 수 있는 값을 담아놓는 객체이다.

- Deferred<T>의 await()메서드가 실행되면 코루틴은 결과가 반환되기까지 기다린다.

  코루틴이 일시 중단되었다라는 의미이다.

  await()메서드는 일시 중단이 가능한 코루틴 내부에서만 사용이 가능하다.

  이후 결과이 반환되었을 때, 코루틴은 재개되고 println(value)가 생행된다.

```kotlin
CoroutineScope(Dispatchers.Main).launch {
    val deferredInt: Deferred<Int> = async {
        println(1)
        1
    }
    val value = deferredInt.await()
    println(value)
}
```

## 예시

```
파일에서 데이터를 받아와 정렬 후 출력하기
```

- 파일 입출력: Dispatchers.IO

  정렬: Dispatchers.Default

  뷰 출력: Dispatchers.Main

위에 맞게 Dispatcher Switching을 진행하면서 편리하게 할 수 있다.

```kotlin
CoroutineScope(Dispatchers.Main).launch { // 0. Main Dispatcher를 기본으로 설정

    // 1. 데이터 입출력을 위한 IO Dispatcher
    val deferredInt: Deferred<Array<Int>> = async(Dispatchers.IO) {
        println(1)
        arrayOf(5, 4, 2, 1, 3)
    }

    // 2. 정렬을 위한 Default Dispatcher
    val sortedDeferred = async(Dispatchers.Default) {
        val value = deferredInt.await()
        value.sortedBy { it }
    }

    // 3. 다른 설정하지 않으면 기본 설정인 Main Dispatcher에 보내진다.
    val job = launch {
        val sortedArray = sortedDeferred.await()
        setTextView(sortedArray)
    }
}
```

## Reference

- [Android Developers Coroutine](https://developer.android.com/kotlin/coroutines?hl=ko)

- [예제](https://kotlinworld.com/144?category=973476)
