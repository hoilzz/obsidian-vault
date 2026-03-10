--- 
creation date: 23-03-18 
modification date: 23-03-18 
tag: 

---

## 페이지 getInitialProps로 변경시 hydrate에서 action.paylod가 없는 이슈

- action.paylod 출처는?
- 어디선가 서버환경에서만 호출되는 API가 있을 거 같다. 


- getInitialProps에서 APi는 제ㅐㄷ로 호출함
- CSR인데 hydrate는 왜 호출됨? 여튼 이게 호출되면서 layout 값이 없음
- 왜 hydrate의 action.payload는 gSSP에서만 있고, initialProps에는 없음?