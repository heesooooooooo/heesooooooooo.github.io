---
layout: post
cover: 'assets/images/cover4.jpg'
navigation: True
title: "[Android/WebRTC] WebRTC 세팅 및 sdp, candidate 생성"
date: 2021-01-31 00:00:00
tags: android
subclass: 'post android'
logo: 'assets/images/ghost.png'
author: heesoo
categories: android
---

### 👊 Setting
``` java
implementation 'org.webrtc:google-webrtc:1.0.+'
```
로 추가 가능하나, 블로그 대부분의 예시가 arr파일로 되어있어서 나도 arr 파일을 사용하였다 😎 
- 아래 코드는 audio를 가져오는 코드로, video도 생성하고 싶은 경우 VideoTrack을 이용하면 쉽게 구현할 수 있다.



### 1. WebRTC 세팅 

{% highlight java linenos %}
<uses-feature android:name="android.hardware.camera" />
<uses-feature android:name="android.hardware.camera.autofocus" />
<uses-feature
    android:glEsVersion="0x00020000"
    android:required="true" />

<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.CHANGE_NETWORK_STATE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
<uses-permission android:name="android.permission.CAMERA" />
{% endhighlight %}

- 먼저, 필요한 permission을 AndroiMenifest.xml에 추가한다.
- 다음으로, WebRTC에 필요한 생성 및 초기화를 진행한다.  

{% highlight java linenos %}
import java.util.*;
/**
 *
 * @author HEESOO
 *
 */
public void initWebRTC(){
    PeerConnectionFactory.initializeAndroidGlobals(this, true, false, true);

    PeerConnectionFactory.Options options = new PeerConnectionFactory.Options();
    peerConnectionFactory = new PeerConnectionFactory(options);

    audioConstraints = new MediaConstraints();
    audioSource = peerConnectionFactory.createAudioSource(audioConstraints);
    localAudioTrack = peerConnectionFactory.createAudioTrack("101", audioSource);
    localAudioTrack.setEnabled(true);

    sdpConstraints = new MediaConstraints();
    sdpConstraints.mandatory.add(new MediaConstraints.KeyValuePair("offerToReceiveAudio", "true"));
    sdpConstraints.mandatory.add(new MediaConstraints.KeyValuePair("offerToReceiveVideo", "false")); 

    stream = peerConnectionFactory.createLocalMediaStream("102");
    stream.addTrack(localAudioTrack);
}
{% endhighlight %}

- audioSource를 생성하기 위해 PeerConnectionFactory을 생성 및 초기화한다.
- audioConstraints를 통해 audio를 생성하고, localAudioTrack으로 audio를 setEnable한다.
- sdpConstraints는 SDP 정보를 생성하는 것으로, 연결하고자 하는 Peer간의 미디어와 네트워크에 관한 정보를 이해하기 위해 사용된다.
- 마지막으로 stream을 통해 생성한 audioTrack을 넣으면 WebRTC 초기 세팅이 끝난다.


### 2. SDP와 IceCandidate 전달

- 두 Peer가 서버와 연결되면, 같은 dest를 가지고 있는 Peer들은 sdp와 candidate를 교환해야 한다.
- SDP(Session Description Protocol)은 미디어 정보를 서로 교환한다.
- sdp는 (연결하고자 하는) A가 만든 sdp(createOffer())와, 상대방(A)의 sdp를 받고 제대로 받았음을 send할 B의 sdp(createAnswer())가 필요하다.

#### createOffer()
{% highlight java linenos %}
public void setPeer(){
    ...
    if(initCall) {
            newPeer.createOffer(new CustomSdpObserver("newPeerCreateOffer") {
                @Override
                public void onCreateSuccess(SessionDescription sessionDescription) {
                    super.onCreateSuccess(sessionDescription);
                    createdDescription(sessionDescription, peerUuid);
                }
            }, sdpConstraints);
        }
    ...
}
{% endhighlight %}

- createOffer()로 sdp를 생성하면, onCreateSuccess()의 파라미터 sessionDescription으로 생성된 sdp를 확인할 수 있다. 이제 생성된 sdp를 서버로 전송해주면 된다.

{% highlight java linenos %}
public void createdDescription(SessionDescription sessionDescription, String peerUuid){
    Log.i(TAG, "createdDescription");

    CustomPeerConnection tempPeer= peerMap.get(peerUuid);
    tempPeer.pc.setLocalDescription(new CustomSdpObserver("createdDescription"), sessionDescription);

    /**
      * createOffer()한 sdp를 서버로 전송
      */
}
{% endhighlight %}

#### createAnswer()
{% highlight java linenos %}
tempPeer.pc.createAnswer(new CustomSdpObserver("offer createAnswer") {
    @Override
    public void onCreateSuccess(SessionDescription sessionDescription) {
        super.onCreateSuccess(sessionDescription);
        tempPeer.pc.setLocalDescription(new CustomSdpObserver("offer setLocal"), sessionDescription);
        createdDescription(sessionDescription, peerUuid);
    }
}, new MediaConstraints());
{% endhighlight %}

- 상대방의 sdp를 제대로 받았음을 전송하는 sdp이다. 
- sdp 교환이 끝나면, Ice Candidates를 교환한다.

### onIceCandidate()
{% highlight java linenos %}
PeerConnection newPeer=peerConnectionFactory.createPeerConnection(iceServerList, sdpConstraints,
    new CustomPeerConnectionObserver("newPeerCreation") {

        @Override
        public void onIceCandidate(IceCandidate iceCandidate) {
            Log.i(TAG, "onIceCandidate");
            super.onIceCandidate(iceCandidate);
            /**
                * 생성된 iceCandidate(파라미터 값)을 서버로 전송
                */
            }
        }

        @Override
        public void onAddStream(MediaStream mediaStream) {
            super.onAddStream(mediaStream);
            gotRemoteStream(mediaStream);
        }
    }
});

newPeer.addStream(stream);
        
{% endhighlight %}

- onAddStream이 호출되면 Peer 연결에 성공했다는 뜻이다.
- A와 B가 iceCandidate 교환에 성공하면, connection되어 audio 전송이 가능해진다.

### 4. 참고
#### CustomSdpObserver.java
{% highlight java linenos %}
class CustomSdpObserver implements SdpObserver {

    private String tag = this.getClass().getCanonicalName();

    CustomSdpObserver(String logTag) {
        this.tag = this.tag + " " + logTag;
    }


    @Override
    public void onCreateSuccess(SessionDescription sessionDescription) {
        Log.d(tag, "onCreateSuccess() called with: sessionDescription = [" + sessionDescription + "]");
    }

    @Override
    public void onSetSuccess() {
        Log.d(tag, "onSetSuccess() called");
    }

    @Override
    public void onCreateFailure(String s) {
        Log.d(tag, "onCreateFailure() called with: s = [" + s + "]");
    }

    @Override
    public void onSetFailure(String s) {
        Log.d(tag, "onSetFailure() called with: s = [" + s + "]");
    }

}
{% endhighlight %}

- CustomSdpObserver는 SdpObserver를 implement하여, SDP와 관련된 이벤트들을 체크한다.
- SdpObserver가 필요한 곳에 override method를 다 써줄 순 없으니까 Custom class를 하나 생성하여, 필요한 메소드가 있을 경우 override해준다.

#### 음량 조절 기능
{% highlight java linenos %}
public void muteAudio(boolean action){
    Log.i(TAG, "muteAudio");
    stream.audioTracks.get(0).setEnabled(action);
}


public void setVolume(long uid, int volume){
    Log.i(TAG, "setVolume "+uid+" "+volume);
    audioMap.get(uid).setVolume((double)volume);
}
{% endhighlight %}

- audio, video는 MediaStream에서 관리한다.
- initWebRTC()에서 audio, video를 세팅해주고 addTrack()으로 나의 audio 데이터를 담았다.
   
- 참고로, MediaStream에는 audioTracks와 videoTracks가 존재한다(둘다 LinkedList).
- 그래서 내 audio는 media.audioTrack.get(0)이 된다.
- 음소거 기능은 setEnabled()로 관리하며, false가 음소거이다.
    
- audio는 LinkedList형태로 peer들이 들어올 때마다 뒤에서 삽입되므로 해당 인덱스를 찾는다면 특정 peer의 음량을 조절할 수 있다.
