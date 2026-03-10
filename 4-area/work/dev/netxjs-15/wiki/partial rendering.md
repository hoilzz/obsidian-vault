---
created: 24-11-29
modified: 24-11-29
tags:
---
https://nextjs.org/docs/app/building-your-application/routing/linking-and-navigating#4-partial-rendering

Partial Rendering은 내비게이션 시 변경되는 경로만 클라에서 다시 렌더링되고, 공유되는 경로는 유지되는 것을 의미한다.

`/dashboard/A` -> `/dashboard/B` 내비게이션간에 A의 page는 언마운트, B페이지가 새로운 상태로 마운트되며, dashboard 레이아웃은 유지된다. 

Partial Rendering이 없을 경우, 내비게이션마다 전체 페이지가 클라에서 다시 렌더링된다. 
> 당연한 얘기를 하고 있네;

