---
title: 블로그 갖고 싶어 시작한 jekyll(2)
layout: post
date: '2019-12-24 09:00:00'
excerpt: Jekyll 테마 "Moon" 적용, 게시판 생성 및 페이지 커스터마이징.
tag:
- jekyll
- github
category: jekyll
comments: true
---

---

## **1. Theme 설정**

좋은 테마를 찾았다. [Moon](https://github.com/TaylanTatli/Moon) 이라는 테마다.<br>좌측 상단에 게시판 링크를 고정한 것이 마음에 든다.<br>2019.12.24 기준으로 내 블로그 테마다.

테마를 받은 후, `_config.yml`이라는 파일을 변경해야 한다.<br>주로 바꿀 부분은 다음과 같다.

```markdown
title:	NLP Study
bio:	'Learn Korean NLP'
url:	127.0.0.1:4000
```

자기 블로그 간판, 설명, 주소를 적으면 된다.<br>지금은 jekyll 작동 시험중이라 주소를 로컬로 적어놨다.

왠지 모르겠는데 여기 한글이 들어가면 작동을 안한다.<br>로컬에서만 그러고 github에서는 괜찮은 듯하다.

`_config.yml` 파일은 어떤 테마든 다 존재하는 환경설정 파일이다.<br>이 파일의 수정내역은 jekyll 재시작 후 반영된다.<br>배경이나 사진이 안 바뀐다고 혼란스러워 하지 말자. ~~내가 혼란에 빠졌다.~~

p.s. <br>나는 Moon 테마로 jekyll을 돌리면 루비 콘솔에서

```ruby
BigDecimal.new is deprecated; use BigDecimal() method instead
```

 라고 자꾸 경고 메시지를 날렸다. <br>콘솔에 문제있다고 하는 파일 경로가 뜬다.<br>메모장 같은거로 파일을 열어서 `BigDecimal.new` 라고 된 부분에서 `.new` 부분을 지웠다.<br>jekyll을 재시작하니 다시는 나타나지 않았다.<br>루비를 잘 모르지만, 아마 루비 버전에 따른 명령어 변화가 아닐까 싶다.

---

<br>

## **2. 기능 추가 리스트**

이제 내가 원하는 기능을 추가할 것이다.<br>추가하고 싶은 리스트는 다음과 같다.

- 게시판
- 검색기능
- 사용자 집계
- ~~google 광고~~

우선 레이아웃을 짜보자.<br>현재 사용하는 Moon 테마는 Home, Project 같은 별도 공간이 존재한다.<br>Posts 에서는 All Posts, All Tags로 볼 수 있는 편의를 제공한다.<br>

우선 다른 사람들이 어떻게 만들었는지 보면서 살펴보는게 편해보인다.

---

<br>

## **3. 다른 사람 블로그 참조하기**

지피지기(知彼知己)면 백전불태(百戰不殆)라.<br>남을 알고 나를 알면 위태로움이 없다는 것이다.<br>남들을 우선 알아두면 블로그 작성 방향은 보인다.

[해당 테마](https://github.com/TaylanTatli/Moon)를 사용하는 사람들은 해당 깃허브 우측 상단에 [fork](https://github.com/TaylanTatli/Moon/network/members)한 사람들을 참조하면 된다.<br>숫자 부분을 누르면 퍼간 사람들이 보인다.<br>기존 테마 이름을 바꾼 애들 주소 복사하고 들어가면 된다.<br>거의 기본테마 수준인 블로그가 많지만, 참신한 블로그도 많다.<br>

<br>

게시판은 Posts를 누르면 All Posts 같은 느낌으로 나오도록 만드는 게 좋아보인다.<br>다른 블로그들은 카테고리도 만들어서 이걸 모아두는 페이지를 게시판인 것처럼 활용한다.<br>하지만 능력의 부족으로 인해 그냥 태그만 가지고 게시판을 만들겠다.

검색기능은 Project 같이 따로 search라는 이름으로 만드는 편이 좋아보인다.<br>거기서 검색 돌리면 바로 가도록 해도 괜찮아보인다.

사용자 집계는... 화면에 항상 떠있게 만들고 싶은데 힘들어 보인다.<br>Home 화면에 구겨 넣을 수 있다면 차라리 나아보인다.<br>음악 넣는 사람도 있었으니 충분히 가능할 것이다.

---

<br>

## **4. 게시판 만들기**

참조: https://nachwon.github.io/

우선 내가 건드린 파일은 다음과 같다.

- /_layouts/post-list.html
- /_includes/nav.html

post-list.html 은 이해를 잘 못해서 참조 사이트 것을 그대로 복사했다.<br>~~정신이 쇠약해져~~ 지식이 부족하여 설명을 못하겠다.<br>이걸 안 해주면 나중에 게시판을 만들 수가 없다는 점만 말해두고 넘어가자.

nav.html은 메뉴 영역을 담당하는 부분이다.<br>내가 바꾼 부분을 적당히 설명한다.

---

**(1) Home**

게시판은 아니지만 같이 작업했으니 설명한다.

원본

```html
<li><a href="{{ site.url }}/">Home</a></li>
```

변형

```html
<li><a href="{{ site.url }}/"><i class="fa fa-home" style="color:#808080">&nbsp;&nbsp;</i>Main</a></li>
```

아이콘을 넣고 이름을 Main으로 바꿨다.

i 부분의 class는 favicon(favorite icon)을 넣은 것이다.<br>[fontawesome](https://fontawesome.com/icons?d=gallery)에서 검색한 뒤 들어가면 해당 아이콘의 html 태그가 나온다.<br>위와 같은 식으로 붙이면 된다.<br>(fas fa-home 같은 애들은 안돼서 fa fa-home으로 대체)

색깔은 RGB 색상 대충 맘에 드는 거 때려 넣었다.

`&nbsp;`은 띄어쓰기다. tab이 안 먹어서 그냥 띄어쓰기 몇 번을 썼다.

---

**(2) Posts**

원본

```html
<li>
	<a href="#">Posts</a>	
	<ul class="dl-submenu">
        <li><a href="{{ site.url }}/posts/">All Posts</a></li>
		<li><a href="{{ site.url }}/tags/">All Tags</a></li>
	</ul>
</li>
```

변형

```html
<li>
	<a href="#"><i class="fa fa-clone" style="color:#808080"></i>&nbsp;&nbsp;Posts</a>	
	<ul class="dl-submenu">
		<li>
			<a href="#"><i class="fa fa-list" style="color:#808080"></i>&nbsp;&nbsp;Boards</a>
			<ul class="dl-submenu">
				<li>
					<a href="{{ site.url }}/posts/etc/">etc</a><!-- 게시판 부분-->>
				</li>
			</ul>
		</li>
		<li><a href="{{ site.url }}/tags/"><i class="fa fa-tags" style="color:#808080"></i>&nbsp;&nbsp;tags</a></li>
		<li><a href="{{ site.url }}/posts/"><i class="fa fa-globe" style="color:#808080"></i>&nbsp;&nbsp;All</a></li>
	</ul>
</li>
```

아이콘은 아까와 같은 방법으로 넣었다.

`<ul class="dl-submenu">` 부분을 넣어줌으로써 메뉴를 다중으로 다룰 수 있게 되었다.<br>Posts를 누르고 Board를 누르고 etc를 누르면 etc에 해당하는 글들이 등장한다.

저 위에 해당 폴더 경로를 잘 설정할 필요가 있다.<br>폴더 root에 보면 각 메뉴에 해당하는 폴더가 있고, 그 안에 index.md가 있을 것이다.<br>파일과 폴더를 만들어주고 여기서 해당하는 경로를 설정해야 글이 잘 들어간다.

---

<br>

## **5. 각종 페이지 정비**

지금까지 살펴보면 페이지 정비를 위해 들른 폴더는 다음과 같다.

- _includes
- _layout

레이아웃에 들어갈 물품(ex. nav.html)들은 _includes에 있었다.<br>이들을 제외하면 대부분 레이아웃만 조정하면 된다.<br>즉, _layout에 있는 페이지들을 정비할 필요가 있다.

home 화면(나한테는 main 화면)은 home.html을 조정하면 된다.<br>그 외 나머지는 index.md 등 인덱스 파일이 어떤 layout을 지정했는지 확인하자.<br>그 애들을 조정하면 해결된다.

코드 위치와 색상, class, 링크 수정 정도만 해도 난 충분하다고 생각했다.<br>위에서 했던 수준으로 하니까 할 수 있어서 자세히 설명하진 않는다.

난 SNS 마크가 거슬려서 박스를 지우고 아래로 치워버렸다.<br>메인 화면에 검색 기능이 바로 보이도록 하고 싶었지만,<br>모바일 환경에서만 이쁘고 데스크탑에선 안 이뻐서 그냥 nav에 통합했다.

Home 화면 버튼 색깔의 경우, 테마 다운 받으면 있던 글들에 정보가 있었다.<br>마크다운 문법 등 가이드가 많이 있어서 어느 정도 참조했다.

---

<br>

## **6. 검색 기능 만들기**

검색창은 어찌저찌 만들었지만, 검색 기능이 없으면 쓸모가 없다.<br>이제부터 검색기능에 대해 알아보려 한다.

검색기능을 만들기 위해서는 _layout 폴더에 해당 기능 레이아웃이 있어야 한다.<br>여기서는 page를 기반으로 검색기능을 넣으면 될 듯 하다. <br><span style="color:#F0F0F0">~~지금이라도 쉽다고 한 사기꾼 잡아와야 한다니까~~</span>

[검색기능 추가](https://wayhome25.github.io/etc/2017/02/23/blog-search/)는 금방 만들 수 있어보인다.<br>나는 <span style="color:#F0F0F0">~~푼돈~~</span>편의를 위해 구글 맞춤 검색을 이용할 것이다.<br>다만, 구글 맞춤 검색의 경우, 정식 호스팅 후에 구동이 가능해서 나중에 하도록 한다.


