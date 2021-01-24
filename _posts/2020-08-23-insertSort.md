---
layout: post
title: "선택 정렬"
categories: 알고리즘
author: bn-tw2020
---
* content
{:toc}

# 정렬 알고리즘

-   정렬(Sorting)이란 `데이터를 특정한 기준에 따라 순서대로 나열`하는 것
-   일반적으로 문제 상황에 따라서 적절한 정렬 알고리즘이 공식처럼 사용





---

### 선택정렬

-   처리되지 않은 데이터 중에서 가장 작은 데이터를 선택해 맨 앞에 있는 데이터와 바꾸는 것을 반복

> 7 5 9 0 3 1 6 2 4 8<br>

`0` 5 9 7 3 1 6 2 4 8<br>
`0` 1 9 7 3 5 6 2 4 8<br>
`0` `1` 9 7 3 5 6 2 4 8<br>
`0` `1` `2` 7 3 5 6 9 4 8<br>
`0` `1` `2` `3` 7 5 6 9 4 8<br>
`0` `1` `2` `3` `4` 5 6 9 7 8<br>
`0` `1` `2` `3` `4` `5` 6 9 7 8<br>
`0` `1` `2` `3` `4` `5` `6` 9 7 8<br>
`0` `1` `2` `3` `4` `5` `6` `7` 9 8<br>
`0` `1` `2` `3` `4` `5` `6` `7` `8` 9<br>
`0` `1` `2` `3` `4` `5` `6` `7` `8` `9`<br>

> 마지막은 자동으로 정렬된다.

### 소스코드

```python
array = [7, 5, 9, 0, 3, 1, 6, 2, 4, 8]

for i in range(len(array)):
    min_index = i #가장 작은 원소의 인덱스
    for j in range(i+1, len(array)):
        if array[min_index] > array[j]:
            min_index = j
    array[i], array[min_index] = array[min_index], array[i] #스와프

print(array)

```

```c++
#include <bits/stdc++.h>
using namespace std;

int n = 10;
int target[10] = {7, 5, 9, 0, 3, 1, 6, 2, 4, 8};

int main(){
    for(int i=0; i<n; i++){
        int min_index = i;
        for(int j=i+1; j<n; j++){
            if(target[min_index] > target[j]){
                min_index = j;
            }
        }
        swap(target[i], target[min_index]);
    }

    for(auto a : target){
        cout<<a<<' ';
    }
    return 0;
}

```

---

### 삽입 정렬의 시간 복잡도

-   삽입 정렬은 N번 만큼 가장 작은 수를 찾아서 맨 앞으로 보내야 한다.
-   구현 방식에 따라서 사소한 오차는 있을 수 있지만, 전체 연산 횟수는 `N + (N-1) + (N-2) + ... + 2`
-   이는 $$((N^2) + N - 2) / 2$$ 로 표현이 되는데, 빅오 표기법에 따라서 $$O(N^2)$$
