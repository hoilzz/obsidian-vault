--- 
creation date: 23-03-07 
modification date: 23-03-07 
tag: 

---

[[ssr & next - 2]]에 이어서

https://github.com/vercel/next.js/discussions/19611
https://twitter.com/TkDodo/status/1558752788393476096

- gSSP는 모든 요청에서 실행된다. 그래서 client-side에서 데이터를 로드하기 위해 기존 캐시 사용할 방법이 없음. 데이터가 클라이언트 캐시에 있는덷도 서버에 불필요한 요청이 일어남.
- 클라이언트에서 라우트 변경되는 걸 감지하기 위해 getInitialProps
- gSSP 사용하려면 제일 좋은 방법은 초기 페이지 로드시 딱 한 번 실행되는거.  클라측에서 경로 변환될 때 호출 안되야
