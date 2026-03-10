---
created: 24-11-29
modified: 24-11-29
tags:
---
[캐싱](https://nextjs.org/docs/app/building-your-application/caching)
## Summary

페이지 요청시 다음 순서대로 확인

1. Router cache
2. Full Route Cache
3. Request Memoization
4. Data Cache

#### Full Route Cache

1. Server에서의 React 렌더링: 서버컴포넌트를 RSC로 렌더링
2. Server에서 캐싱: RSC payload와 HTML을 캐싱
3. Client에서: Hydaration, Reconciliation
4. Client에서 캐싱: RSC payload 캐싱 (only static rendering)
5. 페이지 이동간에 RSC payload가 Router Cache에 저장되어있는지 확인

빌드시점에 라우트 캐싱여부를 알 수 있다. 페이지의 Static, Dyanmic 렌더링을 결정하기 때문이다.

## overview

기본적으로 routes를 정적으로 렌더되고 데이터 요청은 캐시된다. 
캐싱 동작은 다음 조건에 따라 변경된다
- statically or dynamically rendered
- data is cached or uncahced
- request is part of an initial visit or subsequent navigation


![](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fcaching-overview.png&w=3840&q=75&dpl=dpl_A94AMRojTrsJaESsxoTj8KmPdq46)
## Full Route Cache

넥스트는 자동으로 빌드타임에 라우트를 렌더하고 캐시한다. (모든 서버 요청 마다 렌더링 안하려고)

이걸 이해하려면 React가 렌더링을 처리하고 next가 결과를 어떻게 캐싱하는지 알아야한다

#### 1. Server에서의 React 렌더링
- 렌더링 작업은 chunk로 쪼갠다: 개별 라우트(segment)와 Suspense 경계를 기준으로 청크 단위로 분할 (다음은 분할 단계)
	1. React는 서버 컴포넌트를 RSC(스트리밍에 최적화된 데이터 포맷)로 렌더링 
	2. next는 서버에서 HTML로 렌더한다. 이 때 RSC payload와 Client Component JS 지시어를 사용
- 이 방식은 모든 렌더링이 완료될 때까지 기다릴 필요 없이 작업이 완료됨에 따라 캐싱하거나 응답을 스트리밍으로 보낼 수 있음을 의미.

> RSC payload?
> 클라에서 React가 브라우저 DOM을 업데이트 하는데 사용. RSC payload는 다음을 포함
> - Server Component 렌더링 결과
> - 클라 컴포넌트가 렌더링 되어야할 위치에 대한 자리 표시자와 JS 파일에 대한 참조
> - Server Component에서 클라 컴포넌트로 전달된 모든 props

### 2. Server에서 next의 캐싱 (Full Route Cache)
next는 기본적으로 렌더링 결과(RSC payload and HTML)를 캐싱한다. 빌드 시점의 정적 렌더링 라우트 혹은 revalidation 동안 적용된다. 

### 3. React Hydaration과 클라에서 Reconciliation

요청 시점에서 클라는:

1. HTML은 클라와 서버 컴포넌트의 빠른 non-interactive 초기 프리뷰를 표시하는데 사용
2. RSC payload는 클라와 렌더링된 서버 컴포넌트 트리를 재조정(Reconcile)하고 DOM을 업데이트 하는데 사용
3. JS 명령어는 클라를 hydrate하여 interactive로 만듬.

### 4. Client에서 next 캐싱 (Router Cache)
RSC payload는 [[#Client-side Router Cache]] 에 저장된다. 이 캐시는 개별 라우트 별로 분리된 in-memory 캐시다. 이전에 방문한 경로 저장, 앞으로 방문한 경로 prefetch하여 네비게이션 경험을 향상시킨다.


### 5. Subsequent Navigations

탐색하거나 prefetch하는 동안 next는 RSC payload가 Router Cache에 저장되어있는지 확인. 만약 캐시가 저장되어 있다면 새 요청 안보낸다. 반대로, 라우트 세그먼트 캐에 없으면 next는 서버에서 RSC payload를 가져와 클라 Router캐시에 저장한다.


## Static and Dynamic Rendering

빌드 시점에 라우트 캐싱여부는 static, dynamic 에 따라 결정된다.
- 정적 렌더링: 캐싱
- 동적 렌더링: 요청 시점에 렌더링, 캐싱 안됨

















