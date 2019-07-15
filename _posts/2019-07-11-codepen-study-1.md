---
layout: post
title:  코드펜 따라하기 - 내비게이션 메뉴 (1)
date: 2019-07-13
tags: codepen html css
authors: eojinlee
cover: /assets/post_image/20190628/test4.jpg
---

HTML, CSS, JS를 각각 어느정도 배웠는데, 언어만 배우다보니 점점 지루해졌다. 아무래도 디자인 전공 출신이라 내가 배운 내용을 토대로 직접 화면을 구현하고 싶었다. 하지만 혼자서 웹페이지나 앱 전체를 구현한 경험은 없어서 어디서부터 시작해야할지 갈피를 잡지 못하고 있었다. 

그런데 마침 [**Codepen**](https://codepen.io/)이라는 사이트에서 내가 배운 언어들만으로 재밌는 웹 UI를 만들 수 있다는 걸 알게 되었다! 그래서 Codepen에 올라온 작업 중 매주 한 개를 골라서 따라 만들어보기로 했다.

______

## 언더바가 있는 내비게이션 메뉴 만들기

![예제](/assets/post_image/imgs-jina/example.png)
이번에 만들 것은 메뉴 텍스트를 클릭하면 텍스트를 강조하는 언더바가 해당 위치로 움직이는 내비게이션 메뉴다. 이 [**원본 작업 링크**](https://codepen.io/knyttneve/pen/LKrGBy)를 클릭하면 내가 참고한 원래 작업의 코드와 구현된 UI를 볼 수 있다. JS를 사용하면 예제처럼 움직이게 만들 수 있는데, 이번에는 JS를 사용하지 않고 hover 트랜지션까지만 구현해보았다. JS는 다음에... 

> 사용한 언어: HTML, CSS (Vanilla)


### 1. HTML - 텍스트

우선 뼈대로 HTML을 만든다. CSS, JS파일은 따로 만들어서 연결할거라 `<head>`에 링크로 연결해놓고, 일단 주석처리했다. CSS, JS파일을 만들면 주석을 풀 것이다.

`<body>` 구조로 큰 `<nav>` 박스가 있고, 그 안에 들어갈 메뉴 텍스트로 `<a>`가 여러개 있다. 습관적으로 메뉴를 HTML로 코딩할 때 `<li>`를 사용하곤 했는데, 이번 내비게이션 메뉴는 가로로 긴 형태라서 굳이 `<li>`를 쓰지 않았다.

```
<html>
    <head>
        <meta charset='utf-8'>
        <title>Animated Nav</title>
        <!--<link rel='stylesheet' href='main.css'>-->
        <!--<script src='navbar.js'>-->
    </head>
    <body>
        <nav>
            <a href='#' class='item'>홈</a>
            <a href='#' class='item'>피드</a>
            <a href='#' class='item'>검색</a>
            <a href='#' class='item'>프로필</a>
            <a href='#' class='item'>설정</a>
        </nav>
    </body>
</html>
```
![메뉴 텍스트가 생겼다.](/assets/post_image/imgs-jina/html_1.png)
> 메뉴 텍스트가 생겼다. 음... 예쁘지는 않다. CSS를 넣으면 나아질거다.

### 2. CSS - box 스타일, 타이포그래피

텍스트와 내비게이션바가 실제 웹 메뉴처럼 보이도록 스타일을 더한다. 

예전부터 쓰고싶었던 웹폰트인 Noto Serif를 이 기회에 써보기로 했다. Noto Serif는 2019년 현재 [**Google Fonts**](https://fonts.google.com/specimen/Noto+Serif+KR)에서 웹폰트로 사용할 수 있다. 상업적으로 사용가능하고, 퀄리티가 좋은 한글 바탕체라서 마음에 들었다.

```
@import url('https://fonts.googleapis.com/css?family=Noto+Serif+KR:400,700&display=swap&subset=korean');
```
그리고 기본적인 box 스타일과 타이포그래피를 더해봤다. box 스타일에서 `display`에 평소에 잘 쓰지않던 [**flex**](https://heropy.blog/2018/11/24/css-flexible-box/)와 [**grid**](https://developer.mozilla.org/ko/docs/Web/CSS/CSS_Grid_Layout/Basic_concepts_of_grid_layout)를 연습삼아 써봤다. 

`grid`는 익숙한 개념이라 쉽게 이해했지만, `flex`는 아직 원리를 잘 모르겠다. 그리고 `width`와 `height` 가변폭을 썼더니 스크롤이 이상하게 남는데, 가변폭을 쓰지않으면 중앙정렬이 딱 맞지 않는다. 어쩔 수 없이 `overflow: hidden;`을 추가했다.

```
body {
    display: flex;
    width: 100%;
    height: 100vh;
    justify-content: center;
    align-items: center;
    overflow: hidden;
    text-align: center;
    font-family: 'Noto Serif KR', serif;
    background-image: url('bg.jpg');
    background-size: cover;
}
nav {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    height: 48px;
    width: 680px;
    padding: 18px 40px 0;
    font-size: 20px;
    background-color: #fff;
    border-radius: 40px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, .2);
}
```
이제 메뉴 텍스트에 hover 트랜지션을 넣는다. `.item`에 pseudo class 스타일로 `:hover`를 추가했다. 주의할 점은, `transition:`을 `.item`이 아닌 `:hover`에 두면 텍스트에서 마우스를 떼어낼 때 트랜지션이 먹히지 않는다는 것이다. `.item`에 `transition`을 넣어야 마우스를 떼어낼 때에도 트랜지션이 붙는다.

```
.item {
    font-weight: 700;
    text-decoration: none;
    color: #8d8d8d;
    transition: .3s;
}
.item:hover {
    color: #333;
}
```
![박스 스타일과 타이포그래피 스타일을 추가했다.](/assets/post_image/imgs-jina/css_1.png)
> CSS를 추가하니 보기 좋아졌다. 이제 언더바를 구현해도 될 것 같다.

### 3. CSS - 언더바

언더바는 `.item:before`로 구현한다. pseudo class 스타일 중 은근히 자주 쓰이는 `:before`, `:after`는 말 그대로 class의 앞, 뒤에 붙는 컨텐츠를 뜻한다. `:after`는 보통 `:last-child:after`와 같이 쓰이는데, 이걸 IE9 미만 브라우저에서 지원하지 않기 때문에 대신 `:before`를 많이 쓴다.

`.item`과 `:before`에 각각 `position` 관계를 (relative-absolute) 더해주고, `:before` 스타일을 더한다. `content: '';`는 텍스트로 읽히지 않는 장식적 UI를 뜻한다. `.item:hover:before`도 추가한다. 적절한 `bottom`과 `height` 값, `background-color`와 `background-image`의 명도를 맞추는 것이 자연스러운 트랜지션 연출의 포인트였다.

```
.item {
    position: relative;
    .
    .
    .
}
.item:hover {
    color: #333;
}
.item:before {
    content: '';
    position: absolute;
    left: 0;
    bottom: -6px;
    width: 100%;
    height: 5px;
    opacity: 0;
    background-color: #cdcdcd;
    border-radius: 6px 6px 0 0;
    transition: .3s;
}
.item:hover:before {
    bottom: 0;
    opacity: 1;
}
```
![언더바를 만들었다.](/assets/post_image/imgs-jina/css_2.png)
> 언더바를 만들었다. 이제 hover 상태에서 언더바가 등장한다.

최종 결과물은 [**이 링크**](https://eojin-lee.github.io/CodepenStudy/20190713/navbar.html)로 들어가면 볼 수 있다.