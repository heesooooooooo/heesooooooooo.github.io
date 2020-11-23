---
layout: post
cover: 'assets/images/cover4.jpg'
navigation: True
title: "[Android/React-Native] Windows 설치 및 프로젝트 실행"
date: 2020-11-22 00:00:00
tags: android
subclass: 'post tag-etc'
logo: 'assets/images/ghost.png'
author: heesoo
categories: etc
---
## 설치

### 1. nvm과 node
- <https://github.com/coreybutler/nvm-windows/releases>에서 nvm-setup.zip 다운로드
- 압축해제 및 설치
- nvm install <버전>  
 
{% highlight java linenos %}
nvm install 10.16.2 // 설치
nvm use 10.16.2 // 활성화

// 버전 확인으로 제대로 설치되었는지 체크
node -v
npm -v
{% endhighlight %}
  
### 2. Java open jdk
- Java 유저라면 있을테니까 패스
- 버전 체크로 설치되어 있는지 체크
{% highlight java linenos %}
java -version
{% endhighlight %}
- 설치 후 환경변수(JAVA_HOME) 등록

### 3. Python
- <https://www.python.org/downloads/>
{% highlight java linenos %}
python --version
{% endhighlight %}
- 버전 체크로 설치 확인
- 나의 경우 구체적인 버전이 나오지 않고 Python이라고만 출력되었는데도 문제 X
  
### 4. React Native CLI
{% highlight java linenos %}
npm install -g react-native-cli
react-native --version //버전 체크
{% endhighlight %}

### 5. 안드로이드 스튜디오
- 기존에 있어서 설치 패스
- 설치 후 시스템 환경 변수 편집> 환경 변수> 시스템 변수> 새로 만들기
- 변수 이름: ANDROID_HOME   
  변수 값: 안드로이드 실행> File> Settings> Appearance&Behavior> System Setting> Android SDK 에 Android SDK Location
  ![이미지1](./assets/images/201122_2.PNG)
- 이번에는 시스템 변수 Path 편집
- 새로 만들기> 위에 변수값\platform-tools 추가
- cmd 켜놓았다면 다 끄고 재실행
{% highlight java linenos %}
adb // adb 명령어가 먹는지 확인
{% endhighlight %}

### 6. VS Code
- 이미 설치되어 있어서 패스

## 프로젝트 생성 및 실행

### 1. VS Code로 프로젝트 생성
- 상단에서 터미널 열기
{% highlight java linenos %}
cd <위치> // 프로젝트 생성할 폴더로 이동
react-native init <프로젝트 이름> //프로젝트 생성
{% endhighlight %}
- 파일> 폴더 열기로 생성한 프로젝트 불러오기

### 2. 안드로이드 에뮬레이터 또는 디바이스 실행
- 안드로이드 스튜디오 실행
- 에뮬레이터를 사용하려고 한다면 미리 실행시켜 놓고 react를 돌려야 함
- 에뮬레이터 설치 안했다면 설치
- build.gradle Module의 targetSdkVersion과 맞는 에뮬레이터 설치
- AVD Manager에서 Create Virutal Device로 설치 가능
- 상단에서 AVD Manager 클릭후 Action에서 실행하여 에뮬레이터 구동
 ![이미지2](./assets/images/201122_3.PNG)
- 다시 vs 터미널
{% highlight java linenos %}
adb devices // 현재 연결된 디바이스와 에뮬레이터 확인 가능
npx react-native run-android // 실행
{% endhighlight %}