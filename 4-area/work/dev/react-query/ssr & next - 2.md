

### 문제 1. getServersideProps 페이지 진입 -> 일반 페이지 진입 -> 뒤로가기시에 기존 캐시가 유지 안됨

- 재현 1. getServersideProps 아닌 페이지에서도 (검색 페이지) 캐시가 초기화되나? 
	- 아님
	- 검색 데이터 stale: Infinity 걸어도 위 재현과정에서 캐시가 잘 유지됨.
- 재현 2. getInitialProps로 변경하여 클라일 때 queryClient 전달 안하면 기존 캐시 유지될까?
	- ㅇㅇ
	- 그럼 이걸 전달안하던가 아니면... 
	- 근데 hydrate 문서를 보면 기존 캐시가 있는 경우 아무동작 안한다고 명시되어있음
		- 이게 제대로 동작을 안하는걸까?

- 이상한점 1. hydration 되기 전부터 \_app의 queryClient는 첫페이지 캐시만 남아있다. 
	- 