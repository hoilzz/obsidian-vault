---
created: 24-12-02
modified: 24-12-02
tags:
---
클라에서 React가 브라우저 DOM을 업데이트 하는데 사용. RSC payload는 다음을 포함
 - Server Component 렌더링 결과
 - 클라 컴포넌트가 렌더링 되어야할 위치에 대한 자리 표시자와 JS 파일에 대한 참조
 - Server Component에서 클라 컴포넌트로 전달된 모든 props