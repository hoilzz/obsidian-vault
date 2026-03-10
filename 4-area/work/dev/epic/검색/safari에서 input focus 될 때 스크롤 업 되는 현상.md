### issues with position fixed & scrolling on iOS

#### focus jumping

fixed 포지션 내부에 포커싱 가능한 엘리먼트가 있다면 (input) 이것은 fixed element가 제자리에서 튀어나올 수 있다. 유저가 스크롤 했을 때만 생기는 버그다. 



1. scollTo 방식
	- 덜커덩
	- 좀 뭔가 부자연스러움
	- ]
2. overflow: auto 를 body, html에 붙이기
	- 스티키가 계속 떠 있음
	- 주소창이 줄어들지 않음.

- 네이버가 저희랑 똑같이 상단에 붙어있는 input인데 얘네도 가끔 발생함.
- 구글, 아마존, 리디는 input이 위에 fixed 된 형태가 아니라 재현이 안됨. 



viewport 이벤트 발생.
- 높이값도 들어오긴함.
- resize가 안일어남. 
- iOS && mobile
- requestanimationframe



### scrollY, scrollTop

- window.scrollY
	- 문서가 수직으로 얼마나 스크롤됐는지.
	- window는 DOM 문서를 담은 창을 의미한다. 
	- document 속성이 창에 불러온 DOM 문서를 가리킨다. 

























