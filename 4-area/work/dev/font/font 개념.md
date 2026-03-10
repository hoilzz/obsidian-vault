
`font-family` 는 선택된 요소에 우선 순위가 지정된 font family 이름과 generic family 이름을 지정할 수 있다. 

브라우저는 폰트 목록에서 컴퓨터에 설치되어있거나 `@font-face` 규칙을 이용해 다운 받은 폰트 중 특정 폰트 중 가장 첫번째 폰트 선택한다.

웹개발자는 font-family 목록에 최소 generic family 하나늘 추가해야한다.
- 시스템이나 `@font-face` 규칙을 이용해 다운 받은 폰트 중 특정 폰트가 있다는 것을 보장 못하기 때문
- generic family는 대체 폰트임.

```css
/* font family, generic family */
font-family: "Goudy Bookletter 1991", sans-serif
```

### @font-face 규칙

- 온라인 폰트 적용 가능한 rule
- 이걸 사용하여 개발자가 원하는 폰트를 사용할 수 있음
- 컴퓨터에 설치된 폰트만을 사용한 제약 없앤 syntax

```css
@font-face {
	/* font 속성에서 폰트명(font face)으로 지정될 이름*/
	font-family: 'Pretendard';
	font-weight: 900;
	font-display: swap;
	/* 원격 폰트 파일의 위치*/
	src: local('Pretendard Black'), url('../../../packages/pretendard/dist/web/static/woff2/Pretendard-Black.woff2') format('woff2'), url('../../../packages/pretendard/dist/web/static/woff/Pretendard-Black.woff') format('woff');
}
```