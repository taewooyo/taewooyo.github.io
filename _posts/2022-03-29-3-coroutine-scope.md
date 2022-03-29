---
layout: post
title:  "코루틴 스코프, 코루틴 컨텍스"
categories: Android
author: bn-tw2020
---
* content
{:toc}

## CoroutineScope

- 코루틴이 실행되는 범위로, 코루틴을 실행하고 싶은 Lifecycle에 따라 원하는 Scope를 생성한다.

  코루틴이 실행될 작업 범위를 지정할 수 있다.





> CoroutineScope

- 사용자 정의 스코프

- CoroutineScope(Dispatcher.Main)

  CoroutineScope(Dispatcher.IO)

  CoroutineScope(Dispatcher.Default)

```kotlin
// 메인 스레드에서 실행될 사용자 정의 스코프
val scope = CoroutineScope(Dispatchers.Main)
scope.launch {
    // 메인 스레드 작업...
}

CoroutineScope(Dispatchers.IO).launch {
    // 백그라운드 작업....
}
```

> GlobalScope

- 앱이 실행될 때부터 종료될 때까지 실행

```kotlin
// 앱의 lifecycle 동안 실행될 스코프
GlobalScope.launch {
    launch(Dispatchers.IO) {} // 백그라운드로 전환해서 작업
    launch(Dispatchers.Main) {} // 메인스레드로 전환해서 작업
}
```

> ViewModelScope

- 안드로이드 jetpack 라이브러리(AAC)에서 코루틴을 쉽게 사용할 수 있도록 라이프사이클에 맞는 스코프를 제공해준다.

- viewModelScope는 Closeable를 구현해서 close() 호출시 coroutineContext를 cancel시키고 있기 때문이다.

```kotlin
class MyViewModel: ViewModel() {
    init {
        viewModelScope.lauch {
            // viewModel이 제거되면 코루틴도 자동 취소가 된다.
        }
    }
}
```

> LifecycleScope

- Lifecycle 객체 대상(Activity, Fragment, Service...) Lifecycle이 끝날 때 코루틴 작업이 자동 취소된다.

```kotlin
class MyActivity: AppCompatActivity() {
    init {
        lifecycleScope.launch {
            // lifecycle이 끝날 때 코루틴 작업이 자동 취소
        }
    }
}
```


## CoroutineContext

- 코루틴 작업을 어떤 스레드에서 실행할 것인지에 대한 동작을 정의하고 제어하는 요소이다.

  Job, Dispatchers 가 존재한다.
 
> Dispatchers

- 코루틴을 어떤 스레드에서 실행할 것인지에 대한 동작을 정의한다.

  [Dispatchers](https://bn-tw2020.github.io/2022/03/29/2-coroutine/)

> Job

- 코루틴을 공유하게 식별하고 코루틴을 제어한다.

```kotlin
val job = CoroutineScope(Dispatchers.IO).launch {
    // 비동기 작업
}

job.join() // 작업이 완료되기까지 대기
job.cancel() // 작업 취소
```

## Reference

- [Android Developers Coroutine](https://developer.android.com/kotlin/coroutines?hl=ko)
