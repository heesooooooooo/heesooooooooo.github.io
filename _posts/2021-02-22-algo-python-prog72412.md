---
layout: post
cover: 'assets/images/cover4.jpg'
navigation: True
title: "[Python/프로그래머스] 2021 KAKAO BLIND RECRUITMENT: 순위 검색"
date: 2021-02-22 00:00:00
tags: programmers python
subclass: 'post tag-programmers'
logo: 'assets/images/ghost.png'
author: heesoo
categories: programmers
---
## <span style="color:navy">👀 문제</span>
<https://programmers.co.kr/learn/courses/30/lessons/72412>

## <span style="color:navy">👊 도전</span>

### 1. 설계
1. info[i]의 점수를 제외한 데이터와 '-'를 가지고 모든 경우의 수를 만든다.
2. 만든 경우의 수는 key가 되고, 점수는 value가 된다. value는 리스트 형태로 만들어서 오름차순 정렬 후 이분탐색으로 사용한다.
3. query[i]가 해시맵의 key에 있다면 그 value를 통해 점수 조건에 맞는 개수를 answer에 넣으면 된다.


### 2. 구현 
{% highlight python linenos %}
from itertools import combinations
import bisect

def solution(info, query):
    answer = []
    hashmap={}
    for i in info:
        i=i.split()
        data=i[:-1]
        score=int(i[-1])
        for n in range(5):
            comb=list(combinations(range(4), n))
            for c in comb:
                _data=data.copy()
                for c_idx in c:
                    _data[c_idx]='-'
                _data=''.join(_data)
                if _data in hashmap:
                    hashmap[_data].append(score)
                else:
                    hashmap[_data]=[score]
                    
    for value in hashmap.values():
        value.sort()
    
    for q in query:
        q=[i for i in q.split() if i!='and']
        q_score=int(q[-1])
        q_cnd=q[:-1]
        q_cnd=''.join(q_cnd)
        if q_cnd in hashmap:
            value_list=hashmap[q_cnd]
            answer.append(len(value_list)-bisect.bisect_left(value_list, q_score, 0, len(value_list)))
        else:
            answer.append(0)
        
    return answer
{% endhighlight %}

### 3. 결과
![실행결과](./assets/images/210222_1.PNG)
🤟 성공 🤟  


### 4. 설명
1. **<span style="color:navy">조합을 구한다</span>**
- 조합으로 만드는 인덱스는 data(info[i]의 점수를 제외한 데이터들)에 '-'를 넣을 위치가 된다.
- (), (0), (1), ... (0,1,2,3) == '-' 위치
- 생성한 경우의 수 리스트(_data)를 join연산으로 하나의 문자열로 바꾼 후 _data에 넣는다.
- _data가 hashmap에 있다면 value(리스트 형) 뒤에 추가한다.
- 아니라면, hashmap에 추가.
- value 이분탐색을 위해 오름차순 정렬한다.

2. **<span style="color:navy">해시맵에서 조건에 맞는 값을 리턴한다</span>**
- q를 스페이스 기준, and를 제외하고 배열에 넣는다.
- q_cnd(점수를 제외한 데이터)가 해시맵에 존재한다면, 해당 value에서 이분탐색으로 q_score의 삽입 위치를 찾는다(bisect_left 사용)
- 리턴받은 숫자가 조건에 맞지 않는 개수이므로 value.size()에서 리턴숫자를 뺀 값이 정답이 된다. answer에 넣는다.
- 해시맵에 없다면 0을 answer에 넣는다.

## <span style="color:navy">👏 해결 완료!</span>
- [프로그래머스] 순위 검색 / Python <https://dev-note-97.tistory.com/131>
- [프로그래머스] 순위 검색(Python) <https://velog.io/@study-dev347/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EC%88%9C%EC%9C%84-%EA%B2%80%EC%83%89Python>