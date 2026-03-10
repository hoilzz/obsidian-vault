
rq느ㄴ 서버에서 여러개의 쿼리 prefetching을 지원하고 queryClient에 그 쿼리를 dehydrate 할 수 있다. 서버는 prerender markup할 수 있고 리액트 쿼리는 그 쿼리들을 hydrate할 수 있다. 

서버에서 쿼리 캐싱과 hydration setup 지원하기 위해 
- `QueryClient` instance를 app 내부에서 만들어라. 
	- 이것은 데이터가 다른 유저와 요청간에 공유되지 않는 걸 보장해준다.
	- 반면에 component lifecycle 당 queryclient 1개 만든다.ㅐ

---

월요일에 할꺼
- 절대 경로 수정
- https://github.com/TanStack/query/issues/1458
- https://github.com/SWM-re-pashion/repashion-client/pull/99/files

---

### Using Hydration
- _app.tsx_ 에서 서로 다른 유저와 요청 사이에 데이터를 공유하지 않는 걸 보장
	- + 컴포넌트 라이프사이클당 query클라이언트 만듬
	- 위에꺼 하면 ssr, ssg에서 사용할 준비됨
- ssg에서 이제 어떻게 쓸거냐
	- 페이지 요청마다 queryClient 인스턴스 새로 만들거임
		- 이래야 유저와 요청 사이에 데이터 공유되지 않는 거 보장
	- 쿼리 캐시를 dehydrate하고 dehydratedState prop으로 페이지에 전달
		- _app.tsx에서_  pick되는 캐시와 동일한 prop임

## caveats

#### staleness는 쿼리가 서버에서 fetch된 시점에 측정된다
- staletime은 기본적으로 0이기 때문에 쿼리는 기본적으로 background에서 refetch될 거다.
- 더블 fetch를 피하려고 더 높은 staletime을 설정할 수 있다. (특히 markup cach하기를 원치 않는다면)


- ["seriesListViewLandingList.infinite",{"dataKey":"page/landing/517","size":25}]


---
- 이슈 : ssr에서 생성하여 dehydrated된 queryClient의 내부 cache가 유지 안되는 거 같다.
	- https://github.com/TanStack/query/discussions/3563
	- https://tanstack.com/query/v4/docs/react/plugins/persistQueryClient

### persistQueryClient plugin

- 서로 다른 persisters는 client에 저장될 수 있음

#### how it works







































