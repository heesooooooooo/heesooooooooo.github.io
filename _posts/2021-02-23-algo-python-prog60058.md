---
layout: post
cover: 'assets/images/cover4.jpg'
navigation: True
title: "[Python/프로그래머스] 2020 KAKAO BLIND RECRUITMENT: 괄호 변환"
date: 2021-02-23 00:00:00
tags: python
subclass: 'post tag-programmers'
logo: 'assets/images/ghost.png'
author: heesoo
categories: programmers
---
## <span style="color:navy">👀 문제</span>
<https://programmers.co.kr/learn/courses/30/lessons/60058>

## <span style="color:navy">👊 도전</span>

### 1. 설계
1. 문제 조건에 맞게 진행


### 2. 구현 
{% highlight python linenos %}
def balance(w):
    open=close=0
    for idx, val in enumerate(list(w)):
        if val=='(': open+=1
        else: close+=1
        if open==close: return idx+1
    return 0

def correct(w):
    stack=[]
    for ch in list(w):
        if ch=='(': stack.append('(')
        else:
            if len(stack)==0: return False
            elif stack[-1]=='(': stack.pop()
            else: stack.append(')')
    if len(stack)==0: return True
    else: return False
    
def reverse(w):
    result=""
    for ch in list(w):
        if ch=='(': result+=')'
        else: result+='('
    return result

def solution(p):
    if p=='': return ''
    
    answer = ''
    idx=balance(p)
    u=p[:idx]
    v=p[idx:]
    if(correct(u)):
        return u+solution(v)
    else:
        answer='('+solution(v)+')'
        u=u[1:len(u)-1]
        answer+=reverse(u)
    return answer
{% endhighlight %}

### 3. 결과
![실행결과](./assets/images/210223_2.PNG)
🤟 성공 🤟  


### 4. 설명
1. **<span style="color:navy">균형잡힌 문자열 체크</span>**
- balance(w)
- w를 순회하며 여는괄호(open), 닫는괄호(close) 개수가 같아지는 인덱스를 리턴한다.
- for문을 끝까지 돈다면 w는 균형잡힌 문자열이 아니므로 0을 리턴한다.
- list(w)하면 문자열 w이 Char Array로 저장된다.
- enumerate()하면 튜플 형태로 반환되어 for문에서 index, value 형태로 사용할 수 있다.

2. **<span style="color:navy">올바른 괄호 문자열 체크</span>**
- correct(w)
- 스택을 이용한다. '('면 push한다.
- ')'면 스택에 값이 존재하고 top이 '('일 때 pop한다.
- top이 ')'면 push.
- 스택이 비어있으면 잘못된 문자열이므로 False를 리턴한다.
- for문을 다 돌았을 때, 스택이 비어있어야 올바른 문자열이다.

3. **<span style="color:navy">올바른 괄호 문자열이 아니라면</span>**
- 4-4에서 u의 첫 번째, 마지막을 slice로 지운 후, reverse()로 괄호 방향을 뒤집어서 붙인다.

## <span style="color:navy">👏 해결 완료!</span>