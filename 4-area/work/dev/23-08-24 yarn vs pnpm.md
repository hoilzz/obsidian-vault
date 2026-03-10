--- 

creation date: 23-08-24 
modification date: 23-08-24 
tag: #dev/pnpm

---

- turborepo와 궁합이 좋은거
- 마이그레이션 더 쉬운거
- yarn vs pnpm 패키지 관리 방식 차이
	- node_modules를 버리고 선택한 방식
		- yarn Berry의 pnp
		- pnpm에서 node_modules 의존성을 더욱 효율적으로 저장하기 위해 몇가지 새로운 개념
- yarn -> pnpm 옮긴 이유 블로그
- node_linker 쓰는 이유
- yarn에서는 다 zip file 넣어버리는데 typescript는 어떻게 지원할까? 그래서 sdk쓰라는말일까?

https://npmtrends.com/pnpm-vs-yarn

### npm

npm, yarn v1은 중복 설치되는 node_modeuls를 아끼기 위해 hoisting 기법을 사용.

![hoisting](https://static.toss-internal.com/ipd-tcs/toss_core/staging/90646656-6012-4da1-95b4-f4b7479beb50)


- 왼쪽에서는 A, B 두번 설치로 디스크 공간 낭비
- 그래서 오른쪽으로 바꿈 
- 근데 여기서 호이스팅으로 직접 의존하고 있지 않은 라이브러리를 import 할 수 있는데 이게 __유령 의존성__ 
	- package.json에 명시되지 않은 라이브러리 사용가능
	- 이 때 그 라이브러리를 하위 의존성으로 가진(호이스팅된) 얘를 제거하면 코드에서 사용불가. 갑자기 에러 발생.
- 유령 의존성 문제 때문에 pnp 나옴

### yarn berry(pnp)

- node_modules 없음
	- 중첩된 폴더 구조 아닌 단일 파일
- 의존성 조회 테이블 pnp.cjs 생성
	- yarn berry의 독자적인 resolution 방식 사용
	- I/O 없이 어떤 패키지가 어떤 라이브러리에 의존하는지 바로 알 수 있음
- ZipFS
	- .zip으로 파일관리 (디스크 공간 덜 차지)
	- node_modules 디렉토리 구조 생성할 필요가 없어서 설치 빠름
	- 각 패키지는 버전마다 zip 파일 가지니까 중복 설치 안됨.
	- 의존성 구성하는 파일 수가 많지 않으므로 변경사항 감지나 전체 의존성 삭제하는 작업 빠름.
- 결론
	- 의존성 검색 빠름(node_modules와 같은 중첩구조 아니라서, 불필요한 I/O도 의존 테이블 파일만 보면 되니까 줄어듬)
	- 같은 버전의 패키지가 여러번 복사되어 설치시간 단축 가능
	- 의존성 끌어올리기 없어서 유령 패키지 쓸일 없음. pacakge.json에 명시된 얘만 사용
	- 노드모듈 가끔 의존성 관리 잘 안되면 다 삭제하고 재설치하면 잘되는 경우 많음.
		- zip파일만으로 플랫하게 패키지 관리하니까 의존성 찾기 + 파일 변경 찾기 쉬움
	- zero-install
		- 의존성을 아예 git에다 올려버림. 그정도로 파일 크기가 작음 zip이라!
			- 삭제 안하니까 git checkout 때 yarn install 이득
- https://toss.tech/article/node-modules-and-yarn-berry

### pnpm

- content-addressable storage
	- 전역 저장소에 패키지를 저장
		- 의존성의 모든 버전은 해당 폴더에 한 번만 저장되어 디스크 공간 절약
		- ![pnpm content-addressable storage](https://paper-attachments.dropbox.com/s_06628BA9DA0E8DC14A93E4CBCCE4BA837F5D33E2B8D8370E3436FBBAA1B6F755_1642690228383_image.png)
- symlink된 node_modules 구조
	- pnpm은 symlink를 사용하여 프로젝트의 직접적인 의존성만을 모듈 디렉토리의 루트로 추가
	- ![symbolic link](data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiIHN0YW5kYWxvbmU9Im5vIj8+CjwhRE9DVFlQRSBzdmcgUFVCTElDICItLy9XM0MvL0RURCBTVkcgMS4xLy9FTiIgImh0dHA6Ly93d3cudzMub3JnL0dyYXBoaWNzL1NWRy8xLjEvRFREL3N2ZzExLmR0ZCI+CjxzdmcgeG1sbnM6eGw9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGxpbmsiIHhtbG5zOmRjPSJodHRwOi8vcHVybC5vcmcvZGMvZWxlbWVudHMvMS4xLyIgdmVyc2lvbj0iMS4xIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjE1Ny40NTMxMiAxNjQgMjI0Ljc2NTYzIDMyMy4zNTkzOCIgd2lkdGg9IjIyNC43NjU2MyIgaGVpZ2h0PSIzMjMuMzU5MzgiPgogIDxkZWZzPgogICAgPG1hcmtlciBvcmllbnQ9ImF1dG8iIG92ZXJmbG93PSJ2aXNpYmxlIiBtYXJrZXJVbml0cz0ic3Ryb2tlV2lkdGgiIGlkPSJGaWxsZWRBcnJvd19NYXJrZXIiIHN0cm9rZS1saW5lam9pbj0ibWl0ZXIiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgdmlld0JveD0iLTEgLTQgMTAgOCIgbWFya2VyV2lkdGg9IjEwIiBtYXJrZXJIZWlnaHQ9IjgiIGNvbG9yPSIjN2Y4MDgwIj4KICAgICAgPGc+CiAgICAgICAgPHBhdGggZD0iTSA4IDAgTCAwIC0zIEwgMCAzIFoiIGZpbGw9ImN1cnJlbnRDb2xvciIgc3Ryb2tlPSJjdXJyZW50Q29sb3IiIHN0cm9rZS13aWR0aD0iMSIvPgogICAgICA8L2c+CiAgICA8L21hcmtlcj4KICA8L2RlZnM+CiAgPGcgaWQ9IkNhbnZhc18xIiBzdHJva2UtZGFzaGFycmF5PSJub25lIiBmaWxsPSJub25lIiBzdHJva2Utb3BhY2l0eT0iMSIgZmlsbC1vcGFjaXR5PSIxIiBzdHJva2U9Im5vbmUiPgogICAgPHRpdGxlPkNhbnZhcyAxPC90aXRsZT4KICAgIDxyZWN0IGZpbGw9IndoaXRlIiB4PSIxNTcuNDUzMTIiIHk9IjE2NCIgd2lkdGg9IjIyNC43NjU2MyIgaGVpZ2h0PSIzMjMuMzU5MzgiLz4KICAgIDxnIGlkPSJDYW52YXNfMV9MYXllcl8xIj4KICAgICAgPHRpdGxlPkxheWVyIDE8L3RpdGxlPgogICAgICA8ZyBpZD0iR3JhcGhpY18yIj4KICAgICAgICA8dGV4dCB0cmFuc2Zvcm09InRyYW5zbGF0ZSgxNjcuNDUzMTIgMTc0KSIgZmlsbD0iYmxhY2siPgogICAgICAgICAgPHRzcGFuIGZvbnQtZmFtaWx5PSJNb25hY28iIGZvbnQtc2l6ZT0iMTYiIGZpbGw9ImJsYWNrIiB4PSIwIiB5PSIxNiI+bm9kZV9tb2R1bGVzPC90c3Bhbj4KICAgICAgICA8L3RleHQ+CiAgICAgIDwvZz4KICAgICAgPGcgaWQ9IkdyYXBoaWNfMyI+CiAgICAgICAgPHRleHQgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMTk3IDIwNS4zMzU5NCkiIGZpbGw9IiMwMDk2ZmYiPgogICAgICAgICAgPHRzcGFuIGZvbnQtZmFtaWx5PSJNb25hY28iIGZvbnQtc2l6ZT0iMTYiIGZpbGw9IiMwMDk2ZmYiIHg9IjAiIHk9IjE2Ij5leHByZXNzPC90c3Bhbj4KICAgICAgICA8L3RleHQ+CiAgICAgIDwvZz4KICAgICAgPGcgaWQ9IkdyYXBoaWNfNCI+CiAgICAgICAgPHRleHQgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMTk3IDIzNi42NzE4OCkiIGZpbGw9ImJsYWNrIj4KICAgICAgICAgIDx0c3BhbiBmb250LWZhbWlseT0iTW9uYWNvIiBmb250LXNpemU9IjE2IiBmaWxsPSJibGFjayIgeD0iMCIgeT0iMTYiPi5wbnBtPC90c3Bhbj4KICAgICAgICA8L3RleHQ+CiAgICAgIDwvZz4KICAgICAgPGcgaWQ9IkdyYXBoaWNfNSI+CiAgICAgICAgPHRleHQgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMjI3IDI2OC4wMDc4KSIgZmlsbD0iYmxhY2siPgogICAgICAgICAgPHRzcGFuIGZvbnQtZmFtaWx5PSJNb25hY28iIGZvbnQtc2l6ZT0iMTYiIGZpbGw9ImJsYWNrIiB4PSIwIiB5PSIxNiI+ZXhwcmVzc0A0LjE3LjM8L3RzcGFuPgogICAgICAgIDwvdGV4dD4KICAgICAgPC9nPgogICAgICA8ZyBpZD0iR3JhcGhpY182Ij4KICAgICAgICA8dGV4dCB0cmFuc2Zvcm09InRyYW5zbGF0ZSgyNTcgMjk5LjM0Mzc1KSIgZmlsbD0iYmxhY2siPgogICAgICAgICAgPHRzcGFuIGZvbnQtZmFtaWx5PSJNb25hY28iIGZvbnQtc2l6ZT0iMTYiIGZpbGw9ImJsYWNrIiB4PSIwIiB5PSIxNiI+bm9kZV9tb2R1bGVzPC90c3Bhbj4KICAgICAgICA8L3RleHQ+CiAgICAgIDwvZz4KICAgICAgPGcgaWQ9IkdyYXBoaWNfNyI+CiAgICAgICAgPHRleHQgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMjg3IDMzMC42Nzk3KSIgZmlsbD0iYmxhY2siPgogICAgICAgICAgPHRzcGFuIGZvbnQtZmFtaWx5PSJNb25hY28iIGZvbnQtc2l6ZT0iMTYiIGZpbGw9ImJsYWNrIiB4PSIwIiB5PSIxNiI+ZXhwcmVzczwvdHNwYW4+CiAgICAgICAgPC90ZXh0PgogICAgICA8L2c+CiAgICAgIDxnIGlkPSJHcmFwaGljXzgiPgogICAgICAgIDx0ZXh0IHRyYW5zZm9ybT0idHJhbnNsYXRlKDI4NyAzNjIuMDE1NjIpIiBmaWxsPSIjMDA5NmZmIj4KICAgICAgICAgIDx0c3BhbiBmb250LWZhbWlseT0iTW9uYWNvIiBmb250LXNpemU9IjE2IiBmaWxsPSIjMDA5NmZmIiB4PSIwIiB5PSIxNiI+Y29va2llPC90c3Bhbj4KICAgICAgICA8L3RleHQ+CiAgICAgIDwvZz4KICAgICAgPGcgaWQ9IkdyYXBoaWNfOSI+CiAgICAgICAgPHRleHQgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMjI3IDM5My4zNTE1NikiIGZpbGw9ImJsYWNrIj4KICAgICAgICAgIDx0c3BhbiBmb250LWZhbWlseT0iTW9uYWNvIiBmb250LXNpemU9IjE2IiBmaWxsPSJibGFjayIgeD0iMCIgeT0iMTYiPmNvb2tpZUAwLjQuMjwvdHNwYW4+CiAgICAgICAgPC90ZXh0PgogICAgICA8L2c+CiAgICAgIDxnIGlkPSJHcmFwaGljXzEwIj4KICAgICAgICA8dGV4dCB0cmFuc2Zvcm09InRyYW5zbGF0ZSgyNTcgNDI0LjY4NzUpIiBmaWxsPSJibGFjayI+CiAgICAgICAgICA8dHNwYW4gZm9udC1mYW1pbHk9Ik1vbmFjbyIgZm9udC1zaXplPSIxNiIgZmlsbD0iYmxhY2siIHg9IjAiIHk9IjE2Ij5ub2RlX21vZHVsZXM8L3RzcGFuPgogICAgICAgIDwvdGV4dD4KICAgICAgPC9nPgogICAgICA8ZyBpZD0iR3JhcGhpY18xMSI+CiAgICAgICAgPHRleHQgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMjg3IDQ1Ni4wMjM0NCkiIGZpbGw9ImJsYWNrIj4KICAgICAgICAgIDx0c3BhbiBmb250LWZhbWlseT0iTW9uYWNvIiBmb250LXNpemU9IjE2IiBmaWxsPSJibGFjayIgeD0iMCIgeT0iMTYiPmNvb2tpZTwvdHNwYW4+CiAgICAgICAgPC90ZXh0PgogICAgICA8L2c+CiAgICAgIDxnIGlkPSJMaW5lXzIwIj4KICAgICAgICA8cGF0aCBkPSJNIDE5MiAyMTYuMDAzOSBMIDE4MiAyMTYuMDAzOSBMIDE4MC43NSAyMTYuMTI1IEwgMTc5LjUgMjE2LjEyNSBMIDE3OS41IDM0MS4zNDc2NiBMIDI3Mi4xIDM0MS4zNDc2NiIgbWFya2VyLWVuZD0idXJsKCNGaWxsZWRBcnJvd19NYXJrZXIpIiBzdHJva2U9IiM3ZjgwODAiIHN0cm9rZS1saW5lY2FwPSJyb3VuZCIgc3Ryb2tlLWxpbmVqb2luPSJyb3VuZCIgc3Ryb2tlLXdpZHRoPSIxIi8+CiAgICAgIDwvZz4KICAgICAgPGcgaWQ9IkxpbmVfMjEiPgogICAgICAgIDxwYXRoIGQ9Ik0gMjgyIDM3Mi42ODM2IEwgMjcyIDM3Mi42ODM2IEwgMjE3LjUgMzczIEwgMjE4IDQ1Ny41IEwgMjE4IDQ2Ni42OTE0IEwgMjcyLjEgNDY2LjY5MTQiIG1hcmtlci1lbmQ9InVybCgjRmlsbGVkQXJyb3dfTWFya2VyKSIgc3Ryb2tlPSIjN2Y4MDgwIiBzdHJva2UtbGluZWNhcD0icm91bmQiIHN0cm9rZS1saW5lam9pbj0icm91bmQiIHN0cm9rZS13aWR0aD0iMSIvPgogICAgICA8L2c+CiAgICA8L2c+CiAgPC9nPgo8L3N2Zz4K)
	- 이 구조로 실제 의존성 있는 패키지에만 액세스 가능 



[pnpm 개발자가 본 npm, yarn이 가진 문제](https://medium.com/pnpm/why-should-we-use-pnpm-75ca4bfe7d93)
- Content-addressable storage: 호이스팅 대신 의존성 해결 전략 대안
	- 심볼릭 링크를 사용하여 의존성의 중첩 구조 생성
		- node_modules 중첩구조를 그대로 이용하면서 심볼릭 링크를 사용한다는 말일까?
	- ~/.pnpm-store 의 전역 저장소에 패키지를 저장하는 중첩된 node_modules 폴더 생성
	- 의존성의 모든 버전은 해당 폴더에 물리적으로 한 번만 저장되어 
		- 단일 정보 소스 구성, 디스크 공간 절약
- non-flat node_modules 디렉토리
	- npm 또는 yarn 클래식을 통해 의존성 설치시 루트로 모두 호이스트
	- 위에서 말한 symlink를 사용하여 프로젝트의 직접적인 의존성만을 모듈 디렉토리 루트로 추가
https://pnpm.io/ko/blog/2020/05/27/flat-node-modules-is-not-the-only-way
https://pnpm.io/ko/symlinked-node-modules-structure
https://pnpm.io/feature-comparison

##### yarn berry에서 pnpm, symlink 지원
- nodeLinker: [pnpm](https://yarnpkg.com/features/linkers#nodelinker-pnpm)
	- 프로젝트의 각 종속성에 대해 하나의 폴더를 포함하는 flat folder가 node_modules/.store 안에 생성됨. 
	- 각 종속성 폴더는 시스템의 모든 프로젝트에 공통된 중앙 저장소($HOME/.yarn/berry/index)에서 가져온 하드링크로 채워진다. 
		- 마지막으로 플랫 스토어에서 관련 폴더로 연결되는 심볼릭 링크에 node_modules 폴더에 배치됨.
	- Pros: Content-addressable store, __some__ ghost 종속성을 보호함.
	- Cons: 심링크는 툴이 항상 지원하지 않음.??, 하드 링크는 이상한 동작 유발 할 수 있음. 일반 종속성 오류



### 후기들
- [ab180 yarn -> pnpm](https://engineering.ab180.co/stories/yarn-to-pnpm)
	- yarn pnp가 git에 지속적으로 주는 부하
		- 커밋 수 10000개 넘고 yarn/cache 파일이 많아져서 git checkout, branching속도에 영향 줄 거 같다고 함
	- PnP가 다른 패키지와 호환되지 않아 빌드 깨지면서 PnP대신 nodeLinker로 변경해서 Yarn berry 쓸 이유가 없었다고 함
	- 유령 의존성 문제 발생
		- Yarn berry 의 의존성 테이블이 유령의존성 안가져오도록 할텐데 nodeLinker로 변경해서 발생한 문제일듯..?
	- pnpm 도입 후 패키지 설치 시간 줄어들고 node버전 관리 용도로도 잘 쓰고 있다고 함 (물론 nvm처럼 프로젝트별 노드 버전 관리는 안됨)
- [화해 모노레포 yarn vs pnpm](https://blog.hwahae.co.kr/all/tech/11962)
	- yarn, pnpm 캐시 유무에 따라 의존성 설치 완료 시간 나와있음
	- cache가 없을 때 yarn pnp가 2배 느림
	- cache 있으면 yarn pnp가 2배 이상 빠르거나 설치 생략가능
	- 캐시 전략이 있다면 yarn pnp로 
	- 
	-  ![[스크린샷 2023-08-25 오전 10.19.08.png]]
	- https://blog.logrocket.com/javascript-package-managers-compared/#performance-disk-space-efficiency