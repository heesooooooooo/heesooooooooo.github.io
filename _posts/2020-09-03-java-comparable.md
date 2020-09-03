---
layout: post
cover: 'assets/images/cover6.jpg'
navigation: True
title: "[JAVA/자료구조] Comparable과 Comparator 차이"
date: 2020-09-03 00:00:00
tags: java 
subclass: 'post tag-java'
logo: 'assets/images/ghost.png'
author: heesoo
categories: java
---
## <span style="color:navy">Comparable과 Comparator 차이</span>

### 1. Comparable
- 오름차순, 가나다 등 기본 정렬 기준을 따르는데, 새 클레스에서 정렬할 기준(변수)을 잡아줄 때 사용
- 기존 Integer, String 등의 compareTo()를 이용하는게 보통  
- compareTo()를 오버라이드해서 사용, 파라미터는 하나
- 나 자신과 파라미터를 비교

{% highlight java linenos %}
import java.util.HashMap;
/**
 *
 * @author HEESOO
 *
 */
class Music implements Comparable<Music>{
    String name;
    int idx;
    int times;

    ...

    @Override
    public int compareTo(Music o1){
        return name.compareTo(o1.name); // name을 기준으로 사전순 정렬
    }
}
{% endhighlight %}


### 2. Comparator
- 내림차순, 사전 역순 등 기존 정렬 기준이 아닌 새로운 것을 짤 때 사용  
- compare()을 오버라이드
  
{% highlight java linenos %}
import java.util.HashMap;
/**
 *
 * @author HEESOO
 *
 */
class Main{
	...
    Arrays.sort(array, new Comparator<Music>(){
        @Override
        public int compare(Music m1, Music m2){ // name 기준 사전 역순으로 정렬
            if(m1.name<m2.name) return 1;
            else if(m1.name==m2.name) return 0;
            else return -1;
        }
    })
}
{% endhighlight %}

