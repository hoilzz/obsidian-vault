--- 
creation date: 23-03-08 
modification date: 23-03-08 
tag: #dev/react-query

---

## 문제
- SSR 환경의 쿼리에 설정한 option이 dehydration에서 없어진다
	- 가설1. 애초에 설정도 안됨
	- 아래 보면 잘 설정되어있음 
	  ```
	   queriesMap: {
pageweb-v2:     '["seriesListViewLandingList.infinite",{"dataKey":"page/landing/517","size":25}]': Query {
pageweb-v2:       abortSignalConsumed: false,
pageweb-v2:       defaultOptions: undefined,
pageweb-v2:       options: [Object],
pageweb-v2:       cacheTime: Infinity, // 무한으로 잘 설정됨.
pageweb-v2:       observers: [],
pageweb-v2:       cache: [Circular *1],
pageweb-v2:       logger: [Object [console]],
pageweb-v2:       queryKey: [Array],
pageweb-v2:       queryHash: '["seriesListViewLandingList.infinite",{"dataKey":"page/landing/517","size":25}]',
pageweb-v2:       initialState: [Object],
pageweb-v2:       state: [Object],
pageweb-v2:       revertState: [Object],
pageweb-v2:       retryer: [Object],
pageweb-v2:       promise: [Promise],
pageweb-v2:       isFetchingOptimistic: false
pageweb-v2:     }
pageweb-v2:   }
pageweb-v2: }

		```

	- 가설 2. 설정은 됐으나 dehydrate 메서드에서 제거함


## 결론
https://github.com/TanStack/query/discussions/2443

- 서버에서의 옵션이 클라와 의도적으로 다른 경우가 많기 때문에 dehydrate에서 제거된다고함.
- 걍 option을 서버와 클라간에 공유해서 쓰라고함.
- 이 글쓴이와 동일하게 기본 옵션을 클라와 공유하는 것은 각 쿼리에 대한 옵션 유지하는것과 다름. 그래서 직접 구현함.

### 1. how to update

```ts
query[key] = options[query.queryHash]?.[key]


interface QueryOption {
  [key in string /** queryHash */]: 
}


```
