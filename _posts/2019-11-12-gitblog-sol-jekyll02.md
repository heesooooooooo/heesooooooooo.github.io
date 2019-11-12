---
layout: post
cover: 'assets/images/cover4.jpg'
navigation: True
title: "[Github블로그/Jekyll] Liquid Exception: Liquild syntax error 해결"
date: 2019-11-12 00:00:00
tags: git gitblog jekyll error
subclass: 'post tag-git'
logo: 'assets/images/ghost.png'
author: heesoo
categories: git
---
![결과화면](./assets/images/191112_6.PNG)

### 원인
Jekyll에서 사용되는 liquid는 `{% raw %}{{`와 `}}{% endraw %}`를 escape 문자로 사용하는데, md문서에 {% raw %}{{, }}{% endraw %}가 있는 경우 에러 메시지를 출력한다.

### 해결 방법
![결과화면](./assets/images/191112_5.PNG)  
위와 같이 여는 중괄호가 시작하기 전에 raw를, 뒤에는 endraw를 추가한다.

### 참고
- Jekyll에서 liquid warning 처리 <http://jmjeong.com/escape-in-liquid-syntax/>
