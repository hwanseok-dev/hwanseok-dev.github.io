---
title: "[이론][완전탐색][N자리 K진수] Chapter 4"
excerpt: "N자리 K진수 2차원에 적용하기"
date: 2020-03-30
last_modified_at: 2021-01-08
categories:
  - Algorithm-Theory
tags:
  - Algorithm-Theory 
  - back-tracking
toc : true
toc_label: "Table of contents"
toc_icon: "list"  # corresponding Font Awesome icon name (without fa prefix)
toc_sticky: false
---

## [소문난 칠공주](https://www.acmicpc.net/problem/1941)

문제의 정답은 [여기](https://gist.github.com/niklasjang/888c3087147ac9a057cfc29237c7f25b)에서 확인할 수 있습니다. 

이 문제는 1차원 백트레킹을 2차원으로 확장해서 적용해야하는 문제입니다. 반대로 생각하면 2차원의 입력을 1차원으로 바꿔서 생각하면 백트레킹으로 접근할 수 있게 됩니다. 

1. 일단 25개의 좌표 중에서 7개를 선택합니다.
1. 7개 중 'S'가 4개 이상 선택되었는지 판단하고
1. 7개에 대해서 connected component의 갯수가 1인지 판단합니다.
1. 여기까지 통과하면 ans++;를 진행합니다. 

먼저 25개의 좌표 중에서 7개를 선택하는 것은 \[x\]\[y\]에서 항상 \[x\]\[y+1\]로 이동한다고 가정하는 방법으로 접근합니다. 일단 무조건 y를 1씩 이동하면서 진행시키고, 다음에 recur를 진행하기 전에 if(y==5){ x++; y = 0;}을 진행해서 다음 줄의 가장 왼쪽 칸으로 이동합니다. 여기서 x,y는 map\[x\]\[y\]를 선택할지 말지 정하는 좌표를 의미합니다.  

그리고 이전 포스팅에서 풀었던 문제와 같이 중복 제거는 오름차순으로 선택하면, 오름차순은 선택한다/안한다의 개념을 적용하면 자동으로 이루어집니다.  

## [스도쿠](https://www.acmicpc.net/problem/2239)

문제의 정답은 [여기](https://gist.github.com/niklasjang/e2c2c3e4cd284b4eb86b66d49708be7d)에서 확인할 수 있습니다. 

아래는 위 코드를 작성하면서 고려해야했던 7가지 특징들입니다.  

1. 소문난 칠공주 문제의 경우 선택하는 횟수(cnt)로 종료 조건 만들었다면, 스도쿠는 x,y좌표로만 가지고 종료 조건 설정하기
1. 특정 좌표의 값이 0이 아닌 경우 바로 recur(x,y+1)
1. 특정 좌표의 값이 0인 경우 for()문 순회
1. 특정 좌표의 값을 채우고, correct()를 통해서 가로 9개, 세로 9개, 3\*3 9개 칸만 확인해서 불필요한 recur() 없애기
1. recur()타고 쭉쭉 들어갔는데 특정 좌표에서 1~9 모두 들어가지 못하는 경우 원래 0이었던 칸을 다시 0으로 만들면서 back-tracking 진행하기
1. 최초의 정답 출력 후 모든 recur() 무마시키기
1. check() 호출 중 임의의 위치에서 return false 되었다가 다시 check()호출될 때도 memset(visited)되도록 memset 위치 설정하기


