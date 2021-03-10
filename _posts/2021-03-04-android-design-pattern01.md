---
layout: post
cover: 'assets/images/cover4.jpg'
navigation: True
title: "[Android] 디자인 패턴 정리"
date: 2021-03-04 00:00:00
tags: android
subclass: 'post android'
logo: 'assets/images/ghost.png'
author: heesoo
categories: android
---


다양한 문제 상황에 대한 재사용 가능한 해결책  
Creational(창조), Structural(구조), Behavioral(행동)으로 구분된다.

### Creational Pattern
- 클래스의 인스턴스를 만드는 것과 관련이 있다.  

*Singleton Pattern*
- 싱글톤 패턴이란, 하나의 인스턴스를 하나만 생성한다는 뜻이다. 
- 하나의 클래스에 대해 최초 한 번만 메모리에 할당한다.  
- 고정된 메모리 영역에 하나의 인스턴스만을 생성한다.

{% highlight python linenos %}
public class Single{
    private static Single instance=null;
    
    private Single(){}

    public static Single getInstance(){
        if(instance==null){
            instance=new Single();
        }
        return instance;
    }
}
{% endhighlight %}  

**장점**
- 고정된 영역에 인스턴스를 하나만 생성하기 때문에 메모리 낭비를 방지한다.
- 인스턴스를 전역적으로 사용할 수 있다. -> 다른 클래스의 인스턴스들과 데이터를 공유하고 변경할 수 있다.
**단점**
- 싱글톤 인스턴스에게 많은 일을 위임하면 다른 클래스의 인스턴스간에 결합도가 높아진다. -> 개방폐쇄원칙 위배
- 멀티스레드 환경에서 데이터 동기화 문제가 발생할 수 있다. -> synchroized 키워드 이용

### Structural Pattern
- 클래스와 인스턴스의 관계를 조정하고 맞춰가는 것이 목적이다.  

*Adapter Pattern*
- 상호 접근이 불가한 객체들간에 연결해주는 역할을 한다.
- 클래스나 인스턴스의 관계를 조정하고 구조를 짜맞추는 패턴들이 포함된다.
- MainActivity의 RecyclerView와 ViewHolder의 itemView는 상호 접근이 불가한 객체이다. AdapterSample 클래스를 구현하여 Adapter 패턴을 구현하여 두 객체를 연결해준다.

{% highlight java linenos %}
public class AdapterSample extends RecyclerView.Adapter<AdapterSample.ViewHolder> {
    private ArrayList<String> mData = null;
    public class ViewHolder extends RecyclerView.ViewHolder {
        TextView tv_main;
        public ViewHolder(@NonNull View itemView) {
            super(itemView);
            tv_main = itemView.findViewById(R.id.tv_main);
        }
    }
    public AdapterSample(ArrayList<String> list){
        mData = list;
    }
    @NonNull
    @Override
    public ViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        Context context = parent.getContext();
        LayoutInflater inflater = (LayoutInflater) context.getSystemService(
                Context.LAYOUT_INFLATER_SERVICE);
        View view = inflater.inflate(R.layout.rv_item_main, parent, false);
        AdapterSample.ViewHolder vh = new AdapterSample.ViewHolder(view);
        return vh;
    }
    @Override
    public void onBindViewHolder(@NonNull ViewHolder holder, int position) {
        String text = mData.get(position);
        holder.tv_main.setText(text);
    }
    @Override
    public int getItemCount() {
        return mData.size();
    }
}
{% endhighlight %} 

### Behavior Pattern
- 클래스와 인스턴스가 동작하는 방식이나 소통하는 방식을 다루는 패턴이다.

*Template Method*
- 어떤 동작의 알고리즘을 단위 기능 모듈로 분류하고 이들간의 동작 순서를 정의한다. 그리고 나서 단위 기능을 바로 구현하는 것이 아니라, 몇몇은 그 클래스를 상속할 자식 클래스에 위임한다.

{% highlight java linenos %}
public class AutoCar {
    public void playWithOwenr(){
        Log.e("on","시동켜기");
        Log.e("side_break","해제");
        Log.e("start","D");
        Log.e("operation","자동");
        Log.e("break","브레이크");
    }
}

public class ManualCar {
    public void playWithOwenr(){
        Log.e("on","시동켜기");
        Log.e("side_break","해제");
        Log.e("start","2단");
        Log.e("operation","수동");
        Log.e("break","브레이크");
    }
}
{% endhighlight %} 
- AutoCar와 ManualCar는 start, operation를 제외하고 로그가 같다. 이것을 Template Method를 적용할 수 있다.


{% highlight java linenos %}
public abstract class Car {
    public void playWithOwner(){
        Log.e("on","시동켜기");
        Log.e("side_break","해제");
        play();
        stopBreak();
    }
    abstract void play();
    void stopBreak() {
        Log.e("break","브레이크");
    }
}

public class AutoCar extends Car{
    @Override
    void play() {
        Log.e("start","2단");
        Log.e("operation","수동");
    }
    @Override
    void stopBreak() {
        super.stopBreak();
        Log.e("break","강력한 브레이크");
    }
}

public class ManualCar extends Car{
    @Override
    void play() {
        Log.e("start","2단");
        Log.e("operation","수동");
    }
    @Override
    void stopBreak() {
        super.stopBreak();
        Log.e("break","부드러운 브레이크");
    }
}

{% endhighlight %} 
- 상위 클래스에게 공통적인 로직은 템플릿 메소드로 두고, 구체적인 클래스에서 스타일에 맞게 구현을 강제하기 위해 추상메소드를 사용하고, Hook 메소드를 두는 패턴을 템플릿 메소드 패턴 (Template Method Pattern) 이라고 한다. 

##### 참고
- [Android] 디자인패턴 1 - 디자인패턴이란? <https://jroomstudio.tistory.com/20>