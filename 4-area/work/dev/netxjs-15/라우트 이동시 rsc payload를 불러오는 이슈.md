---
created: 24-11-30
modified: 24-11-30
tags:
---
![image](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fcaching-overview.png&w=3840&q=75&dpl=dpl_A94AMRojTrsJaESsxoTj8KmPdq46)

### 현상

`/A` -> `/A/modal` -> `/A` 

A -> A/modal -> A 와 같은 라우트가 있다. A에서는 list를 보여주고 A/modal 에서는 list에 새 아이템을 추가할 수 있다. 추가 완료 후 A의 list를 새로 받아온다. A는 서버컴포넌트인데 캐시 미스가 발생하여 서버에서 A의 서버컴포넌트를 다시 생성한다. 이 때 A 서버 컴포넌트에서 A의 list를 새로 받아온다. 

A와 A/modal의 page.js는 dynamic rendering 된다. (next build로 확인)

그러면 b,c는 라우트 캐시 되지않는다

왜냐면 [[Dynamic Rendering]]은 라우트 캐시 되지 않기 때문이다. 
- 이 말은 매번 서버에 요청해야함. 
- 근데 매번 요청안하고 특정 시간이 지나면 요청함. 이 말은 캐싱되고 있다는 말이 아닐까?
- 캐싱이 된다면 라우트간에 [[RSC payload]]를  저장한다는 그 말이 해당되는듯

[next 14버전의 경로 세그먼트 캐싱](https://nextjs.org/docs/14/app/building-your-application/caching#opting-out-3) 을 보면 30초 저장한다는 것을 알 수 있다. 실제로 테스트 해보면 30초 동안 라우트캐시에 저장된다. 

### 해결책

- 라우트 캐시를 무한([stale time](https://nextjs.org/docs/app/api-reference/next-config-js/staleTimes))으로 이용 
	- 브라우저에서 라우트간 이동시 데이터의 업데이트는 리액트쿼리에게 맡기자. 