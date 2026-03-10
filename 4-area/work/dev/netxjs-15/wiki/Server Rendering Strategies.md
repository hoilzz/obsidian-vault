---
created: 24-11-30
modified: 24-11-30
tags:
---
[Server Rendering Streategies](https://nextjs.org/docs/app/building-your-application/rendering/server-components#server-rendering-strategies)

서버 렌더링 전략은 [[Static Rendering]], [[Dynamic Rendering]], [[Streaming]]이 있다.


## Summary
- dynamic, static 명시적으로 선택할 필요 없다. next가 알아서 선택할거임
- 대신 두가지에 집중
	- 특정 데이터의 캐싱과 데이터 검증 시점을 결정
	- UI의 특정부분을 [[#Streaming]] 할지 여부


Dynamic
- 사용자 요청 시점마다 렌더링
- Dyanmic Routes with Cached Data
	- 캐싱된 제품 데이터와 주기적으로 revalidate되는 페이지
	- RSC payload가 별도로 캐싱되니까 캐싱 데이터 + not 캐싱 데이터를 모두 포함하는 dynamic rendering 사용 가능
	- [[Caching (Full Route Cache)#Full Route Cache]] 와 [[Data Cache]]를 이용
- 쿠키, searchParams prop을 사용하는 개인화 페이지

Streaming
- App Router 쓰면 기본 내장된 걸로 사용
- loading.js, Suspense로 구현



> 지금 드는 생각은
> RSC payload로 react-query 데이터를 업데이트 해야할거 같다.
> 모달에서 invalidate하지말고..


