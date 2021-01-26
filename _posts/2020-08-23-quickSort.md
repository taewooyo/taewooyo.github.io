---
layout: post
title: "퀵 정렬"
categories: 알고리즘
author: bn-tw2020
---
* content
{:toc}




### 퀵 정렬

* `기준 데이터를 설정`하고 그 기준보다 큰 데이터와 작은 데이터의 위치를 바꾸는 방법
* 일반적인 상황에서 가장 많이 사용되는 정렬 알고리즘
* 병합 정렬과 더불어 대부분의 프로그래밍 언어의 정렬 라이브러리의 근간이 되는 알고리즘
* 가장 기본적인 퀵 정렬은 `첫 번째 데이터를 기준 데이터(Pivot)로 설정`

---

> 5 7 9 0 3 1 6 2 4 8<br>

* 현재 피벗의 값은 `5`<br>
  왼쪽에서부터 `5`보다 큰 데이터를 선택하므로 `7` 선택<br>
  오른쪽에서부터 `5`보다 작은 데이터를 선택하므로 `4`가 선택<br>
  이제 두 데이터의 위치를 서로 변경

<center>
<span style="color:red">5</span> <span style="color:blue">7</span> 9 0 3 1 6 2 <span style="color:blue">4</span> 8<br>
<span style="color:red">5</span> 4 9 0 3 1 6 2 7 8<br>
<span style="color:red">5</span> 4 <span style="color:blue">9</span> 0 3 1 6 <span style="color:blue">2</span> 7 8<br>
<span style="color:red">5</span> 4 2 0 3 1 6 9 7 8<br>
<span style="color:red">5</span> 4 2 0 3 <span style="color:blue">1</span> <span style="color:blue">6</span> 9 7 8<br>

</center>

* 위치가 엇갈리는 경우 '피벗'과 '작은 데이터'의 위치를 서로 변경

<center>
1 4 2 0 3 <span style="color:red">5</span> 6 9 7 8
</center>

* `분활 완료` 피벗을 기준으로 데이터 묶음을 나누는 작업을 분할이라 합니다.

* 위에서 한 방법으로 왼쪽 덩어리, 오른쪽 덩어리를 똑같이 반복해줍니다.

### 소스코드

```c++
#include <bits/stdc++.h>
using namespace std;

int n = 10;
int target[10] = {7, 5, 9, 0, 3, 1, 6, 2, 4, 8};

void quickSort(int* target, int start, int end){
    if(start>= end) return;
    int pivot= start;
    int left= start+1;
    int right = end;
    while(left<= right){
        while(left<= end && target[left]<= target[pivot]) left++;
        while(right > start && target[right] >= target[pivot]) right--;
        if(left > right) swap(target[pivot], target[right]);
        else swap(target[left], target[right]);
    }
    quickSort(target, start, right-1);
    qucikSort(target, right+1, end);
}

int main(){
    quickSort(target, 0, n-1);

    for(auto a : target){
        cout<<a<<' ';
    }
    return 0;
}

```

---

### 퀵 정렬의 시간 복잡도

-   퀵 정렬의 평균의 경우 O(NlogN) 의 시간 복잡도를 가집니다.
-   하지만 최악의 경우 O(N^2)의 시간 복잡도를 가집니다.

    -   첫 번째 원소를 피벗으로 삼을 때, 이미 정렬된 배열에 대해서 퀵 정렬을 수행하면 느립니다.
    -   왜냐하면 선형 탐색 이후에 분할이 되는데 분할이 되면서 한쪽으로 치우치기 때문에 퀵 정렬을 이용하게 되면 최악의 경우가 됩니다.

    > 데이터가 거의 정렬되어 있다면 삽입정렬이 빠릅니다.

    > 데이터가 거의 정렬되어 있다면 퀵 정렬이 훨씬 느리다.

    ***

    ### 퀵 정렬이 빠른 이유 : 직관적인 이해

    -   이상적인 경우 분할이 절반씩 일어난다면 전체 연산 횟수는 O(NlogN)와 비례할 것을 예상합니다.
        -   너비 x 높이 = NlogN

```
0  0  0  0  0  0  0  0
----------------------

0 0 0 0        0 0 0 0
-------        -------

0 0   0 0    0 0   0 0
---   ---    ---   ---

0  0  0  0  0  0  0  0
-  -  -  -  -  -  -  -
```

> 위의 그림을 보면 쉽게 이해가 될 것입니다.
