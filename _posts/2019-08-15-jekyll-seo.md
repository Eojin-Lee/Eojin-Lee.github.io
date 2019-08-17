---
layout: post
title:  깃허브 지킬 블로그 SEO 작업
date: 2019-08-15
tags: jekyll seo
authors: eojinlee
---

## SEO란?

[**SEO**](https://ko.wikipedia.org/wiki/%EA%B2%80%EC%83%89_%EC%97%94%EC%A7%84_%EC%B5%9C%EC%A0%81%ED%99%94)의 뜻은 '검색엔진 최적화'인데, 사람들이 웹에서 사이트를 찾아보기 쉽도록 보완하는 작업이다. 링크 공유시 등장하는 미리보기도 SEO에 포함된다. 네이버 블로그처럼 대형 블로그 플랫폼은 SEO가 이미 잘 되어있지만, 이 블로그는 깃허브의 지킬(jekyll)을 사용해서 만든 설치형 블로그이기 때문에... 추가적인 세팅이 조금 필요하다.

처음하는 SEO 작업은 조금 느긋하게 일정을 잡고 진행해야한다. 검색엔진 또는 SNS 측에서 [**크롤링 (사이트 갱신 및 체크)**](https://ko.wikipedia.org/wiki/%EC%9B%B9_%ED%81%AC%EB%A1%A4%EB%9F%AC)하는 타이밍은 크롤러 맘이기 때문에... 답답한 마음에 SEO 작업 중에는 수시로 캐시를 제거해주곤 한다.

----

## robots.txt 생성하기

검색엔진 크롤러는 블로그를 읽기 위해 사이트의 [**robots.txt**](https://support.google.com/webmasters/answer/6062608?hl=ko)를 먼저 확인한다. `robots.txt`는 사이트를 읽을 크롤러의 권한을 설정하는 파일이다. 검색되기 원하지 않는 파일이 있거나, 크롤러의 검색 빈도를 조절하고 싶을 때, 또는 특정 검색엔진에서만 검색되고싶을 때 이 파일로 설정할 수 있다. 지금은 첫 SEO니까 기본 설정으로 잡는다. 

```
User-agent: *
Allow: /
Sitemap: http://사이트주소/sitemap.xml
```

생성한 `robots.txt`를 루트 폴더에 둔다. 이미 `robots.txt`가 있다면 해당 파일을 수정해주고 루트 폴더로 옮긴다.

----

## 구글에 블로그 등록하기

구글에 블로그를 등록하려면 [**Google Search Console**](https://search.google.com/search-console/about?hl=ko)에 블로그 주소를 등록해야 한다. Search Console에서 사이트를 등록하면 사이트의 트래픽이나 구글 기준의 웹 최적화 여부를 체크할 수 있다.

![google search console](/assets/post_image/imgs-jina/20190815/1.png)

### 1. Search Console에 사이트 소유권 인증하기

구글 ID로 로그인하고 들어가면 도메인과 URL 접두어 둘 중 하나의 속성 유형을 선택하라고 나오는데, 도메인은 사이트에 독자적인 도메인을 구매해서 사용하는 경우, 도메인을 구매한 사이트(고대디 등)에서 DNS 인증이 가능할 때 선택하면 된다. 지금은 도메인을 따로 구매하지 않았으므로 URL 접두어를 선택해서 진행한다.

![google search console - 속성유형선택](/assets/post_image/imgs-jina/20190815/2.png)

소유권 확인 팝업이 뜨면 구글이 제공하는 HTML 파일을 다운받아서 루트 경로에 추가하거나, `<head>`에 구글이 제공하는 메타태그를 메타태그가 있는 위치에 복붙하면 된다. 커밋 & 푸쉬 후 확인을 누르면 사이트가 인증된다.

![google search console - 소유권확인](/assets/post_image/imgs-jina/20190815/3.png)


### 2. Search Console에서 사이트맵 추가하기

[**사이트맵**](https://support.google.com/webmasters/answer/156184?hl=ko)은 크롤러가 사이트에서 더 중요한 정보를 읽을 수 있도록 도와주는 길안내 역할을 한다. 사이트맵을 추가해야 구글에 제대로 검색결과가 뜬다.

사이트 루트 폴더에 있는 `sitemap.xml`을 추가하면 끝. 파일이 있는지 웹에서 확인하려면 '사이트 주소/sitemap.xml'을 주소창에 입력하면 된다. 없다면 지킬에서 [**jekyll-sitemap 플러그인**](https://twpower.github.io/101-create-sitemap-and-feed-in-jekyll)을 설치한다.

![google search console - 사이트맵추가](/assets/post_image/imgs-jina/20190815/4.png)

----

## 네이버에 블로그 등록하기

네이버에 블로그를 등록하려면 [**네이버 웹마스터도구**](https://webmastertool.naver.com/)에 블로그 주소를 등록해야 한다. 웹마스터도구는 Search Console과 거의 비슷한 기능을 하지만, 네이버의 최적화 기준이 조금 더 까다롭다.

![naver webmaster tool](/assets/post_image/imgs-jina/20190815/5.png)

### 1. 웹마스터도구에서 사이트 소유권 인증하기

네이버 ID로 로그인하고 들어가면 바로 사이트 주소를 입력하고 추가한다. 구글과 동일하게 소유권을 인증하는 절차가 있는데, 루트 폴더에 HTML을 업로드하거나 메타태그를 `<head>`에 추가하면 된다. 구글과 다른 점은 잘 안 보이는 보안문자를 추가로 입력하라는 것.

![naver webmaster tool - 소유권확인](/assets/post_image/imgs-jina/20190815/6.png)

### 2. 웹마스터도구에서 사이트맵 추가하기

사이트맵은 구글에 추가한 것과 동일한 사이트맵 파일을 추가하면 된다. 

![naver webmaster tool - 사이트맵추가](/assets/post_image/imgs-jina/20190815/7.png)

### 3. 웹마스터도구에서 RSS 추가하기

문제는 RSS인데, 기존 지킬 템플릿에 내장되었던 jekyll-feed 플러그인이 만들어주는 `feed.xml` 파일은 네이버 웹마스터도구에 등록되지 않는다. (아니, 구글에서는 잘만 등록되던데 왜...?) 

![naver webmaster tool - RSS추가오류](/assets/post_image/imgs-jina/20190815/8.png)

어쩔 수 없이 직접 RSS 파일을 만들어야한다. 다행히 [**지킬에서 플러그인 없이 RSS파일을 만드는 방법**](https://jekyllcodex.org/without-plugin/rss-feed/)을 알아내서 해결할 수 있었다. 기존 jekyll-feed 플러그인은 주석으로 없애고, 새로 파일을 만들어서 루트파일에 위치시킨다. 

```
plugins:
# - jekyll-feed
  - jekyll-paginate
  - jekyll-sitemap
```

`<head>`에 아래와 비슷한 코드가 있는지 확인한다. (아마 기존에 feed.xml 파일이 있었다면 비슷한 코드가 있었을거다.) 로컬에서 feed.xml 링크가 잘 작동하는지 확인하고, 이상 없으면 커밋 & 푸쉬한다.

```
<link rel="alternate" type="application/rss+xml" href="{{ site.url }}/feed.xml" title="{{ site.title }}">
```

이제 웹마스터에 무사히 RSS를 제출하면 된다!

----

## 오픈그래프 적용하기

[**오픈그래프**](https://ogp.me/)는 페이스북에서 처음 만들어진 프로토콜로, 웹의 meta 정보들을 사용자가 알아보기 쉽게 표시할 수 있도록 만들어둔 규칙이다. 카톡으로 링크를 공유할 때, SNS로 링크를 공유할 때 카드 형태로 사진과 링크 제목이 표기되는데, 이게 오픈그래프 덕분이다. (몰랐다.) 이 블로그에 [**오픈그래프(Open Graph)에 대한 설명**](http://blog.airbridge.io/open-graph-as-a-website-preview/)이 자세히 되어있다. 페이스북, 트위터가 서로 지원하는 태그 형식이 다르기 때문에, 오픈그래프 적용 기준은 페이스북으로 잡되 트위터 기준 코드를 따로 작성해야 한다고...

![open graph in kakaotalk](/assets/post_image/imgs-jina/20190815/9.png)
> 요런 카드가 오픈그래프 덕분에 나오는 거였다.

### 1. Sharing Debugger에서 og metadata 점검하기

다행히도 기존 블로그 템플릿은 오픈그래프 태그가 이미 준비된 상태였고, 이걸 마음대로 응용하기만 하면 되었다. 페이스북의 [**Sharing Debugger**](https://developers.facebook.com/tools/debug/sharing)에서 현재의 og 데이터를 디버그할 수 있기 때문에, 오랫동안 헤메지 않고 오류를 정리할 수 있었다. 

![facebook sharing debugger](/assets/post_image/imgs-jina/20190815/10.png)

### 2. Sharing Debugger에 나타나는 오류 해결하기

`og:image`로 사용될 커버이미지들의 형식과 크기를 더 최적화시켰고, 기존에 url이 `site.url`로만 적용되는 이슈가 있어서 포스팅 페이지 경우의 수를 추가했다. 포스팅 타이틀도 기존 사이트타이틀이 계속 표시되도록 수정했다.

```
<title>{% if page.title %}{{ page.title }} | {{site.title}}{% else %}{{ site.title }}{% endif %}</title>
.
.
.
<meta property="og:url" content="{% if page.url %}{{ site.url }}{{ page.url }}{% else %}{{ site.url }}{% endif %}" />
```

커버이미지를 쓰지 않아도 포스트에 사용된 첫 번째 이미지를 og:image로 가져오고 싶었는데, 그러기 위해서는 `_config.yml`에 변수를 또 추가하고 관련 템플릿 파일들을 많이 수정해야할 것 같아서... 그건 다음으로 미루고 적용이 잘 되는지만 확인하고 마무리!












