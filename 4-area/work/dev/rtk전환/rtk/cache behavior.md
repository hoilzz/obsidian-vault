https://redux-toolkit.js.org/rtk-query/usage/cache-behavior

RTK Query의 핵심 기능은 캐시 데이터 관리다. 서버에서 데이터 받아오면, RTK Query는 스토어에 `cache` 로 저장한다. 동일 데이터에 대해 추가 요청 수행할 때 RTK Query는 기존 캐시 데이터를 제공한다.

RTK Query에서 캐싱은 다음 기반
- API endpoint definition
- 컴포넌트가 endpoint에서 데이터를 구독할 때 사용된 시리얼라이징된 쿼리파라미터
- active 구독 참조 카운트

매개변수가 직렬화되어 queryCachekey 내부적으로 저장된다. 동일한 `queryCacheKey`를 만드는 미래의 요청은 (동일한 파라미터로 호출, 직렬화 인수분해) original에 대해 중복 제거되며 동일한 데이터 및 업데이트를 공유합니다. ==__동일한 요청을 하는 두개의 별개 컴포넌트는 동일한 캐시 데이터를 사용할 것입니다.__==

요청 날아갈 때, 데이터는 이미 캐시에 존재함. 그 데이터는 제공되고 새 요청은 서버에 안날아감. 

### cache lifetime & subscription example

```ts
import { useGetUserQuery } from './api.ts'  
  
function ComponentOne() {  
// component subscribes to the data  
const { data } = useGetUserQuery(1)  
  
return <div>...</div>  
}  
  
function ComponentTwo() {  
// component subscribes to the data  
const { data } = useGetUserQuery(2)  
  
return <div>...</div>  
}  
  
function ComponentThree() {  
// component subscribes to the data  
const { data } = useGetUserQuery(3)  
  
return <div>...</div>  
}  
  
function ComponentFour() {  
// component subscribes to the *same* data as ComponentThree,  
// as it has the same query parameters  
const { data } = useGetUserQuery(3)  
  
return <div>...</div>  
}
```

4개의 컴포넌트가 endpoint을 구독하는 동안, 서로다른 endpoint + query parameter 조합이 3개 생김.
1,2 각 subscripber, 3은 2개의 subscripber

적어도 한 명의 활성 구독자가 endpoint + query parameter 조합에 관심이 있는한 데이터는 캐시에 보관됨. ==(즉, unmount 되서 구독자 0되면 캐시데이터 버리는듯)==
subscriber reference count가 0이 될때, 타이머가 설정되고 만약 새로운 구독자가 타이머 만료될 때까지 없으면 데이터 지워버림. default는 1분이다.(API전체에 걸 수도 있고, endpoint 별로 걸 수도 있오)

### 캐시 행동 조작하기

RTK 쿼리는 '새로 고침' 하기에 적합한 시나리오에서 더 __일찍__ 데이터를 가져오는 여러 가지 방법을 제공

#### keepUnusedDataFor 구독 시간 제거하기

위에서 언급한대로 `Default Cache Behavior` 와 `Cache lifetime & subscription example` 은 ==기본적으로 데이터는 subscriber 참조 횟수가 0이 된 후에 60초 동안 캐시된다.==

이 값은 `keepUnusedDataFor` option으로 설정될 수 있다. (per-endpoint)

### on demand로 refetch 하기 `refetch`/`initiate`

refetch 호출하면 강제로 데이터 다시 가져온다.

또는 `initiate` thunk action을 호출할 수 있다. (forceRefetch:true) 설정하여서

### `refetchOnMountOrArgChange`로 다시 가져오기 권장

`refetchOnMountOrArgChange` property 속성을 통해 평소보다 더 자주 다시 가져오도록 권장함.
endpoint 전체에 전달되거나 개별 hook에 전달되거나 `initiate` action을 dispatch 할 때 전달될 수 있다. (action creator option은 `forceRefetch`임)

- boolean value, number(time) 인자로 받음
	- boolean: (false default) 기본 캐시 동작
		- true 
			- subscriber 추가될 때마다 항상 refetch
			- 개별 hook 에 전달되면 그 hook에만 적용
	- `number`
		- 쿼리 구독 생성 당시에
			- 기존 커ㅜ리가 캐시에 있다면 현재 시간과 마지막 fulfilled 타임스탬프를 비교
			- 제공된 시간 경과하면 다시 가져옴
		- 쿼리 없으면 바로 데이터 가져옴
		- 기존 쿼리가 있지만 마지막 쿼리 이후 지정된 시간 경과하지 않은 경우, 기존 캐시 데이터 제공


### re-fetching on window focus with `refetchOnFocus`

## tradeoff

#### No Normalized or de-duplicated cache
































