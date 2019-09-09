---
layout: post
cover: 'assets/images/cover4.jpg'
navigation: True
title: "[Github블로그] 블로그에 css 적용이 안될 때, 깨질 때 해결 방법"
date: 2019-09-05 20:50:00
tags: gitblog error
subclass: 'post tag-gitblog'
logo: 'assets/images/ghost.png'
author: heesoo
categories: gitblog
---
Github 블로그를 fork하여 개설하는 과정에서 css가 깨지는 현상을 목격하였다. 메인 홈에서는 아무런 문제가 없었지만 다른 카테고리에서는 전부 css 적용이 안됐다ㅠ^ㅠ

원인: 기존의 작성된 경로와 fork를 딴 후 나의 경로는 다르기 때문에 파일 경로를 찾지 못해 오류가 발생한다.

따라서 **baseurl의 주소를 변경해야한다!**

해결 방법:
config.yml파일의 baseurl을 repo이름으로 바꾼다.
나의 경우 baseurl:/blog/가 되는 것이다!

그래도 잘 모르겠다면
<https://stackoverflow.com/questions/25466166/stylesheets-not-working-for-jekyll-theme-freelancer-bootstrap>
을 참고해보자^.^


----2019.09.06 추가----
보통 baseurl은 ""나 /를 통해 공백으로 두는 경우가 대부분인데, 나와 같이 repo의 이름으로 작성해야하는 경우는
블로그가 프로젝트 주소 페이지로 생성되었기 때문이다.

(나는 구글이 시키는 대로 했는데 언제 프로젝트로 생성된건지 모르겠다..)

<https://nolboo.kim/blog/2013/10/15/free-blog-with-github-jekyll/>
를 참고하면 github블로그 주소 생성 방식이 USERNAME.github.io 형태나 USERNAME.github.io/repo 형태가 있다는 것을 확인할 수 있다.
