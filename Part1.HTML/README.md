## 1. HTML공부법

1. 인라인 요소, 블록요소 개념 사라져서 혼용가능하며 Flow Content, Phrasing Content로 표현가능
2. <a href="https://html.spec.whatwg.org">HTML을 유지하고 발전하는 커뮤니티</a> : 브라우저 제조사 연합. HTML표준은 탈권위적이고 살아있는 것이 됨.<br/>
   <a href="https://caniuse.com">부가적으로 브라우저 구현, 호환정도 확인 가능한 페이지</a> : 구현가능 기능 페이지


### Flow Content
 - body안에서 사용 가능한 컨텐츠

### Metadata Content
 - 콘텐츠와 문서를 구조화하는 요소를 의미. 다른 컨텐츠의 표시, 동작, 관계 등을 설정<br>
 ex) `base, link, meta, noscript, script, style, template, title`

### Heading Content
 - 섹셔닝 콘텐츠의 헤더<br>
 ex) `h1~h6, hgroup`

### Sectioning Content
 - 문서의 개요를 형성, 헤딩, 헤더, 풋터의 범위<br>
 ex) `article, aside, nav, section`<br>
 **헤딩 컨텐츠**를 함께 사용하면 명시적인 개요를 설정

### Phrasing Content(inline Content)
 - 구문콘텐츠, 단락을 형성하는 콘텐츠<br>
 ex) `link, meta, noscript, script, template...`

### Embedded Content
 - 외부 자원을 참조하는 콘텐츠<br>
 ex) `audio, canvas, iframe, img, math, picture, svg, video...`<br>
 **모든 임베디드는 Phrasing Content이다. 외부자원을 지원하지 않는경우 대체자원을 명시가능**

### Interactive Content
 - 사용자와 상호 작용할 수 있는 콘텐츠<br>
 ex) `a, audio, button, ...`

## 기타

1. Palpable Content : 비어있지 않은, 볼수 있는 콘텐츠(hidden)속성이 없는 콘텐츠
2. 투명한 요소 : 해당 태그가 빠져도 문제가 없어야 한다.


---
 
## 2. 검색엔진 밥상차려 주기

### SEO에 영향을 주는 요인들
1. **페이지 타이틀**
2. **메타 디스크립션**
3. SSL사용여부, 로딩속도, 노출 대비 클릭율, 백링크(다른 페이지로부터 인용되는 횟수), 사용자 경험(LCP: 최대 컨텐츠 블록 그리기, CLS: 누적 배치 변경)

### 2-1 **페이지 타이틀 명세**
```js
분류 : 메타데이터 컨텐츠.(<title></title>은 필수)
문맥: head 요소의 자식.
콘텐츠 모델: 텍스트, 공백만으로 구성할 수 없음.
태그생략 : 시작, 종료 태그 모두 생략 불가능.
```
**NOTE**: 타이틀과 명세는 자세히 요점만 간단하게 적어라! 그리고 `-`로 구분해서 적는거 추천

### **페이지 타이틀 Accessibility(웹 접근성)**
 - 화면 낭독기 사용자는 웹페이지 접속 시 타이틀을 음성으로 전달 받는다.
 - 페이지 타이틀을 통해 요청한 페이지 접속 여부 판단 가능 

 **NOTE**: 동적으로 추가된 헤딩도 잘 크롤링함.

 ### 타이틀 잘 적용된 사례

 ```js
 갤럭시 S | Samsung 대한민국
 All Standards and Drafts - W3C
 ```

 ## 요약
 ```js
 1. 본문을 가장 잘 설명하는 키워드 중심으로
 2. 페이지마다 구체적이고 고유한 키워드
 3. 페이지마다 반복하는 키워드 최소화
 4. 구체적 키워드 앞에 배치
 5. 가능한 짧게
 ```

 ### 2-2 **메타데이터** - html, head

 ```html
 <html lang="ko">
    <head>
       <meta charset="utf-8">
       <meta name="description" content="A description of the page">
       <meta name="viewport" content="width=device-width, initial-scale=1">
       <title>Page Title - Site name</title>
       <meta property="og:url" content="">
       <meta property="og:title" content="">
       <meta property="og:description" content="">
       <meta property="og:img" content="">
    </head>
    <body>
       <script type="application/ld+json">...</script>
    </body>
 ```
 1. lang : 웹접근성상 중요하다. 밑의 언어에 대해서 판단 가능(하지만 구글은 신뢰하지않음)
 2. utf-8 표준
 3. description: 검색결과 화면에 노출되는 텍스트 - 사이트에 대한 설명
 4. viewport: 모바일 디바이스에서 viewport설정. 상단의 뜻은 모바일 디바이스 크기만큼 뷰포트 잡아주는 기본속성
 5. property-og : sns 노출을 결정하는 영역
 6. application/ld+json: 네이버 노출 연관콘텐츠 등록가능


--- 

 ## 3. HTMl 개요 알고리즘 이해
 - 구글 크롬 HeadingsMap Extension을 사용하면 아웃라인을 쉽게 확인 가능
 - h1부터 h6까지 순차적으로 가야한다.

 ### Title vs Heading
 - title요소는 문서의 제목. 문서에서 한번만 사용가능
 - heading은 무서 개요를 형성하는 필수 아이템. (생략불가)

 ### Sectioning Content
  - <article> : 기사, 본문, 맥락 독립적으로 배포가능(독립적이중요)
  - <aside> : 페이지의 주요 내용이 아닌 내용
  - <nav> : 사이트의 네비게이션
  - <section> : 주제별로 나누거나 묶는
  **NOTE**: 적절한 헤딩 자식요소를 두어야함, 섹셔닝 요소를 적극 사용하되, 아웃라인 알고리즘 명세에는 의존하지 마라

### 명시적 섹션 vs 암시적 섹션
- 명시적 : 헤딩과 함께 섹셔닝 콘텐츠를 사용하여 섹션의 범위를 명시적으로 알 수 있는 섹션.
- 암시적 : 섹셔닝 컨텐츠를 사용하지 않고 헤딩만을 사용하여 암시적으로 개요가 생성된 섹션.


### 어색한 섹션
- 최상위 헤딩이 없는 경우, 헤딩이 건너 뛰어지는 경우
- 문법은 오류가 아니지만 아웃라인이 없게됨.

### 요약
1. 헤딩을 사용하고 헤딩과 섹션을 1:1로 맵핑하기
2. HTML5 개요 알고리즘에 의존하지 않기

---


## 4. HTML 의미론

### **div, span요소의 의미** ✨✨✨✨✨
```markup
div, span은 아무 의미없음.
이것을 많이 사용하는 것은 태그에 의미를 주지 않는 것이다.
```
**이는 선택할 요소가 없을때 사용할 수 있는 태그**
- div 대체할만한 태그 : `article, aside, nav, sectoin, dialog, hgroup, summary, figure, main, footer, header, details...`
- span 대체 태그 : `a, tile, data, mark, output, progress, strong, em, label, code, small...`

### Sectioning
- `article, aside, nav, section`과 헤딩을 함께 사용. 
- `hx, hgroup, header, footer` : 함께 쓰면 좋은 요소

### header, footer
1. `header` : 도입부,헤딩, 헤딩 그룹, 목차, 검색, 로고등
2. `footer` : 저자, 저작권, 연락처 등

### section, article ✨✨✨✨✨
1. `section` : 제목이 있는 **주제별 콘텐츠 그룹**
2. `article` : **독립적**으로 배포 가능한 완결형 콘텐츠(뉴스기사, 블로그 본문, 사용자의 댓글 등)
**NOTE** : `h1~h6`요소를 포함하는 것을 강력 추천, header, footer를 포함하는건 선택사항

### nav ✨✨✨✨✨
- 주요 탐색섹션(네비게이션)
- h1~h6 함께 사용 추천

### aside ✨✨✨✨✨
- 페이지의 주된 내용과 관련이 약해서 구분이 필요한 섹션
- ex) 배너, 페이지 바로가기 링크 등

### main
- 헤딩이 필요한 요소는아님(섹셔닝 콘텐츠만 필수)
- 문서의 핵심 주제, 핵심 기능과 직접 관련 있는 컨텐츠 영역을 의미
- header, footer요소의 범위와 관련없음.
- **body, div** 를 제외한 자손이 될 수 없다.

**NOTE**: <main><section><article>...</main>

### dialog ✨✨✨✨✨
- 팝업, 모달 등
- 열릴때 dialog 요소 안쪽으로 마우스, 키보드가 이동해야됨
- tab키 만을 사용해서 모달 컨트롤 할 수 있어야함

### figure ✨✨✨✨✨
- 문서의 주된 흐름을 위해 참조하는 독립적인 완결형 요소, 이미지, 도표, 코드드의 내용물과 설명을 포함한다.
```html
<figure>
   <img>
   <figcaption>...</figcation>
</figure>
```

### mark ✨✨✨✨✨
- 독자의 주의를 끌기 위한 하이라이트
- 검색결과 표시 등.

### address ✨✨✨✨✨
- 연락처 정보를 나타내기보다 footer에서 저작권정보등을 나타내는 태그
- p안에 address는 못들어옴. 반대는 가능

### ins, del ✨✨✨
- `ins`는 추가한 내용을 의미 `del`은 삭제한 내용을 의미
- 콘텐츠 모델이 투명

### progress ✨
- 로딩바, 진척도 바 등
- 낡은 브라우저를 위해 value값과 별도로 컨텐츠를 제공하는 것이 좋다.

### strong, em, del, ins 고려
- b는 강조할 생각없는 볼드체임

---

## 상호작용 컨텐츠의 올바른 용법

### `<a>` vs `<button>`
- 같은 외형이라도 a,button 요소를 구별해서 사용
- 실형결과를 가리킬 수 있는 **URL**이 있으면 `a` 요소.
- 참조할 URL이 없으면 `button`
- 커서 모양이 다름에 유의
- a가 버튼보다 지원하는 기능이 많다.
- `cursor: pointer`는 링크(a)일때만 주는게 맞다.

```html
<a href="" target="_blank"> 는 js를 통해 부모 창의 제어권한을 획득할 수 있음. 따라서 `window.opener` 객체를 제거해야함(탭가로채기 공격)
<a href="" target="_blank" rel="noopener noreferrer">를 통해 제어를 막을 수 있다.(최신브라우저는 자동 지원)
```

### `<details>`  vs  `<summary>`
- `details`요소는 열림 상태일 때 정보를 표시하는위젯, **open**속성을 넣으면 열린 상태로 표시.
- `summary` 요소는 details 요소의 나머지 부분에 대한 요약, 캡션, 범례를 의미

```html
<details open>
   <summary>
      ::marker
      "details dythfks?"
   </summary>
</details>
```

### placeholder
- `label`대신에 사용하지 말것. 필요한 설명은 label에 작성해야 웹 접근성 측면에서 좋다.

### datalist
- 다른 컨트롤을 위해 미리 정의된 옵션 세트를 의미한다.
```html
<label for="local">지역번호: <label>
<input type="text" id="local" value="?" list="local-list">

<datalist id="local-list">
   <option value="02" label="서울"></option>
   <option value="031" label="경기"></option>
</datalist>
```


--- 



## ⭐️⭐️⭐️ 이미지 마크업 최적화 ⭐️⭐️⭐️

**`<picture>`**요소를 잘 사용하기 

1. jpg, png : 모든 브라우저에서 지원하는 폴백 이미지.(과거의 유산)
2. webp : jpg/png 대비 30~70% 수준의 용량(하지만 IE미지원)
3. avif : 저용량+고품질(크롬, 안드로이드 지원)


### `<picture>` vs `<source>` vs `<img>`
1. `picture`
- 1-1 picture의 type 분기 ⭐️⭐️⭐️
- 알맞는 지원 포맷에 따른 이미지를 자동으로 아래처럼 분기처리 해줌.
```html
<picture>
   <source srcset="x.avif" type="image/avif">
   <source srcset="x.webp" type="image/webp">
   <img src="x.jpg">
</picture>
```
```js
위의 코드는 아래와 같다.
if(avif) x.avif
else if(webp) x.webp
else x.jpg
```
- 1-2 picture의 media 분기 ⭐️⭐️⭐️
```html
<picture>
   <source srcset="x.webp" media="(max-width:960px)">
   <img src="x.webp">
</picture>
```

- 1-3 picture의 resolution 분기
- 2배를 적합하게 분기처리 하고 아닌경우 1배수만 출력가능
```html
<picture>
   <source srcset="2x.webp 2x, 1x.webp" type="image/webp">
   <img srcset="2x.jpg 2x" src="1x.jpg">
</picture>
```

2. `<img>`
```html
<img 
   loading="lazy" //레이지 로딩 지원(이전에는 js로 구현했었는데 이제 지원함)
   decoding="async" // 디코딩 지원(이미지 디코딩을 병렬로 처리해서 이미지 외 다른 콘텐츠가 빠르게 표시되는걸 도움)
>
```

### 요약
1. avif 포맷을 제공하고 webp, jpg를 대체 수단으로 사용할 것
2. picture, source, img요소와 srcset, type, media 속성의 문법을 익혀둘것
3. 빠른 로딩 속도를 통해 UX를 개선하고 이미지 전송 비용을 절약할 것