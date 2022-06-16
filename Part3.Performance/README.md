## CSS 코드 최적화(CSS Optimization)
- 구글 `Light House` 사용히면서 문제를 해결한다.

**NOTE** : 사용하지 않는 css를 제거할것, 렌더 차단 리소스 제거

1) 사용하지 않는 CSS 제거 기법
- css는 페이지 렌더링을 차단하기 때문에 브라우저가 스타일을 계산하는데 잠재적으로 더 많은 시간을 소비하게 된다. 때문에 제거해야됨.
- 2KB이상 미사용 css를 lightHouse로 검출할 수 있다.
- **LightHouse 사용법**
```
LightHouse 탭 에서 Categories내 Performance 클릭 -> Device 내 Mobile 클릭 -> Generate Report
- Coverage에서(`command+shift+p` 단축키 누르고 입력후 새로고침) css만 확인가능
```
**빨간삼각형 경고 표시는 꼭 해결할 것**

2) 렌더 블로킹 자원 제거(Render blocking resources)
- 브라우저가 외부 리소르를 다운로드하고 파싱하는 동안 페이지 콘텐츠를 파싱하거나 렌더링하지 않기 때문에 페이지 표시 속도 저하의 원인이됨.
- 대표적으로 사용하지 않는 css가 렌더 블로킹을 가중하는 요인이다.
- 검사오류 `Eliminate render-blocking resources` 항목
- 렌더 블로킹 리소스 표시 조건 : `defer`, `async`속성이 없는 `<head>`요소의 script 태그 , media 속성과 값이 없는 `<link rel='stylesheet'>`태그
```html
<script async src=""> <!-- 병렬 다운로드, 즉시 실행 -->
<script defer src=""> <!-- 병렬 다운로드, 지연 실행(모두 그려지고 DOM이 들어왔을때, 끝나고)⭐️⭐️ -->
```
- 렌더 블로킹 `<link rel="stylesheet">` : **`media` 속성이 없거나 값이 all**이면 렌더 차단 리소스이다.
```html
다음은 렌더 차단 스크립트 
<link href="style.css" rel="stylesheet">
<link href="style.css" rel="stylesheet" media="all">

다음은 차단 안되는 스크립트
<link href="style.css" rel="stylesheet" media="orientation:portrait"> --> orientation:portrait 할때만 스크립트 해석
<link href="style.css" rel="stylesheet" media="print"> --> prinit할때만 스크립트 해석
```
**NOTE** : `media`를 통해서 데스크탑, 모바일에 따라 다른 스크립트를 적용할 수도 있다.

⭐️ Render Blocking media 를 통한 예제 1: **반응형 별 분기처리**
- 반응형인 경우 해상도 구간별로 css 파일을 분리하고 media 속성으로 분기한다.
```html
<link href="*.css" rel="stylesheet" media="(max-width:639px)"> - 모바일
<link href="*.css" rel="stylesheet" media="(max-width:640px) and (max-width:960px)"> - 태블릿
<link href="*.css" rel="stylesheet" media="(max-width:961px)"> - 데탑
``` 
⭐️ Render Blocking 처리를 통한 예제 2 : `preload` -> 굉장히 빠르게 적용됨
- 필수 스타일은 페이지 `<head>`에 `<style>`형식으로 작성.
- 지연 스타일은 `<link rel="preload">` 속성으로 병렬로딩 후 지연 적용.
```html
<style>/* 필수 스타일은 여기 */</style>
<link rel="preload" as="style" href="x.css" onload="this.onload=null; this.rel='stylesheet'">
```
**NOTE** : 외부 스타일 파일이 렌더링(`FCP(First Contentful Paint)`)을 차단하지 않음

⭐️  렌더 블로킹 `<script>`Tip
- 필수 스크립트는 html에 `<script>`형식으로 작성.
- 기타 스크립트는 `</body>` 종료 태그 직전에 선언.
- 마지막에 파싱해도 문제 없으면 `defer` 속성
- 가능한 빠른 시점에 실행 필요하면 `async` 속성


## 요약 ⭐️⭐️⭐️⭐️
1) 사용하지 않는 JS, CSS 제거
2) 필수 코드는 `<style></style>`, `<script></script>`에 작성할 것
3) 필수 아닌 JS는 `</body>` 종료 직전 위치를 고려해서 `defer`로 적용할 것
4) 필수 아닌 Css는 병렬 로딩(preload) 처리하고 지연적용 하기

---

## LCP(Largest Contentful Paint) : 가장 큰 덩어리 컨텐츠
- 구글 핵심 성능 지표
- 2.5초 이내 로딩시키는 것이 좋은 상태이다.

### LCP 개선 사례
- LCP 3.6초 개선한 이야기

1) 라이브러리 의존 줄이기
- `jquery`,`lodash`, `normalize`...
- 사용하는 것보다 안쓰는 unused 코드를 다 받아야 하기 때문에 렌더 블록의 요소로 적용될 수 있다.
- [라이브러리 바닐라로 바꿔주는 사이트](https://youmightnotneed.com)

2) 사용하지 않는 CSS 제거
3) `Preconnect`, `Preload` 기법 적용
```html
<link rel="preconnect" href="https://fonts.gatatic.com"> --> preconnect
=> 도메인을 알지만 자원의 최종경로를 모르는 경우 서버와의 연결을 미리 설정한다. 
DNS(Domain Name Server), TCP(Transmission Control Protocol), TLS(Transfer Layer Security) 왕복에 
필요한 시간을 단축하고 서드 파티 자원 연결에 적합하다. **** 보통 구글 웹폰트 참조에 많이 사용 ⭐️

<link rel="preload" as="style" href="x.css" onload="this.onload=null; this.rel='stylesheet'"> --> preload
=> 필요한 자원을 병렬 다운로드
as 속성을 함께 명시해 주어야함 (as="style", as="script", as="image")
```

- 히어로 이미지(가장 핵심 대문짝만한 이미지) preload
```html
<head>
    <link rel="preload" as="image" media="(max-width:640px)" href="small.avif">
    <link rel="preload" as="image" media="(min-width:641px)" href="large.avif">
</head>
```

4) Feature detection : Image type, Viewport width 감지
- 이미지 최적화 나쁜 예시
```html
   <img src="large.jpg" alt>
```
- 이미지 최적화 좋은 예시
```html
<picture>
    <!-- avif이면서 small 스크린인 경우-->
    <source srcset="small.avif" type="image/avif" media="(max-width:640px)">
    <!-- avif이면서 large 스크린인 경우-->
    <source srcset="large.avif" type="image/avif">
    <!-- webp이면서 small 스크린인 경우-->
    <source srcset="small.webp" type="image/webp" media="(max-width:640px)">
    <!-- webp이면서 large 스크린인 경우-->
    <source srcset="large.webp" type="image/webp">
    <!-- 다 없을 때 -->
    <img src="large.jpg" alt>
</picture>
```

5) `Image Loading`, `Decoding` 기법
- loading 과 decoding 적용할 것
- `loading="lazy"` 를 통해서 기존 js로 lazy loading 적용을 viewport 내부에 들어왔을때 띄울 수 잇음(viewport 1~2배 지점까지 근접하면 로딩된다.)
- `decoding="async"` 이미지 디코딩(복호화)를 병렬로 처리. 디코딩을 지연시켜 다른 콘텐츠의 표시 속도가 빨리진다.
```html
<img 
    src="x.jpg"
    loading="lazy"
    decoding="async"
>
```

## 요약 ⭐️⭐️⭐️
1)  LCP는 뷰포트에 표시하는 가장 큰 콘텐츠의 렌더링 성능이다 (보통 히어로 이미지)
2) 이 LCP를 2.5s안에 표시해야한다.
3) JS/CSS 라이브러리 의존도를 낮춰야 한다.
4) preconnect/preload 속성으로 외부 자원 최적화.
5) photo 요소의 type, media 속성으로 이미지 전송량 최적화.
6) loading/decoding 속성으로 이미지 렌더링 최적화.