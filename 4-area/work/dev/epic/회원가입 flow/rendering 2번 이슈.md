## 

### 다른 페이지는 문제가 안되는가?
1. IsLoginChecker 컴포넌트만 로드 (여기서 2번 리렌더하니까 문제 안될듯.)
2. 로그인 후에 다시 컴포넌트 그릴 때 의도된 동작만 함..

### Automatic Static Optimization
- 블로킹하는 데이터 요청이 없으면 페이지가 Static 하게 결정된다. 
	- 블로킹하는 데이터 요청 = getInitialProps or getServerSideProps 존재 여부
- 정적으로 생성된 페이지는 reactive하다.
	- 넥스트는 클라측과 하이드레이트하여 상호작용 제공한다.
- 요거 장점은 최적화된 페이지는 SSR 계산이 필요없다.
	- 엔드유저에게 스트리밍한다.

#### 동작방법
- getServersideProps or getInitialProps가 있으면 next는 on-demand (SSR)로 바꿈
- 위의 경우가 아니면 Next는 static HTML을 prerendering하여 정적으로 최적화한다.
- prerendering 동안 query obejct는 비어있다. 
- hydration 후에 next는 query 객체에 route 파라미터를 제공하여 앱에 업데이트한다.


- `next build`는 정적으로 최적화된 페이지인  `.html` 파일을 만든다.
- 예컨대 pages/about.js 는 pages/about.html이 된다.
	- 만약 getserversideprops 를 추가하면 html이 아닌 JS가 된다.
	- /pages/about.js

### Caveats
- getInitialProps를 가진 custom `App` 이 있다면 위 최적화는 꺼진다. 


---
내 뇌피셜
- pre-render후에 hydration 과정에서 두번 그려진다.
- 왜 문제가 안됐나?
	- dispatch 사용할 경우 batch 형태로 수행된다.
	- isLoginChecker와 같은 얘가 막아줬다.
	- 쿼리는?
		- 아마 캐시된 얘를 들고 오지 않았을까..
		- 캐시된 얘 풀린거 많을텐데 거기선 문제 없었는디;
---

## react-hydration-error

(SSR/SSG) 에서 프리렌더링한 리액트 트리와 브라우저에서 첫번쨰 렌더 동안 렌더링된 리액트 트리가 차이가 있을 수 있다. Hydration 이라고 부르는 첫번째 렌더는 [React의 기능](https://reactjs.org/docs/react-dom.html#hydrate)이다.  그래서 React 트리가 DOM과 동기화 되지않고 예기치 않은 콘텐츠/속성이 나타날 수 있다. 

#### 이걸 고치려면

특정 라이브러리나 app code때문에 일어나늗네. 요건 프리렌더링과 브라우저 간에 다른 무언가를 의존하기 때문이다. `window ` 를 사용하는 예제
`