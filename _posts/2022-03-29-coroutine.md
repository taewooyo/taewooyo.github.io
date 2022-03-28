---
layout: post
title:  "코틀린 코루틴(coroutine)"
categories: Android
author: bn-tw2020
---
* content
{:toc}


## Intro

- 코루틴은 여러 언어(C#, Go, Javascript...)에서 지원하고 있는 개념이다.

  Javascript의 async, await를 사용하는 것과 비슷하다고 느꼈고 이미 코루틴을 사용한 것 같았다.

  실제로, 코루틴은 새로운 것이 아니라고 한다.





## 코루틴을 해야하는 이유

- 앱, 웹에서 비동기 처리가 핵심이고 RX Programming이다.

  구글이 안드로이드 공식 언어를 코틀린으로 이야기하고 비동기 처리를 코루틴으로 권장하고 있다.


## 코루틴

- 코루틴은 **협력형 멀티 태스킹**, **동시성 프로그래밍 지원**, **비동기 처리를 쉽게** 도와준다.

> 협력형 멀티태스킹

- 코루틴은 `Co + Routine`이다. 즉, 협력형 멀티 태스킹이다.

  Routine은 main, sub routine이 존재한다. 

```kotlin
fun main() {
    val result = plusOne(10)
}

fun plusOne(value: Int): Int {
    return value + 1
}
```

- main함수라는 메인 함수와 plusOne라는 서브 함수가 존재한다. 서브 루틴이라고 부르기도 한다.
- 함수는 반드시 진입점이 존재하고 빠져나오는 지점이 존재한다.
- 코루틴도 하나의 루틴이다. 코루틴은 진입할 수 있는 진입점도 여러 개고, 빠져나오는 지점도 여러개이다.
  
  언제든지, 중간에 나갈 수 있고 다시 들어올 수 있다.

```kotlin
fun loadRobot() {
    makeCoroutine {
        loadHead()
        loadBody()
        loadArms()
    }
}

suspend fun loadHead() { ... }
suspend fun loadBody() { ... }
suspend fun loadArms() { ... }
```
- 쓰레드의 Main함수가 loadRobot()을 호출하면 하나의 코루틴(블록)이 생성된다.

  loadRobot()은 언제든지 진입, 탈출할 수 있다.

- 코루틴 함수가 실행되는 과정에서 `suspend`를 가진 함수를 만나면,

  더 이상 아래 코드를 실행하지 않고(loadBody()) suspend(멈추고) 코루틴(블록)을 탈출한다.

- 메인 쓰레드의 다른 코드들이 실행된다. 하지만, `loadHead()`는 계속 그려지고 있다.

- 메인 쓰레드의 다른 코드들이 실행되다가 `loadHead()`가 끝나면 다시 코루틴으로 진입한다.

  `loadBody()`부터 다시 실행된다.

- 그래서 코루틴 함수는 언제든지 나왔다가 들어올 수 있다.

> 동시성 프로그래밍

- 함수를 중간에 빠져나왔다가, 다른 함수에 진입하고 다시 돌아와 시작하는 특성은 동시성 프로그래밍을 할 수 있다.

  동시성 프로그래밍은 병렬성 프로그래밍이 아니다. 동시성 프로그래밍은 빠른 시간 내에 나눠가면서 실행하는 것이다.

- 병렬성 프로그래밍은 나눠가는 것이 아니라. 같은 시간동안에 실행되는 것이다.

- 코루틴도 루틴이기에, 쓰레드가 아니라 일반 서브루틴과 비슷하기에 하나의 쓰레드에 여러개가 존재할 수 있다.


## 예제

```
1. 기상
2. 샤워
3. 옷입기
4. 외출하기
5. 회사도착
6. 일하기
반드시 순서대로 이루어져야 한다. 기상했는데 옷을 입으면 안된다.
```

```kotlin
fun myRoutine(person: Person) {
    val 잠자는스티치 = person
    wakeUp(잠자는_스티치) { 막일어난스티치 ->
        takeShower(막일어나는스티치) { 샤워한스티치 ->
            putOnClothes(샤워한스티치) { 옷입은스티치 ->
                outing(옷입은스티치) { 외출한스티치 ->
                    val 회사도착한스티치 = arriveCompany(외출한스티치)
                    회사도착한스티치.일하기()
                }
            }
        }
    }
}
```

- 에너르기파처럼 콜백지옥의 형태가 나타난다. 타인이 보기에 이해하기 어렵다.

```kotlin
suspend fun myRoutine(person: Person) {
    val 잠자는스티치 = person
    kotlin.runCatching { 
        val 막일어난스티치 = wakeUp(잠자는스티치)
        val 샤워한스티치 = takeShower(막일어난스티치)
        val 옷입은스티치 = putOnClothes(샤워한스티치)
        val 외출한스티치 = outing(옷입은스티치)
        val 회사도착한스티치 = arriveCompany(외출한스티치)
        회사도착한스티치.일하기()
    }
}
```

- 코루틴을 이용한 코드이다. 차례대로 언제 끝날지 모르는 비동기 작업들이다.

  정확히 함수들의 순서가 정확히 지켜진다. wakeUp()함수가 끝나야 takeShower()가 실행된다.

- myRoutine() 함수가 코루틴이기 때문에 wakeUp()을 만나면 wakeUp()실행하고 동시에 백그라운드 스레드에서 돌아간다.

  그리고 myRoutine()를 빠져나가고 wakeUp()이 끝나면, 다시 myRoutine()으로 돌아온다.


## Reference

- [Android Developers Coroutine](https://developer.android.com/kotlin/coroutines?hl=ko)
