## app directory
https://beta.nextjs.org/docs/routing/fundamentals#the-app-directory

- app directory는 pages와 나란히 위치함(디렉토리 레벨이 같음)
- 요렇게 하면
	- 특정 라우트는 새 동작 넣을 수 있고, `pages` 안의 라우트는 이전 동작 유지할 수 있음
- 디렉터리 간 route는 동일한 URL로 리졸브되면 안됨 (이렇게 되면 에러 발생함)
- 기본적으로 app안의 컴포넌트는 React Server Component임 (퍼포먼스 최적화 때문)
	- Client Component도 사용가능. (next12의 `pages/` directory)
	- https://beta.nextjs.org/docs/rendering/server-and-client-components

### app 안의 folder와 files

- Folder
	- router 정의 app/dashboard/settings = {HOST}/dashboard/settings
- files
	- 라우트 세그먼트에 보여지는 UI 생성

#### File Conventions
- page.js
	- 경로의 고유한 UI를 만들고 이 경로를 공개적으로 액세스 가능하도록함.
- layout.js
	- 공유 UI segment와 그 children


---
