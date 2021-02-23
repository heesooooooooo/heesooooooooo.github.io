---
layout: post
cover: 'assets/images/cover4.jpg'
navigation: True
title: "[Python/프로그래머스] 2020 KAKAO BLIND RECRUITMENT: 문자열 압축"
date: 2021-02-23 00:00:00
tags: programmers python
subclass: 'post tag-programmers'
logo: 'assets/images/ghost.png'
author: heesoo
categories: programmers
---
## <span style="color:navy">👀 문제</span>
<https://programmers.co.kr/learn/courses/30/lessons/60057>

## <span style="color:navy">👊 도전</span>

### 1. 설계
1. 문자열 자를 사이즈를 구한다.
2. 기준 문자열(temp_s)와 현재 문자열(s[i:i+size])가 같으면 cnt++한다.
3. 아니라면, result에 저장한다.
4. 압축된 문자열 길이와 answer 중 min 값을 answer에 저장한다.


### 2. 구현 
{% highlight python linenos %}
def solution(s):
    answer = 1000
    if len(s)==1: return 1    
    
    for size in range(1, len(s)//2+1): # 압축 길이
        temp_s=s[:size]
        cnt=1
        result=""
        for i in range(size, len(s), size):
            if s[i:i+size]==temp_s:
                cnt+=1
            else:
                if cnt>1: result+=str(cnt)
                result+=temp_s
                temp_s=s[i:i+size]
                cnt=1
        if cnt>1: result+=str(cnt)
        result+=temp_s
        answer=min(answer, len(result))
    return answer
{% endhighlight %}

### 3. 결과
![실행결과](./assets/images/210223_1.PNG)
🤟 성공 🤟  


### 4. 설명
1. **<span style="color:navy">압축 길이를 구한다</span>**
- for문을 1~len(s)//2+1까지 체크하면 된다.
- 압축 길이가 s의 반 이상을 넘어가면 뒤에 문자열로는 압축시킬 수 없으므로 len(s)/2 이상은 체크하지 않아도 된다.

2. **<span style="color:navy">조건에 맞게 문자열을 압축한다</span>**
- 문자열이 같으면 cnt++한다.
- 다르다면, result에 현재 문자열을 넣는다. cnt는 cnt>1만 result에 넣는다.
- 이제 다음 체크할 문자열을 temp_s에 넣는다. 현재 s[i]가 다른 문자이므로 i를 포함하고 size만큼 길이를 가진다.
- cnt도 초기화.
- 마지막 i가 temp_s와 같으면 result에 추가되지않고 for문을 종료하게 된다. 이 값을 넣어줘야 하므로 for문 밖에서 result에 다시 추가해준다.

## <span style="color:navy">👏 해결 완료!</span>
- 문자열 압축-프로그래머스(python)(2020 Kakao 공채) <https://velog.io/@devjuun_s/%EB%AC%B8%EC%9E%90%EC%97%B4-%EC%95%95%EC%B6%95-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4python2020-Kakao-%EA%B3%B5%EC%B1%84>