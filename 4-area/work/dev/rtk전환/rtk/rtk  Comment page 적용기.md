## 개선하려는거
- use 어쩌고 로직 라이브러리 도움 받아 제거
- 


## problems

### 1. 결국 레이어 하나 더 추가해야함
- 기존 방식에서 fetch** 네이밍으로 createAsyncThunk로 thunk 함수 더 만듬
- 요기서 codegen variable과 thunk variable를 나눌 수 있음
- 그러니까 요런 케이스는 builder.query를 다시 작성해서 한땀한땀해야함.

### 2. 타입 지원 부족
아무래도 queryFn을 이렇게 쓰도록 의도하진 않은 거 같음

### 3. 무한스크롤 미지원
- 일단 TODO로 남아있음.
- getState 써서 누적하는 수밖에 없음.
	- getState 타입 미지원

---
## code review

- fetchCommentList : 하는일이 너무 많다.
	- type Best 
		- login -> bestCommentListAndBlockUserListQuery
		- not login -> bestCommentListQueyr
	- type My
		- MyCommentListQuery
	- Not type (More)
		- login -> CommentListAndBlockUserListQuery
		- not login -> CommentListQuery
- solution
	- custom hook 만들어서 isLogin 인자로 받음
	- isLogin에 따라 hook의 fetch 함수 호출 다르게 ㄱㄱ 






### rtk 톺아보기

- data fetching & 캐싱(리덕스) 로직 직접 작성하는 걸 줄이고, 데이터 가져오는 일반적인 케이스를 단순화하는데 중점
	- 로딩 상태 추적
	- 동일 데이터 중복 요청 줄이기
	- 유저가 UI 인터랙션할 때 캐시 validation 관리
- action을 dispatch하고 reducer 작성하는 걸 줄이고자 나옴.
	- 그래서 상태 관리 대한 생각 안해도됨.
	- cached data 관리하는것만 생각하도록 의도
		- 데이터 어디로 불러오고
		- 이 업데이트 어떻게 보낼까
		- 캐싱된 데이터 언제 다시 가져올까
		- 캐싱된 데이터 업데이트 방법















































