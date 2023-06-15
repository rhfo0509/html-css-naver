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

