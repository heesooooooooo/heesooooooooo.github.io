---
layout: post
cover: 'assets/images/cover7.jpg'
navigation: True
title: "[Algorithm] 백트래킹(BackTracking)"
date: 2020-03-12 00:00:00
tags: algorithm
subclass: 'post tag-algo'
logo: 'assets/images/ghost.png'
author: heesoo
categories: algo
---

## <span style="color:navy">백트래킹</span>

### 1. 기본 개념
- 모든 경우를 구할 때 사용한다.
- 그리디 알고리즘(Greedy Algorithm)처럼 모든 가능성을 조회한다는 것은 같으나, 백트래킹은 계산 도중 아닌 것 같으면 종료한다(그리디는 진짜 다 구한다).
- DFS를 이용한다.
- visit[]를 두어 사용 여부를 체크한다.
- 수행 후 다시 재귀호출로 Depth를 만족시킨다.
- 복귀 후 visit[i]=false로 바꾸어 다음 사용을 가능하게 한다.
- 퀸 문제가 대표적이다. <https://www.acmicpc.net/problem/9663>
- 스도쿠에도 이용된다. <https://www.acmicpc.net/problem/2580>
- 순열(순서있게 배열), 조합(순서 상관없이 뽑는 것에 집중)에도 이용된다.

### 2. 코드
```java
/**
 *
 * @author HEESOO
 *
 */
/*...*/
public static void recur(...){
    if(/*재귀 종료 조건*/){
        //맨 마지막 Depth에 도달 시 수행해야 할 코드 작성
        return;//다시 호출 지점으로 복귀
    }

    for(int i=0;i<n;i++){
        if(!visit[i]){
            visit[i]=true;//해당 값 사용
            recur(...);
            visit[i]=false;//다음 사용을 위함
        }
    }
}

/*...*/
```

### 4. 결과
- visit[]로 중복 사용을 없애고, 다시 재귀호출하여 DFS의 depth조건을 만족시킨다.
- recur로 끝까지 방문하고 return으로 재귀 호출 지점으로 복귀한 후에는, 또 다른 경우의 수에 따른 계산을 위해 visit[i]=false로 바꾸어 다시 재사용할 수 있게 한다.

