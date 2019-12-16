---
layout: post
cover: 'assets/images/cover7.jpg'
navigation: True
title: "[Algorithm] 플로이드 와샬 알고리즘(Floyd-Warshall Algorithm)"
date: 2019-12-12 00:00:00
tags: algorithm
subclass: 'post tag-algo'
logo: 'assets/images/ghost.png'
author: heesoo
categories: algo
---

## <span style="color:navy">플로이드 와샬 알고리즘</span>

### 1. 기본 개념
- 그래프에서 모든 꼭짓점 사이의 최단경로의 거리를 구하는 알고리즘이다.
- 최단 경로를 찾기에 좋은 알고리즘이다.
- O(n^3), 시간이 많이 소요된다는 단점이 있다.

### 2. 초기화

| i/j | 1 | 2 | 3 | 4 | 5 |
| :----: | :----: | :----: | :----: | :----: | :----: |
| 1 | 0 | INF | INF | INF | INF |
| 2 | INF | 0 | INF | INF | INF |
| 3 | INF | INF | 0 | INF | INF |
| 4 | INF | INF | INF | 0 | INF |
| 5 | INF | INF | INF | INF | 0 |

- i==j인 곳은 0으로, 나머지는 INF(적당히 큰 값)으로 초기화한다.
- 이때 INF가 Integer.MAX_VALUE일 경우, 두 값을 더해서 int형에 저장한다면 오버플로우가 발생하므로 INF는 적당히 큰 값으로 초기화하는 것이 좋다(ex. 987654321).

### 3. 코드
```java
/**
 *
 * @author HEESOO
 *
 */
/*...*/
for(int k=1;k<=n;k++){//거쳐가는 꼭짓점
  for(int i=1;i<=n;i++){//출발하는 꼭짓점
    for(int j=1;i<=n;j++){//도착하는 꼭짓점
      if(d[i][j]>d[i][k]+d[k][j]){//더 짧은 경로 찾았다면
        d[i][j]=d[i][k]+d[k][j];
      }
    }
  }
}
/*...*/
```

### 4. 결과
- 0과 INF를 제외한 값들이 최단 경로이다.

### 참고
- 플로이드-워셜 알고리즘(Floyd-Warshall Algorithm) <https://hsp1116.tistory.com/45>
- 플로이드 와샬 알고리즘 :: 마이구미 <https://mygumi.tistory.com/110>
