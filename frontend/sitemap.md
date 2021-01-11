# SiteMap

## 사이트맵이란?
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
