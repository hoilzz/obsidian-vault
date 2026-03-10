---
created: 24-07-27
modified: 24-07-27
tags:
---
# 요약

- layout.js, page.js, template.js 로 route의 UI 만들 수 있음. 
- 이 특별한 파일을 언제 어떻게 사용하는지 알아보자
- root/layout.js
	- html, body 포함
	- _app, _document 파일을 대체함.
- 레이아웃은 
	- 서버 컴포넌트가 기본인데 클라 컴포넌트도 사용 가능. 
	- fetch Data 가능.
	- Parent 와 자식간에 데이터 전달 불가능.
		- 근데 같은 경로에서 동일한 데이터를 2번 이상 페치하는 건 가능. 성능 영향 없음. 요청 자동 중복 제거
	- 아래 경로 세그먼트에 액세스 불가. 모든 경로 세그먼트에 액세스 하려면 클라 컴포넌트에서 hook사용해야함. 
- 템플릿
	- 자식 레이아웃 or page를 래핑하는 점에서 레이아웃과 비슷
	- 레이아웃과 비슷한데 라우팅마다 상태 유지x, 컴포넌트 리렌더링 -> DOM recreate
	- 로깅이나 페이지별 다른 피드백 줄 때 사용

# 기록

### Pages
- page는 항상 라우트 서브트리의 리프노드여야함
- Pages는 기본적으로 Server Component, 근데 Client Component도 세팅 가능
- Page는 data fetch 가능. 

## Layouts

- 여러 경로 간에 공유됨
- 상태 보존, interactive 유지, 리렌더 안함
- 중첩 가능


### Root Layout (필수)
- app directory의 가장 상위 레벨로 정의되고 모든 라우트에 적용됨.
- `html`, `body` 태그를 무조건 포함

### Nesting Layouts
- 폴더 계층에서 중첩될 수 있음 -> `children` prop으로 자식 레이아웃을 래핑할 수 있다는 것
- 특정 라우트 세그먼트 (folders) 내부에 layout js를 추가하여 중첩 가능. 
- 2개 레이아웃 합치기 
```
<Layout> <-- app/layout.js
  <DashBoardLayout /> <-- app/dashboard/layout.js
</Layout>
```
![](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fnested-layouts-ui.png&w=3840&q=75)


### Templates

- 자식 레이아웃 or page를 래핑하는 점에서 레이아웃하고 비슷함
- 라우팅간에 상태 유지하는 레이아웃과 다르게 템플릿은 new instance를 만듬.
	- 유저가 유저가 템플릿을 공유하는 라우트로 이동할 때, component new instance mount, DOM recreated, state is not preserved
- 위와 같은 동작이 필요할 때 template이 적절한 옵션임
	- logging 등
	- 기본 프레임워크 동작 변경
		- 레이아웃 내부의 서스펜스 바운더리는 처음 로드할때만 폴백 표시. 페이지 전환시에는 표시안함.

```tsx
<Layout> {/* Note that the template is given a unique key. */} 
	<Template key={routeParam}>{children}</Template>
</Layout>
```


## Metadata
- `title`  과 같은 `<head>`,를 수정할 수 있음. 



















