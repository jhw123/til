# SEO (Search Engine Optimization)

## SiteMap
시맨틱 html, 메타 태그 등이 단일 페이지의 내용을 잘 이해할 수 있도록 돕는다면, 
사이트맵은 좀 더 포괄적으로 사이트 내 페이지끼리의 구조와 페이지들에서 사용되는 미디어 파일들의 메타 정보를 더욱 자세히 알려주는 역할을 한다.

크롤러가 사이트를 분석하는 방식은 먼저 외부에 url이 노출된 페이지로 접근한 뒤, 
그 페이지 내에 존재하는 링크를 통해 사이트 내 다른 페이지들을 방문하여 사이트의 이해해 나간다.
하지만 `<a>` 태그와 같이 명시적 링크를 사용하지 않고 스크립트를 통해 페이지 라우팅을 하거나 페이지끼리 분리가 되어 있는 구조라면, 
크롤러가 사이트의 모든 페이지를 찾아내기는 어렵고 이는 크롤러가 사이트를 제대로 이해하지 못하게 한다. 
이런 경우를 방지하기 위해 사이트 맵에 접근 가능한 페이지들을 나열해 크롤러가 빠짐없이 모든 페이지를 방문하고 분석하게 하여 보다 높은 이해도를 갖게 한다.
또한 아래와 같이 각 페이지마다 추가적으로 업데이트 주기와 사이트 내 페이지의 중요도 등을 명시해 크롤러가 효율적인 주기로 크롤링하도록 돕고, 
미디어 파일의 다양한 메타 정보도 추가하여 크롤러가 동영상이나 이미지의 내용을 함께 고려하여 사이트를 분석하도록 돕는다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"
        xmlns:video="http://www.google.com/schemas/sitemap-video/1.1">
   <url>
      <loc>http://www.example.com/videos/some_video_landing_page.html</loc>
      <lastmod>2021-01-10</lastmod>
      <changefreq>weekly</changefreq>
      <priority>1.0</priority>
       <video:video>
           <video:thumbnail_loc>http://www.example.com/thumbs/123.jpg</video:thumbnail_loc>
           <video:title>Grilling steaks for summer</video:title>
           <video:description>Alkis shows you how to get perfectly done steaks every
               time</video:description>
           ...
       </video:video>
   </url>
</urlset> 
```

### 참고
- [검색엔진 최적화(SEO) 초보자 가이드](https://developers.google.com/search/docs/beginner/seo-starter-guide?hl=ko)
- [사이트맵에 대해 자세히 알아보기](https://developers.google.com/search/docs/advanced/sitemaps/overview?hl=ko)

## Semantic Tags

### Nav Element
`<nav>`는 사이트의 다른 페이지로 이동하는 메뉴와 같이 링크가 있는 영역을 의미한다. 
가끔 메뉴 중 상위 메뉴 선택 후 하위 메뉴를 선택하는 UI도 있지만, 이러한 경우에도 `<nav>` 태그는 중첩해서 사용해선 안된다.
또한 `<nav>`는 네비게이션 링크를 담은 `<section>`이므로 내부에 `h1`, `h2`, 등을 사용할 수 있다.

```html
<nav>
  <ul>
    <li><a href="#">primary link</a></li>
    <li>
      <a href="#">primary link</a>
      <ul>
<!--  하위 메뉴가 있더라도 <nav>를 중첩해 사용하지 않는다. -->
        <li><a href="#">secondary link</a></li>
        <li><a href="#">secondary link</a></li>
      </ul>
    </li>
  </ul>
</nav>
```

### Article & Section Element

`<article>`은 독립적으로 존재할 수 있는 정보 영역을 나타낸다.
예를 들어 블로그 글이나 위젯과 같이 홀로 존재해도 온전히 정보를 전할 수 있는 경우 사용한다. 

`<section>`은 내용을 비슷한 내용을 가진 정보끼리 분리하는데에 사용한다.
예를 들어 블로그 글에서 서론, 본론, 결론이 있다면 각각을 `<section>`으로 나타낼 수 있다.

```html
<article>
    <header> ... </header>
    <p> ... </p>
    <aside> ... </aside>
<!-- <article>과 <section>은 서로 중첩해 사용할 수 있다.   -->
    <section>
        <article>...</article>
    </section>
</article>
```

### Aside Element

`<aside>`는 메인 정보 영역과 관련은 있지만 플로우 상 완전히 분리된 영역을 나타낸다.
추가 설명 영역, 사이드바, 또는 광고 영역의 경우 보조 기능을 제공할 뿐 사이트를 사용에 직접적인 영향을 주지 않는다.

### Header & Footer Element

`<header>`는 본래 `<h1>`, `<h2>`, ...와 같은 heading을 포함하는 영역으로 사용되지만,
페이지의 로고, 목차, 네비게이션을 포함하는 등 그 용례가 한정되어 있지 않다.
다만, outline 알고리즘에서 하나의 섹션으로 인식되지 않기 때문에 `<section>`처럼 정보를 단순히 분리하는 데에 사용하지 않기를 권고하고 있다.
예를 들어 아래와 같이 `<h1>`, `<h2>`가 각각 섹션으로 나누고 있고 이들이 `<header>`로 묶여 있어도 
`<header>` 밖의 `<p>`는 마지막 섹션인 `<h2>`에 연결되는 내용으로 취급한다.

```html
<header>
    <h1> ... </h1>
    <h2> ... </h2>
</header>
<p> ... </p>
```

`<footer>`는 `<header>`와 마찬가지로 일반적으로 목차, 연락처, 저작권 등의 정보를 담는 영역이지만,
그 용례가 한정되어 있지 않다.
`<footer>` 또한 섹션으로 인식되지 않는데 애초에 `<footer>`와 `<header>`는 마크업의 구조에 영향을 주는 요소들이 아닌,
마크업의 작성자가 보다 내용을 보기 편하고 쉽게 관리해주기 위한 요소이다.

### 참고
- [Using HTML sections and outlines](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Using_HTML_sections_and_outlines#The_HTML5_outline_algorithm)
- [4.3.4 The nav element - WHATWG.org](https://html.spec.whatwg.org/multipage/sections.html#the-nav-element)
