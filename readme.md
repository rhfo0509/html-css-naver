# 네이버 클론코딩 (1)

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
검색창을 `form`으로 하게 되면 `input` 창에서 엔터를 쳤을 때 자동으로 검색 결과 페이지로 넘어가도록 할 수 있음<br>
그리고 "검색어 입력"과 "검색" 부분의 `class`를 `blind`로 설정함으로써 실제로는 보이지 않지만 시각장애인에게 검색창과 검색 버튼의 존재 여부를 알려줄 수 있다.

또한 `#search` 부분이 화면의 제일 상단에 위치해 있는 것을 확인할 수 있는데, 이는 `#header`의 `height`이 `0`이기 때문이다. `#header`의 자식 태그들의 `position`이 모두 `absolute`라서 부모 태그에게 영향을 주지 않으므로 `#header`는 높이를 가지지 않게 된다. 따라서 `#header`에 명시적으로 `height: 64px`을 지정해준다.

### `style`은 무조건 `head` 태그에 넣어줄 필요가 없다?

원칙적으로는 `head` 태그 내에 넣는 것이 맞으나, 개발의 편의를 위해 특정 태그 위에 작성하는 식으로 `style` 태그를 분리할 수 있다. 다만 `head` 이외에 작성하는 것은 규정 위반일 뿐만 아니라 페이지를 불러올 때 성능 저하나 예상치 않은 결과를 가져올 수 있으므로 나중에 한꺼번에 `head` 태그로 옮기거나 별도의 css 파일로 저장하는 것이 좋다.

---

## `inline-block`의 문제점과 `vertical-align`에 대한 오해

### `inline-block`의 문제점

![image](https://github.com/rhfo0509/html-css-naver/assets/85874042/1cd954f8-df97-4a62-8468-bd729a462996)

`a` 태그와 `input` 태그 사이에 `margin`을 제외한 추가적인 공백이 존재한다.

![image](https://github.com/rhfo0509/html-css-naver/assets/85874042/62b5f3ac-571c-4434-8829-23d8c826d373)

이는 `a`와 `input` 태그가 `inline-block`이기 때문에 발생하는 현상으로, 동그라미로 표시한 여백을 지우면 문제가 해결되나 코드의 가독성 문제가 발생한다. 따라서 이 경우 나중에 설명할 `inline-flex`를 사용하여 해결할 수 있다.

### `vertical-align`에 대한 오해

> `vertical-align`은 실제로 세로 가운데 정렬을 의미하지 않는다.

