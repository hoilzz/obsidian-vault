### builder.addCase

- single exact action type으ㄹ 처리하는 리듀서 케이스를 추가
- builder.addCase는  builder.addMatcher 전에 무조건 와야함.

### builder.addMatcher

- action.type 속성이 아닌 your own filter 함수에 대해 들어오는 action을 일치시킬 수 있음
- 만약 여러개 matcher reducer가 일치한다면, 정의된 순서대로 전부 실행될 거임.
	- case reducer가 이미 매치되더라도




 [RTK action matching utilities](https://redux-toolkit.js.org/api/matching-utilities) thunk가 dispatch하는 pending, fulfiled, rejected action과 매치된다. createSlice.extraReducers 


https://redux-toolkit.js.org/api/matching-utilities

## matching utilities

- 특정 action을 체크할 수 있는 type-safe action을 export할 수 있음.
- 주로 `builder.addMatcher()`