---
title: jekyll 번외 - 댓글 생성하기
layout: post
date: '2019-12-26 23:00:00'
excerpt: 댓글 기능 생성
tag:
- jekyll
- github
- disqus
category: jekyll
comments: true
---

---

## **1. 댓글창 만들기**

분명 theme에 댓글 기능이 있다고 들었는데 댓글이 작동하지 않았다.<br>알고 보니 댓글은 [disqus](https://disqus.com/) 에서 제공해주는 것이었다.<br>서둘러 가입해서 댓글기능을 활성화시켰다.

---

<br>

## 2. disqus 가입 및 활성화

글은 너무 시간낭비 같아서 최대한 사진으로 대체한다.

<figure>
    <a href="\posts_image\jekyll_guide\disqus_sign.JPG"><img src="\posts_image\jekyll_guide\disqus_sign.JPG"></a>
    <figcaption><center><b>가입한 e-mail로 인증 날아온다.</b></center></figcaption>
</figure>

<figure>
    <a href="\posts_image\jekyll_guide\disqus_start.JPG"><img src="\posts_image\jekyll_guide\disqus_start.JPG"></a>
    <figcaption><center><b>install Disqus 들어가기</b></center></figcaption>
</figure>

다음 창에서는 이름 적당히 정하고 카테고리 고른 후 만들기 누르면 된다.<br>~~중국, 일본, 베트남 다 있는데 Korean이 안 보인다. 남들은 한글버전 쓰던데~~

<figure>
    <a href="\posts_image\jekyll_guide\disqus_price.JPG"><img src="\posts_image\jekyll_guide\disqus_price.JPG"></a>
    <figcaption><center><b>그냥 basic 고르자</b></center></figcaption>
</figure>

다음 창에서는 jekyll 고르면 된다.<br>

<figure>
    <a href="\posts_image\jekyll_guide\disqus_install.JPG"><img src="\posts_image\jekyll_guide\disqus_install.JPG"></a>
    <figcaption><center><b>인스톨 사항</b></center></figcaption>
</figure>

필요한 layout에 `comments: true`를 달아주면 default 값으로 사용할 수 있다.<br>2번에 `Universal Embed Code`라고 나온 부분은 잘 모르겠다.<br>따로 설치해야하는 건가;;

<figure> 
    <a href="\posts_image\jekyll_guide\disqus_set.JPG"><img src="\posts_image\jekyll_guide\disqus_set.JPG"></a>
    <figcaption><center><b>자기 블로그 주소 입력</b></center></figcaption>
</figure>

<span style="color:#ff0000">여기서는 주의할 점이 있다.</span><br>`_config.yml` 에 적었던 url 그대로 적어야 한다.<br>나는 https://tinygun.github.io 에다 `/` 를 붙여서 적었는데, <br>알고 보니 내 주소는 마지막 `/` 가 없어서 댓글 활성화가 안 됐다.

<figure> 
    <a href="\posts_image\jekyll_guide\disqus_set.JPG"><img src="\posts_image\jekyll_guide\disqus_set.JPG"></a>
    <figcaption><center><b>결국 댓글 활성화</b></center></figcaption>
</figure>

---

<br>

## 3. 마치며

jekyll 에서 disqus 댓글창을 달아주는 작업을 했다.<br>나중에 Korean이 내 눈에 보이면 바로 수정해줘야겠다.



---

<br>

#### 참고

- [https://17billion.github.io/jekyll/disqus/reply/2017/06/01/jekyll_disqus.html](https://17billion.github.io/jekyll/disqus/reply/2017/06/01/jekyll_disqus.html)



