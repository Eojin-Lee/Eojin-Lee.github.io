---
layout: post
title:  웹퍼블리싱 - 인라인블록의 왼쪽정렬
date: 2019-07-05
tags: html css publishing
authors: eojinlee
cover: /assets/post_image/20190628/test4.jpg
---

## 문제상황
inline-block element(nav, li) 를 left-align 하면 element 마다 의도한 적 없는 spacing 이 생긴다.


## 해결과정

1. **element의 태그를 닫지 않기**
    
    바로 해결되지만, [html(마크업)이 망가진다.](https://css-tricks.com/fighting-the-space-between-inline-block-elements/#article-header-id-2) 마크업을 건드리지 않고 css에서 해결할 방법은 없을까?
2. **parent element(부모요소)에게 `font-size:0;`을 주기**
    
    바로 해결되지만, 부모요소에 font-size: 0;은 그다지 직관적인 해결방법이 아닌 것 같다. 그리고 [안드로이드와 사파리에 web-font 관련 이슈를 주는 것 같다.](https://css-tricks.com/fighting-the-space-between-inline-block-elements/#article-header-id-3)
3. **element에게 `margin-right: -i px;`주기**
    
    당장 떠오른 해답이 margin인데, 안타깝게도 [브라우저 이슈가 있다고 한다.](https://css-tricks.com/fighting-the-space-between-inline-block-elements/#article-header-id-1) 마지막 element의 space가 조금 남는다.
4. **element에게 `float: left;`주기**
    
    이게 가장 직관적인 해결방법인 것 같다. 마크업을 건드리지 않고 부모요소에도 영향을 주지 않으니까. float은 브라우저 이슈도 없다. display와 잘못 꼬이면 해결이 어려울 뿐... 하지만 여기서는 display: list-item 이라 꼬이지 않는다.

    [float-CSS | MDN](https://developer.mozilla.org/ko/docs/Web/CSS/float)


## 해결방안 
**element에게 `float: left;`주기**

```
<!DOCTYPE html>
	<head>
		<style>
			ul {list-style: none;}
			li {float: left;/*inline-block element issue has been cleared by using float: left;*/ padding: 4px; border: 4px solid black; background-color: yellow;}
		</style>
	</head>
	<body>
		<ul>
			<li>1</li>
			<li>2</li>
			<li>3</li>
		</ul>
	</body>
</html>
```
