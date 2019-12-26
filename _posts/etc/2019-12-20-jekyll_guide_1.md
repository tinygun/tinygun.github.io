---
layout: post
title: 블로그 갖고 싶어 시작한 jekyll(1)
date: '2019-12-20 22:15:00'
excerpt: "Github 블로그 갖기 위한 여정"
tag:
- jekyll 
- github
category: etc
comments: true
---

## **1. github 블로그 시작**

공부하다 보니 코드를 보기 쉽게 정리해둘 필요성을 느꼈다.<br>주석을 간단히 달아두는 정도로는 보기 너무 불편했다.<br>그래서 생각한 것이 블로그였다.<br>하지만 기존 블로그들은 뭔가 마음에 들지 않았을 때 보인 블로그 형식이 바로,<br>

**"github.io"** <br>

뭔가... 간지나는 저런도메인을 쓰고 싶었다. <br>

처음엔 n\*ver나 d\*um처럼 개인 공간 주고 꾸미는 줄 알았다. <br>얼핏 듣기로도 만들기<span style="color:#D0D0D0">~~만 하는 거~~</span>는 쉽다고 들었다.<br>

하지만 내가 직접 호스팅하는 것이었고...<br>그렇게 블로그 제작의 수렁으로 빠졌다.<br><span style="color:#F0F0F0">~~쉽다고 한 사기꾼 잡아와라 이 놈들아~~</span>

---

<br>

## **2. jekyll 시작하기에 앞서서...**

github에 호스팅하는 것은 여러 방법이 있는 듯 하다.<br>**jekyll**은 그 중 하나다. ~~이거면 블로그 엄청 편하게 만들 수 있겠지? 응 아니야~~<br>[한국어 jekyll 설명서](https://jekyllrb-ko.github.io/docs/home/)를 보면서 해보면 어느 정도 쉽게(?) 만들 수 있다.

<figure>
    <a href="/posts_image/jekyll_guide/simplejekyll.JPG"><img src="/posts_image/jekyll_guide/simplejekyll.JPG"></a>
    <figcaption><center><b>그렇다. 아주 심플하게 날 속였다.</b></center></figcaption>
</figure>



나는 Windows 10 에서 만들고자 한다.<br>~~Mac이나 Linux였다면 좀 더 쉬웠을까? 아니, Ruby도 모르는 주제에 쉬운게 어딨음~~<br>필요한 절차는 다음과 같다.<br><br>

(1) [Ruby, Ruby+Devkit](https://rubyinstaller.org/downloads/) 다운 받기<br>

(2) **`Start Command Prompt with Ruby`**를 윈도우 검색창에 치고 관리자 권한으로 실행<br>

(3) 다음 코드 실행

```ruby
chcp 65001 
gem install bundler jekyll 
jekyll new my-awesome-site 
cd my-awesome-site
bundle exec jekyll serve

###### 코드 설명 ######
# 1. 혹시 모를 인코딩 오류를 방지하기 위한 코드
# 2. bundler, jekyll 받기
# 3. 작업중인 디렉토리에서 "my-awesome-site"라는 폴더 만들고 거기다 설정파일 생성.
## cd / 치고 드라이브 root에서 입력하는 것이 왔다갔다하기 편하다.
# 4. 만든 폴더로 들어가기
# 5. jekyll 실행.
```



bundle을 이용하지 않고 jekyll serve만 실행하면 각종 dependency에 미쳐버린다.<br>(주로 매우 구버전에서 만든 것에서 호환성 문제가 많은 듯하다. 가끔 bundle도 답이 없다.)<br>

위와 같이 실행한 후, [127.0.0.1:4000](http://127.0.0.1:4000)로 들어가거나 [localhost:4000](http://localhost:4000/)로 들어가면 기본 화면이 나온다.

<figure>
    <a href="/posts_image/jekyll_guide/jekyll_base.JPG"><img src="/posts_image/jekyll_guide/jekyll_base.JPG"></a>
    <figcaption><center><b>Jekyll이 잘 작동중이다</b></center></figcaption>
</figure>



---

<br>

## **3. 게시판을 만들어보자...?**

<span style="color:#D0D0D0">~~하지만 이게 시작일 줄이야~~</span><br>

글들을 정리해서 넣기 위해 각종 게시판을 만들려고 했다.<br>그런데, 아무리 봐도 내가 생각하던 블로그가 아니다.<br>게시판을 만들 수 있는 관리자 링크가 없다.<br>아니, 애초에 글쓰기 부분도 없다. <br>

이를 어찌하면 좋은가?

---

<br>

## **4. 글이라도 써보기**

우선 첫 화면에 Welcome to Jekyll! 이라는 글처럼 하나 써보기라도 해야하지 않는가?<br>이건 그래도 markdown 문서를 사용하면 가능하다.<br>난 Typora 를 이용하고 있다. markdown 편집 프로그램은 여럿 있으니 편한거 이용하자.<br>jupyter notebook 덕에 markdown을 많이 사용했는데, 이게 제작진행에 도움이 되었다.<br>~~몰랐다면 여기서 끝냈을 텐데;; 모르는 게 약이었음을 나중에 알았다.~~

만든 폴더를 살펴보면 `_posts`라는 폴더가 보인다.<br>`yyyy-mm-dd-welcome-to-jekyll.markdown` 같은 이름의 문서가 들어있다. <br>이 문서 보면서 비슷하게 만들면 된다. <br>`YEAR-MONTH-DAY-title.md` 형식으로 파일을 만들어주면 포스트로 인식해준다.<br>파일 이름에 한글이 있으면 업로드가 안 되는데... <br>그냥 영문도 모른채 영문으로 작성했다.<br>

첫 줄을 보면 아래와 같은 부분이 있다. 필요한 부분을 수정하자.

```ruby
layout: post				# post에 올리기
title:  "Welcome to Jekyll!"		# 글 제목을 "" 사이에 넣기. 이건 한글도 괜찮다.
date:   2019-12-20 17:48:56 +0900	# 날짜. 시간은 안 넣어도 괜찮다.
categories: jekyll update		# 카테고리. 당장은 수정하는 느낌이 안 난다.
```

~~난 여기까지 하고 포기했어야 했다고 생각한다. 분명 목적은 공부한 코드 보기 쉽게 정리하려던 건데 왜 홈피 제작을 하고 있지? ㅠㅠ~~

---

<br>

## **5. 꾸미는 방법들**

템플릿은 Liquid 라는 언어로 구성되어 있다고 한다. ~~Liquid는 또 뭐야;;~~<br>그럼 이걸로 바로 만들면 되겠지만...<br>내 스펙으로 웹문서 작성은 너무 힘들다.<br>

| 휘황찬란한 스펙                                              |
| :----------------------------------------------------------- |
| 1. **`중학교 컴퓨터 시간`**에 메모장 같은 기본 툴로도 복잡한 html 문서작성 <br>2. head, body, a color 등 매우 다양한 태그를 이용하여 **`"Hello world!" 작성 가능`**<br>3. **`10번 정도`** 밀도높은 크롤링을 통한 너무나도 높은 태그 이해도 |

이 정도면 Web 세계에서 강력범으로 즉각 수배될 것이다.<br><br>

### **5-1. jekyll admin 이용하기**

[jekyll admin 설명 원문](https://github.com/jekyll/jekyll-admin/blob/master/README.md)

jekyll은 나같은 범죄자에게도 사회생활의 기회를 주었다.<br>무려 관리 페이지를 만들어주는 기능이다.<br>ruby 콘솔에 `gem install jekyll-admin` 쳐서 다운받자.<br>그 다음, 만든 폴더 내부에 파일 확장자 없이 `Gemfile`이라고만 써있는 것이 하나 있다.<br>얘를 열어서 마지막 줄에 `gem 'jekyll-admin', group: :jekyll_plugins` 를 추가한다.<br>ruby 콘솔 창에 `bundle install` 쳐주면 설치는 완료되었다.<br>

작동법은 다음과 같다. <br>

(1) 아까처럼 `bundle exec jekyll serve`을 ruby 콘솔창에 쳐서 서버 작동.<br>

(2) `http://localhost:4000/admin` 으로 접속.



<figure>
    <a href="/posts_image/jekyll_guide/jekyll_admin.JPG"><img src="/posts_image/jekyll_guide/jekyll_admin.JPG"></a>
    <figcaption><center><b>뭔가 그럴싸한 관리페이지가 생겼다</b></center></figcaption>
</figure>

글 쓰는 부분이나 페이지 만드는 부분들이 있다. 보통 블로그보단 불친절하다.<br>글은 markdown 편집기로 쓴 다음, 시간같은 메타데이터를 여기서 변경하면 좋아보인다.<br>

~~여기까지 하는데 5시간 넘게 걸렸네;;~~<br>~~괜찮다. 어차피 최소 5일 할 건데 뭘~~<br><br>

### **5-2. jekyll theme 이용하기**

jekyll은 [각종 테마를 제공](http://jekyllthemes.org/)하고 있다.<br>다른 성인군자들께서 만든 것을 공유하는 듯하다. 마음에 드는 테마 들어가서 다운로드 받자.<br>

다운로드 말고 `Fork`를 통해 바로 github에 집어넣는 방법도 있다.<br>하지만, 나는 로컬에서 돌려보는 것이 목적이라 바로 다운로드 받았다.

압축 해제한 폴더로 ruby 콘솔로 들어간 후, 아까처럼 `bundle exec jekyll serve` 치자.<br>(다시 bundle install을 해도 몇 개는 작동되지 않았다. 안되는 거는 버리자. 몇 시간을 여기다 버렸다.)<br>폴더에 들어가서 `_config.yml` 파일과 다른 파일들을 수정하면 거의 호스팅 완료 단계까지 만들어진다.<br>

그러나, 맹점이 하나 있다. 수정이 어렵다.<br>내 스펙이 증명하듯, 수정하려면 뭘 알아야 하는데 모르니까 너무 힘들었다.<br>많이 쓰는 테마들은 맘에 안들고,<br>별로 안 쓰는 테마들은 친절한 수정 설명이 부족했다.<br>

결국, 내가 사용하고 싶은 테마의 기능을 만들기 위해서는 다른 사람들 github 파일이 어찌 다른지 확인하는 편이 빨라보였다.<br>~~jupyter notebook 간단한 색깔 설정 바꾸는 것도 css 잘 몰라서 2일 걸렸는데~~<br>

다음을 기약하며 빠른 수면을 택했다. <br>

<span style="color:#F0F0F0">~~다시 말하지만 쉽다고 한 사기꾼 잡아와라 이 놈들아~~</span>

