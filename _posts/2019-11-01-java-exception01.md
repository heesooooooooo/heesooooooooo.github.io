---
layout: post
cover: 'assets/images/cover6.jpg'
navigation: True
title: "[JAVA/예외처리] java.util.ConcurrentModificationException"
date: 2019-11-01 00:00:00
tags: java exception error
subclass: 'post tag-java'
logo: 'assets/images/ghost.png'
author: heesoo
categories: java
---
![결과화면](./assets/images/191101_2.PNG)

### 예시
{% highlight java linenos %}
for(Truck t:q){//1초 추가
    t.time++;
    answer++;
    if(t.time==bridge_length){//경과시간이 다 채워지면 삭제
        q.poll();
    }
}
{% endhighlight %}
코드 설명: for문을 통해 Queue<Truck> q를 순회한다. if 조건에 맞다면 큐의 값을 삭제한다.

### 원인
for문과 같은 루프문을 통해 데이터를 접근하는 도중에, 데이터 변경이 일어날 때 발생한다.

### 해결 방법
Iterator를 사용하여 큐의 원소에 접근, 삭제한다.

{% highlight java linenos %}
Iterator iter=q.iterator();
while(iter.hasNext()){
    Truck t=(Truck)iter.next();
    t.time++;
    if(t.time==bridge_length){
        iter.remove();
    }
}
{% endhighlight %}
iterator()메소드를 이용해 iter를 선언, hasNext()로 현재 iter의 다음 원소가 있는지 파악한 후 있다면 next()로 값을 가져온다. remove()로 원소를 삭제한다.
