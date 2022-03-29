---
layout: post 
title:  "안드로이드 Looper와 Handler"
categories: Android 
author: bn-tw2020
---
* content {:toc}

## Looper와 Handler

- 안드로이드에서의 UI는 싱글스레드 모델로 작동한다.

  그래서 성능 저하가 발생할 수가 있다.

  긴 시간이 걸리는 작업을 메인스레드에서 작업하게 되면 애플리케이션 성능이 저하된다.

  많은 시간이 걸리게 되면 ANR(Application Not Responding) 상태로 전환된다.

  시간이 걸리는 작업을 수행할 때는 새로운 스레드를 활용해 메인스레드와 분리해서 작업해야하며,

  메인스레드와 다른 스레드가 통신해야 하는 방법이 있어야 한다.





## 사용이유

- 메인 스레드와 다른 스레드가 TextView의 text 값을 수정한다면,

  어떤 스레드 수정한 값이 적용되는지 알 수가 없다.

  두 개 이상의 스레드에서 사용할 때의 동기화 이슈를 막기 위해서 Looper와 Handler을 사용한다.

## 동작원리

- 메인스레드는 내부적으로 Looper를 가지고 있으며, 그 안에는 Message Queue가 포함된다.

- Message Queue는 스레드가 다른 스레드나 혹은 자기 자신으로부터 전달받은 Message를 선입선출하는 방식으로 전달하는 큐이다.

- Looper는 Message Queue에서 Message나 Runnable 객체를 꺼내 Handler가 처리하도록 전달한다.

- Handler는 Looper로부터 받은 Message를 실행 처리하거나,

  다른 스레드로부터 메시지를 받아서 Message Queue에 넣는 역할을 하는 스레드간의 통신 장치이다.

```
1. Message를 handler의 sendMessage()를 통해 handler에게 전달한다.
2. handler에게 전달된 메시지는 Looper의 MessageQueue에 들어간다.
3. Looper의 loop() 메소드를 통해서 처리할 메시지를 Handler에게 보낸다.
4. Handler는 handleMessage() 메소드를 통해 메시지를 처리한다.
```

## Handler

- Handler는 스레드의 Message Queue와 연계하여 Message나 Runnable 객체를 받거나 처리하여 스레드 간의 통신을 할 수 있다.

- Handler 객체는 하나의 스레드의 Message Queue에 종속된다.

- 새로 Handler 객체를 만든 경우, 이를 만든 스레드와 해당 스레드의 Message Queue에 연결된다.

- 다른 스레드가 특정 스레드에게 메시지를 전달하기 위해서 특정 스레드에 속한 handler의 post, sendMessage를 호출한다.

- Message Queue는 선입선출의 방식으로 보관한다.


## Looper

- Looper는 무한히 루프를 돌며 자신이 속한 스레드의 Message Queue에 들어온 Message나 Runnable 객체를 차례로 꺼내서 처리할 Handler에게 전달하는 역할이다.

- 메인 스레드는 기본적으로 Looper가 생성되어 있지만,

  새로 생성한 스레드는 기본적으로 Looper를 가지고 있지 않고 run메서드만 실행한 후 종료하기 때문에 메시지를 받을 수 없다.

- 기본 스레드에서 메시지를 전달받으려면 prepare() 메서드를 통해 Looper을 생성하고 

  loop() 메서드를 통해 Looper가 무한히 루프를 돌며 Message Queue에 쌓인 Message나 Runnable 객체를 꺼내

  Handler에 전달하도록 한다.

- 활성화된 Looper는 quit() 혹은 quitSafely() 메서드로 중단할 수 있다.

  quit() 메서드가 호출되면 Looper는 즉시 종료되고

  quitSafely() 메서드가 호출되면 Message Queue에 쌓인 메시지를 처리하고 종료한다.


## Message와 Runnable

- Message는 스레드가 통신할 내용을 담은 객체이며

  Queue에 들어갈 단위로 Handler를 통해 보낼 수 있다.

> Runnable

- 스레드는 Thread() 생성자를 만들어서 내부적으로 run()를 구현하는 방법

  Thread() 생성자로 만들어서 Runnable 인터페이스를 구현한 객체를 생성하여 전달하는 방법이 존재한다.

- Message는 통신할 내용을 담는다면

  Runnable은 실행할 run() 메서드와 그 내부에서 실행될 코드를 담는다.


## HandlerThread

- 안드로이드의 스레드는 Java의 스레드를 사용하기 때문에 내부적으로 Looper를 가지고 있지 않다.

  Looper를 보유하는 스레드 클래스를 직접 만들어야하지만, 

  개선점으로 Looper를 보유하는 스레드 클래스를 만든 HandlerThread가 존재한다.


## Summary

- Coroutine을 학습하고 Thread, Looper, Handler을 학습하면서, 개발하면서 코루틴이 더 직관적인 것 같다.

- Thread는 작업 단위가 쓰레드이고 Coroutine은 Coroutine Object이다.

- 동시성 보장을 위해 스레드는 Context Switching을 통해 블로킹이 되서 자원을 사용하지 못할 수도 있다.

  코루틴은 프로그래머의 코드를 통해 Switching을 맘대로 정할 수 있다. 

  블로킹이 아니라 하나의 스레드에서 Thread는 유효하기 때문에 Context Switching이 필요없어서 Light-weight Thread라고 부르는 것 같다. 

## Reference

- [Thread, Looper, Handler](https://academy.realm.io/kr/posts/android-thread-looper-handler/?w=1)