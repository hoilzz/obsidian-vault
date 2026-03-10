### 1. 캐시 상태

- stale time
	- fresh -> stale로 전환되는 유효시간
			- 긴 stale time 설정은 데이터를 자주 가져 오지 않음.
	- 데이터는 항상 캐시에서 가져오고 네트워크 요청 발생하지 않음
	- 하지만 쿼리가 오래된 경우 캐시에서 데이터를 가져오긴 하지만 특정상황에서 백그라운드 refetch 발생할 수 있음.
		-   New instances of the query mount
		-   The window is refocused
		-   The network is reconnected
		-   The query is optionally configured with a refetch interval

- cache time
	- 비활성(inactive) 쿼리가 캐시에서 제거될 때까지의 시간.
		- 등록된 구독자가 없는 즉시 비활성상태로 전환된다. 
	- 기본값 5분 (얘는 건드릴일이 거의 없었다.)
- dev tool 상태
	- fresh
	- fetching
	- paused
		 - 쿼리 가져오려했는데 정지된 경우. 네트워크 커넥션이 다시 연결될 때까지 정지된 상태임.
	- stale
	- inactive
		- inactive된 쿼리는 cachetime(기본 5분) 후 가비지 컬렉팅됨.


- Status Check
	- loading: 쿼리에 데이터가 없고 처음으로 로드중 (이후부턴 false)
	- fetching: 요청이 진행중일 때마다 true가 됨.


### 2. refetch를 모든 
https://stackoverflow.com/questions/71286123/reactquery-useinfinitequery-refetching-issue
- resetQueries
	- https://tanstack.com/query/v4/docs/react/reference/QueryClient#queryclientresetqueries
	- 캐시안의 쿼리를 초기상태로 리셋. 
	- 모든 구독자에게 pre-loaded 상태로 쿼리를 리셋하라고 알림(구독자 모두 제거하는 clear 메서드와 다름)
	- 쿼리가  active 상태라면 refetch함

### 3. codege의 select 리턴타입 변경 불가
- https://github.com/TanStack/query/discussions/1410
- https://github.com/TanStack/query/issues/3065