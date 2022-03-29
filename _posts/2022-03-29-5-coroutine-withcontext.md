---
layout: post title:  "withContext"
categories: Android author: bn-tw2020
---

* content {:toc}

## withContext

- async처럼 결과값을 반환하는 빌더로 async와 유사하면서 다른점이 존재한다.

  async는 반환하는 Deferred<T> 객체로 결과 값을 원하는 시점에 await()함수를 통해 결과 값을 얻는다.

  withContext()는 Deferred<T> 객체로 반환하지 않고, 결과(T)를 반환한다.

- async의 Deferred<T>는 지연객체로 생각된다. 원하는 시점에 await()로 결과 받기 때문이다.

```kotlin
suspend fun <T> withContext(
    context: CoroutineContext,
    block: suspend CoroutineScope.() -> T
): T (source)
```

### 활용법

> withContext + NonCancellable

- withContext는 코루틴 취소(Cancellation) 될 때 finally{} 블록에서 활용한다.

  취소할 경우 코루틴은 CancellationException 예외를 발생시킨다.

  취소 예외 발생 시, try ~ finally의 finally{} 블록에서 자원을 반납하거나 예외 처리를 한다.

- finally 블록에서 이미 코루틴이 취소되었기 때문에 중단함수(suspend) delay() 을 호출 할 수 없다.

  여기서 launch 등 새로운 코루틴을 생성하면 CancellationException 예외를 발생시킨다.

  그래서, 중단함수를 사용할 수가 없다.

- withContext + NonCancellable을 사용하면 된다.

  withContext는 async와 다르게 내부 코루틴 블록이 끝나고 결과값 반환까지 종료하지 않는다.

- NonCancellable의 구조에서 isActive 속성은 항상 true을 반환하도록 구성되어 있다.

  코루틴이 Completed(종료)될 때 까지 무조건 true 반환으로 취소예외에 영향없이 작업을 수행한다.

```kotlin
fun main() = runBlocking {
    val job = launch {
        try {
            repeat(1000) { i ->
                println("I'm sleeping $i...")
                delay(500L)
            }
        } finally {
            withContext(NonCancellable) {
                delay(1000)
                println("main : I'm running finally!")
            }
        }
    }
    delay(1300L)
    job.cancelAndJoin()
}
```

> withContext

```kotlin
GlobalScope.launch(Dispatchers.IO) {
    val name = withContext(Dispatchers.Main) {
        sleep(2000)
        "stitch"
    }
    println(name)
}
```

- launch 코루틴은 IO 쓰레드에서 동작

  withContext는 main 쓰레드에서 동작

  withContext 코루틴이 완료될 때 까지 println(name)은 실행되지 않는다.

  withContext 코루틴이 종료되면 println(name) 실행

- async와 비슷한 결과값을 리턴하는 코루틴이지만, await()으로 대기하고 결과를 받아올 필요가 없다.

## Reference

- [Android Developers Coroutine](https://developer.android.com/kotlin/coroutines?hl=ko)
