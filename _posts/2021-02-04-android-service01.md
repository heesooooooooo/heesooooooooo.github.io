---
layout: post
cover: 'assets/images/cover4.jpg'
navigation: True
title: "[Android] Service 정리"
date: 2021-02-04 00:00:00
tags: android
subclass: 'post android'
logo: 'assets/images/ghost.png'
author: heesoo
categories: android
---

### Service
- FG, BG, binder
- back으로 종료하는 경우 service는 살아있고, overview로 종료하면 서비스가 종료된다고 알고 있다.
- startService로 실행한 서비스는 (중간에 bindService 진행해도) stopService로 종료해야 끝난다.
- bindSerivce로 실행하면 unbindService로 종료한다.
- 참고로 bindService는 바인딩이 모두 해제되어야 종료된다.
- startService는 onStartCommand()를 진행, bindService는 바로 onBind() 진행한다.
- 보통 service 호출 및 종료를 같은 액티비티(프래그먼트)에서 진행하지 않는데, 확실하게 죽이고 싶으면 같은 곳에서 하는게 좋다.

### stopService가 onDestroy()를 호출하지 않을 때
{% highlight java linenos %}
getActivity().bindService(LoginActivity.getPushIntent(), mConnection, Context.BIND_ADJUST_WITH_ACTIVITY);
{% endhighlight %}
- flag를 Context.BIND_AUTO_CREATE가 아니라 위의 값으로 변경
- 근데 같은 코드지만 디바이스마다 onDestory()가 호출되는 시간이 달라보였다🤷‍♂️.