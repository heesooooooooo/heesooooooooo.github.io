---
layout: post
cover: 'assets/images/cover6.jpg'
navigation: True
title: "[JAVA/자료구조] 우선순위큐: CompareTo를 이용한 우선순위 구현"
date: 2019-11-07 00:00:00
tags: java priorityqueue
subclass: 'post tag-java'
logo: 'assets/images/ghost.png'
author: heesoo
categories: java
---

우선순위큐뿐만 아니라 자료구조의 정렬 조건을 바꾸고 싶을 때 CompareTo를 Override하는 방법을 택한다.

### 1. 기본 개념
- 리턴값이 음수일 경우 우선순위가 높아진다. 주로 -1, 0, 1을 이용하여 우선순위를 나눈다.

### 2. 예시
```java
/**
 *
 * @author HEESOO
 *
 */
class Job implements Comparable<Job>{
    int start;
    int time;
    public Job(int s, int t){
        this.start=s;
        this.time=t;
    }
    @Override
    public int compareTo(Job j){
        //소요시간 작은 게 우선순위 높음
        if(this.time<j.time)return -1;
        //소요시간이 같다면
        else if(this.time==j.time){
            if(this.start<j.start)return -1;//요청시점 작은게 우선순위 높음
            else return 1;
        }
        else return 1;
    }
}  
```
- 클래스 뒤에 `implements Comparable`를 붙인다.
- 기존 메소드에 수정하는 것이므로 `@Override`를 붙인다.
- 위의 경우 time이 작을수록 우선순위가 크다.
- 만약 time이 같다면 start가 작을수록 우선순위가 커진다.
- 이후 sort메소드를 호출할 경우 Override된 compareTo에 따라 정렬된다.
