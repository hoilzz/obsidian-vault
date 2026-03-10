
request-coped 컨텍스트를 어디서든 get, set 가능

- user 상태, JWT의 claim, request/correlations IDs, 요청 스코프 데이터를 저장하기 좋고음
- Context는 비동기에서도 보존된다.





## quote

생각난김에 http-context 관련해서 좀 적어보아요  
팀 내에서 기술부채 염려하셨던 것도 있고(스티브만 아는 기술처럼 생각되는 것 같아서..) 서버에서 파일 변수 사용하면 안되는 이유를 팀 전체가 깊게 이해하면 좋을 것 같아서 주절주절 적어봅니다 ㅎㅎ클라이언트별로 인스턴스가 존재하는 클라 코드와 달리 서버코드는 단 하나의 인스턴스만 존재하지요.  
특히 node.js 는 NIO 라고.. 자바같은 쓰레드 기반 코드가 아닌 단일 쓰레드로 모든 요청을 처리합니다. (내부적으로 더 들어가보면 OS 의 file 로 요청을 위임하죠. 이걸 Event 기반으로 하는거고)쓰레드 하나에 모든게 이벤트 드리븐하다보니 여러 함수 내부에서 공유하는 변수 만들기가 쉽지 않습니다. 자바에선 요청별로 쓰레드 하나니 ThreadLocal 을 이용하면 됐지만 노드는 그런게 없죠.그런데! 서버사이드 개발할 때 변수 공유는 필수입니다. 이게 없으면 모든걸 파라미터로 들고다녀야하는데 굉장히 개발이 불편해지죠. 그래서 이쪽 계열에선 어떻게 하냐.. 바로 node.js 에서 제공하는 async_hook 을 이용합니다.이걸 딥하게 얘기하면 끝이 없을 것 같고, 각자 꼭 공부하셨으면 합니다. 누가 한번 공부해서 공유해주셔도 좋을 것 같구요.우리가 공부하고 딥하게 이해해야 하는 하는 이유는  

- next.js app directory 에서 이 기술을 전반으로 사용하고 있습니다.

- app directory 에서 공유용으로 사용하라고 하는 req.headers, req.cookie 가 이 기술을 사용해서 공유하고 있죠 - [https://github.com/vercel/next.js/blob/32759b48b7af5f1f9df47b6515d0676aa5697cef/packages/next/src/client/components/async-local-storage.ts](https://github.com/vercel/next.js/blob/32759b48b7af5f1f9df47b6515d0676aa5697cef/packages/next/src/client/components/async-local-storage.ts)
- 슬슬 이런 팁들도 나오고 있네요 [https://dev.to/rexessilfie/using-asynclocalstorage-in-nextjs-44c8](https://dev.to/rexessilfie/using-asynclocalstorage-in-nextjs-44c8)
- 서버사이드에서 모든게 이루어질 app directory, react 서버 컴포넌트에서는 서버간 컨택스트 관리가 필수입니다. 알아두면 손해볼 일은 없습니다

- 생각보다 업계에서 많이 사용합니다

- 카카오웹툰에 잠깐 DataDog 의 node.js APM 을 도입했었는데 여기도 이 기술 그대로 사용합니다. 당시에 너무 궁금해서 까봤는데 http-context 사용하고 있더라구요.
- datadog 이 이쪽 업계 1위나 다름 없어서.. 성능은 검증됐다고 봐도 됩니다.

서버사이드 컨택스트 <- 이 부분 꼭 다들 개인적이나 팀적으로 공부하면 좋을 것 같아 한번 언급합니다 (편집됨)