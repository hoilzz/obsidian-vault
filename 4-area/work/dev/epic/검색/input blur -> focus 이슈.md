### 기존코드
- 스크롤 할 때 blur가 안되니 touchstart 에 blur를 직접 준다.

#### 재현 
1. 아이폰 최근검색어 누르고 input focus 후에 X버튼 누름


##### focus상태에서 취소버튼 누르면 blur->focus
1. 취소버튼 터치시 touchstart event 발생



### 이상한거
- 포커스가 되어 있어도 autocomplete이 안뜸
- 엑스 눌러도 X의 부모 div가 터치이벤트 발생



### 발견한거
- iOS에서 input에 포커스할 때 blur -> focus가 일어난다.



### event
#### touchstart
Sent when the user places a touch point on the touch surface. The event's target will be the [`element`](https://developer.mozilla.org/en-US/docs/Web/API/Element) in which the touch occurred.

#### touchend
Sent when the user removes a touch point from the surface; that is, when they lift a finger or stylus from the surface. This is also sent if the touch point moves off the edge of the surface; for example, if the user's finger slides off the edge of the screen.

The event's target is the same [`element`](https://developer.mozilla.org/en-US/docs/Web/API/Element) that received the `touchstart` event corresponding to the touch point, even if the touch point has moved outside that element.

The touch point (or points) that were removed from the surface can be found in the [`TouchList`](https://developer.mozilla.org/en-US/docs/Web/API/TouchList) specified by the `changedTouches` attribute.



####

- 저 블러 없이도 잘 되는거



----
