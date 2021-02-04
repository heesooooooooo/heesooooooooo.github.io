---
layout: post
cover: 'assets/images/cover4.jpg'
navigation: True
title: "[Android/STOMP] STOMP 사용"
date: 2021-02-04 00:00:00
tags: android
subclass: 'post android'
logo: 'assets/images/ghost.png'
author: heesoo
categories: android
---

Simple Text Oriented Messaging Protocol

## 👊 Setting
``` java
repositories { 
    jcenter()
    maven { url "https://jitpack.io" }
}
dependencies {
    implementation 'com.github.NaikSoftware:StompProtocolAndroid:1.6.6'
}
```
<https://github.com/NaikSoftware/StompProtocolAndroid>를 이용하였다.



### 1. STOMP 생성 및 Connect

{% highlight java linenos %}
private StompClient stompClient;
private List<StompHeader> headerList;

public void initStomp(){
    stompClient= Stomp.over(Stomp.ConnectionProvider.OKHTTP, wsServerUrl);

    stompClient.lifecycle().subscribe(lifecycleEvent -> {
        switch (lifecycleEvent.getType()) {
            case OPENED:
                Log.d(TAG, "Stomp connection opened");
                break;
            case ERROR:
                Log.e(TAG, "Error", lifecycleEvent.getException());
                if(lifecycleEvent.getException().getMessage().contains("EOF")){
                    isUnexpectedClosed=true;
                }
                break;
            case CLOSED:
                Log.d(TAG, "Stomp connection closed");
                if(isUnexpectedClosed){
                    /**
                        * EOF Error
                        */
                    initStomp();
                    isUnexpectedClosed=false;
                }
                break;
        }
    });

    // add Header
    headerList=new ArrayList<>();
    headerList.add(new StompHeader("Authorization", G.accessToken));
    stompClient.connect(headerList);
}

{% endhighlight %}

- 먼저 stomp.over()로 stompClient를 생성한다.
- 첫 번째 파라미터는 connection이다. 현재 OkHttp와 JWS가 가능하다.
- 두 번째 파라미터는 연결하고자 하는 서버 주소이다.
- 다음은 stompClient의 lifeCycle에 따라 필요한 조건이 있을 경우 선언한다.
- 헤더에 추가해야할 것이 있다면 StompHeader()를 이용해서 만든다. 
- 이제 connect()로 연결!
  
#### ⭐⭐주소는 ws
- 연결하려는 주소가 http로 시작하면 ws, https는 wss로 시작하며, 마지막에 /websocket을 꼭 붙여야 한다.
- connect에 성공하면 101을 받는다.


### 2. Send, Subscribe

{% highlight java linenos %}
/**
* 보낼 JSONObject 만들기
* */
stompClient.send("send할 주소", jsonObject.toJSONString()).subscribe();
        

stompClient.topic("subscribe할 주소").subscribe(topicMessage -> {
    JSONParser parser=new JSONParser();
    Object obj=parser.parse(topicMessage.getPayload());
    ...
});
{% endhighlight %}

- tooooooo easy!0!
- send()할때 뒤에 .subscribe() 잊지 말자.
