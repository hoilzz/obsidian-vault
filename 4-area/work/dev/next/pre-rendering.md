# pre-rendering

Next.js는 모든 페이지를 _pre-render_ 합니다. 이것은 client-side JavaScript로 실행되는 것 대신에, Next.js가 미리 각 페이지에 대한 HTML을 생성한다는 뜻입니다. Pre-rendering은 SEO와 더 나은 퍼포먼스 결과를 낳습니다. 

각각 생성된 HTML은 각 페이지에 필요한 최소한의 JavaScript code와 연결됩니다. 브라우저가 페이지를 로드할 때, 해당 JS 코드는 실행되고 페이지를 완전히 인터랙티브하게 만듭니다. (이 과정을 _hydration_ 이라고 부릅니다.)

## Pre-rendering의 2가지 형태

Next.js는 __Stiatic Generation__ 그리고 __Server-side Rendering__ 이 두가지 형태를 가진다.
차이점은 page에 대한 HTML 생성 __시점__ 입니다.

- __Static Generation (Recommended)__ : HTML은 __build time__ 에 생성되고 각 요청마다 재사용 될 것입니다.
- __Server-side Rendering__ : HTML은 __request 마다__ 생성됩니다.

중요한 것은 Next.js가 각 페이지에 대한 pre-rendering 형태를 개발자가 __선택__ 하도록 합니다. 대부분의 페이지를 Static Generation을 나머지는 SSR을 사용하여 "hybrid" Next.js app을 생성할 수 있습니다.

몇가지 퍼포먼스 이유로 SSR보다 __Static Generation__ 을 이용하기를 추천합니다. 스태틱하게 생성된 페이지는 성능 향상을 위한 추가 설정 없이 CDN에서 캐시될 수 있습니다. 그러나, 몇몇 경우에 SSR이 유일한 옵션일 경우도 있습니다.

Static Generation or SSR과 함꼐 CSR을 이용할 수도 있습니다. 몇몇 페이지는 client side JS로 완전히 렌더링 될 수 있습니다. 

...

### Static Genration 언제 사용해야 할까?
가능하면 __Static Generation__ 을 추천한다. 왜냐하면 page는 한번 빌드되고 CDN이 서빙한다. 이것은 모든 요청마다 page를 만드는 SSR보다 훨씬 빠르다.

- Marketing pages
- Blog posts and portfolio
- E-commerce product listing
- Help and documentation

사용자가 요청하기 전에 페이지를 미리 렌더링할 수 있나요? 라고 물을 수 있다.
만약 대답이 yes라면, Static Generation을 선택해야만한다. 

반면에 만약 유저가 요청하기 전에 페이지를 미리 생성할 수 없다면 Static Generation은 좋은 생각이 아니다. 아마 페이지는 업데이트된 데이터를 빈번하게 보여줘야하고 페이지 컨텐츠는 모든 요청마다 변경될 것이다. 

이와 같은 경우에 다음 중 한가지를 할 수 있다.
- CSR로 Static Generation을 사용하기: 페이지의 일정 부분 렌더링을 스킵하고 나서 client-side JS를 사용하여 채울 수 있다. 이부분은 Data Fetching 문서를 읽어보자.
- SSR 사용하기: Next.js는 모든 요청마다 페이지를 pre-render한다. CDN에 캐시되지 않기 때문에 더 느릴거다. 하지만 pre-rendered page는 항상 최신상태다. 