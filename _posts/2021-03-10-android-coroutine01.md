---
layout: post
cover: 'assets/images/cover4.jpg'
navigation: True
title: "[Android] 코루틴(Coroutine) 정리"
date: 2021-03-08 00:00:00
tags: android
subclass: 'post android'
logo: 'assets/images/ghost.png'
author: heesoo
categories: android
---

코루틴의 핵심은 *light-weight thread*이다.  

### GlobalScope
{% highlight java linenos %}
import kotlinx.coroutines.* 
fun main() { 
    GlobalScope.launch { // launch new coroutine in background and continue 
        delay(1000L) // non-blocking delay for 1 second (default time unit is ms) 
        println("World!") // print after delay 
    } 
    println("Hello,") // main thread continues while coroutine is delayed 
    Thread.sleep(2000L) // block main thread for 2 seconds to keep JVM alive 
}

{% endhighlight %} 

- 위 예제는 GlobalScope에서 코루틴을 실행한 모습이다.
- launch는 coroutine의 Builder이며, 이를 이용해서 코드를 coroutineScope안에서 실행시킨다.
- GlobalScope의 lifetime은 전체 application process에 의존하므로, application이 종료되면 같이 끝나기 때문에, 끝에서 sleep을 걸고 기다려야 launch 내부 동작을 실행할 수 있다.
- 안드로이드에서는 sleep을 걸지 않아도 "Hello, World!"가 출력된다. Activity를 finish()하더라도 process 자체가 죽지 않기 때문에, sleep이 없어도 coroutine 내부 코드는 그대로 실행된다. 하지만 저가형 단말에서는 메모리 문제로 finish()와 함께 process가 kill될 수도 있기 때문에 반드시 coroutine 동작이 유지된다고 판단할 수도 없다.
- delay는 thread를 차단하지 않으며, coroutine에서만 사용할 수 있다.

### runBlocking
{% highlight java linenos %}
fun test2() { 
    runBlocking<Unit> { 
        launch { 
            delay(1000L) 
            Log.e(TAG, "World") 
        } 
        Log.e(TAG, "Hello") 
        delay(2000L) 
    } 
    Log.e(TAG, "End function") 
}

{% endhighlight %} 

- runBlocking을 만나면 main thread는 내부 코드가 완료될 때까지 block된다.
- Hello \n World \ End function 순으로 찍힌다.


{% highlight java linenos %}
GlobalScope.launch { // launch new coroutine in background and continue
    delay(1000L) // non-blocking delay for 1 second (default time unit is ms)
    Log.e("sequence","4") // print after delay
}

Log.e("sequence","1")

runBlocking { // but this expression blocks the main thread
    delay(4000L) // ... while we delay for 2 seconds to keep JVM alive
    Log.e("sequence","3")
}

Log.e("sequence","2")

{% endhighlight %} 

- 위 코드는 1 4 3 2 순으로 찍힌다.
- *GlobalScope의 delay는 main thread를 막지 않고, runBlocking은 막는다*

### join()
- 다른 코루틴이 작동하는 동안 delay하는 것은 좋지 않은 방법이다.
- 백그라운드 작업이 완료될 때까지 join()을 사용하여 명시적으로 기다려야 한다.

{% highlight java linenos %}
fun main() = runBlocking {
    val job = GlobalScope.launch { // launch new coroutine and keep a reference to its Job
    delay(1000L)
    println("World!")
    }
    println("Hello,")
    job.join() // wait until child coroutine completes
}

- 
{% endhighlight %} 
##### 참고
- [Kotlin] 코틀린 - 코루틴#1 기본! <https://tourspace.tistory.com/150>
- 안드로이드 Kotlin Coroutines 정리 Part 1 <https://eso0609.tistory.com/73>