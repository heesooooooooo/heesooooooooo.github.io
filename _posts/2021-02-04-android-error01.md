---
layout: post
cover: 'assets/images/cover4.jpg'
navigation: True
title: "[Android/error] ListView Item에 중복된 값이 나오는 문제 해결"
date: 2021-02-04 00:00:00
tags: android
subclass: 'post android'
logo: 'assets/images/ghost.png'
author: heesoo
categories: android
---


### 🤦‍ 문제
- Adapter를 이용하여 ListView에 item을 띄우는 과정에서, 화면을 스크롤하면 밑에 있는 값들이 제대로 안나옴
- List에는 제대로 들어가는데, 화면에 이상하게 띄우는 듯


### 💃 해결

{% highlight java linenos %}
 @Override
    public View getView(int position, View view, ViewGroup viewGroup){
        GroupItem item= groupItems.get(position);
        if(view==null){
            View itemView=layoutInflater.inflate(R.layout.item_group, viewGroup, false);
            CircleImageView iv=itemView.findViewById(R.id.iv_group);
            TextView tvTitle=itemView.findViewById(R.id.tv_groupmsg);
            tvTitle.setText(item.getName());
            Glide.with(itemView).load(item.getProfile()).error(R.drawable.profile_default).into(iv);

            return itemView;
        }
        else{
            CircleImageView iv=view.findViewById(R.id.iv_group);
            TextView tvTitle=view.findViewById(R.id.tv_groupmsg);
            tvTitle.setText(item.getName());
            Glide.with(view).load(item.getProfile()).error(R.drawable.profile_default).into(iv);
            return view;
        }
    }
}

{% endhighlight %}

- 이런식으로 view가 null인지 아닌지로 나눠서 코드를 작성해야 한다.
- UI가 view를 reuse하기 때문에, view가 null이면 새로 만들고, 아니라면 기존 인스턴스에 item을 넣어야 된다고 한다.
  
### 참고
- Duplicated entries in ListView <https://stackoverflow.com/questions/8261376/duplicated-entries-in-listview>