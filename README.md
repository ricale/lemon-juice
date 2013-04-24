# handmade markdown decoder

0. 기본 정보
1. 개요
2. 사용 방법
2. hmd 마크다운 문법
3. 문서 정보

## 기본 정보

* 프로젝트 이름 : handmade markdown decoder (hmd)
* 구현 언어 : JavaScript (jQuery)
* 작성자 : ricale
* 버전 : 0.1

## 개요

handmade markdown decoder (hmd)는 마크다운 문법으로 작성된 문자열을 HTML 형식으로 번역해주는 자바스크립트 클래스를 구현하는 프로젝트이다. 

마크다운 문법은 기본적으로 [이 곳][syntax]의 문법을 따른다. 단 근본적인 문법을 변경하지 않는 선에서 세부사항이 변경될 수 있다. 현재 hmd에서의 마크다운 문법의 상세는 아래에서 확인할 수 있다.

[syntax]: http://daringfireball.net/projects/markdown/syntax

## 사용 방법

    var decoder = new RICALE.MarkdownDecoder(targetElement);
	decoder.translate(string);
	
`RICALE.MarkdownDecoder` 클래스의 객체를 생성한다. 생성 시 번역의 결과과 출력될 HTML 요소(`targetElement`)를 인자로 받는다. 그 후 `RICALE.MarkdownDecoder` 객체의 `translate(string)` 함수를 호출한다. 인자로 번역할 마크다운 문자열(`string`)을 받아 생성 시 입력 받았던 HTML 요소에 번역한 결과 값을 출력한다.

## hmd 마크다운 문법

1. 블록 요소 Block Element
   1. 문단 p
   2. 제목 h1 h2 h3 h4 h5 h6
   3. 인용 blockquote
   4. 목록
      1. 비순서적 목록 ul
      2. 순서적 목록 ol
      3. 목록 공통
   5. 코드 블록 pre & code
   6. 수평선 hr
2. 인라인 요소 Inline Element
   1. 링크 a
      1. 인라인 스타일
      2. 레퍼런스 스타일
      3. 자동 링크
   2. 강조 em, strong
   3. 코드 code
   4. 이미지 img
3. 기타
   1. 전 줄과 관련된 규칙

### 블록 요소

#### 문단

문단은 하나 이상의 빈 문장으로 나뉜다. 빈 문장은 아무것도 포함하지 않거나 공백(space, tab)만을 포함한 문장이다.

줄을 아무리 띄어도 문단p으로 적용될 뿐 줄바꿈br이 적용되지 않는다. 줄바꿈을 적용하길 원한다면 줄의 마지막에 공백을 두 개 이상 넣으면 된다.

#### 제목

\#를 사용한다.

>     # h1스타일의제목
>     ## h2스타일의제목
>     ### h3스타일의제목
>     #### h4스타일의제목
>     ##### h5스타일의제목
>     ###### h6스타일의제목

\# 스타일의 경우 닫는 것도 가능하다. 닫을 때 #의 개수는 몇 개든 상관없다.

>     # 닫기 #
>     ## 또 닫기 ##
>     ### 다시 닫기 #####

문법상 ===== 스타일과 ----- 스타일도 존재하지만

>     h1스타일의제목  
>     =========== (=의 개수는 몇 개든 상관없다)
>     h2스타일의제목  
>     ----------- (-의 개수는 몇 개든 상관없다)

현재 hmd에 적용되어 있지는 않다.

#### 인용

\>를 사용한다.

>     > blockquote스타일
>
>     > 인용 내에서의 문단 나눔도 빈 줄로 한다.
>     > - 다른 마크다운 중첩도 가능하다.

#### 목록

##### 비순서적 목록

\* 또는

>     * 목록하나
>     * 둘
>     * 셋

\- 또는

>     - 하나
>     - 둘
>     - 셋

\+ 를 사용한다.

>     + 하나
>     + 둘
>     + 셋

##### 순서적 목록

숫자를 사용한다.

>     1. 순서 목록
>     2. 둘
>     3. 셋

숫자의 순서는 틀려도 상관없다.

>     3. 순서
>     2. 목록
>     100. 이다

##### 목록 공통

목록 내에서 줄을 바꿔도 줄바꿈이 적용되지 않는다.

>     1. 순서
>     목록
>     이다

###### 결과

> 1. 순서
>  목록
>  이다.

줄바꿈을 유도하려면 빈 줄을 만들어 문단을 나눠야 한다.

>     1. 순서
>
>      목록
>
>      이다

###### 결과

> 1. 순서
> 
>  목록
>
>  이다
 
들여쓰기에 주의하라. 한 칸 이상의 들여쓰기가 없다면 해당 줄을 목록의 연장으로 판단하지 않고 새로운 문단으로 판단한다.

본래 문법에 의하면 목록 요소 내부에 또다른 블록 요소가 들어가는 것이 가능하지만 현재 hmd에서는 p 이외의 블록 요소가 목록 요소 내부로 들어가지 못한다. __따라서 아래 나오는 목록 내부의 코드 블록 또한 적용되지 않는다.__

리스트에 코드 블록을 넣고 싶다면 빈 줄 이후 기존 코드 블록의 두 배의 공백을 주면 된다.

>     1. 테스트
>
>             코드블록테스트

###### 결과

> 1.   응
>
>         응

목록 요소의 각 레벨의 들여쓰기는 아래와 같다. 기준에 맞지 않을 시에는 목록 요소로서 표현되지 않을 수도 있다. 들여쓰기 단위는 공백(space)이다.

* 레벨 1 - 0개 ~ 3개
* 레벨 2 - (레벨 1의 공백 + 1)개 ~ 7개
- 레벨 3 - (레벨 2의 공백 + 1)개 ~ 11개
* 레벨 n - (레벨 n-1의 공백 + 1)개 ~ (n * 4 - 1)개

#### 코드 블록

공백 네 번(4 space) 혹은 탭문자 한 번(1 tab)을 사용한다.

>         테스트
>         코드블록테스트

###### 결과

    테스트
    코드블록테스트

블록 내의 모습은 그대로 적용된다.

#### 수평선

\- 또는 \_ 또는 \*을 한 줄에 셋 이상 입력한다.

>     ---
>     ___
>     ***

###### 결과

> ---
> ___
> ***

문자들 사이에 공백이 포함되어 있어도 가능하다.

>     -   -    -
>       _     _ _
>      *  *  *

###### 결과

> -   -    -
>   _     _ _
>  *  *  *

본래 \--- 바로 윗줄에 글이 있다면 h2 스타일로 적용된다 (사이에 공백이 있다면 상관없다). __하지만 현재는 ---스타일이 구현되어있지 않기 때문에 그냥 수평선으로 적용된다.__

### 인라인 요소

#### 링크

##### 인라인 스타일

아래의 스타일을 사용한다. 제목은 생략할 수 있다.

>     [텍스트](http://google.com "제목")
>     [텍스트](http://google.com)

###### 결과

> [텍스트](http://google.com "제목")
> [텍스트](http://google.com)

##### 레퍼런스 스타일

링크 텍스트와 url 부분을 나누어 쓴다. 주소는 <>로 감싸도 된다. 제목은 "" 혹은 '' 혹은 ()로 감싸거나 생략할 수 있다.

>     [텍스트][id]
>     [id]: http://google.com "제목"
>     [id]: http://google.com '제목'
>     [id]: http://google.com (제목)
>     [id]: <http://google.com> "제목"
>     [id]: <http://google.com> '제목'
>     [id]: <http://google.com> (제목)
>     [id]: http://google.com
>     [id]: <http://google.com>

###### 결과

> [텍스트][id]
[id]: http://google.com "타이틀"

텍스트와 아이디 사이에 공백이 허용된다.

>     [텍스트] [id]

아이디를 생략할 수도 있는데 아이디를 생략하면 텍스트가 아이디 역할까지 맡는다.

>     [텍스트][]
>     [텍스트]: http://google.com

###### 결과

> [텍스트][]
[텍스트]: http://google.com

##### 자동 링크

url을 <>로 감싸면 자동으로 링크가 적용된다.

>     <http://google.com>

###### 결과

> <http://google.com>

단 <> 안의 문자열이 http:// 혹은 https:// 으로 시작해야지만 적용된다. 이는 본래 문법에 있는 것인지는 확인되지 않고, hmd에서는 적용된 사항이다.

이메일 링크는 자동 암호화한다는 데 뭔지 모르겠다. 구현하지 않는 것으로 하고 넘어가자.

#### 강조

글을 * 혹은 _로 감싸면 em 효과가 적용된다.

>     _내용_
>     *내용*

###### 결과

> _내용_
> *내용*

글을 ** 혹은 __로 감싸면 strong 효과가 적용된다.

>     __내용__
>     **내용**

둘을 섞어 쓰면 안되지만 일부 에디터에서는 아래처럼 섞어 쓰기도 한다. hmd는 섞어 쓰는 것을 지원하지 않는다.

>     _*내용**
>     _*내용_*
>     __내용_*
>     *내용_

#### 코드

\`로 감싸면 코드 태그가 적용된다.

>     `코드`

###### 결과

`코드`

코드 내부에서 \`문자를 사용할 때를 위해 \`\`로 감쌀 수도 있다.

>     ``코드``

###### 결과

``코드``

코드의 첫/마지막 글자에 \`를 사용하고 싶다면 한 칸 공백을 준다.

>     `` `코드` ``

###### 결과

`` `코드` ``

#### 이미지

맨 앞에 !가 붙는 것과 []안의 텍스트가 alt 역할을 한다는 것만 빼면 링크와 동일하다. 상세 내용은 생략한다. 링크를 참고 바란다.

## 문서 정보

- 작성자 : ricale
- 문서버전 : 0.2 (hmd 0.1)
- 작성일 : 2013. 4. 24.