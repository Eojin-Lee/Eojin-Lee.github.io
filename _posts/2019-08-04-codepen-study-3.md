---
layout: post
title:  코드펜 따라하기 - 다크모드 스위치
date: 2019-08-04
tags: codepen html css javascript
authors: eojinlee
cover: /assets/post_image/20190628/test4.jpg
---

## 다크모드로 변하는 토글 스위치 만들기

모바일에서 무언가를 켜고 끌 때 많이 사용하는 토글 스위치, 토글 버튼을 만들어본다. 토글 스위치는 JS말고 CSS만으로도 작업할 수도 있는데, [**W3Schools**](https://www.w3schools.com/howto/howto_css_switch.asp)에 들어가면 CSS만으로 만든 토글 스위치 코드를 볼 수 있다.

![예제](/assets/post_image/imgs-jina/20190804/example2.png)

이번에는 다크모드로 변환되는 토글 스위치를 만들어 볼 것이다. 이 [**원본 작업 링크**](https://codepen.io/russpate/pen/zMqmGJ)를 클릭하면 내가 참고한 원래 작업의 코드와 구현된 UI를 볼 수 있다. 코드를 보면 생각보다 간단해서, 포기하지 않고 한 번에 완성할 수 있을 것 같다.

> 사용한 언어: HTML, CSS, JS (Vanilla)

----

### 1-1. HTML - 기본 구조

토글 스위치와 모드 텍스트를 각각 `div`로 감싸는 HTML을 만든다. 토글 스위치는 input type에서 checkbox를 사용해 만든다. CSS와 JS Script는 주석으로 달아놓는다. 

```
<html>
    <head>
        <meta charset="utf-8">
        <!--<link rel=stylesheet type="text/css" href="main.css">-->
    </head>
    <body>
        <section class="setting">
            <div class="wrapper">
                <input type="checkbox" class="switch">
            </div>
            <div class="wrapper">
                <p class="opt-1">밝은 모드</p>
                <p class="opt-2">어두운 모드</p>
            </div>
        </section>
        <!--<script src="darkmode.js"></script>-->
    </body>
</html>
```

체크박스 div와 텍스트 div를 좌우로 배치하려면 CSS `display: inline-block` 속성을 적용해야하기 때문에 `.wrapper`로 묶는다. 만약 위아래로 배치하려 한다면 display 적용은 따로 안해도 되고, HTML의 `p`를 `span`으로 변경하면 된다.

글꼴은 평소 쓰고싶었던 한글 웹폰트인 나눔스퀘어(NanumSquare)를 쓴다. 구글링하다 운이 좋게 [**나눔스퀘어를 cdn서버로 제공하는 웹폰트주소**](https://github.com/moonspam/NanumSquare)를 발견해서 HTML에 import 했다. 


```
<head>
    <meta charset="utf-8">
    <link rel=stylesheet type="text/css" href="https://cdn.jsdelivr.net/gh/moonspam/NanumSquare@1.0/nanumsquare.css">
    <link rel=stylesheet type="text/css" href="main.css">
</head>
``` 

### 1-2. 웹폰트 로딩방식

이전 포스트에서는 웹폰트를 CSS에서 import 했는데, 이번에는 HTML에서 링크로 불러왔다. HTML에서 링크로 불러오는게 로딩이 더 빠르기 때문이다. 첫 로딩에서 타이포그래피가 망가지는 현상을 방지하기 위해 [**웹폰트 로딩**](https://nolboo.kim/blog/2013/10/22/google-web-font-faster-tip/)을 최대한 빠르게 하는 것이 중요하다. CSS에서 웹폰트를 import하면 기본 폰트에서 지정한 웹폰트로 변환될 때 잠깐 타이포그래피가 망가져보이는 현상이 나타나는데, 웹폰트를 HTML 링크로 CSS보다 먼저 불러오면 그 현상을 조금이나마 방지할 수 있다. 

HTML, CSS 말고도 early access인 구글 웹폰트를 사용하기 위해 webfont.js(google webfont loader)로 연동하는 방법도 있다는데, 나는 아직 사용해보지 않았다.


### 1-3. CSS - 기본 스타일

이제 기본 CSS 세팅을 한다. 영역을 구분하기 위해 임시로 border값을 준다. `.wrapper` 마진, `.switch` 너비는 임의로 설정했다. 


```
/* bg color */
body {
   font-family: 'NanumSquare', sans-serif;
   font-weight: 700;
   font-size: 16px;
   text-align: center;
   margin: 0;
   background: #fafafa;
}

/* temp border */
section, .wrapper {
   border: 1px solid black;
}
.wrapper {
   display: inline-block;
   margin: 3em .6em;
   text-align: left;
}

/* switch */
input[type=checkbox].switch {
   cursor: pointer;
   width: 1.5em;
   height: 3.5em;
}

/* option text */
.opt-1 {
   color: black;
}
.opt-2 {
   color: gray;
}
```

![기본 뼈대를 잡은 후 화면](/assets/post_image/imgs-jina/20190804/1.png)
> 기본 뼈대와 텍스트를 정했다. 이제 체크박스를 토글스위치로 꾸며야한다.

----

### 2-1. CSS - 기존 체크박스 숨김

체크박스와 텍스트가 1:1 대응이 되는 관계라면 텍스트를 `p`가 아닌 `label`로 설정하고 CSS에서 `label:before` 또는 `input[type="checkbox"].class:after`로 활성화 상태의 체크박스를 꾸민다. 이번 토글스위치는 1:1 대응이 아니라서 `:after`를 쓴다. `content: ''`으로 설정한 다음 나만의 checkbox 스타일을 만든다. 

이때 기존의 투박한 네모버튼 checkbox는 보이지 않도록 해야한다. `.switch`에 `display: none`을 사용하면 checkbox 영역 자체가 사라지는데, 그러면 너비를 미리 지정한 의미가 없어진다. 너비는 유지하고 checkbox 스타일만 사라지게 하려면 [**appearance**](https://css-tricks.com/almanac/properties/a/appearance/)를 사용해야한다. appearance는 html로 지원하는 element의 스타일을 변경하고싶을 때 사용하는 스타일 속성이다. `.switch`에 `apperance: none`을 브라우저별로 적용하면 된다.


```
input[type=checkbox].switch {
    appearance: none;
    -moz-appearance: none;
    -webkit-appearance: none;
    cursor: pointer;
   
    width: 1.5em;
    height: 3.5em;
}
```

![appearance와 display의 차이](/assets/post_image/imgs-jina/20190804/2.png)
> appearance: none 을 적용했을 때 화면과 display: none 을 적용했을 때 화면


### 2-2. CSS - 새 체크박스 스타일

체크박스가 안보여서 사라졌다고 생각할 수 있지만, 체크박스 자체는 아직 기능한다. 영역이 그대로 남아있고, hover 하면 `cursor: pointer`가 동작하고 클릭하면 active 영역이 보인다. 새 스타일로 `.switch`는 기다란 스위치 통로가 되고, `.switch:after`는 스위치 버튼이 되도록 만들고싶다. 우선 `.switch`와 `.switch:after`의 position 관계를 만들고, 각각의 스타일을 수정한다. `:checked`  스타일도 만든다.

```
/*section, .wrapper {
   border: 1px solid black;
}*/

.wrapper {
   display: inline-block;
   margin: 3em 0;
   text-align: left;
}

input[type=checkbox].switch {
   ...
   width: 1.5em;
   height: 3.5em;
   margin: 1em 1.2em;
   background-color: lightgray; 
   border-radius: 3em; 
}
input[type=checkbox].switch:after {
   ...
   top: 0;
   left: -3.35px;
   width: 2em; 
   height: 2em;
   background-color: white;
   border-radius: 3em;
   transform: translateY(-20%);
   box-shadow: 2px 0px 5px 0 rgba(0,0,0,0.15);
}
input[type=checkbox].switch:checked:after {
   top: 3.5em;
}
```

### 2-3. inline-block & left-align 간격 이슈

그런데 스위치와 텍스트의 위쪽 간격이 서로 맞지 않는다. inspector로 보면 마진값은 분명히 동일한데, 마진도 패딩도 아닌 알 수 없는 간격이 생겼다.

이 간격은 `display: inline-block` 속성에서 `text-align: left` 일 때 생기는 이슈다. 이 경우 [**여러가지 해결 방식**](https://eojin-lee.github.io/2019/07/05/publishing-inline-block-spacing-issue.html)이 있지만, 가장 좋은 해결방법은 해당 element에 `float: left`를 주는 것이다. [**float**](https://developer.mozilla.org/ko/docs/Web/CSS/float)을 쓰는 방식은 다른 해결 방식과 달리 브라우저 이슈가 없고, 마크업을 망가뜨리지 않고, 부모요소에 영향을 주지 않아도 된다. 대신, float 자체 개념이 어렵고, display와 꼬이면 해결하기 쉽지 않다는 단점이 있다.

`float: left`를 주면 `body {text-align: center}`가 먹히지 않는다. 그래서 컨테이너를 사용하는 반응형 양쪽마진을 주어서 wrapper를 포함하는 컨테이너를 가운데로 배치해야한다. 이 방법은 반응형 페이지를 만들 때 자주 사용하는 방식이다. 여기서는 컨테이너로 `section`을 사용했다.

```
body {
   ...
   /*text-align: center;*/
   margin: 0;
   background: #fafafa;
}

section {
   width: 10em;
   height: 10em;
   margin: 0 auto;
}
.wrapper {
   display: inline-block;
   float: left;
   margin: 3em 0;
   text-align: left;
}
```

![inline-block left-align issue](/assets/post_image/imgs-jina/20190804/3.png)
> inline-block 요소가 left-align 되어있을 때 의도치않은 margin이 생기는 이슈가 있다. 퍼블리싱할 때 자주 등장하는 이슈다. float: left로 해결하면 위쪽 마진이 맞는다.

### 2-4. CSS - 트랜지션

checked 상태로 넘어갈 때 보이는 약간의 모션을 transition으로 만들어본다. 

```
body {
   ...
   transition: .2s all;
}
input[type=checkbox].switch {
   ...
   transition: 0.09s ease-in-out;
}
input[type=checkbox].switch:after {
   ...
   transition: top .3s cubic-bezier(0.175, 0.885, 0.320, 1.275), padding .3s ease;
}
input[type=checkbox].switch:active:after {
   padding-bottom: 1em;
}
```

모션을 세부적으로 조정하고 싶을 때가 있는데, cubic bezior 숫자를 일일이 조정하는건 직관적이지 않아서 불편하다. chrome inspector에서 제공하는 그래프 기능을 이용하는게 편하다. 모션 베지어 그래프 말고도 shadow 수치, rgba 값 등 다양한 수치를 직관적으로 확인할 수 있다. 다른 브라우저의 inspector에서도 이런 기능을 제공하는지는 잘 모르겠다.

![inspector transition graph-custom](/assets/post_image/imgs-jina/20190804/4.png)
> inspector에서 직관적으로 모션을 보고, 수정할 수 있다.

----

### 3-1. CSS - active

필수 스타일을 다 꾸몄다면 이제 `is-active` class를 추가해서 다크모드일 때의 스위치, background, 텍스트 속성을 만든다.

```
body.is-active {
   background: #161616;
}

input[type=checkbox].switch:checked {
   background-color: #333;
}
input[type=checkbox].switch:checked:after {
   top: 3.5em;
   background-color: #464646;
}

.opt-1.is-active{
   color: gray;
}
.opt-2.is-active {
   color: white;
}
```

![darkmode1](/assets/post_image/imgs-jina/20190804/5.png)
> 어두운 모드일 때 이런 화면이 나온다고 친다. 아직은 반영이 안된다.

----

### 3-2. JS - checkbox active 확인 함수

JS로 스위치가 체크되었는지 확인하는 함수를 만든다. 대응하는 변수들을 `querySelector`로 불러오고, 변수들에 `element.classList.toggle(.className)`를 사용한다. [**element.classList.**](https://developer.mozilla.org/ko/docs/Web/API/Element/classList) 메소드는 대응하는 element가 한 개보다 많으면 작동하지 않기 때문에, 각각의 element를 다 변수로 만들어서 써야했다. toggle은 토글스위치처럼 className의 유무에 따라 제거 또는 추가해주는 편리한 키워드다. 마지막에 클릭 이벤트리스너를 만들었다.

```
const body = document.querySelector('body');
const toggleSwitch = document.querySelector('input[type=checkbox].switch');
const opt1 = document.querySelector('.opt-1');
const opt2 = document.querySelector('.opt-2');

function checkActive(){
  body.classList.toggle('is-active');
  toggleSwitch.classList.toggle('is-active');
  opt1.classList.toggle('is-active');
  opt2.classList.toggle('is-active');
  console.log("checked");
}

toggleSwitch.addEventListener('click', checkActive(), false);
```

![click 이벤트가 의도와 다르게 적용된 화면](/assets/post_image/imgs-jina/20190804/6.png)
> 처음 들어가자마자 스위치는 밝은모드, 나머지는 어두운모드로 보인다. 클릭하지 않았는데도 콘솔에 'checked'가 뜬다. 어디가 잘못 되었을까?

이벤트리스너 문제인가 싶어서 구문을 삭제하고 HTML에 `onClick` 속성을 추가했다. 

```
<input type="checkbox" onClick="checkActive()" class="switch">
```

![토글 스위치 작동 완료](/assets/post_image/imgs-jina/20190804/7.png)
> 잘 동작한다!

최종 결과물은 [**이 링크**](https://eojin-lee.github.io/CodepenStudy/20190804/darkmode_toggle.html)로 들어가면 볼 수 있다.