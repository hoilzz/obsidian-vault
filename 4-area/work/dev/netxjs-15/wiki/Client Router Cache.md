---
created: 24-11-30
modified: 24-11-30
tags:
---


next는 in-memory 클라이언트 라우터 캐시를 제공한다. 이 캐시는 레이아웃, 로딩 상태, 페이지 등으로 나뉜 라우트 세그먼트의 RSC payload를 저장한다.

### 라우터 캐시는 라우트 방문간에 이뤄진다.

라우터 캐시는 ==라우트 방문 간에 이뤄진다. 방문 간에 라우트 세그먼트를 캐싱한다.== 그래서 즉시 뒤로,앞으로 이동 가능, 풀페이지 로드 없음 등이 가능하다

라우터 캐시는
- 레이아웃: 캐싱되어 navigation에 재사용된다. ([[partial rendering]])
- Loading States(loading.js)는 캐싱되고 내비게이션에서 재사용된다. [instant navigation](https://nextjs.org/docs/app/building-your-application/routing/loading-ui-and-streaming#instant-loading-states)
- Page는 기본적으로 캐싱되지 않는다. 브라우저 뒤로, 앞으로 가기 간에 재사용된다. experimental `staletime`으로 캐싱할 수 있다. [stale time](https://nextjs.org/docs/app/api-reference/next-config-js/staleTimes)

##### 캐시 지속 시간
캐시는 브라우저 임시 메모리에 저장된다. 지속 시간은 다음 두 가지 요인에 의해 결정
1. 세션
   - 캐시는 탐색 중에 유지
   - 페이지 새로고침시 초기화
2. 자동 무효화 기간
   - default prefetching: dynamic page는 캐시 안함. static page는 5분

##### 라우터 캐시 무효화하기
- Server Action
	- revalidatePath
	- revalidateTag
	- cookie.set, cookies.delete
- client
	- router.refresh