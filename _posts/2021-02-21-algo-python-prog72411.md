---
layout: post
cover: 'assets/images/cover4.jpg'
navigation: True
title: "[Python/프로그래머스] 2021 KAKAO BLIND RECRUITMENT: 메뉴 리뉴얼"
date: 2021-02-21 00:00:00
tags: python
subclass: 'post tag-programmers'
logo: 'assets/images/ghost.png'
author: heesoo
categories: programmers
---
## <span style="color:navy">👀 문제</span>
<https://programmers.co.kr/learn/courses/30/lessons/72411>

## <span style="color:navy">👊 도전</span>

### 1. 설계
1. order의 조합을 구한다.
2. hashmap에 조합 별 개수를 세서 저장한다.
3. 조합 길이의 최댓값을 size에 저장한다.
4. 조합 빈도수(hashmap)이 2 이상이고 최댓값(size)인 결과를 answer에 넣는다.
5. answer을 오름차순 정렬하여 리턴한다.


### 2. 구현 
{% highlight python linenos %}
/**
 *
 * @author HEESOO
 *
 */
from itertools import combinations

def solution(orders, course):
    answer = []
    hashmap={}
    size={}
    for order in orders:
        for i in course:
            for comb in combinations(order, i):
                menu="".join(sorted(comb))
                if menu in hashmap:
                    hashmap[menu]+=1
                else:
                    hashmap[menu]=1
                if len(menu) in size:
                    size[len(menu)]=max(size[len(menu)], hashmap[menu])
                else:
                    size[len(menu)]=hashmap[menu]
    
    for i in hashmap:
        if hashmap[i]>1 and hashmap[i]==size[len(i)]:
            answer.append(i)
    return sorted(answer)
{% endhighlight %}

### 3. 결과
![실행결과](./assets/images/210221_2.PNG)
🤟 성공 🤟  


### 4. 설명
1. **<span style="color:navy">조합을 구한다</span>**
- combinations로 조합을 구한다.
- "XY", "YX"는 같은 것으로 생각하기 때문에 sorted()로 오름차순 정렬한 후, join()으로 문자열을 합친다.
- hashmap에 menu가 있다면 하나 증가, 없다면 1을 넣어준다.
- size는 문자열 길이의 max를 저장한다. size에 len(menu) 값이 들어가있다면 최댓값으로 갱신, 없다면 현재 hashmap[menu]를 넣는다.

2. **<span style="color:navy">조합 중 조건에 맞는 값을 리턴한다</span>**
- 조합 횟수가 2 이상이고 길이가 최댓값인 것만 answer에 넣는다.
- 오름차순으로 정렬하여 리턴한다.

## <span style="color:navy">👏 해결 완료!</span>
