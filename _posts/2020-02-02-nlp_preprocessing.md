---
title: [NLP] 한국어 임베딩(2019) 3장 정리(진행중)
layout: post
date: '2020-02-02 18:00:00'
excerpt: 한국어 데이터의 전처리 과정
tag:
- nlp
- mecab
- khaiii
- konlpy
category: nlp
comments: true
---

---

※ 이 글은 [한국어 임베딩(2019)](http://acornpub.co.kr/book/korean-embedding) 책을 개인적으로 정리하고 잡다한 것들을 추가한 글입니다.<br>원문과 똑같은 부분은... 이해를 못해서 그대로 쓴 부분입니다.

※ 책은 [깃허브](https://github.com/ratsgo/embedding)에 있는 소스들을 사용하고 있습니다. 셸을 사용하는 경우 여기 있는 파일들을 돌리는 겁니다.

<br>

## 1. 데이터 확보

책에서는 다음과 같은 오픈 소스를 알려주고 있다.

- 한국어 위키백과
- KorQuAD
- 네이버 영화 리뷰 말뭉치

`preprocess.sh` 파일이 제공되어 편하게 데이터를 추출할 수 있다.<br>다운로드, 각종 전처리 등 여러 커맨드가 들어있다.<br>나는 빨리 다뤄보고 싶은 자료들이 있어서 텍스트 추출은 넘어가려고 한다.

마침 전처리 완료된 데이터 다운로드도 제공하고 있기 때문에,<br>우분투에서 이것만 실행하고 마칠 것이다.

```bash
bash preprocess.sh dump-processed
```

<br>

---

## 2. 지도 학습 기반 형태소 분석

좋은 임베딩을 만드려면 문장, 단어의 경계를 알려줘야 한다.<br>그렇지 않으면 어휘 집합에 속한 단어 수가 기하급수적으로 늘어나고, 처리속도 역시 떨어진다.<br>특히, 한국어는 어미, 조사가 발달한 **교착어(agglutinative language)**이기 때문에 섬세함이 필요하다.

`가다`를 예시로 들어보자.<br>`가다`는 문맥에 따라 `가다`, `가겠다`, `가더라`, `가겠더라` 등으로 쓸 수 있다.<br>예시만 따져봐도 `가다`라는 의미를 가진 단어가 4개나 된다.<br>이런 중복을 줄이기 위해 형태소 분석이 필요하다.

위에 제시한 단어들을 `konlpy`의 `Mecab` 기준으로 분석하면,<br>`가`,`겠`,`다`,`더라` 등 4개의 형태소가 나온다.<br>만약 `오다`, `오겠다`, `오더라`, `오겠더라`라는 단어가 등장했을 때, 원래라면 4개 단어가 늘어날 것이다.<br>하지만 형태소 분석기를 사용하면 `오`라는 형태소만 추가하면 된다.<br>데이터가 적게 늘어나니까 데이터 연산처리 또한 효율적이다.

교착어인 한국어는 한정된 종류의 조사와 어미를 자주 이용한다.<br>때문에 각각에 대응하는 명사, 용언(형용사, 둉사) 어간만 어휘 집합에 추가하면 단어 숫자를 많이 줄일 수 있다.



**(1) KoNLPy**

[KoNLPy](http://konlpy.org/ko/latest/)는 오픈소스 형태소 분석기를 **파이썬**에서 사용할 수 있도록 인터페이스를 통일한 패키지다.<br>당연히 파이썬 환경에서 사용하도록 한다.

`Mecab`, `Okt`, `Komoran`, `Hannanum`, `Kkma` 등 총 5가지 분석기가 있다.<br>([은전한닢](http://eunjeon.blogspot.com/), [Okt](https://github.com/twitter/twitter-korean-text)(트위터), [코모란](https://www.shineware.co.kr/products/komoran/), [한나눔](https://kldp.net/hannanum/), [꼬꼬마](http://kkma.snu.ac.kr/documents/) 분석기라는군요.)

형태소 분석과 품사 분석은 아래와 같이 할 수 있다.<br><u>※ Mecab은 윈도우에서 사용 불가능합니다.</u>

```python
from konlpy.tag import Mecab, Okt, Komoran, Hannanum, Kkma
tn = Mecab() # 다른 분석기를 사용하려면 Okt() 등을 사용하면 됩니다.
print(tn.morphs("아버지가방에들어가신다"))
print(tn.pos("아버지가방에들어가신다"))
```

결과물은 다음과 같다.

```markdown
['아버지', '가', '방', '에', '들어가', '신다']
[('아버지', 'NNG'), ('가', 'JKS'), ('방', 'NNG'), ('에', 'JKB'), ('들어가', 'VV'), ('신다', 'EP+EC')]
```

[konlpy 설명 문서](http://konlpy.org/ko/latest/morph/#comparison-between-pos-tagging-classes)에 의하면, 형태소 분석기별 속도는 다음과 같다.

| 분석기명     | 로딩 시간 | 실행 시간 |
| ------------ | --------- | --------- |
| Kkma         | 5.6988    | 35.7163   |
| Komoran      | 5.4866    | 25.6008   |
| Hannanum     | 0.6591    | 8.8251    |
| Okt(Twitter) | 1.4870    | 2.4714    |
| Mecab        | 0.0007    | 0.2838    |

한국어 임베딩 저자의 [포스팅]([https://ratsgo.github.io/from%20frequency%20to%20semantics/2017/05/10/postag/](https://ratsgo.github.io/from frequency to semantics/2017/05/10/postag/))을 살펴보니, 품사 태그 정보가 속도에 영향을 미치는 듯하다.

성능 분석도 참조하면서 골라서 쓰는 것이 좋겠다.<br>(성능 역시 [konlpy 설명 문서](http://konlpy.org/ko/latest/morph/#comparison-between-pos-tagging-classes)에 있으니 참조하자.)<br>Mecab이 월등한 속도에 성능도 적절해서 현업에서 많이 쓴다고 한다.



**(2) Khaiii**

[Khaiii](https://tech.kakao.com/2018/12/13/khaiii/)(Kakao Hangul Analyzer III)는 카카오에서 만든 한국어 형태소 분석기다.<br>세종코퍼스를 이용해 CNN모델 적용해 학습했다고 한다.<br>GPU 없이 형태소 분석 가능하고 속도 역시 빠르다고 한다.

<u>※ Khaii 역시 윈도우에서 사용 불가능합니다.</u>

설치는 음... 좀 귀찮아보인다.<br>C++ 기반이라 파이썬과 연동하기 전에 여러 과정이 필요하다.<br>설치는 [공식 문서](https://github.com/kakao/khaiii/wiki/빌드-및-설치)를 참고하자. <br>(애초에 난 컴퓨터를 잘 모른다...하라는대로 했더니 되긴 했다.)

좀 막힌 부분들이 있어서 내가 거친 과정을 적어둔다.<br>나는 환경이 꼬여서 sudo는 안되고 root에서 하니까 됐다.<br>git을 sudo로 해서 파일들 권한이 root로 설정되서 그런가 생각한다.

```bash
# 빌드
pip install cmake
git clone https://github.com/kakao/khaiii
cd khaiii
mkdir build
cd build
cmake ..
make all
make resource
# 설치
make install
make package_python
cd package_python
pip install .
```

이제 파이썬에서 아래 코드가 들어먹는지 확인하면 된다.

```python
from khaiii import KhaiiiApi
tn = KhaiiiApi()
data = tn.analyze('안녕, 세상.')
token1 = []
token2 = []

print("기본출력:",[str(a) for a in data]) # 기본 출력 데이터
for word in data:
    token1.extend([str(m).split("/")[0] for m in word.morphs])
    token2.extend([str(m) for m in word.morphs])
print("글자출력:",token1) # 글자만 출력
print("품사출력:",token2) # 품사도 출력
```

결과물은 다음과 같다.

```markdown
기본출력: ['안녕,\t안녕/IC + ,/SP', '세상.\t세상/NNG + ./SF']
글자출력: ['안녕', ',', '세상', '.']
품사출력: ['안녕/IC', ',/SP', '세상/NNG', './SF']
```

konlpy에 담긴 5개 분석기와 khaiii를 [같이 비교한 문서]([https://iostream.tistory.com/144?utm_source=gaerae.com&utm_campaign=%EA%B0%9C%EB%B0%9C%EC%9E%90%EC%8A%A4%EB%9F%BD%EB%8B%A4&utm_medium=social&fbclid=IwAR2A87bBOU93ePHgdLsGeAal-2iLszDJzPJl1_IuYeHcd8RoNd8Cd6f054g](https://iostream.tistory.com/144?utm_source=gaerae.com&utm_campaign=개발자스럽다&utm_medium=social&fbclid=IwAR2A87bBOU93ePHgdLsGeAal-2iLszDJzPJl1_IuYeHcd8RoNd8Cd6f054g))가 있다.<br>요약하자면, 여전히 `Mecab`이 제일 빠르고 품질도 상위권이다.



**(3) Mecab 사용자 사전 등록**

분석을 하다 보면 사용자 사전이 필요할 때가 있다.<br>조선왕조실록을 예시로 들고자 한다.<br>[태조실록](http://sillok.history.go.kr/id/kaa_10409108_001)에 <u>"오도리(吾都里) 상만호 동맹가첩목아(童猛哥帖木兒) 등 5인이 와서 토산물을 바쳤다."</u>는 내용이 있다.<br>이를 분석하면 다음과 같이 나온다.

```python
['오도리', '(', '吾', '都', '里', ')', '상만호', '동맹가', '첩목아', '(', '童', '猛', '哥', '帖木兒', ')', '등', '5', '인', '이', '와서', '토산물', '을', '바쳤', '다', '.']
```

`동맹가첩목아`는 사람 이름인데 분석기는 `동맹가`,`첩목아`로 토막살인을 저질렀다.<br>이런 참혹한 현장을 막기 위해 사람 이름임을 명시해야 한다.

책에서는 사전을 컴파일할 수 있는 셸 파일을 제공하고 있다.<br>하지만 직접 만들어보고 싶어서 다른 방법을 설명하고자 한다.<br>도커 환경이 아닌 스스로 환경을 구축하면서 공부하는게 좋아서 이런 것도 최대한 직접 하고있다.

우선 user-dic 폴더에 들어간다. (내 경우 /tmp/mecab-ko-dic-2.1.1-20180720/user-dic 였다.)<br>[공식 홈피](https://bitbucket.org/eunjeon/mecab-ko-dic/src/df15a487444d88565ea18f8250330276497cc9b9/final/user-dic/)에 사용법이 나와있다.<br>place.csv, person.csv 등의 예시 파일이 있는데, 대략 아래와 같다.

```python
# nnp.csv
대우,,,,NNP,*,F,대우,*,*,*,*
구글,,,,NNP,*,T,구글,*,*,*,*
# person.csv
까비,,,,NNP,인명,F,까비,*,*,*,*
# place.csv
세종,,,,NNP,지명,T,세종,*,*,*,*
세종시,,,,NNP,지명,F,세종시,Compound,*,*,세종/NNP/지명+시/NNG/* #..뭐지
```

자료구조는 다음과 같다고 한다.

- 표층형/0/0/0/품사태그/의미분류/종성유무/읽기/타입/첫번째품사/마지막품사/표현

여기에 사용자 사전 csv 파일을 만들면 된다.<br>이름을 뭘로 정하든 csv이고, 위와 같은 양식으로 작성하면 상관없다.<br>난 귀찮아서... person.csv를 다음과 같이 수정하고 진행할 것이다.

```
동맹가첩목아,,,,NNP,인명,F,동맹가첩목아,*,*,*,*
```

사전에 내용을 변경했다면, 이제 변경한 내용을 적용시킬 차례다.<br>user-dic 상위폴더(본인의 경우 mecab-ko-dic-2.1.1-20180720)으로 나오면 tools 디렉토리에 `add-userdic.sh`파일이 있다.<br>이 파일을 실행하면 사전이 컴파일되는데... 나는 약간 문제가 있었다.

```bash
# 1. user-dic, tools 등 하위폴더에서 add-userdic.sh 실행
../tools/add-userdic.sh
# 2. 상위폴더(mecab-ko-dic-2.1.1-20180720)에서 add-userdic.sh 실행
tools/add-userdic.sh
```

위 과정을 거친 후 `make install`을 실행하면 사전이 설치되어야 정상이다.<br>하지만 1번 과정대로 하면 `make: *** No rule to make target 'install'.  Stop.`라는 오류가 떴다.<br>2번 과정대로 하면 문제가 없이 사전이 적용되었다.<br>위에서도 말했지만, 권한이 꼬여서 그런거 같다.

어쨌든 우여곡절 끝에 모든 과정을 마친 후 얻은 결과는 다음과 같다.

```python
['오도리', '(', '吾', '都', '里', ')', '상만호', '동맹가첩목아', '(', '童', '猛', '哥', '帖木兒', ')', '등', '5', '인', '이', '와서', '토산물', '을', '바쳤', '다', '.']
```

아까 `동맹가`, `목첩아`로 분리된 이름이 `동맹가첩목아`로 이어진 모습을 볼 수 있다.<br>물론 한자 부분은 능지처참되었지만, 붙여주고 싶다면 같은 방식으로 사전에 등록하면 된다.<br>하지만 한자 부분을 붙이는 것이 의미가 있으려면, 한글부분과 한자부분이 동일인임을 증명할 수 있어야 한다.<br>`동맹가첩목아`, `童猛哥帖木兒`, `동맹가첩목아(童猛哥帖木兒)` 모두 동일인임을 증명할 방법이 있다면 셋 다 등록해야겠으나,<br>나는 그런 방법을 알지 못하기 때문에 한글 부분인 `동맹가첩목아`만 등록할 것이다.

<br>

---

## 3. 비지도 학습 기반 형태소 분석

지도 학습 기법은 형태소 경계나 품사 정보를 모델에 가르쳐준 것이다.<br>여기서 다룰 비지도 학습 기법들은 데이터에 자주 등장하는 단어들을 형태소로 인식하도록 학습한다.



**(1) soynlp**

(사용하진 않을 예정이라 간단히 정리만 했다.)

[soynlp](https://github.com/lovit/soynlp)는 파이썬 기반 한국어 자연어 처리 패키지다.<br>비지도 학습 특성상 패턴을 스스로 학습하기 때문에, 어느 정도 규모가 있는 동질적인 문서 집합에서 잘 작동한다.<br>영화 댓글, 특정일자 뉴스 기사 등이 대표적인 사례다.

soynlp의 형태소 분석기는 데이터 통계량에 따른 단어 점수표로 작동한다고 한다.<br>**응집 확률(Cohesion Probability)**, **브랜칭 엔트로피(Branching Entropy)**를 활용한다.<br>`꿀잼 ㅎㅎ`, `영화 꿀잼`를 예시로 들겠다.<br>`꿀잼`은 자주 나타난 단어이며, 이는 **응집확률**이 높다고 판단하여 형태소로 인식된다.<br>`꿀잼`의 앞뒤로 `ㅎㅎ`, `영화` 등 다양한 패턴이 나타났으며, 이는 **브랜칭 엔트로피**가 높다고 판단하여 형태소로 인식된다.

띄어쓰기 교정이 있는게 흥미로운 부분이다.<br>혹시 필요하다면 써보도록 하자.



**(2) Google Sentencepiece**

센텐스피스(sentencepiece)는 구글(Kudo&Richardson, 2018)에서 공개한 비지도 기반 형태소 분석 패키지다.<br>바이트 페어 인코딩(Bite Pair Encoding, BPE) 등을 지원하며, pip 설치가 가능하다.

BPE는 가장 많이 등장한 문자열을 압축하는 것이다.<br>`aaabdaaabac`를 예시로 들어보자.<br>`aa`가 많이 나왔고, 이를 `X`로 치환하여 `XabdXabac`로 만든다.<br>`ab`도 많이 나왔고, 이를 `Y`로 치환하여 `XYdXYac`로 만든다.

**BERT** 모델이 BPE로 학습한 어휘 집합을 사용한다.<br>비지도 학습 기법이기 때문에 어떤 언어에든 적용 가능하다.<br>나중에 문장 임베딩에서 다룰 것이기 때문에 이만 마치도록 하겠다.

<br>

---

## 4. 주요 내용

- 임베딩 학습용 말뭉치는 라인 하나가 문서면 좋다.
- 지도 학습 기반의 형태소 분석 모델은 언어학 전문가들이 태깅한 형태소 분석 말뭉치로부터 학습된 기법이다. 사람이 알려준 정답 패턴에 최대한 가깝께 토크나이즈한다. KoNLPy, Khaiii 등이 대표적이다.
- 비지도 학습 기반의 형태소 분석 모델은 데이터의 패턴을 모델 스스로 학습하게 함으로써 형태소를 나누는 기법이다. 데이터에 자주 등장하는 단어들을 형태소로 인식한다. soynlp, Google Sentencepiece 등이 대표적이다.

---

<br>

#### 참고

- [한국어 임베딩(2019)](http://acornpub.co.kr/book/korean-embedding)



