
> 배울거
> - 캐시 관리, refetch 하기 위해 ID 로 태그 이용하는 방법
> - RTK query cache가 리액트 밖에서 동작하는 방법
> - 응답 데이터 관리하는 기법
> - optimistic updates and streaming updates

## cache data subscription lifetimes

- 여러개의 컴포넌트에서 동일 데이터를 구독하는 거 허용. 
	- 각각의 고유한 데이터가 한 번만 fetch 되도록한다. 
	- useGetPostQuery(42) 를 다른 컴포넌트에서하고 또 다른데서 하면
	- 두번째부터는 요청 안가고 캐시된 거 가져옴
- active 구독이 0으로 내려갈 때, 쿼리의 내부 타이머가 시작된다. 
	- __만약 new 구독이 추가되기 전에 타이머가 만료되면, RTK query는 자동으로 캐시 데이터 제거__
	- 초 설정 가능

## Invalidating specific items

- 현재 상황
	- 포스팅 리스트 가져오는 페이지에서 포스팅 보기 페이지 이동  
	- 포스팅 에디트 버튼을 통해 에딧페이지 이동
	- 에딧페이지는 캐싱된 데이터 가져와서 오래된 데이터만 보여줌
	- 리스트 가져오는 페이지로 이동시 동일하게 오래된 데이터 보여줌

__개별 게시물 항목과 전체 게시물 목록을 강제로 다시 가져오는 방법 필요__

- "tag" 로 캐싱 데이터 invalidate 하는 방법 봤음
	- getPosts 의 'post', addNewPost의 'post' 
	- 새 post 추가할 때 마다 getQuery 강제 재실행
- 'post' tag를 getPost와 editPost mutation 둘 다 추가해야함.
	- 이렇게 되면 모든 개별 post가 refetch 되도록 강제됨
	- __RTK query에서 캐시 리셋에 더 선택적인 특정 태그를 만들 수 있음__
	- __`{type: 'Post', id: 123}`__
- `providesTag` 필드를 이용하여 `result`, `arg` 를 받아서 배열을 리턴하는 함수임
	- fetch된 데이터의 ID를 기반으로 tag 전체를 만들 수 있음 

```js
export const apiSlice = createApi({  
	reducerPath: 'api',  
	baseQuery: fetchBaseQuery({ baseUrl: '/fakeApi' }),  
	tagTypes: ['Post'],  
	endpoints: builder => ({  
		getPosts: builder.query({  
			query: () => '/posts',  
			providesTags: (result = [], error, arg) => [  
			'Post',  
			...result.map(({ id }) => ({ type: 'Post', id }))  
			]  
		}),  
		getPost: builder.query({  
			query: postId => `/posts/${postId}`,  
			providesTags: (result, error, arg) => [{ type: 'Post', id: arg }]  
		}),  
		addNewPost: builder.mutation({  
				query: initialPost => ({  
				url: '/posts',  
				method: 'POST',  
				body: initialPost  
			}),  
			invalidatesTags: ['Post']  
		}),  
		editPost: builder.mutation({  
				query: post => ({  
				url: `posts/${post.id}`,  
				method: 'PATCH',  
				body: post  
			}),  
			invalidatesTags: (result, error, arg) => [{ type: 'Post', id: arg.id }]  
		})  
	})  
})
```

- `result`  argument는 응답값이 없거나 error가 있을 때 undefined로 전달됨.
	- 그래서 안전함
- `getPosts`는 argument ID를 기반으로 signle-item array를 리턴하도록 함. (`= []`)
- editPost는 post의 ID를 알고 있음

-> post를 편집하면
- PATCH /posts/:postId 뮤테이션
- GET /posts/:postId 리페치

그리고 나서 메인 posts tab 돌아가면
- /posts 쿼리 리페치

요렇게 tag를 이용하여 endpoint간 관계를 제공하기 때문에, __RTK query는 해당 ID를 가진 특정 태그가 무효화 되었을 때 개별 게시물과 게시물 목록을 다시 가져와야하는 걸 알고 있다.__
편집하는데 오래걸려서 타이머가 만료되는 케이스에도 알아서 데이터 리페치한다. 

- 한가지 주의사항 있음
	- 모든 개별 포스트를 리페치하도록 된다. 
	- getPost 엔드포인트에 대한 게시물 목록을 다시 가져오려면 (`type: 'Post', id: 'LIST'`)와 같은 임시 ID를 가진 추가 태그를 포함하고 대신 해당 태그 무효화할 수 있다. 
	- [Tag Invalidation Behavior](https://redux-toolkit.js.org/rtk-query/usage/automated-refetching#tag-invalidation-behavior)

> RTK query는 데이터 가져오는 방식과 시기를 컨트롤 할 수 있음
> `conditional fetching`, `lazy queries`, `prefetching` 쿼리 정의는 커스터마이징 될 수 있음.




































