---
title: 블로그 갖고 싶어 시작한 jekyll(3)
layout: post
date: '2019-12-26 12:00:00'
excerpt: 블로그 호스팅 절차 및 Jekyll 검색기능 등 추가 기능 생성.
tag:
- jekyll
- github
category: etc
comments: true
---

---

## **1. 블로그 호스팅**

구글 기능들을 블로그에 박기 위해 세상에 드러낼 필요가 있다.<br>이런 저질 블로그를 올려도 되는지 걱정될 수 있으나, 걱정할 필요가 없다.<br>구글에서 검색이 가능하려면 [추가적 절차](https://wayhome25.github.io/etc/2017/02/20/google-search-sitemap-jekyll/)가 필요하다.<br>이건 그냥 호스팅일 뿐.

우선 github에 자신 주소로 사용할 이름의 저장소를 만들자.<br>나는 **[tinygun.github.io](https://github.com/tinygun/tinygun.github.io)** 라고 만들었다.<br>이렇게 해야 호스팅이 된다고 한다.<br>미리 이름을 정하고 만들면 404 뜨는 듯하다.

원래 귀찮아서 github에 드래그로 옮길라고 했다.<br>그런데 파일이 100개 이상이라서 안 올라간다.<br>나중에 관리상 문제도 있어서 [git](https://gitforwindows.org/) 으로 올린다.

---

<br>

## **2. 간단한 git 사용법**

다른 분들 검색하면 잘 나오긴 하지만, 그냥 편하게 여기다 쓴다.<br>완전 처음일 경우를 가정한다.

Windows 기준으로 [git bash](https://git-scm.com/) 실행 후, 블로그 작업 폴더에 들어가서 다음과 같이 실행한다.

```markdown
git init
git add .

git config --global user.name "username"
git config --global user.email "username@example.com"

git commit -m "first commit"
git remote add origin "https://github.com/username/username.github.io.git"

git config --global http.postBuffer 20971520

git push -u origin master
```

(1) git에 필요한 파일을 만든다.<br>(2) 자기 신원을 적어둔다.<br>(3) 저장소에 연결한다.<br>(4) 파일 전송 크기를 조정한다. (선택사항)<br>(5) github에 올린다.

<br>

나중에 파일을 수정하고 다시 올릴 때는 이러면 된다.

```markdown
git add .
git commit -m "수정내용"
git push -u origin master
```

확인해보면 github에 잘 올라가있다.<br>`_config.yml` 파일에서 자기가 지정한 주소로 접속하면 변경사항이 바로 안 뜰 수도 있다.<br>길게는 1분 정도 있어야 변경사항이 적용된다.

---

<br>

## 3. 검색기능 활성화

참고: [https://wayhome25.github.io/](https://wayhome25.github.io/)

이제 호스팅을 했으니 깡통으로 만든 검색창에 기능을 추가하고자 한다.

<br>

(1) 인증

구글에 검색이 가능하도록 **`인증`**을 먼저 진행한다.<br>[Search Console](https://search.google.com/search-console/welcome?hl=ko&utm_source=wmx&utm_medium=deprecation-pane&utm_content=home)로 들어가보자.

첫 화면 입력창에 자기 블로그 주소를 넣어주면  html을 받을 수 있다.<br>창은 나중에 꺼두자.<br>이걸 받고 github에 올려준 뒤 html 받던 창에서 확인을 눌러준다.

<br>

(2) sitemap 제출

그 후, `sitemap.xml` 파일을 만들어야 한다.<br>난 [이 분](https://github.com/wayhome25/wayhome25.github.io/blob/master/sitemap.xml) 것을 그대로 복사했다.<br>만들어 준 뒤 다시 github에 올려준다.<br>Search Console을 다시 들어가보면, 왼쪽에 `색인-Sitemaps` 부분이 있다.<br>여기 들어간 뒤 상단에 `sitemap.xml` 쓰고 사이트맵 추가하면 끝난다.

---

<br>

## 4. 검색기능 추가

드디어 검색기능을 활성화 시켰다.<br>이제 [구글 맞춤검색](https://cse.google.co.kr/cse/)으로 들어가보자.

<figure>
    <a href="\posts_image\jekyll_guide\googlesearch.JPG"><img src="\posts_image\jekyll_guide\googlesearch.JPG"></a>
    <figcaption><center><b>맞춤검색 엔진 사이트</b></center></figcaption>
</figure>

만들기 들어가서 자기 주소에 대한 검색엔진을 만들어보자.

검색엔진 수정을 통해 약간 수정한 뒤 `코드생성`이 있다.<br>해당 코드를 웹페이지에 추가하면 된다.<br>나는 layout 중 page를 기반으로 search의 index.html을 수정했다.

검색이 되려면, 좀 오래 걸린다고 들었다.<br>언젠가 될 때를 기약하자. 

가끔 블로그 업데이트가 안 될 때가 있다.<br>나는 html 파일이 맛이 가서 안 된다고 메일만 20개 넘게 온 듯하다.<br>1분 넘으면 뭔가 문제가 있는 것이니 해결하자.

---

<br>

## 5. 마치며

드디어 github에 블로그를 개설했다.<br>많이 힘들었지만, 어느 정도 해결할 수 있어서 좋았다.<br>많은 블로그들을 돌아다니면서 공부해서 좋았다.



---

<br>

## 그 동안 참고한 페이지들

#### 주요 참고

- [https://jekyllrb-ko.github.io/](https://jekyllrb-ko.github.io/): 지킬 공식 한국어 홈피.

- [https://github.com/TaylanTatli/Moon](https://github.com/TaylanTatli/Moon): 기본 테마.

- [https://nachwon.github.io/](https://nachwon.github.io/): 다중 nav 참고. 수록 포스팅 들도 괜찮았던 블로그.

- [https://wayhome25.github.io/](https://wayhome25.github.io/): search 기능 제작에 큰 도움.

  

  <br>

#### 살짝 참고

- [https://cropius.github.io/](https://cropius.github.io/): sns 아이콘 정리에 도움.
- [http://dmcwo.github.io/Notebook-Moon//](http://dmcwo.github.io/Notebook-Moon//): 폰트도 바꿔보면 좋겠다는 생각.
- [https://evasivepangolin.github.io/midtndsa//](https://evasivepangolin.github.io/midtndsa//): 홈 화면을 깔끔하게 만들어서 신선.
- [https://rodriguez-r.com/](https://rodriguez-r.com/): 최근 게시물을 올리는 것이 재미있어 보였다. 근데 나한텐 더러워보여
- [http://blog.knowgari.com/categories/Ajax/](http://blog.knowgari.com/categories/Ajax/): 카테고리 페이지를 따로 만들어서 좋아보였다. 근데 모바일에선 적용이 되질 않아서 가져오진 않았다.
- [http://blog.knowgari.com/categories/Ajax/](http://labs.brandi.co.kr/2018/05/14/chunbs.html): admin 기능을 소개해줬는데, 딱히 내게 많은 편의를 주는 기능은 아니었다. 저자 기능이 있으니 한 번 추가하고 싶다면 가도 좋다.
- [http://jihyeleee.com/blog/second-designer-can-make-jekyll-blog/](http://jihyeleee.com/blog/second-designer-can-make-jekyll-blog/): 내가 git을 잘 사용하지 못해서 나중에 더 참고하면 좋아보인다.

- 그 외 기본 테마에 [fork](https://github.com/TaylanTatli/Moon/network/members) 해서 내게 영감을 준 블로그들.

