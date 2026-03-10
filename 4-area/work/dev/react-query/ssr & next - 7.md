--- 
creation date: 23-03-20 
modification date: 23-03-20 
tag: 

---


## 고정랜딩 페이지

- 요일
	- 무한스크롤 O
- 이벤트
	- 무한스크롤 X
	- 페이지 값을 받긴함.
- 무료시리즈
	- O
- 오리지널
	- O
- 랭킹
	- O
- 오늘신작
	- X
- 오늘UP
	- O


---

- motivation
	- 리덕스와 연결된 컴포넌트 렌더링 하려면 별개 스토어 인스턴스가 필요한데 이게 복잡함
	- page의 getInitialProps 동안 Store 접근이 필요함
	- next-redux-wrapper가 자동으로 동일한 상태갖는 스토어 인스턴스 만들어줌. 
- usage
	- 

- HYDRATE Action은 언제 dispatch 되는가
	- page가 getStaticProps나 getServerSideProps 가지고 있는 페이지 열릴 때 
	- 초기 페이지 로드와 regular page nvigation 동안 일어남. 
	- payload는 static generation or server side rendering하는 순간의 `state`를 포함한다.
	- 그래서 리듀서는 기존 클라이언트 상태와 적절하게 머지해야한다. 


### how it works





















































