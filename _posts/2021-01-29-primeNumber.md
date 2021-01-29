---
layout: post
title: "에라토스테네스의 체"
categories: 알고리즘
author: bn-tw2020
---
* content
{:toc}

## Problem

에라토스테네스의 체는 소수를 구할 때 사용되는 방법입니다.  
소수를 구하는 여러 가지 방법을 정리하는 글입니다.




---

## 순수하게 소수를 구하는 방법

* 소수인지 판별하고자 하는 수를 2부터 자신의 수까지 반복하면서 나머지 연산을 구해 구현하는 방법

```c++
bool isOk;
for(int i = start; i <=end; i++) {
    isOk = true;
    for(int j = 2; j <= i; j++) {
        if(i % j == 0) {
            isOk = false;
            break;
        }
    }
    if(isOk && i != 1) cout << i << "\n";
}
```

```
문제점
1. 데이터가 커지면 커질수록 루프의 연산횟수가 증가합니다.
2. 시간복잡도로는 O(n^2)
```

## 제곱근을 이용한 소수 구하기

* 자신의 수까지 도는 방법이 아닌 수의 제곱근까지 씌우는 것이다.

```c++
for(int i = start; i <= end; i++) {
    isOk = true;
    for(int j = 2; j <= (int)sqrt(i); j++) {
        if(i % j == 0){
            isOk = false;
            break;
        }
    }
    if(isOk && i != 1) cout << i << "\n";
}
```

## 에라토스테네스의 체

* 소수를 판별한 범위만큼 배열을 할당하여, 해당하는 값을 넣어주고, 이후에 하나씩 줘워나가는 방법

```
동작원리
1. 배열을 생성하여 초기화를 한다.
2. 2부터 시작해서 특정 수의 배수에 해당하는 수를 모두 지운다.
   지울 때 자신은 지우지 않고 지워진 수는 지나간다.
3. 2부터 남아있는 수는 모두 소수이므로 출력한다.
```

```c++
for(int i = 2; i <= n; i++)
    arr[i] = false;

for(int i = 2; i <= (int)sqrt(n); i++) {
    if(arr[i]) continue;
    for(int j = i+i; j <= 1000000; j += i) {
        arr[i] = true;
    }
}

for(int i = 2; i <= n; i++)
    if(!arr[i]) cout << arr[i] << "\n";
```

## 참고자료

[소수를 제곱근까지 계산하는 이유](http://sprexatura.blogspot.com/2016/05/n-n-square-root-of-n.html)