---
created: 24-12-02
modified: 24-12-02
tags:
---
## summary

SSR은 페이지 블락킹이 심해서 이에 대한 해결책으로 Streaming을 통해 페이지의 일부를 빠르게 보여줄 수 있다.


### SSR vs Streaming
##### SSR limitation
![](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fserver-rendering-without-streaming-chart.png&w=3840&q=75&dpl=dpl_A94AMRojTrsJaESsxoTj8KmPdq46)
위 이미지의 스텝은 sequential하고 blocking한걸 알 수 있다. A과정이 모두 끝나야만 HTML 렌더링 가능하기 때문이다. 
그래서 아래와 같은 문제가 발생한다. 서버에서 데이터 페치(A)가 완료되기 전까지 페이지는 보여질 수 없다.
![](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fserver-rendering-without-streaming.png&w=3840&q=75&dpl=dpl_A94AMRojTrsJaESsxoTj8KmPdq46)

---

#### Streaming

페이지를 작은 청크단위로 쪼개고 점진적으로 청크를 서버에서 클라로 보낸다. 이것을 통해 모든 데이터가 로드될 때까지 기다리지 않고 페이지의 __일부__ 를빠르게 표시할 수있다.
![](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fserver-rendering-with-streaming.png&w=3840&q=75&dpl=dpl_A94AMRojTrsJaESsxoTj8KmPdq46)

![](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fserver-rendering-with-streaming-chart.png&w=3840&q=75&dpl=dpl_A94AMRojTrsJaESsxoTj8KmPdq46)

스트리밍은 TTFB, FCP를 단축할 . 수있고 느린 기기에서 TTI를 개선하는데도 도움이 된다. 
