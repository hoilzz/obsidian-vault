--- 
creation date: 24-01-09 
tags: 

<< [[2024-01-08]] | [[2024-01-10]] >>

---

## TODO
- figma-plugin 새로 추가될 동작들
	- .svg, .png 붙이기
	- enum "" 붙는거 처리
	- enum은 export const 로 변경
- 타파스에 적용하기
	- ResourceImage component 추가
	- 

---

### 다크모드 src대응

1. enumType 끝에 Dark를 붙이기
```ts
return isColorMode ? 
	imageTypeInfoMap[imageType] : iamgeTypeInfoMap[`${imageType}Dark`]
```
- 생성 방법
	- src에 dark가 마지막 단어라면?
		- src에 파스칼케이스 -> Dark 붙이기 
		- or `darkModeTypeKey: ${ImageEnumType.currentKey}Dark`
	- Enum값이 dark까지 들어갈 거 같음
	- 사용하는 유저는 그냥 해당 key만 넣으면 dark, light 알아서 되는구나로 인지되면 좋을 것 같은데 enum key에 이게 뜨면 좀 불편할 거 같음.

1. 단일 key에 srcLight, srcDark 붙이기
   - 단어 끝이 
      - static 이라면 그냥 등록
      - static 아니고 맨 뒤에 dark가 안붙어있다면
         - src에 그대로 등록
         - srcDark: 현재 src에_dark 추가
   - enum에 맨 뒤에 dark 붙은얘는 빼고 생성





