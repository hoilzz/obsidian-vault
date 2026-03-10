
https://redux.js.org/tutorials/essentials/part-7-rtk-query-basics

### summary
- rtk는  data fetching & 캐싱 로직 직접 작성하는 걸 줄이고, 데이터 가져오는 일반적인 케이스를 단순화한다
	- 로딩 상태 추적하기
	- 동일 데이터 중복 요청 줄이기
	- UI 빨라 보이게 하는 Optimistic updates
	- 유저가 UI와 인터랙션 할 때 캐시 validation 관리

### intro

- RTK query 사용 방식
- 데이터 가져오기
- 리덕스 앱에서 캐싱 해결책
- 데이터 가져오는 과정 단순화하고 컴포넌트에서 사용하기

## RTK Query Overview

- data fetching & caching logic 한땀 작성 제거


### motivation

- UI spinner 보여주기위한 로딩 스태이트 추적
- 동일 데이터 중복 요청 히가ㅣ
- UI 빨라 보이게 Optimistic update
- 유저가 UI와 상호작용할 때 캐시 수명 관리

`createAsyuncThunk`, `createSlice` 써도 몇가지 더 작성해야됨
- 로딩 상태 관리
	- `extraReducer` 에서 `pending/fulfilled/rejected` 케이스 처리해줘야함

_시간이 지나면서 __data fetching and caching__ 이 상태 관리와 실제로 다른 문제임을 알게됨._

### what's included

RTK Query는 주로 2가지 API로 구성된다

- createApi()
	- 여러개의 endpoint로부터 데이터를 가져오는 방식을 정의
		- fetch와 데이터 변경 방식 설정을 포함한다
		- 앱당 1개 사용하고 base URL당 슬라이스 1개가 룰이다.
- fetchBaseQuery()
	- 요청을 단순화하는 `fetch` 의 wrapper


### RTK Query Caching 으로 생각하기

리덕스는 예측 가능성과 명시적 동작에 중점을 둔다. 모든 리덕스 로직은 __action을 dispatch 하고 상태를 업데이트하는 일반적인 패턴__ 을 따른다. 이거는 일어난 일에 대해 코드를 더 작성해야 한다는 것을 의미한다.

RTK core API는 기본 데이터 흐름을 변경하지 않는다. 똑같이 action dispatch, reducer 작성할텐데 직접 손으로 작성하는 코드가 줄어들 것이다.  RTK query는 추상화 레벨이 추가된 정도다. 내부적으로는 여태 봐왔던 비동기 요청과 응답을 관리하는 동일한 동작을 한다.

근데 RTK query 사용하면 mindset이 달라짐. 
- 상태 관리에 대한 생각 안해도됨.
- 대신에 `cached data` 를 관리하는 걸 생각해야됨
- 리듀서 직접 작성하는 대신에 이걸 정의하는데 초점을 둬야함
	- __데이터 어디서 온거임?__
	- 이 업데이트 어케 보낼까?
	- 캐싱된 데이터 언제 다시 가져와야할까?
	- 캐싱된 데이터  업데이트 방법

### Setting Up RTK Query

createAsyncThunk, createSlice의 기존 사용을 마이그레이션하여 RTK query API를 사용하는 방법


### I. API Slice 정의하기

Posts, Users, Notifications 와 같이 각 data type 별로 slice를 분리했다. 각 슬라이스는 자신의 reducer, 각 슬라이스의 action 과 thunk 를 정의. 

==__RTK query 로는 캐싱된 데이터 관리하는 로직이 app 당 단일 "API slice" 로 중앙관린된다.__== 

apiSlice 작성할 건데, 기존에 "feature" 에 국한되지 않으므로 `features/api` 폴더를 추가하여 apiSlice.js 를 넣습니다.

```js
// Import the RTK Query methods from the React-specific entry point  
import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react'  
  
// Define our single API slice object  
export const apiSlice = createApi({  
	// The cache reducer expects to be added at `state.api` (already default - this is optional)  
	reducerPath: 'api',  
	// All of our requests will have URLs starting with '/fakeApi'  
	baseQuery: fetchBaseQuery({ baseUrl: '/fakeApi' }),  
	// The "endpoints" represent operations and requests for this server  
	endpoints: builder => ({  
			// The `getPosts` endpoint is a "query" operation that returns data  
			getPosts: builder.query({  
				// The URL for the request is '/fakeApi/posts'  
				query: () => '/posts'  
		})  
	})  
})  
  
// Export the auto-generated hook for the `getPosts` query endpoint  
export const { useGetPostsQuery } = apiSlice
```

RTK query 함수는 단일 메서드를 기초로 한다. (createApi)
__@reduxjs/toolkit/query/react__ 얘 써 걍. 리액트 친화적이야.


> TIP
> 어플리케이션에 createApi 가 딱 한 번 호출될 걸로 예상함.
> 이 API slice는 동일 base URL인 모든 endpoint 정의를 포함해야한다. 
> api/posts, /api/users 얘네 동일 서버니까 하나로 묶어서 써
> endpoint는 createApi call 내부에서 직접 정의됨. 
> ==endpoint를 여러개 파일로 쪼갤 거면 여기 [참고](https://redux.js.org/tutorials/essentials/part-8-rtk-query-advanced#injecting-endpoints)해==

- reducerPath
	- top-level state slice field의 이름 정의
	- postsSlice 같은 얘들의 경우 `state.posts` 업데이트 보장 안함. 리듀서 직접 붙여서 사용해야함.

##### 1. endpoint 정의하기

- base path 설정
- builder.query 로 query endpoint 정의하기

##### 2. API slice와 hook export 하기

기존에는 action creator와 slice reducer를 export 함
이제는 전체 API slice 객체 자체를 export 할거임. 왜냐면 유용한 별개의 필드를 가지고 있으니까.
- `useGetPostsQuery`  를 export 

### II. store 구성하기

### III. Query로 Posts 보여주기

##### 컴포넌트 내부에서 Query hook 사용하기

- ==useSelector, useDispatch, useEffect를 useGetPostsQuery에서 다 처리해줌.==

### 캐시 무효화로 자동 리프레쉬

hook에서 주는 refetch함수로 데이터 가져오는거는 좋은 솔루션 아님. 

==RTK Query _tag_ 를 이용하여 자동 데이터 가져오기를 할 수 있다.==
==tag는 데이터 유형 네이밍을 하고 cache의 부분을 invalidate할 수 있는 스트링 or small object 다.==

기본 태그는 API slice에 3조각의 정보를 제공
- tagTypes: `Post` 와 같은 데이터 유형
- providesTags: 쿼리에서 데이터를 설명하는 tag set list
- invalidatesTags: mutation 실행될 때마다 invalidated tag 리스트

```js
export const apiSlice = createApi({  
	reducerPath: 'api',  
	baseQuery: fetchBaseQuery({ baseUrl: '/fakeApi' }),  
	tagTypes: ['Post'],  
	endpoints: builder => ({  
		getPosts: builder.query({  
			query: () => '/posts',  
			providesTags: ['Post']  
		}),  
		getPost: builder.query({  
			query: postId => `/posts/${postId}`  
		}),  
		addNewPost: builder.mutation({  
				query: initialPost => ({  
				url: '/posts',  
				method: 'POST',  
				body: initialPost  
			}),  
			invalidatesTags: ['Post']  
		})  
	})  
})
```

- Save Post 클릭하면, PostsList component 자동으로 리렌더하면서 새롭게 추가된 post가 맨위에있음
- 별다를게 없음 `Post` 스트링이 그냥 있는건데, Fred, qwerty 아무거나 써도됨
	- ==각 필드에 동일한 스트링이 있어야함== 그래야 RTK query가 "언제 뮤테이션이 일어나느지" 알고 동일한 tag string을 가진 엔드포인트를 invalidate 함.















































































