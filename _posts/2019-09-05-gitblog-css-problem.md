---
layout: post
cover: 'assets/images/cover4.jpg'
navigation: True
title: "[Github블로그] 블로그에 css 적용이 안될 때, 깨질 때 해결 방법"
date: 2019-09-05 20:50:00
tags: github blog
subclass: 'post tag-test tag-content'
logo: 'assets/images/ghost.png'
author: heesoo
categories: casper
---
Github 블로그를 fork하여 개설하는 과정에서 css가 깨지는 현상을 목격하였다. 메인 홈에서는 아무런 문제가 없었지만 다른 카테고리에서는 전부 css 적용이 안됐다ㅠ^ㅠ

원인: 기존의 작성된 경로와 fork를 딴 후 나의 경로는 다르기 때문에 파일 경로를 찾지 못해 오류가 발생한다.

따라서 **baseurl의 주소를 변경해야한다!**

해결 방법:
config.yml파일의 baseurl을 repo주소로 바꾼다.
나의 경우 baseurl:(USERNAME).github.io가 되는 것이다!

그래도 잘 모르겠다면
<https://stackoverflow.com/questions/25466166/stylesheets-not-working-for-jekyll-theme-freelancer-bootstrap>
을 참고해보자^.^
