# 웹접근성 대표 에러 12가지 경험 공유
- [한국지능정보사회진흥원](https://t.ly/EnwZ) 에서 모든 웹접근성 목록과 소스 확인 가능.

### 적절한 대체 텍스트 제공
- 텍스트 아닌 콘텐츠는 그 목적에 상응하는 대체 텍스트를 포함한 상태로 표시해야 한다.(`WCAG 2.X` 준수) - 준수율 24.1%
1) 장식 목적 이미지 대체 텍스트 제공 금지
```html
<img src="" alt="장식"> ❌
<img src="" alt> 👌
```
2) 주변 문맥을 통해 이미 충분히 설명하고 있는 내용은 대체 텍스트 불필요, 중복금지<br>
ex) 약도 같은 건 필요없음
```html
<a href="">
    <img src="" alt>👌
    우리 사이는
</a>
```
**NOTE** : alt를 작성하면 우리 사이는 을 2번 읽게 됨

3) Image Replacement<br>
 css 배경 이미지에 의미가 포함됫는데 img가 아닌경우 IR기법 사용
```css
.ally{
    /* 이렇게 hidden 처리해줄 것 */
    position: absolute;
    opacity: 0;
    /* 아래는 공학기가 읽지 않음.*/
    display: none; ❌
    visibility: hidden; ❌
    width | heigth: 0; ❌
    font-size: 0; ❌
}
```

4) Bypass Blocks<br>
- 여러 웹페이지에서 반복되는 컨텐츠 블록을 우회할 수 있어야 한다.
- 키보드 사용자가 tab 키를 누르면 메뉴건너뛰기와 같은 링크가 나와서 편하게 메뉴를 스킵할 수 있음
```css
/* 모바일, 테블릿에서는 아예 막아둠*/
@media(max-width:960px) {
    .skipToMain{display none;}
}
/* 컴퓨터 */
@media(min-width: 961px){
    .skipToMain{
        display: block;
        /* 화면에 존재해도 안보이게 됨 */
        height: 1px;
        overflow: hidden;
        margin-bottom: -1px;
        /* */
        text-align: center;
        line-height: 48px;
    }
    /* 키보드 포커스가 들어가면 보이게됨 */
    .skipToMain: focus{
        height: auto;
        margin-bottom: 0;
    }
}
```

5) 표의 구성
- 표는 이해하기 쉽게 구성해야 한다.
```html
<table>
    <!-- 캡션 제공할 것 -->
    <caption>대한민국 지역별 인구 통계</caption>
    <thead>
        <tr>
            <!-- th 맞게 사용하기 -->
            <th scope="col">년도</th>
            <!-- th는 scope를 함께 사용해야한다. columnd 헤더라는 뜻-->
            <th scope="col">서울</th>
            <th scope="col">대전</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <!-- row 헤더라는 뜻-->
            <th scope="row">2021</th>
            <td>내용1</td>
            <td>내용2</td>
        </tr>
        <tr>
            <th scope="row">2020</th>
            <td>내용1</td>
            <td>내용2</td>
        </tr>
    </tbody>
</table>
```

6) label 제공
```html
<input value="아이디"> ❌
<input placeholder="아이디"> ❌

<!-- 예제 1 -->
<input id="xxx">
<label for="xxx">아이디</label> 👌 (추천)

<!-- 예제 2 -->
<input aria-label="아이디"> 👌(이거보단 예제 1 사용할 것)
```

7) 자막제공
- 녹음된 음성, 영상 컨텐츠에 자막을 제공해야 한다.
- 미디어가 문자를 대체하기 위한 설명이면 예외이다.

```html
<video poster="myvideo.png" controls>
    <source src="*.mp4" srclang="en" type="video/mp4">
    <!-- track 속성과 vtt 포맷으로 자막 제공 가능 -->
    <track src="*.vtt" kind="captions" srclang="en" label="English">
</video>
```
- 영상에 자막을 입히기 어려운경우 `대본 제공`이나 `수어제공`으로 허용 가능

8) Page Title / Heading and Labels ⭐️⭐️
- 페이지는 주제나 목적을 설명하는 제목이 있어야 한다.
- 제목과 레이블은 주제 또는 목적을 설명해야 한다.

```html
<title>XX 홈페이지에 오신 것을 환영합니다! </title> ❌
<title> %^& 특수 문자 타이틀</title> ❌

<title>다른 페이지와 중복 없는 고유한 제목</title>
```
- 헤딩은 h1, h2, h3 잘 나누어서 사용할 것. (예를들어 h3는 2보다 중요한 헤딩이면 안됨)

**NOTE** : `iframe`제공 방법
```html
<iframe src="*.html"></iframe> ❌

<!-- aria-label을 통해서 제공, label및 헤딩 타이틀은 iframe 쪽에서 추천하지 않음 -->
<iframe src="*.html" aria-label="공시 자료"></iframe> 👍
<iframe src="*.html" aria-label="빈 프레임"></iframe> 👍
```

9) 정지 기능 제공
- 콘텐츠에 시간 조절이 들어가 있다면 `끄기, 조절, 연장` 이 들어가 있어야 함
- 자동으로 변경되는 컨텐츠는 움직임을 제어할 수 있어야 한다(슬라이드 배너) : `이전, 다음, 정지` 기능을 제공할 것<br>
ex) 보통 사용자가 만지면 자동재생 멈춤, 다른데 포커스가 가면 다시 자동재생 on

10) 모든 기능은 키보드 인터페이스로 조작이 가능해야함
- **장치 독립적 이벤트 핸들러**를 사용해라.⭐️
```js
//장치 독립적 이벤트 핸들러
onblur
onchange
onclick
onfocus
oninput
onselect
```

11) 키보드로 초점 이동
- 포커스는 키보드 인터페이스 만으로 구성요소 이동과 **떠날 수** 있어야 한다.

12) 링크나 input, button 등에 포커스가 갔을때 시각적으로 표시되야함(블루테두리)
- 고의적으로 없애는 경우가 있는데 없애면 안된다.(`outline: none`을 하면 안됨)

13) 명도 대비
- 문자와 문자 이미지의 시각적인 표현은 최소한 4.5:1의 명암 대비를 부여해야 한다.
- 글꼴 크기는 16px정도 되는 글꼴 이상이 제일 좋음.

14) 주로 사용하는 언어를 명시해야 한다.
```html
<html lang="ko"> 👍
```

15) 오류 정정
- 입력 오류가 감지되면 오류 항목을 인지하고 해결법을 알려줘야함<br>
ex) 이메일 형식이 아닙니다, 패스워드를 확인해주세요 등등

---

## ARIA
- 접근 가능한 고기능 인터넷 애플리케이션
- HTML에 역할, 상태, 속성 정보를 부여하여 보조기기의 웹 문서 접근을 지원.

**NOTE** : 웹 접근성 문제의 80%는 HTMl 문제이다. ARIA를 사용하기 앞서 HTML을 제대로 사용했는지 볼 것

1) 실전 ARIA - `role`
```html
<!-- tab | tablist | tabpanel  : 탭 스타일을 의미하는 것이 아님(nav | aside > h2+ul 로 마크업) -->
<element role="tablist"> ⭐️ - 전체 그룹핑
<element role="tab"> ⭐️ - 탭 하나하나 
<element role="tabpanel"> ⭐️ - 탭에 따른 밑에 내용

<!-- tooltip : 초점이 이동하면 안됨/ 마우스를 빼거나 ESC시 사라져야함 -->
<element role="tooltip"> ⭐️

<!-- status : alert과 함께 기억 할것, 실시간 결과 정보이지만 alert만큼 중요한 사항은 아님 , 초점을 받으면 안됨-->
<element role="status"> ⭐️  
<!-- alert :  시간에 민감하고 중요한 실시간 콘텐츠 -->
<element role="alert"> ⭐️

<element role="alertdialog"> == <dialog>
<element role="dialog"> == <dialog>
<element role="navigation"> == <nav>
<element role="complementary"> == <aside>
<element role="none"> == <div>
```

- `tooltip`
```html
1 - 인풋 툴팁
<label for="tel">전화번호</label>
<input id="tel" type="tel" aria-describedby="TIP-TEL">
<p id="TIP-TEL" role="tooltip" hidden> 하이픈(-)없이 숫자만 입력.</p>

2 - 인풋 툴팁
<button aria-describedby="TIP-DEL">게시물 삭제</button>
<p id="TIP-DEL" role="tooltip" hidden> 게시물 삭제 후 복원할 수 없음 </p>
```

- `status` : 자동으로 두가지 속성이 적용됨 - `aria-live="polite"`(이미 진행중인 음성을 끊지 않고 기다렷다가 전달) , `aira-atomic="true"`(내용이 갱신되었을때 문장을 다시 읽어줌)
```html
<p role="status">회원가입 양식 전송완료</p> - 토스트 같은 느낌에 적용하면 좋다.
<p role="status">송금 얼마 성공</p>
```

- `alert` : 자동으로 두가지 속성이 적용됨 - `alia-atomic="true"`, `aria-live="assertive"`(진행중인 음성을 끊고 바로 전달)
```html
<p role="alert">로그인 후 이용 가능</p>
<p role="alert">송금 실패</p>
<p role="alert">입력 오류</p>
```

2) 실전 ARIA - `status`
```html
<element aria-current="page | step | location | data | time | true | false(default)"> ⭐️ 
<element aria-selected="false | true | undefined(default)"> ⭐️ 
<element aria-haspopup="true | menu | dialog | listbox | tree | grid | false(default)"> ⭐️ 
<element aria-expanded="false | true | undefined(default)"> ⭐️ 
<element aria-hidden="false | true | undefined(default)"> ⭐️ 
<element aria-invalid="false(default) | true | grammer | spelling"> ⭐️ 
```

- `aria-current` : 현재 맥락과 일치하는 항목을 의미
```html
page : 현재 페이지와 일치하는 시각적으로 강조한 링크⭐️ 
step : 현재 단계오 일치하는 시각적으로 강조한 링크 ⭐️ 

<!-- 현재 페이지 링크-->
<nav>
    <h2>글로벌 네비게이션</h2>
    <ul>
        <li><a href="/home" aria-current="page">홈</a></li>
        <li><a href="/ongoing">연재</a></li>
        <li><a href="/ranking">랭킹</a></li>
    </ul>
</nav>

<!-- 현재 단계 링크-->
<nav>
    <h2>회원 가입</h2>
    <ul>
        <li><a href="/accept-terms" aria-current="step">약관 동의</a></li>
        <li><a href="/id-password">아이디 비번 생성</a></li>
        <li><a href="/email-authentication">이메일 연동</a></li>
    </ul>
</nav>
```

- `aria-selected` : 단일, 다중 선택이 가능한 요소에 선택 상태를 명시 (`role = tab`요소에 대부분 사용)
```html
<div role="tablist">
    <a id="mon-anchor" href="#mon" role="tab" aria-selected="true">월</a>
    <a id="tue-anchor" href="#tue" role="tab" aria-selected="false">화</a>
</div>
```
**NOTE** : `aria-selected='true'인 요소에만 css style도 적용가능` 해서 유용하다

- `aria-haspopup` : 요소에 연결되어 있는 팝업 정보를 제공.(menu, dialog가 대부분)
```html
<!-- menu | true-->
<button type="button" id="menu-button" aira-haspopup="menu" aira-controls="menu-list" aria-expanded="false">메뉴</button>
<ul>
    <li>
        <a href="/completed">완결</a>
    </li>
</ul>

<!-- 로그인 팝업 창 dialog-->
<a href="#login-dialog" aria-haspopup="dialog">로그인</a>
<section id="login-dialog" role="dialog" aria-labelledby="login-heading" aria-modal="true" hidden>
    <h2 id="login-heading">로그인<h2>
</section>
```

- `aria-expanded`: 대상이 축소/ 확대 상태를 표시, `aria-controls`속성(제어대상)과 함께 사용(팝업이나, 툴팁에 많이사용)
```html
<!-- 어코디언 ui-->
<dt>
    <button type="button" aira-controls="answer-99" aria-expanded="false">보너스 코인은 언제 소진되나요?</button>
</dt>
<dd id="answer-99" hidden>
    <p>만료 기한이 짧은 코인은 먼저 소진 됩니다.</p>
</dd>

<!-- 팝업 -->
<a id="popular-btn" href="#popular" aria-haspopup="menu" aria-expanded="false">인기</a>
<ul id="popup" role="menu" aria-labelledby="popular-btn" hidden>
    <li><a href="#romance">로맨스</a></li>
</ul>
```

- `aria-hidden` : 보조기기에 정보를 전달하지 않는다, 접근성 API 차단(true)/사용(false)
```html
<body>
    <div class="container" aria-hidden="true">
        //보조기기가 무시하는 영역
    </div>
    ...
</body>
```

- `aria-invalid` : 주로 `<input>`에 사용. 사용자가 입력한 값이 요구하는 형식과 일치하는지 여부를 선언<br>
cf) false(오류없음) 인 경우 그냥 선언하지 않고 true인 경우만 선언해준다
```html
<label for="email">이메일</label>
<input id="email" type="email" required value="..." aria-invalid="true" aria-errormessage="email-error-msg">
<p id="email-error-msg" aria-role="alert" aria-live="assertive">이메일 형식이 유효하지 않습니다.</p>
```


3) ARIA - 속성(properties)
```html
<element aria-controls="ID reference list"> ⭐️
<element aria-live="polite | assertive | off(default)"> ⭐️
<element aria-labelledby="ID reference list"> ⭐️
<element aria-label="string"> 
<element aria-describedby="ID reference list"> ⭐️
<element aria-errormessage="ID reference"> ⭐️
<element aria-modal="true | false(default)"> ⭐️
```

- `aria-controls` : `role="tab"`, `aria-haspopup`, `aria-expanded`속성과 함께 명시. `<button>`이 무엇을 제어하는지 명시한다.
```html
<button type="button" id="mon-anchor" aria-controles="mon" role="tab" aria-selected="true">월</button>

<button type="button" aria-haspopup="dialog" aria-controls="login-dialog">로그인</button>

<button type="button" aria-controls="answer-99" aria-expanded="false">보너스 코인은 언제 소진되나요</button>
```

- `aria-live`: 실시간으로 내용을 갱신하는 영역(polite-중요도 낮음, assertive-중요도 높음)
```html
<div role="alert" aria-live="assertive">
    <p>로그인 후 이용할 수 있습니다.</p>
</div>
```

- `aria-labelledby` : label을 대체한다. ID값을 이용하여 내용을 참조(hx, a, button 요소를 참조하면 적절하다.)
```html
<section aria-labelledby="LZ-PATH" hidden>
    <h2 id="LZ-PATH">레진패스란?</h2>
    <p>이 작품이 유료 에피소드 열람 시 자동으로 구매합니다</p>
</section>

<a id="LZ-PATH" href="#LZ-PATH-TEXT">레진 패스란</a>
<div id="LZ-PATH-TEXT" aria-labelledby="LZ-PATH" hidden>
    <p>이 작품의 유료 에피소드 열람 시 자동으로 구매합니다.</p>
</div>
```

- `aria-label` : 간결한 설명/ 그래도 `aria-labelledby`를 왠만해선 적용해라
- `aria-describedby` : 자세한 설명, 장황한 설명(툴팁, 인풋 등에 적용)
```html
<button aria-describedby="TIP-DEL">게시물 삭제</button>
<p id="TIP-DEL" role="tooltip" hidden>게시물 삭제 후 복원할 수 없음.</p>
```

- `aria-errormessage`: `<input>` 요소에 선언, `aria-invalid`와 보통 함께사용 / 오류 원인과 해결방법을 포함해야한다.
```html
<label for="email">이메일</label>
<input id="email" type="email" required value="..." aria-invalid="true" aria-errormessage="email-error-msg">
<p id="email-error-msg" aria-role="alert" aria-live="assertive">이메일 형식이 유효하지 않습니다.</p>
```

- `aria-modal` : 요소가 모달인지 전달
```html
<div role="alertdialog" aria-modal="true" aria-labelledby="TITLE" aria-describedby="DESCRIPTION">
    <h2 id="TITLE">레진 패스 안내</h2>
    <p id="DESCRIPTION">이 작품의 유료 에피소드 열람 시 자동으로 구매합니다. 레진패스를 적용하시겠습니까?</p>
</div>
```

## 요약
- ARIA는 역할,상태,속성 정보를 통해 HTML의 웹접근성 문제를 해결하는 방법이다.