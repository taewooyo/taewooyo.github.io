---
layout: post
title: "문자열 분리"
categories: 알고리즘
author: bn-tw2020
---
* content
{:toc}

## Intro

```
C++에서 공백을 포함한 문자열을 분리하는 방법을 정리하는 글입니다.
```




---

## [1] Code

```c++
#include<bits/stdc++.h>
using namespace std;

int main() {
    string s = "java backend junior pizza 150";

    vector<string> input;
    int preIdx = 0;
    for(int i = 0; i < s.size(); i++) {
        if(s[i] == ' ') {
            input.push_back(s.substr(preIdx, i - preIdx));
            preIdx = i + 1;
        }
    }
    input.push_back(s.substr(preIdx));

    for(auto i : input)
        cout << i << '\n';
}
```

## [2] Code

```c++
#include<bits/stdc++.h>
using namespace std;

int main() {
    string s = "java backend junior pizza 150";

    stringstream ss;
    string input[5];
    
    ss.str(s);
    ss >> input[0] >> input[1] >> input[2] >> input[3] >> input[4];

    cout << input[0] << '\n';
    cout << input[1] << '\n';
    cout << input[2] << '\n';
    cout << input[3] << '\n';
    cout << input[4] << '\n';
}
```