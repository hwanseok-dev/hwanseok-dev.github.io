---
title: "[BOJ][구현][브4][1550] template 문서"
excerpt: ""
date: 2021-01-01
last_modified_at: 
categories:
  - Algorithm-Practice
tags:
  - Algorithm-Practice
  - Practice
  - Implementation
  - 구현
  - BOJ
  - "1550"
  - 16진수
toc : true
toc_label: "Table of contents"
toc_icon: "list"  # corresponding Font Awesome icon name (without fa prefix)
toc_sticky: true
---

## 문제 링크

[문제링크](https://www.acmicpc.net/problem/1550)  

## 첫 번째 풀이

### 알고리즘

주어지는 16진수의 길이가 최대 6입니다. 예를 들어 FFFFFF가 입력될 수 있습니다. 

### 정답 코드

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
int main()
{
    //freopen("input.txt", "r", stdin);
    ios_base::sync_with_stdio(0); cin.tie(0);

    string s;
    cin >> s;
    ll ans = 0;

    for (int i = s.size()-1; i>=0 ; --i) {
        int coff = 0;
        if ('A' <= s[i]) {
            coff = s[i] - 'A' + 10;
        }
        else if ('0' <= s[i]) {
            coff = s[i] - '0';
        }
        ans += coff * pow(16, s.size() - 1 - i);
    }
    cout << ans;
    return 0;
}
```

**Success Notice:**
소수 개념을 사용한 문제를 풀어봤습니다. 수고하셨습니다. :+1:
{: .notice--success}


**Info Notice:** 
본 포스팅은 템플릿 문서입니다.
{: .notice--info}

**Warning Notice:**
{: .notice--warning}

**Danger Notice:**
{: .notice--danger}

**Success Notice:**
{: .notice--success}