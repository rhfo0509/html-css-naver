# 네이버 클론코딩

## block vs inline-block vs inline

* block

`div` 태그의 경우 `display`의 기본값은 **`block`**<br>
`block`은 `width`를 100%를 차지하기 때문에 `width`를 따로 지정하는 경우 나머지 부분은 `margin`으로 남게 된다.

* inline-block

`div` 태그의 `width: 100%` 특성을 없애기 위해서 `display: inline-block`으로 설정하면 직접 지정한 `width` 값이 적용되어 `margin` 부분이 사라지게 된다.<br>
이제 부모 태그에서 적용했던 `text-align: center`가 정상 적용되어 가운데 정렬이 되는 것을 확인할 수 있다.

* inline

`display: inline`의 경우 `width`와 `height`를 무시하고 내부 콘텐츠의 영역만을 차지한다.

---

## 브라우저의 기본 css 없애기

브라우저마다 다르지만 `html`이나 `body` 태그에 `margin`이나 `padding` 값이 기본적으로 들어있다. (chrome의 경우, `body` 태그에 `margin: 8px`이 적용)

```css
html, body {
  margin: 0;
  padding: 0;
}
```

이렇게 적어주면 기본 css를 없앨 수 있다.

---

## 이미지 스프라이트 + background로 위치 조절하기

### 이미지 스프라이트를 쓰는 이유

**이미지 스프라이트**는 여러 개의 작은 이미지를 하나의 큰 이미지로 결합하여 사용하는 방법<br>

여러 이미지를 사용하는 경우, 각 이미지마다 HTTP 요청을 하게 되면 특히 "구형 브라우저"의 경우 성능 저하를 초래하게 된다.<br> 
따라서 이미지 스프라이트의 `background-position` 속성으로 개별 이미지를 잘라서 사용하면 웹 페이지의 성능 개선과 사용자 경험이 향상될 것이다.

### background로 위치 조절하기

* `background-repeat`의 기본값은 `repeat`이라서 이미지가 연속적으로 반복되어 연결된 것처럼 보이게 된다. -> `no-repeat`으로 더 이상 반복되지 않도록 설정

* `background-size`로 미리 사이즈를 지정한 후에 `background-position`으로 나타낼 이미지의 위치를 조절한다.

> 이 때 `padding`을 적용하면 늘어난 사이즈만큼 이미지 스프라이트가 보이기 때문에 부모 태그를 만들어서 `padding`을 적용하여 해결한다.

---

## padding vs margin 결정하기 + CSS 변수(custom property)

### padding vs margin

`padding`은 `border`와 함께 요소에 포함되는 내부 영역이고, `margin`은 요소에 포함되지 않은 외부 영역이다.<br>
`padding`을 줌으로써 선택될 수 있는 영역을 확장시킬 수 있다. (`margin`으로 준 부분은 선택되지 않음)

### CSS 변수

┌ 네이버에서 사용하는 CSS 변수
 ```css
 :root {
    --color_title: #080808;
    --color_title_rgb: 8,8,8;
    --color_body: #101010;
    --color_body_rgb: 16,16,16;
    --color_caption1: #404040;
    --color_caption1_rgb: 64,64,64;
    --color_caption2: #606060;
    --color_caption2_rgb: 96,96,96;
    --color_search: #000;
    --color_search_rgb: 0,0,0;
    ...
  }
 ```

* 값을 외울 필요 없이 변수명만으로도 쉽게 값을 사용할 수 있음
* 변수에 해당하는 값을 바꾸면 그 변수를 사용하는 곳에도 전부 적용이 되기 때문에 유지보수성이 높아짐

---

## 각종 position의 차이 알기 + ::before / ::after

### position

모든 태그들의 `position` 기본값은 `static`이며, 이 경우 요소는 일반적인 문서 흐름(위에서 아래로, 왼쪽에서 오른쪽)에 따라 배치된다.

* `position: relative` -> 일반적인 문서 흐름에 따라 배치되며, `top`, `right`, `bottom`, `left` 속성을 사용하여 원래 위치에 대해 상대적인 위치를 조정할 수 있다.

* `position: absolute` -> `top`, `right`, `bottom`, `left` 속성을 주게 되면 **`position`이 `static`이 아닌 부모 요소**를 기준으로 배치되며, 존재하지 않을 경우 문서의 초기 위치(`body`)가 기준이 된다.

* `position: fixed` -> 요소가 뷰포트의 상대적인 위치로 고정되며 스크롤해도 화면에 고정된 상태로 유지된다.

* `position: sticky` -> 평소에는 `static` 상태였다가 스크롤 시 지정한 임계점에 다다르게 되면 `fixed` 상태가 됨

### ::before / ::after

`::before`와 `::after`는 가상 요소(`pseudo element`)의 일부로, 요소 내용 제일 앞이나 뒤에 새 컨텐츠를 추가할 수 있다.

`::before`와 `::after`를 이용하면 HTML 태그나 자바스크립트 없이 CSS만으로 컨텐츠뿐만 아니라 디자인 요소를 추가할 수 있는 특별한 기능을 하게 된다.

---

## z-index + display: none을 쓰면 안될 때(웹 접근성)

### z-index

> `position: static`를 가진 요소는 기본적으로 `z-index` 값을 가질 수 없기 때문에 `position: static`를 가진 요소는 다른 `position` 값 (예: `relative`, `absolute`, `fixed,` `sticky`)를 가진 요소보다 항상 더 아래에 쌓이게 된다.

따라서

```css
#header-hamburger:hover::before {
  position: absolute;
  top: 1px;
  left: 1px;
  width: 44px;
  height: 44px;
  content: '';
  background-color: var(--color_option_bg);
  border-radius: 50%;
}
#header-hamburger:hover > div {
  position: relative;
}
```

* `#header-hamburger:hover`의 자식 태그 중 햄버거 메뉴를 표시하는 `div` 태그를 `position: relative`로 설정해서 `::before` 태그보다 위에 위치하도록 한다.<br>
(기본적으로 `position: relative`만 설정하는 경우 위치가 변하지 않음)

* 아니면 `div`의 `z-index`를 `1`로 설정하고, `::before`의 `z-index`를 `0`으로 설정해서 햄버거 메뉴가 더 앞으로 나오도록 하는 방법도 있다.

* `z-index`는 부모-자식간이 아닌 형제끼리만 적용된다.

### `display: none`을 쓰면 안될 때

시각장애인이 스크린 리더기로 아이콘을 파악하기 위해서 아이콘에 대한 내용 설명이 필요하다. 하지만 비장애인에게는 보여질 필요가 없는 경우, 이 부분을 `display: none`으로 설정하면 괜찮다고 생각할 수 있다.

하지만, `display: none`인 경우 실제로 스크린 리더기가 이 부분을 읽지 않고 지나치게 되기 때문에

```css
.blind {
  position: absolute;
  clip: rect(0 0 0 0);
  width: 1px;
  height: 1px;
  margin: -1px;
  overflow: hidden;
}
```

처럼 일종의 꼼수(?)를 사용해서 스크린 리더기도 읽을 수 있고, 비장애인에게도 보이지 않도록 할 수 있다.

---

## 길어진 css 코드 정리하는 법

![image](https://github.com/rhfo0509/react-nodebird/assets/85874042/0da5731c-70fb-4110-b44a-cbcf924dce97)

"네이버페이" 버튼과 "알림 버튼"을 추가하기 위해 햄버거 메뉴 버튼을 복사

### `border-box` 

`box-sizing`의 기본값은 `content-box`인데, 이 경우 `width` 및 `height`는 오직 **`content`** 영역에만 영향을 받기 때문에 `padding`과 `border`가 존재하는 경우 지정한 `width`와 `height`보다 태그 영역이 더 커질 수 있다.

`border-box`로 설정하면 `width` 및 `height`의 길이가 **`content` + `padding` + `border`** 가 되므로 지정한 `width`, `height` 값이 변하는 경우가 절대 없게 된다.

```css
* {
  box-sizing: border-box;
}
```

"네이버페이" 아이콘은 나머지 두 아이콘에 비해 `width`가 `4px` 더 길다.<br>
아이콘을 더 길게 하면, 버튼도 그만큼 더 길어지게 되는데 이를 막기 위해서는 `box-sizing: border-box` 상태에서 버튼의 `padding`을 길어진 아이콘만큼 더 짧게 설정하면 해결이 가능하다.

### 중복된 부분 포함하기

실제로 세 아이콘은 **위치**("네이버페이"의 경우 버튼의 `padding`도 포함)를 제외하고 기능이 겹친다.

따라서 공통 부분을 상단에 작성하고, 서로 다른 부분만 따로 작성하면 가독성이 더 높아지게 된다.

```css
#header-hamburger, #header-naverpay, #header-notice {
  display: inline-block;
  padding: 10px;
  border: none;
  background: none;
  cursor: pointer;
  position: absolute;
  top: 18px;
}
#header-hamburger:hover::before, #header-naverpay:hover::before, #header-notice:hover::before {
  position: absolute;
  top: 1px;
  left: 1px;
  width: 44px;
  height: 44px;
  content: '';
  background-color: var(--color_option_bg);
  border-radius: 50%;
}
#header-hamburger:hover > div,
#header-naverpay:hover > div,
#header-notice:hover > div {
  position: relative;
}
#header-hamburger > div,
#header-naverpay > div,
#header-notice > div {
  background-image: url(./sp_main.png);
  background-size: 422px 405px;
  width: 26px;
  height: 26px;
}
#header-hamburger {
  left: -10px;
}
#header-hamburger > div {
  background-position: -335px -284px;
}
#header-naverpay {
  padding: 10px 8px;
  right: 42px;
}
#header-naverpay > div {
  background-position: -31px -316px;
  width: 30px;
}
#header-notice {
  right: -10px;
}
#header-notice > div {
  background-position: -364px -27px;
}
```

모든 태그가 아니더라도, 절반 이상의 태그가 공통인 값을 가지고 있다면(ex. `width`) 일단 공통 값을 상단에 작성한 후 달라지는 부분만 하위 각 태그에 따로 작성한다.

---

## 검색창 만들기

```html
<div id="search">
  <form action="">
    <label for="search-input" class="blind">검색어 입력</label>
    <input id="search-input" type="text">
    <button class="blind">검색</button>
  </form>
</div>
```
검색창을 `form`으로 하게 되면 `input` 창에서 엔터를 쳤을 때 자동으로 검색 결과 페이지로 넘어가도록 할 수 있다.<br>
그리고 "검색어 입력"과 "검색" 부분의 `class`를 `blind`로 설정함으로써 실제로는 보이지 않지만 시각장애인에게 검색창과 검색 버튼의 존재 여부를 알려줄 수 있다.

또한 `#search` 부분이 화면의 제일 상단에 위치해 있는 것을 확인할 수 있는데, 이는 `#header`의 `height`이 `0`이기 때문이다. `#header`의 자식 태그들의 `position`이 모두 `absolute`라서 부모 태그에게 영향을 주지 않으므로 `#header`는 실질적으로 높이를 가지지 않게 된다. 따라서 `#header`에 명시적으로 `height: 64px`을 지정해줌으로써 해결할 수 있다.

### `style`은 무조건 `head` 태그에 넣어줄 필요가 없다?

원칙적으로는 `head` 태그 내에 넣는 것이 맞으나, 개발의 편의를 위해 특정 태그 위에 작성하는 식으로 `style` 태그를 분리할 수 있다. 다만 이럴 경우 규정 위반일 뿐만 아니라 페이지를 불러올 때 성능 저하 및 예상치 않은 결과를 가져올 수 있으므로 나중에 한꺼번에 `head` 태그로 옮기거나 아니면 별도의 css 파일로 저장하는 것이 좋다.

---

## `inline-block`의 문제점

![image](https://github.com/rhfo0509/html-css-naver/assets/85874042/1cd954f8-df97-4a62-8468-bd729a462996)

`a` 태그와 `input` 태그 사이에 `margin`을 제외한 추가적인 공백이 존재한다.

![image](https://github.com/rhfo0509/html-css-naver/assets/85874042/843e5e3c-2c84-4c31-aebd-f2a04f942278)

이는 `a`와 `input` 태그가 `inline-block`이기 때문에 발생하는 현상으로, 동그라미로 표시한 여백을 지우면 문제가 해결되나 코드의 가독성 문제가 발생한다. 따라서 나중에 설명할 `inline-flex`를 사용하여 이 문제를 해결할 수 있다.

---

## `flex`로 간편하게 배치하기

정렬하고 싶은 요소의 부모 요소를 `display: flex`로 설정<br>
그러나 기본적으로 `flex`의 경우 `width: 100%`이므로 `display: inline-block`과 비슷한 `display: inline-flex`로 설정하여 지정한 `width` 값을 가지도록 할 수 있다.

### 자식 요소에서의 `flex` 속성

부모 요소가 `display: flex`일 때, 자식 요소에 `flex: 1`을 주게 되면 자식 요소는 늘어날 수 있는 범위 내에서 최대한으로 늘어나게 된다.

만약, 자식 요소 두 개에 각각 `flex: 2`, `flex: 1` 속성을 주게 되면 요소들은 본래 가지고 있던 `width`를 무시한 채 2 : 1 비율로 너비가 설정된다.

### `#search-right` 설정

![image](https://github.com/rhfo0509/html-css-naver/assets/85874042/95885d8f-14bf-4060-a8c3-015bd0ddf4f7)

크롬 개발자도구에서 간편하게 `flex`를 설정할 수 있다. 오른쪽 정렬을 위해 `justify-content: flex-end`, 각 버튼들을 세로로 쭉 늘리기 위해 `align-items: stretch`로 설정하였다.

### `#search-svg` 설정

기존의 `a` 태그가 `inline-block`이었기 때문에 `inline-flex`로 설정한 후, 가로 세로 가운데 정렬을 위해 `justify-content: center`, `align-items: center`로 설정하였다.

### `#search-keyboard` 설정

`hover` 시 이미지 밝기에 변화를 주기 위해 `filter` 속성을 이용한다.<br>
`filter: brightness(0.7)` -> 기존보다 밝기가 30% 어두워짐

### `#search-input`의 placeholder 색상 설정

css에는 placeholder에 대한 가상 선택자 `::placeholder`를 제공한다.

```css
#search-input::placeholder {
  color: white;
}
#search-input:focus::placeholder {
  color: rgb(230, 230, 230);
}
```

평소에는 보이지 않다가 `focus` 시에만 placeholder가 보이도록 설정할 수 있다.

---

## `focus-within`

![image](https://github.com/rhfo0509/html-css-naver/assets/85874042/f298ae12-3ba4-4023-92b8-a34060f40c53)

`input:focus` 시에 동그라미친 부분에 세로선이 활성화됨 -> `#search-svg`에 `::after`로 가상 선택자로 두어 세로선을 추가한다.

```css
#search-svg {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 68px;
  height: 100%;
  margin-right: 12px;
  padding-left: 10px;
  position: relative;
}
#search-svg::after {
  content: '';
  width: 1px;
  height: 20px;
  position: absolute;
  background-color: #e4e4e4;
  right: 0;
}
```

`::after` 요소의 `position`을 `absolute`로 하지 않으면 다른 요소들의 위치가 밀리는 문제가 발생한다. 그리고 기준점을 `#search-svg`으로 하기 위해 `#search-svg`의 `position`을 `relative`로 설정한다.

### 형제 요소간에 영향을 끼치고 싶을 때?

세로선은 `input`이 `focus`될 때만 활성화되야 함<br>
그러나 `input`과 `::after` 요소를 가지고 있는 `#search-svg`는 부모 관계가 아닌 형제 관계이므로 `input`의 `focus` 여부에 따라 세로선 등장 여부를 결정할 수 없다.

바로 이 때 해결책이 `focus-within`이다.
두 요소의 공통 부모 요소인 `form` 태그에 `focus-within`을 설정하면 **자식 요소의 변화도 `form` 태그에서 감지할 수 있다.**

```css
#search-svg::after {
  display: none;
}
#search > form:focus-within #search-svg::after {
  display: inline-block;
  content: '';
  width: 1px;
  height: 20px;
  position: absolute;
  background-color: #e4e4e4;
  right: 0;
}
```

기본적으로는 세로선이 보이지 않다가 `input:focus`를 감지 시 부모 태그에서 감지하여(`:focus-within`) 세로선이 보이도록 설정하였다.

---

## 리스트를 활용해서 메뉴 만들기

![image](https://github.com/rhfo0509/html-css-naver/assets/85874042/47a13738-e4ed-4fe4-9f09-a64a57aea0fa)

### `nth-of-type` vs `nth-child`

둘다 부모의 `n`번 째 자식 요소를 가리킨다는 점은 동일하나, `type`을 구별하지 않는 `nth-child`와 달리 `nth-of-type`은 `type`을 구별한다.<br>
지금 만드는 메뉴의 경우도 `type`이 `li`인 경우만 고려하기 때문에 `nth-of-type` 사용이 더 적절하다.

### 왜 `font-size`의 단위가 `rem`이면 좋을까?

`rem`은 `html` 태그의 `font-size`를 뜻한다.

예를 들어 글자 크기가 아래와 같이 선택이 가능할 때

`축소: 0.8rem, 기본: 1rem, 확대: 1.2rem`

`html`의 `font-size`만 바꾸더라도 모든 경우에 대해 균일하게 적용되기 때문에 편리하다는 장점이 있다.

---

## 시맨틱 태그와 특이한 선택자(`^=`, `$=`)

### 시맨틱 태그

단순히 태그를 `div`로만 지정하면 이 태그가 정확히 무엇을 의미하는지 파악하기 힘들다. <br>
그러나 `header`, `main`, `footer`, `aside`, `nav` 등의 시맨틱 태그를 사용하면 사용자 뿐만 아니라 웹 브라우저도 문서를 더 쉽게 이해할 수 있기 때문에 웹 접근성 또한 크게 향상된다.

검색엔진은 검색을 수행하는 과정에서 이 시맨틱 태그를 분석하는데, 예를 들어 검색엔진의 검색로봇에서는 `article` 태그가 사용된 콘텐츠를 재배포할 수 있는 콘텐츠로 인식하고 반대로 `section` 태그로 묶은 콘텐츠는 재배포를 금지하는 콘텐츠로 인식한다.

시맨틱 태그들로 아래와 같이 직관적으로 웹사이트를 구성할 수 있다.

시맨틱 태그의 종류는 다음과 같다.

| 태그 | 설명 |
| ---- | ---- |
| `header` | 사이트의 머리부분에 사용  |
| `main` | 메인 콘텐츠를 나타내는데 사용  |
| `section` | 여러 콘텐츠들을 주제별로 묶어주는 역할을 담당, 각 섹션들을 식별할 수단으로 `h1` ~ `h6` 요소를 자식으로 포함 |
| `article` | 개별 콘텐츠를 나타내는 요소 |
| `aside` | 좌우측의 사이드 영역, 주로 사이드바나 광고와 같은 부가 정보가 여기에 들어감  |
| `footer` | 사이트의 바닥부분, 주로 연락처나 제작자 정보 등을 기술하는 부분 |
| `nav` | 웹페이지의 메뉴를 만들 때 사용 |

### `^=`, `$=`

ex) `id`가 `ad`로 끝나는 `aside` 태그만 선택하려는 경우

```css
aside[id$=ad] {
  background-color: gray;
  border-radius: 8px;
  margin-bottom: 16px;
}
```

ex) `id`가 `main`으로 시작하는 `section` 태그만 선택하려는 경우

```css
section[id^=main] {
  border-radius: 8px;
  box-shadow: 0 0 0 1px #e3e5e8, 0 1px 2px 0 rgba(0,0,0,.04);
  margin-bottom: 16px;
}
```

> **요소의 `vertical-align`을 조절할 때, 그 요소의 높이와 부모 요소의 높이가 같은 경우?**<br><br>
![image](https://github.com/rhfo0509/html-css-naver/assets/85874042/689ef3c4-1757-43cf-997f-f8fce89c08ba)<br>
![image](https://github.com/rhfo0509/html-css-naver/assets/85874042/7f1c047d-4b1d-449a-b561-3a1b8db9a8c2)<br>
해당 요소의 위치에는 영향을 미치지 않지만 주변 요소(ex. "NAVER" 아이콘 옆에 위치한 "로그인" 텍스트)의 수직 정렬이 변경될 수 있다.


> **`line-height`가 가지는 의미?**<br><br>
텍스트가 차지하는 영역의 높이를 의미하며, 만약 `height`가 정해지지 않고 텍스트가 한 줄로만 존재하는 경우, `line-height` 값은 `height`와 동일해진다.

---

## `Grid`

### 격자 배치하기

```css
#main-newsstand-grid {
  display: grid;
  width: 790px;
  height: 224px;
  grid-template-rows: repeat(4, 1fr);
  grid-template-columns: repeat(6, 1fr);
  justify-content: center;
  align-items: center;
}
```

* `display`를 `grid`(혹은 `inline-grid`)로 설정
* `repeat(4, 1fr)` -> 1 : 1 : 1 : 1 비율
* `grid-template-rows/columns`에 `fr` 단위를 지정하면 정해진 `width`와 `height` 내에  `fr`에 적힌 비율대로 칸들이 분배된다.
* `flex`와 마찬가지로 `justify-content`와 `align-items`를 이용해 내부 콘텐츠를 정렬할 수 있다.

### 테두리 생성하기

```css
#main-newsstand-grid {
  display: grid;
  width: 790px;
  height: 224px;
  grid-template-rows: repeat(4, 1fr);
  grid-template-columns: repeat(6, 1fr);
  border: 1px solid var(--color_border_in);
  gap: 1px;
  background-color: var(--color_border_in);
}
#main-newsstand-grid > div {
  background-color: white;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
```

* 그리드 자체의 `background-color`를 `var(--color_border_in)`로 설정하고, 각 칸의 `background-color`는 `white`로 설정
* 그리드의 `gap` 속성을 주면 각 칸에 해당되지 않는 틈새 부분만 `var(--color_border_in)`가 적용되므로 마치 내부에 구분선이 생긴 것 같은 현상 발생
* 그리드의 테두리는 그리드 내에서 `border`를 지정해주면 된다.

---

## `ellipsis`

텍스트가 길 때 **`...`** 으로 줄이는 기술

```css
#main-newsstand-gray a:nth-of-type(3) {
  flex: 1;
  margin-right: 50px;
  text-overflow: ellipsis;
  white-space: nowrap;
  overflow: hidden;
}
```

* `white-space: nowrap`: 부모 요소의 가로폭을 넘어가더라도 자동으로 줄바꿈이 일어나지 않음
* `overflow: hidden`: 넘치는 부분은 잘려서 보이지 않도록 함 (이 때 `text-overflow`가 `ellipsis`일 때는 잘린 부분이 `...`로 대체됨)

---

## 정렬할 때 방해되는 요소가 있다면?

![image](https://github.com/rhfo0509/html-css-naver/assets/85874042/a3a4a07f-7e38-47b3-a038-bf8a0aef2a3e)

```html
<footer>
  <button class="prev"><span class="blind">이전</span></button>
  <div class="text"><span class="news">언론사</span> 더보기 1<span class="total">/4</span></div>
  <button class="next"><span class="blind">이후</span></button>
  <button class="list"><span class="blind">목록보기</span></button>
  <button class="grid"><span class="blind">격자보기</span></button>
</footer>
```

`footer` 태그 내에 총 5개의 요소가 존재하는데, 앞 3개 요소는 가운데에, 뒤 2개 요소는 오른쪽에 위치시키려고 한다.

앞의 3개 요소를 하나로 묶어서 가운데 정렬하면 되지 않나 생각할 수도 있지만, **나머지 2개 요소가 차지하는 공간을 제외한 부분**에 대해 가운데 정렬을 수행하기 때문에 예상했던 위치보다 약간 왼쪽으로 치우친 결과를 보게 될 것이다.

따라서 나머지 2개 요소에 대해 `position: absolute`(부모 요소에는 `relative`)를 적용해서 가운데 정렬하는 데 방해가 되지 않도록 해야 한다.

```css
#main-newsstand footer {
  position: relative;
  padding: 10px 0 11px;
  border-top: 1px solid var(--color_border_in);
  font-size: 1.3rem;
  line-height: 35px;
  font-weight: 600;
  display: flex;
  justify-content: center;
  color: var(--color_caption2);
}
#main-newsstand footer .list {
  right: 49px;
  padding: 18px 9px 18px 20px;
}
#main-newsstand footer .grid {
  right: 0;
  padding: 18px 20px 18px 9px;
}
```