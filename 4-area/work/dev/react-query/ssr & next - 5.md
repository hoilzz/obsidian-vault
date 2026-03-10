--- 
creation ㅓate: 23-03-15 
modification date: 23-03-15 
tag: 

---

## 랜딩리스트 SSR React-query 교체



### limits
- 기존과 동일하게 error 발생시 store에 쌓기 위해 fetchQuery 사용
- error가 store에 쌓임
- _app 에서 SSR에서 error를  처리할 줄 알았으나 CSR로 처리됨
- 그래서 useQuery가 중복실행됨
	- hydrate된 데이터가 없어서
- 그럼 기존에도 2번 호출된건가?
	- Fetcher에서는 그냥 로딩 컴포넌트 로딩 여부만 결정함
	- 즉, 에러가 있었다면 그냥 data 없는채였다가 StoreErrorChecker의 userEffect에서 에러를 꺼내서 throw하고 있었을듯. 그래서 자연스러워 보임.