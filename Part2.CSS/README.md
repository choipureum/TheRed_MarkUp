# CSS 공부법

1. css가 어려운 이유?
- 시행 착오에 의존하고 요행을 바람. 문제를 해결해도 원인을 모른다. 

## 값 정의 구문 ⭐️
- css 는 값을 '제대로' 작성해야한다.

1. `keywords`
: 예약된 단어(따옴표 없이 사용. **대소문자 구별 안함**) ex) initial, inherit, unset, block, inline, inline-block, auto...
2. `<*>` 기본 자료형 
: ex) `<absolute-size>` - [xx-small | x-small | small | medium | large...]
3. `<'*'>` 그 이름의 속성 값을 참조
: ex) `<'grid-template'>` : grid-template 속성의 value 형식을 참조. 보통 단축 속성 값 정의 구문에 등장

## Combinators / Multipliers (결합기호, 증기기호)
1. 결합기호(Combinators)
- 공백(and) : 둘 이상의 값 필수, 순서 변경 X
- &&(and) : 둘 이상의 값 필수, 순서 변경 O
- || : 두 값 중 하나 이상 필수. 순서 변경 O
- | : 두 값 중 하나만
- [] :그룹
**NOTE** : 위에서 아래로 우선순위 
<br>
```js
a b | c d - 이거를 읽을때는
a [b | c] d - 이렇게 읽으면 안됨
[a b] | [c d] - 이렇게 우선순위를 가지고 읽어야함
```

2. 증가기호(Multipliers) : 뒤에서 앞에 속성에 대해 작성
- `*` : 횟수 제한 없음
- `+` : 1회 이상
- `?` :  0회 도는 1회
- `{A}` : 정확히 A회
- `{A,B}` : 최소 A부터 최대 B회까지
- `{A,}` : 최소 A회 필요, 최댓값 무제한
- `#` : 1회 이상. 값을 `,(콤마)`로 분리. 횟수 제한 가능. ex) `<length>#{1,4}`
- `[]!` : 그룹에서 적어도 1회 이상.<br><Br>
**NOTE** : 반복 제한 횟수보다 많은 값을 선언하면 무시됨


---


## `reset.css`이 필요한가

- `reset.css` : Eric A. Meyer가 2011년 작성한 reset코드를 기본으로 사용됨
- `normalize.css` : necolas가 2018년 작성된 리셋 코드

### Overridden / Unused CSS
- 대부분 굳이 선언하지 않아도 되는 스타일이 대부분이다.
- 대부분 Unused(사용하지 않는) css 가 되고 습관적으로 사용되는 것이 맞는가?

### Universal selector * reset (Do not)
- 전체 선택자는 사용하지 피하는 것이 좋다.
- 편할 수 있으나 이 스타일을 오버라이드하거나 불필요한 요소가 추가되는 결과를 낳는다.
```css
*,
::before,
::after{
    margin: 0;
    border: 0;
    box-sizing: border-box;
    ...
}
```

### Find unused CSS(Show Coverage)
- 크롬 개발자 도구에서 사용하지 않는 Unused css 커버리지를 볼 수 있다.
- `Elements`내 Coverage 검색 - `show Coverage`에서 커버리지 관측 가능

### 그렇다면 CSS 리셋은 어떻게 해야하는가
- CSS reset reinvent하기 

1. `overflow-wrap`
```css
body{
    margin: 0;
    overflow-wrap: break-word; // 단어가 공백없이 길어지면 중간에 절대 안자르고 깨지는걸 방지해줌(단어 중간에도 자를수있게)
}
/* Do not break Korean words */
:lang(ko) { word-break: keep-all; } //한국어는 중간에 깨지면 이상할 수 있음. 이를 방지
```
2. `img`
- 이미지 크기를 알맞게 제한
```css
img{
    max-width: 100%;
    height: auto;
}
```

3. `class`
- `class` 속성이 들어있는 태그만 
- 사용자들이 복붙했을때 뭔가 방지해 줄 수 있음. 충돌 방지
```css
// 현재 고려 가능한 reset코드. 하지만 불필요하게 아직도 overriden될 수 있음.
[class]{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    list-style: none;
    border: 0;
    background-color: transparent;
    border-collapse: collapse;
    border-spacing: 0;
    -webkit-appearance : none;
    appearance: none;
}
[class]::before,
[class]::after{
    box-sizing: border-box;
}

```
```css
// 필요한 부분만 맞게 적용 가능. where은 쿼리절 키워드랑 비슷한 역할. 하지만 아직은 삼성에서만 적용 가능.
[class]{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
[class]::before,
[class]::after {box-sizing: border-box;}
[class]:where(ol,ul) {list-style: none;}
[class]:where(button, fieldset, iframe, input, select, textarea) {border: 0;}
[class]:where(button, dialog, input, mark, meter, progress) { background-color: transparent;}
[class]:where(table){
    border: 0;
    border-collapse: collapse;
    border-spacing: 0;
}
[class]:where(button, input, meter, progress, select, textarea){
    -webkit-appearance : none;
    appearance: none;
}
```

## `BEM` 네이밍 기법
- 선택자의 이름과 중첩 규칙을 정하기에 도움을 줄 수 있다.
```html
Block : 재사용 가능한 독립적인 블록
Element: 블록을 구성하는 종속적인 하위요소.
Modifier: 블록, 요소의 변형(모양, 상태, 변형)
```
- 특징
1) 의미론적 클래스 선택자 작명규칙
2) 다른 형식의 선택자 사용을 제한
3) 전역에서 유일한 이름 권장
4) 낮은 선택자 특이성 유지
5) HTML/CSS 연결이 느슨. 병렬 개발 가능

- 명명규칙
1) 두개의 언더바(__*)는 하위 요소를 의미
2) 두개의 하이픈(--*)은 상태 변형을 의미
3) 하나의 이름에 요소, 변형은 각 한번만 허용
```css
.block{...}
.block__element{...}
.block__element--modifier{...}
.block--modifier{...}
```
**이 형식 이외 다른 형식은 허용하지 않음**

- 키워드 연결방법(camelCase 추천)
: 다른 라이브러리와 충돌 방지 가능
```css
.IzModal{...}
.IzModal__title{...}
.IzBtn{...}
.IzBtn--small{...}
```

- 잘못된 예시
```css
//의미를 파악할 수 없는 작명
.bx{...}
.cnt{...}
.mt{...}

// 전역 공간을 선점한 흔한 이름
.content{...}
.button {...}
.top{...}

// 선택 규칙을 잘못 관리한 사례
a{ text-decoration: none;}

// 끊임없는 overriden
.module a{ text-decoration: underline; }
#special.module a{text-decoration: none;}
#another#special.module a{ text-decoration: underline;}...
```

- 선택자 우선순위 규칙 ⭐️
- id(100) -> class, [attr], :class(010) -> type, ::element(001)<br>
**NOTE** : 선택자 점수를 낮게 유지하는게 관리하기 용이하다. (020)이하로 관리하는 것이 좋다.


- BEM 올바른 예시
```html
<button class="btn">
<button class="btn btn--submit">
<em class="info__label info__label--warning">
```
- BEM 잘못된 예시
1) 변형 클래스 단독 사용 불가
```html
<button class="btn--submit"> // 블록 btn 클래스가 없음
```
2) 타입선택자는 가급적 피해라
```css
.photo{...} 
.photo img{...}
.photo figcaption{...}
//올바르게 변형
.photo{...}
.photo__img{...}
.photo__figcaption{...}
```

3) 블록/요소 이름 생략 금지. 요소/변형 이름 중복 금지
```css
.__em{...}
.--modi{...}
.block__elem1__elem2{...}
.block--modi1--modi2{...}
```

### 요약
1) 의미론 작명법으로 읽고 이해하기 쉬움
2) 생소한 이름에 약어를 사용하지 않는다.
3) 특이성을 '020'으로 작게 유지
4) 선택자 이름은 전역 공간에서 유일
5) HTML/CSS 병렬 개발 가능


### Atomic/ Utility First CSS
- tailwindcss : 대표적인 유틸리티 클래스 css - html과 css의 연결이 강해져서 병렬개발 불가
- 특징
1) 라이브러리 타입으로 빠르게 구축 가능
2) 다른 방법론과 함께 사용가능
3) **스타일 관점의 작명. 의미론을 전혀 사용 안함**(강한 단점)
4) HTML코드에 스타일이 강하게 연결됨
5) HTML/CSS 병렬 개발 불가. 소규모 팀이나 단일 엔지니어 개발에 적합

---

## Layout, 배치

### Css display, position ⭐️⭐️⭐️
1. `display`
- block, inline-block, (`flow-root, flex, grid, contents`): 추가된 display(중요)

### Changed Display
1) `block`
**NOTE**: float: left|right 를 선언하게 되면 display가 무엇이든 block으로 변경이 된다.
- `block` : 기존에 수평으로 유지되던 컨텐츠가 수직으로 관리된다.(중복으로 할 필요가 없음)
- **수직** 흐름방향, **너비 O 높이 O 마진 O 패딩 O** 

2) `inline`
- **수평** 흐름, **너비X 높이 X 마진 O 패딩 O**

3) `inline-block`
- **수평** 흐름, **너비 O 높이 O 마진 O 패딩 O** 

4) `none`
- 어떤 장치도 표시 안함, 접근 불가
**NOTE** : `hidden` vs `none` - hidden속성은 display none설정과 동일하다
```html
<p class="desc" hidden>
==
.hidden{
    display: none;
}
// 이방법은 대신 접근성에서 안좋음- 표시안됨
```

5) `flow-root` ⭐️⭐️⭐️
- 보통 컨테이너에 선언 하면 좋을듯
- 블록 컨테이너로 됨
- float, margin 속성을 다르게 처리
- 포함한 float 요소는 컨테이너 끝에서 clear 됨
- **부모와 자식 요소의 수직 마진을 병합하지 않음(다른 display block은 중첩됨)**

6) `flex` ⭐️⭐️⭐️
- flex 컨테이너 박스를 생성
- flex 형식 문맥을 설정
- **포함 아이템을 1차원 기반으로 배치**

7) `grid` ⭐️⭐️
- grid 컨테이너 박스를 생성
- grid 형식 문맥을 설정
- **포함 아이템을 2차원 기반(표와 비슷)으로 배치**


### Position
1) `static`
- left,right,top,bottom, z-index X
- 배치 기준 없음, 흐름에 따라 배치

2) `relative`
- left,right,top,bottom, z-index, inset(left,right,top,bottom을 한번에 선언가능) O
- 박스의 **현재 위치**가 배치의 기준
- 배치를 변경할 때 다른 박스의 흐름을 깨지 않음.
- 자식 요소의 `absolute`배치 기준이 됨

3) `absolute`
- left,right,top,bottom, z-index, inset O
- 일반적 흐름에서 완전히 이탈
- 부모, 형제 크기나 위치가 전혀 영향을 미치지 않음.
- 조상 박스가 `relatvie`, `absolute`, `fixed`, `transform`일때 조상 기준으로 배치(애니메이션 처리시 transform이 들어갈때 absolute사용을 조심해야한다)

4) `fixed`
- viewport가 배치 기준
- 조상 요소에 `transform` 속성이 있으면 이를 기준으로 배치됨

5) `sticky`
- left,right,top,bottom, z-index, inset O
- scroll port가 배치기준
- 보통 따라오는 헤더에 많이사용
- 스크롤 포트에 보이는 동안 relative -> 스크롤 밖으로 이탈하면 고정을 멈춤, 부모가 안보이면 다시 사라짐
**NOTE**: z-index는 절대값이 아니라 상대값이다(부모보다 z-index는 높아질 수 없다.)

---

## Layout, 여백

### padding과 margin
- box-sizing: 초기값인 content-box는 패딩값은 별도이다. 그래서 크기와 높이를 설정하는것에 패딩은 포함되지 않음. border-box는 높이와 너비에 패딩값이 포함됨.

- reset.css에서 기존방식대신 아래와 같이작성 : unused , overriden되는 코드를 줄이고 충돌을 방지가능
```css
[class],
[class]::before,
[class]::after{
    box-sizing: border-box;
}
```

1) Relative Length
1. `vw, vh` : viewport를 기준으로 너비 결정(50이면 화면의 절반이다)
2. `vmax, vmin` : viewport의 너비와 너비중에 큰값을 선택해서 비율을 결정함

2) `padding`
- px나 %로 값 설정 가능
- **`%`는 자신이 아닌 부모요소 컨테이너 블럭의 너비 값을 참조**⭐️⭐️ - 이 말인 즉슨 padding-top도 height를 기준으로 하지 않고 width가 기준이다.

### KEEP ASPECT-RATIO(일정 비율 유지 기법)
- 종횡비 유지하기(보통 iframe으로 유투브를 넣게되면 기존에 너비가 고정되서 넘어온다)
- 이를 해결하기 위해
```css
iframe{
    width: 100vw; //다만 마진,패딩과 겹칠수잇어서 그부분은 잘릴수잇다.
    height: 56.25vw;
    aspect-ratio: 100/ 56.25; // ⚠️이부분은 아직 브라우저 지원 안함
}
```
- 그렇기 때문에 현업에서 가장 많이 활용하는 방법 ⭐️⭐️⭐️
```css
.utube{
    position: relative;
    padding-top: 56.25%;
}
.utube__iframe{
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
}
```
- 종횡비 유지 유용성 ⭐️⭐️⭐️
1) 누적배치변경(CLS) 문제 해결
2) 스켈레톤 UI를 제공할 때
3) 이미지 지연로딩 기법을 사용할때(이미지 높이가 로딩 전에 0으로 계산되기 때문에 로딩 이전과 이후 화면이 깨지는 거 처럼 보이는 것을 방지할 수 잇다.)
4) content-visibility: auto: 속성을 사용할 때
**NOTE** : 요소와 문서의 전체 높이를 일정하게 유지하는데 필요하다.


### margin
- 부모 컨테이너 블럭의 너비 값(width) 값을 참조 ⭐️⭐️⭐️

**NOTE** : **수직 마진 병합 규칙**을 조심해야함. 블럭 요소 사이에서 발생하고 양수끼리, 음수끼리 만난 경우 절대 값이 큰 값이 적용된다. 양수와 음수가 만난경우 두값의 합. 마진이 합쳐지는 현상(`flow-root`)⭐️⭐️로 해결가능
<br>-> 예외: 최상위 요소(body)의 수직마진, `flow-root`, 부모의 overflow: hidden|auto|scroll, display: inline-, float: left|right(수평정렬)


---

## float 속성
- 이제는 컬럼을 다루는 속성이 많이 추가 되어 현재는 IE지원을 위해 사용
1) 플로팅 요소의 너비는 수축하고 일반적인 흐름에서 벗어남
2) 블록 요소는 플로팅 요소와 겹치고 인라인 요소는 플로팅 요소 주변으로 흐름
3) clear, flow-root 속성으로 해제할 수 있다.(무조건 해제시켜 줘야한다.)
4) 무엇보다 컬럼을 배치하는 속성이 아님(하지만 이렇게 제일 많이 사용)


**NOTE** : 해제 시키기
```css
.container--after::after{
    content: '';
    display: block;
    clear: both;
}
.conteiner--flowroot{
    display: flow-root;(IE에서는 안됨)
}
```

## Column Layout
- `column` 프로퍼티 :  `column-gap`, `column-rule`, `columns` 설정가능
```css
.container{
    max-width: 640px;
    columns: 310px 2; //<'column-width'> || <'column-count'> 
    column-gap: 20px;
    column-rule: 20px solid #0002;
    background: #eee;
}
```
- `break-inside: avoid` : 박스 내부에서 길이가 길어져도 다음 컬럼으로 중간부터 넘어가지않고 끝까지 첫 컬럼에서 다 작성(디폴트는 넘어감)

## 요약
- floating은 용도에 맞게 사용하되 반드시 해제해야함. 
- flex|grid로 컬럼배치를 제어 해야함

**NOTE* : 게임추천(https://flexboxfroggy.com/#ko)(http://www.flexboxdefense.com/)


---


# Flex, Grid ⭐️⭐️⭐️⭐️⭐️
**NOTE** : `IE`를 지원하지 않는다면 고려할 수 있으며 중요하다.
- `flex`는 박스의 `크기,방향,순서,정렬,간격`을 제어하는 새로운 박스 모델.

1) `justify-content`
- 진행 축 정렬(center)시 행-> 열정렬 가능
2) `align-items` , `align-self`, `align-content`
- 교차 축 정렬
- 1줄, 여러개일때 하나만, 여러줄일때 정렬
3) `flex-direction`, `flex-wrap`, `flex-flow`
- 감싸기

## Flex 용어
1) `flex container` : display 값이 `flex`, `inline-flex`인 박스, 내부에 흐르는 자식은 자연스럽게 `flex item`이됨
```css
.flex-container{
    display: flex;
    flex-flow: row nowrap /* 흐름 방향 + 줄 바꿈 */
    flex-direction: row: /* 흐름 방향 */
    flex-wrap: nowrap; /* 줄 바꿈 */
    justify-content: flex-start; /* 진행 축 정렬 */
    align-items: stretch; /* 교차 축 정렬 */
    align-content: stretch; /* 여러 줄 교차 축 정렬 */
}
```
2) `flex item` : `flex-container`안에 흐르는 아이템
```css
.flex-item{
    flex: 0 1 auto; /* 팽창지수 + 수축지수 + 기준 크기 */
    flex-glow: 0; /* 팽창 지수 */
    flex-shrink: 1; /* 수축 지수 */
    flex-basis: auto; /* 기준 크기 */
    align-self: auto; /* 독립적 교차 축 정렬 */
    order: 0; /* 배치 순서 */
}
```
3) `free space`⭐️ : `flex-item`이 점유하는 영역을 제외하고 남은 공간(마진 아님), `flex-grow`, `flex-shrink`를 통해 flex-item으로 분배할 수 있음(FS라고 함)
4) `main axis`⭐️ : 진행축, 플렉스 아이템 배치 방향으로써 플렉스 컨테이너에 `flex-direction`을 적용하여 설정 가능. 초기값은 `row`
5) `cross axis`⭐️ : 교차축, 진행축의 반대(align-*)의 기준이 되는축

## `flex-item`의 팽창과 수축
```css
flex-grow:0 /*수축되지 않음(기본값)*/
flex-shrink : 1;
flex-basis: auto;/* 보통 auto*/
```

1) `flex-grow`
- 양의 FS 발생 시 플렉스 아이템의 팽창을 제어. `0또는 1`사용(2++도가능)
- 초기값 0
- 적용: `flex-item`
- 0일때는 flex-container에 상관없이 보통 본인의 크기, 1일때는 container맞게 1:1 비율로 균등팽창
- 물론 1, 2로 두개에 설정하면 1:2 배율로 팽창됨

2) `flex-shrink`
- 음의 FS발생시 플렉스 아이템의 수축을 제어한다. 보통 `0또는 1` 사용
- 초기 값 1(건드리지 않는 걸 추천한다)
- 적용: `flex-item`

3) `flex-basis`
- 플렉스 아이템의 진행 방향 기본 크기를 설정함으로써 FS 초기값에 영향을 준다.
- 값 width
- 초기 값 auto(건드리지 않는 걸 추천한다)
- 적용: `flex-item`

4) `flex`
- flex 아이템의 팽창, 수축, 기본 크기 를 제어하는 **단축 속성**
- 값: none | [<'flex-grow'> <'flex-shrink'>? || <'flex-basis'>] 
- 초기 값: 0 1 [auto]

**NOTE** : 실무에서 사용할 확률 높은 `flex`⭐️⭐️
```css
.item{ flex:1 }⭐️⭐️⭐️⭐️
```

## `flex-item`의 방향과 순서
```css
flex-direction : row | column
flex-wrap : nowrap(기본값, 줄바꿈 자동으로 안됨) | wrap(줄바꿈) 
flex-flow : <'flex-direction'> || <'flex=wrap'> ⭐️ /* 단축 속성 */
order : 순서변경
```
**NOTE** : 빈번하게 사용되는 코드
```css
flex-flow: row wrap;
flex-flow: column wrap;
```


## flex-item의 정렬과 간격
```css
justify-content: center⭐️ | space-between⭐️ | flex-start(초기값) | space-around | space-evenly | flex-end
align-items: center⭐️ (자기크기로 중앙정렬) | stretch(초기값)⭐️(교차축으로 꽉채워서 자동으로 늘어남) | flex-start 
align-self: auto | cneter | stretch (아이템 중 단 1개만 적용)
align-content: center⭐️ | sapce-between⭐️ | space-around | space-evenly | flex-end(align-items와 다르게 여러줄일때)
gap⭐️ : margin과 비슷, <'column width'> || <'row width'> (10px 20px)
```

### 요약
- `flex` 단축 속성 권장 
- `justify-content` , `align-items` 속성을 통해 정렬방식을 이해하고 align-self, align-content, flex-direction, flex-wrap등을 학습할것
- 언급 안했던 reverse 사용하지 말것 

**NOTE**: [게임 예제](https:/cssgridgarden.com/#ko)