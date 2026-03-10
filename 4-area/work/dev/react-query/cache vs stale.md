

### stale time
- fresh -> stale 로 가는 기간
- 쿼리가 fresh 상태라면 데이터는 항상 캐시에서 가져온다.
	- 요청 안날림
- 만약 쿼리가 stale이라면 캐시에서 데이터 가져오지만 특정 조건에서 background reftch가 일어날거다.

### cache time
- inactive queries가 캐시에서 제거되는 시간
- 5분이 기본

## 정리
- 보통은 staletime만 수정하고 캐시타임은 거의 수정안함. 


### 예제

#### cache: 5min, statle: 0 (default)
- new instance mount( useQuery(querykey: todos))
	- todoss 쿼리 key로 생성된적 없기 때문에 이 쿼리는 요청 날릴거임
	- 요청 완료 되고 todos key로 캐싱
	- staleTime 지난 후에 데이터는 stale로 리마크됨
- 두번째 인스턴스가 다른데서 마운트됨(useQuery(queryKey: todos))
	- 캐시는 이미 todos로 데이터 이미 가지고 있기 때문에 데이터는 캐시에서 즉시 가져옴
	- 새 인스턴스는 새 요청 날림
	- 요청 완료되면 캐시 데이터 교체
- 2가지 인스턴스가 언마운트되고 사용되지 않음
	- active instance가 없어서 캐시 타임아웃 설정되고 가비지 컬렉터가 쿼리를 가져감.
- cache timeout이 완료되기 전에 다른 인스턴스가 마운트되면
	- 쿼리는 즉시 사용가능한 캐시데이터를 리턴하고 fetchTodos를 백그라운드 실행
	- 완료되면 캐시 교체